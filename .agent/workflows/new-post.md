---
description: Create a new blog post with correct naming and metadata
---

1. Determine the current date (YYYY-MM-DD) and time (HH:MM:SS).
2. Ask the user for the post title if not provided.
3. Slugify the title (lowercase, hyphens instead of spaces).
4. Create a new file in `_posts/` named `YYYY-MM-DD-slug.md`.
5. Populate the file with the following frontmatter:
   ```yaml
   ---
   layout: post
   title: "Post Title"
   date: YYYY-MM-DD HH:MM:SS
   tags: tag1 tag2 tag3
   description: "One sentence summary."
   ---
   ```
6. Remind the user about the tone and asset storage rules in `GEMINI.md`.
