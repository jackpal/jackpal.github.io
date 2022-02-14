---
date: 2022-02-13
description: How to animate along a Path in SwiftUI.
tags: Swift Swift_UI Geometry 2D Animation Path Curve
title: Animating along a SwiftUI Path
---

We can use trigonometry and finite differences to animate rigid objects along a SwiftUI path.

The SwiftUI Path class is missing several useful methods for evaluating properties of a path:

+ finding the position (as a CGPoint) of a given fractional position of the path.
+ finding the heading (as an angle) of a given fractional position of the path.
+ finding the total length of the path, measured in points.

Happily, we can write these methods based on the existing `trimmedPath` method.

With the aid of these methods it's possible to
create animations that move rigid bodies along arbitrary paths.

<!--more-->

``` swift
import SwiftUI

fileprivate let defaultEpsilon = 1e-7

extension Path {
  
  /// Returns the position for a point on the path with the given
  /// fractional path value between 0 and 1.
  func evaluate(at: CGFloat,
      epsilon: CGFloat = defaultEpsilon,
      closed: Bool = false) -> CGPoint {
    // Make sure a and b don't go outside the bounds 0 ... 1.0
    var a = at
    var b = at + epsilon
    if closed {
      b = b.truncatingRemainder(dividingBy: 1.0)
    } else {
      if b > 1.0 {
        b = 1.0
        a = b - epsilon
      }
    }
    let littlePieceOfPathFromAToB = self.trimmedPath(from: a, to: b)
    let boundsOfLittlePiece = littlePieceOfPathFromAToB.boundingRect
    let positionOfA = boundsOfLittlePiece.origin
    return positionOfA
  }

  /// Returns the tangent angle in radians for a given fractional path value between 0 and 1.
  /// The tangent angle ranges from -π to π.
  /// An angle of 0 means the curve is pointing in the positive X direction.
  /// The angle increases in the clockwise direction.
  func evaluateTangent(at: CGFloat,
     lookAhead: CGFloat = defaultEpsilon,
     closed: Bool = false) -> CGFloat {
    var a = at
    var b = at + lookAhead
    if closed {
      b = b.truncatingRemainder(dividingBy: 1.0)
    } else {
      if b > 1.0 - lookAhead {
        b = 1.0 - lookAhead
        a = b - lookAhead
      }
    }
    let pa = evaluate(at: a)
    let pb = evaluate(at: b)
    return atan2(pb.y - pa.y, pb.x - pa.x)
  }
  
  /// Return the path length in pixels.
  var pathLength : CGFloat {
    let epsilon = 1e-7
    let sampleParameter = 0.0
    let a = sampleParameter
    let b = sampleParameter + epsilon
    let littlePieceOfPathFromAToB = self.trimmedPath(from: a, to: b)
    let boundsOfLittlePiece = littlePieceOfPathFromAToB.boundingRect
    let dx = boundsOfLittlePiece.width
    let dy = boundsOfLittlePiece.height
    let distance = sqrt(dx * dx + dy * dy)
    let pathLengthEstimate = distance / epsilon
    return pathLengthEstimate
  }
  
}
```

Here's an example SwiftUI view that animates a short word along an arbitrary path:

```swift
func createPath() -> Path {
  var p = Path()
  p.move(to: CGPoint(x: 10, y: 20))
  p.addLine(to: CGPoint(x: 100, y: 20))
  p.addLine(to: CGPoint(x: 100, y: 100))
  p.addCurve(to: CGPoint(x:100, y: 400),
             control1: CGPoint(x:0, y: 200),
             control2: CGPoint(x:200, y: 300))
  p.addLine(to: CGPoint(x:10, y: 300))

  return p
}

struct ContentView: View {
  @State private var startDate = Date()
  private let path = createPath()
  private let animationDuration: TimeInterval = 10
  private let epsilon = 0.0001
    var body: some View {
      TimelineView(.animation(minimumInterval: 1.0 / 120)) { timeline in
        let elapsed = timeline.date.timeIntervalSince(startDate)
        let animationProgress =
            elapsed.truncatingRemainder(dividingBy: animationDuration)
            / animationDuration
        Canvas { context, size in
          context.stroke(path, with:.color(.green))
          let pos = path.evaluate(at:animationProgress)
          // Use a lookAhead to have the car smoothly animate around sharp corners
          let tangentAngle =
              path.evaluateTangent(at: animationProgress, lookAhead:0.01)
          let oldTransform = context.transform
          context.transform = oldTransform
              .translatedBy(x: pos.x, y: pos.y)
              .rotated(by:tangentAngle)
          context.draw(Text("car"), at: CGPoint(x:0, y:-8))
          context.transform = oldTransform
        }
      }
    }
}
```
