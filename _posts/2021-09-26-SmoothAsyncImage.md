---
date: 2021-09-26
tags: xcode_13 jank swiftui swift
title: A Smooth AsyncImage Replacement
---

iOS 15.0 introduced a AsyncImage SwiftUI view to enable asynchronously loading URL-based images.

AsyncImage works well for many scenarios, but has several limitations:

1. AsyncImage doesn't provide a mechanism for "inflating" the view before drawing it. This means
that large images will take a long time to render the first time that they are drawn. This can
cause  "hitches" or "jank" when scrolling.

2. AsyncImage doesn't provide a mechanism for scaling large UIImages. This can
cause slow rendering and excessive memory use when displaying large images.

Here's a replacement, SmoothAsyncImage, that fixes these two problems.

<!--more-->

This replacement only provides the most general initializer for AsyncImage. Implementing the
other initializers is left as an exercise for the reader.

``` swift
import SwiftUI
import func AVFoundation.AVMakeRect

// Drop-in replacement for AsyncImage that inflates the UIImage
// on a background thread to avoid scroll jank.
// Inspired by https://talk.objc.io/episodes/S01E258-asyncimage
struct SmoothAsyncImage<Content>: View where Content: View {
  private let url: URL?
  private let scale: CGFloat
  private let transaction: Transaction
  private let content: (AsyncImagePhase) -> Content

  @State private var phase = AsyncImagePhase.empty

  public init(url: URL?, scale: CGFloat = 1, transaction: Transaction = Transaction(),
              @ViewBuilder content: @escaping (AsyncImagePhase) -> Content) {
    self.url = url
    self.scale = scale
    self.transaction = transaction
    self.content = content
  }

  var body: some View {
    GeometryReader { proxy in
      let targetSize = proxy.size
      content(phase)
        .task (id: url) {
          guard let url = url else { return }
          do {
            let uiImage = try await ImageLoader.shared.load(url:url, scale:scale,
                                                            targetSize:targetSize)
            withTransaction(transaction) {
              phase = .success(Image(uiImage:uiImage))
            }
          } catch {
            withTransaction(transaction) {
              phase = .failure(error)
            }
          }
        }
    }
  }
}

actor ImageLoader {

  static let shared = ImageLoader()

  enum ImageLoaderError : Error {
    /// The curent Task was canceled.
    case canceled
    /// The image could not be decoded.
    case undecodable
  }

  /// Load an image asynchronously. The resulting image is "inflated" (decoded to pixels)
  /// before being returned. This means it will render very quickly.
  func load(url: URL, scale: CGFloat = 1, targetSize: CGSize?) async throws -> UIImage {
    let (data, _) = try await URLSession.shared.data(from: url)
    guard !Task.isCancelled else { throw ImageLoaderError.canceled }
    guard var uiImage = UIImage(data: data, scale: scale) else {
      throw ImageLoaderError.undecodable
    }
    guard !Task.isCancelled else { throw ImageLoaderError.canceled }
    if let targetSize = targetSize,
       uiImage.size.width > targetSize.width
        || uiImage.size.height > targetSize.height {
      if let resizedImage = resizedImage(
        uiImage: uiImage,
        scale: scale,
        targetSize: targetSize) {
        uiImage = resizedImage
      }
      guard !Task.isCancelled else { throw ImageLoaderError.canceled }
    }
    // This is an undocumented way of inflating the UIImage.
    // https://developer.apple.com/forums/thread/653738
    _ = uiImage.cgImage?.dataProvider?.data
    return uiImage
  }

  // https://nshipster.com/image-resizing/
  // Technique #1
  private func resizedImage(uiImage: UIImage,
                            scale: CGFloat,
                            targetSize: CGSize) -> UIImage? {
    let scaledBounds = AVMakeRect(
      aspectRatio: uiImage.size,
      insideRect: CGRect(origin: .zero, size: targetSize)
    )
    let format = UIGraphicsImageRendererFormat()
    format.scale = scale
    let renderer = UIGraphicsImageRenderer(bounds:scaledBounds,
                                           format: format)
    return renderer.image { (context) in
      uiImage.draw(in: scaledBounds)
    }
  }
}

struct SmoothImage_Previews: PreviewProvider {
  static var previews: some View {
    SmoothAsyncImage(url: URL(string:"https://via.placeholder.com/350x150")) { phase in
      if let image = phase.image {
        image // Displays the loaded image.
      } else if phase.error != nil {
        Color.red // Indicates an error.
      } else {
        Color.blue // Acts as a placeholder.
      }
    }
  }
}
```
