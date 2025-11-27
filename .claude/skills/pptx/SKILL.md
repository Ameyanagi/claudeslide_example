# PPTX Editing Skill

Guide for editing PowerPoint presentations through direct XML manipulation.

## Overview

PPTX files are ZIP archives containing Office Open XML (OOXML) files. This project extracts the PPTX to `work/` for direct editing.

## Directory Structure

| Path | Description |
|------|-------------|
| `work/ppt/slides/` | Individual slide XML files |
| `work/ppt/slideLayouts/` | Layout templates |
| `work/ppt/slideMasters/` | Master slide definitions |
| `work/ppt/theme/` | Theme colors and fonts |
| `work/ppt/media/` | Images, audio, video |
| `work/ppt/_rels/` | Relationship files |
| `work/docProps/` | Document metadata |
| `work/[Content_Types].xml` | MIME type registry |

## Common Tasks

### Edit Text
1. Open `work/ppt/slides/slideN.xml`
2. Find `<a:t>` elements containing text
3. Modify the text content
4. Run `npm run validate` to check

### Add Image
1. Copy image to `work/ppt/media/`
2. Add relationship in `work/ppt/slides/_rels/slideN.xml.rels`
3. Register content type in `work/[Content_Types].xml` if new format
4. Add `<p:pic>` element in slide XML referencing the relationship ID

### Add New Slide
1. Create `work/ppt/slides/slideN.xml` (copy existing slide as template)
2. Create `work/ppt/slides/_rels/slideN.xml.rels`
3. Add slide entry in `work/ppt/presentation.xml`
4. Add relationship in `work/ppt/_rels/presentation.xml.rels`
5. Register in `work/[Content_Types].xml`

### Reorder Slides
Edit `work/ppt/presentation.xml` and reorder `<p:sldId>` elements in `<p:sldIdLst>`.

### Delete Slide
1. Remove `<p:sldId>` from `work/ppt/presentation.xml`
2. Remove relationship from `work/ppt/_rels/presentation.xml.rels`
3. Remove override from `work/[Content_Types].xml`
4. Delete slide XML and its _rels file

## Commands

- `npm run validate` - Validate XML structure
- `npm run save` - Package back to PPTX
- `npm run preview` - Generate PNG previews (requires LibreOffice)

## Tips

- Always validate after making changes
- Keep relationship IDs unique within each _rels file
- Backup before major changes (source.pptx is your original)
- Use existing slides as templates for new content
