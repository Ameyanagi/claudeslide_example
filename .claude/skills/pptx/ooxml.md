# Office Open XML Reference for PowerPoint

Technical reference for PPTX XML structure and editing.

## XML Namespaces

Common namespace prefixes used in PPTX files:

| Prefix | Namespace URI | Purpose |
|--------|--------------|---------|
| `a:` | drawingml/2006/main | DrawingML (shapes, text) |
| `p:` | presentationml/2006/main | PresentationML (slides) |
| `r:` | officeDocument/2006/relationships | Relationships |
| `c:` | drawingml/2006/chart | Charts |

## Slide Structure

```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<p:sld xmlns:a="http://schemas.openxmlformats.org/drawingml/2006/main"
       xmlns:p="http://schemas.openxmlformats.org/presentationml/2006/main"
       xmlns:r="http://schemas.openxmlformats.org/officeDocument/2006/relationships">
  <p:cSld>
    <p:spTree>
      <p:nvGrpSpPr>
        <p:cNvPr id="1" name=""/>
        <p:cNvGrpSpPr/>
        <p:nvPr/>
      </p:nvGrpSpPr>
      <p:grpSpPr/>
      <!-- Shapes and content here -->
    </p:spTree>
  </p:cSld>
</p:sld>
```

## Text Box Shape

```xml
<p:sp>
  <p:nvSpPr>
    <p:cNvPr id="2" name="TextBox 1"/>
    <p:cNvSpPr txBox="1"/>
    <p:nvPr/>
  </p:nvSpPr>
  <p:spPr>
    <a:xfrm>
      <a:off x="914400" y="914400"/>
      <a:ext cx="5486400" cy="914400"/>
    </a:xfrm>
    <a:prstGeom prst="rect">
      <a:avLst/>
    </a:prstGeom>
  </p:spPr>
  <p:txBody>
    <a:bodyPr/>
    <a:lstStyle/>
    <a:p>
      <a:r>
        <a:rPr lang="en-US" dirty="0"/>
        <a:t>Your text here</a:t>
      </a:r>
      <a:endParaRPr lang="en-US" dirty="0"/>
    </a:p>
  </p:txBody>
</p:sp>
```

## Text Formatting

### Bold, Italic, Underline
```xml
<a:r>
  <a:rPr b="1" i="1" u="sng" dirty="0"/>
  <a:t>Bold italic underlined</a:t>
</a:r>
```

Attributes: `b="1"` (bold), `i="1"` (italic), `u="sng"` (single underline)

### Font and Size
```xml
<a:rPr lang="en-US" sz="2400" dirty="0">
  <a:latin typeface="Arial"/>
</a:rPr>
```

Size is in hundredths of a point: `sz="2400"` = 24pt

### Text Color
```xml
<a:rPr dirty="0">
  <a:solidFill>
    <a:srgbClr val="FF0000"/>
  </a:solidFill>
</a:rPr>
```

### Theme Color
```xml
<a:solidFill>
  <a:schemeClr val="accent1"/>
</a:solidFill>
```

Values: `dk1`, `lt1`, `dk2`, `lt2`, `accent1`-`accent6`, `hlink`, `folHlink`

## Bullet Lists

```xml
<a:p>
  <a:pPr lvl="0">
    <a:buChar char="•"/>
  </a:pPr>
  <a:r>
    <a:t>First item</a:t>
  </a:r>
</a:p>
<a:p>
  <a:pPr lvl="1">
    <a:buChar char="○"/>
  </a:pPr>
  <a:r>
    <a:t>Sub-item</a:t>
  </a:r>
</a:p>
```

`lvl` attribute sets indentation level (0-8).

### Numbered Lists
```xml
<a:pPr lvl="0">
  <a:buAutoNum type="arabicPeriod"/>
</a:pPr>
```

Types: `arabicPeriod`, `arabicParenR`, `romanUcPeriod`, `alphaUcPeriod`

## Shapes

