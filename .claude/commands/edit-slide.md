# Edit Slide

Edit the content of a specific slide.

## Instructions

1. Ask the user which slide number to edit (or they may have specified it)
2. Read the slide XML from `work/ppt/slides/slideN.xml`
3. Locate the text elements (`<a:t>` tags) and shapes
4. Make the requested changes
5. Run validation after editing

Common edits:
- Change text content in `<a:t>` tags
- Modify shape positions in `<a:xfrm>` elements
- Update colors and styles