### Rectangle
```xml
<p:sp>
  <p:nvSpPr>
    <p:cNvPr id="3" name="Rectangle 1"/>
    <p:cNvSpPr/>
    <p:nvPr/>
  </p:nvSpPr>
  <p:spPr>
    <a:xfrm>
      <a:off x="1000000" y="1000000"/>
      <a:ext cx="2000000" cy="1000000"/>
    </a:xfrm>
    <a:prstGeom prst="rect">
      <a:avLst/>
    </a:prstGeom>
    <a:solidFill>
      <a:srgbClr val="4472C4"/>
    </a:solidFill>
    <a:ln w="12700">
      <a:solidFill>
        <a:srgbClr val="2F528F"/>
      </a:solidFill>
    </a:ln>
  </p:spPr>
</p:sp>
```

### Common Preset Geometries
- `rect` - Rectangle
- `roundRect` - Rounded rectangle
- `ellipse` - Circle/Ellipse
- `triangle` - Triangle
- `rightArrow` - Arrow
- `star5` - 5-point star
- `line` - Line

## Images

```xml
<p:pic>
  <p:nvPicPr>
    <p:cNvPr id="4" name="Picture 1"/>
    <p:cNvPicPr>
      <a:picLocks noChangeAspect="1"/>
    </p:cNvPicPr>
    <p:nvPr/>
  </p:nvPicPr>
  <p:blipFill>
    <a:blip r:embed="rId2"/>
    <a:stretch>
      <a:fillRect/>
    </a:stretch>
  </p:blipFill>
  <p:spPr>
    <a:xfrm>
      <a:off x="1000000" y="1000000"/>
      <a:ext cx="3000000" cy="2000000"/>
    </a:xfrm>
    <a:prstGeom prst="rect">
      <a:avLst/>
    </a:prstGeom>
  </p:spPr>
</p:pic>
```

The `r:embed` attribute references a relationship ID defined in the slide's .rels file.

## Relationship File

`work/ppt/slides/_rels/slide1.xml.rels`:

```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<Relationships xmlns="http://schemas.openxmlformats.org/package/2006/relationships">
  <Relationship Id="rId1" Type="http://schemas.openxmlformats.org/officeDocument/2006/relationships/slideLayout" Target="../slideLayouts/slideLayout1.xml"/>
  <Relationship Id="rId2" Type="http://schemas.openxmlformats.org/officeDocument/2006/relationships/image" Target="../media/image1.png"/>
</Relationships>
```

## Content Types

`work/[Content_Types].xml`:

```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<Types xmlns="http://schemas.openxmlformats.org/package/2006/content-types">
  <Default Extension="png" ContentType="image/png"/>
  <Default Extension="jpeg" ContentType="image/jpeg"/>
  <Default Extension="xml" ContentType="application/xml"/>
  <Default Extension="rels" ContentType="application/vnd.openxmlformats-package.relationships+xml"/>
  <Override PartName="/ppt/presentation.xml" ContentType="application/vnd.openxmlformats-officedocument.presentationml.presentation.main+xml"/>
  <Override PartName="/ppt/slides/slide1.xml" ContentType="application/vnd.openxmlformats-officedocument.presentationml.slide+xml"/>
</Types>
```

## Units

OOXML uses English Metric Units (EMUs):

| Measurement | EMUs |
|-------------|------|
| 1 inch | 914400 |
| 1 cm | 360000 |
| 1 point | 12700 |
| 1 pixel (96 dpi) | 9525 |

Standard slide size (10" x 7.5"): 9144000 x 6858000 EMUs

## Presentation File

`work/ppt/presentation.xml` contains the slide order:

```xml
<p:sldIdLst>
  <p:sldId id="256" r:id="rId2"/>
  <p:sldId id="257" r:id="rId3"/>
  <p:sldId id="258" r:id="rId4"/>
</p:sldIdLst>
```

To reorder slides, change the order of `<p:sldId>` elements.

## Important Notes

1. **Element Order**: Child elements must appear in schema-defined order
   - `<p:txBody>` must contain: `<a:bodyPr>`, `<a:lstStyle>`, then `<a:p>` elements

2. **Dirty Attribute**: Add `dirty="0"` to `<a:rPr>` and `<a:endParaRPr>` elements

3. **Whitespace**: Use `xml:space="preserve"` on `<a:t>` if preserving spaces

4. **Unique IDs**: Each shape needs a unique `id` attribute within the slide

5. **Relationship IDs**: Must be unique within each .rels file and match references
