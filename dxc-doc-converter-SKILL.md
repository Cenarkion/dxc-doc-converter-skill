---
name: dxc-doc-converter
description: Convert any provided text into a professionally formatted Word document using DXC branding, including color palette, logos, and templates. Use for creating branded documents or presentations.
argument-hint: [document-type] [text-source]
allowed-tools: Read, Bash, Write, AskUserQuestion
disable-model-invocation: false
context: fork
---

# DXC Branded Document Converter Skill

Convert text content into professionally formatted Word documents (.docx) or single-page presentations using DXC's official branding guidelines, color palette, and templates.

## Your Task

Create a DXC-branded Word document from provided text content, following all branding guidelines for colors, logos, and formatting.

**CRITICAL REQUIREMENTS**:
- If the source document contains tables, you MUST preserve them in the output. Tables are key structural elements and must never be omitted. Always scan for and include all tables with proper DXC formatting (Midnight Blue headers with White text, Midnight Blue body text).
- For **document** and **report** formats, you MUST include a Table of Contents (TOC) after the title page. This is a professional standard for multi-page documents and enables easy navigation.
- **ALWAYS use doc.add_heading() with level=1, 2, or 3 for all section headings**. This applies proper Word Heading styles which are REQUIRED for TOC functionality. Never create headings with manual formatting only - the TOC cannot detect them.

### Input Arguments (Optional)

- **First argument**: Document type
  - `document` or `doc`: Multi-page document (default)
  - `presentation` or `pres` or `slide`: Single-page presentation/slide format
  - `report`: Formal report format
  - `memo`: Internal memo format

- **Second argument**: Text source
  - `file:path/to/file.txt`: Read content from a text file
  - `clipboard`: Use content from clipboard (user will provide)
  - Direct text can be provided after the command
  - If not provided, prompt user for content

### DXC Branding Assets

**Base Directory**: `/home/jwilkins25/Documents/BRANDING/`

**Available Resources**:
1. **Word Template**: `Word Template/DXC Word_A4_DEC 25.dotx`
2. **Color Palette Guidelines**: `Color Palette/Colour Palette Guidance.docx`
3. **Logos**: `DXC Logos/` with multiple variations

### DXC Color Palette

#### Primary Colors (Backgrounds & Body Copy)
- **Canvas**: `#F6F3F0` (RGB: 246,243,240) - Primary background
- **Midnight Blue**: `#0E1020` (RGB: 14,16,32) - Primary text and alternate background
- **White**: `#FFFFFF` (RGB: 255,255,255)

#### Accent Colors (Icons, Highlights, Visual Elements)
- **True Blue**: `#4995FF` (RGB: 73,149,255)
- **Royal**: `#004AAC` (RGB: 0,74,172)
- **Sky**: `#A1E6FF` (RGB: 161,230,255)
- **Peach**: `#FFC982` (RGB: 255,201,130)
- **Gold**: `#FFAE41` (RGB: 255,174,65)
- **Melon**: `#FF7E51` (RGB: 255,126,81)
- **Red**: `#D14600` (RGB: 209,70,0)

### DXC Color Usage Guidelines

**IMPORTANT RULES**:
1. **Backgrounds**: Use Canvas (#F6F3F0) as primary background, Midnight Blue (#0E1020) for impact
2. **Body Text**: ONLY use Midnight Blue (#0E1020) or White (#FFFFFF) - NEVER use accent colors for body copy
3. **Accent Colors** are for:
   - Icons and small visual elements
   - Bullets and arrows
   - Section dividers
   - Highlighting key words or phrases (underline/bold)
   - Infographics and data visualization
   - Headers (sparingly)

4. **Accent Colors** are NOT for:
   - Body copy or paragraphs
   - Large solid color backgrounds
   - Entire sections of text

5. **Color Pairing**: When using multiple accent colors, follow logo directionality:
   - Blues on edges, golds in middle
   - Example sequence: Sky → True Blue → Gold → Melon

6. **Accessibility**:
   - On Canvas/White backgrounds: Use Midnight Blue, Royal, Red for text/icons
   - On Midnight Blue backgrounds: Use White, Sky, Peach for text/icons

### Logo Selection Guide

**Logos Directory**: `/home/jwilkins25/Documents/BRANDING/DXC Logos/`

**Logo Types**:
1. **Brand Mark** (Logo only, no tagline)
   - Path: `DXC Logos/Brand Mark/`
   - Use for: Headers, compact spaces

2. **Tagline Lockup** (Logo with "Leading Enterprises Forward" tagline)
   - **Horizontal**: `DXC Logos/Tagline Lockup/Horizontal/`
   - **Vertical**: `DXC Logos/Tagline Lockup/Vertical/`
   - Use for: Title pages, formal documents

**Color Variants**:
- **Full Color**: Multi-colored logo (preferred for documents with color)
- **1 Color Dark**: For light backgrounds (Canvas, White)
- **1 Color Light**: For Midnight Blue backgrounds

**Format Selection**:
- Use **PNG** for Word documents (better compatibility)
- Path pattern: `RGB/DXC-[Type]-[Color].png`

**Recommended Defaults**:
- **Document header**: `DXC Logos/Brand Mark/Full Color/RGB/DXC-Full-Color.png`
- **Title page**: `DXC Logos/Tagline Lockup/Horizontal/Full Color/RGB/DXC-Horizontal-Tagline-Full-Color-Dark.png`
- **Presentation**: `DXC Logos/Tagline Lockup/Horizontal/Full Color/RGB/DXC-Horizontal-Tagline-Full-Color-Dark.png`

### Document Creation Process

#### Step 1: Gather Content
1. If text source argument provided, read the content
2. If no source, use AskUserQuestion to ask for:
   - Document title
   - Document type preference (document/presentation/report/memo)
   - Main content
   - Any specific formatting requests

#### Step 2: Analyze & Structure Content
1. Identify document structure:
   - Title and subtitle
   - Main sections/headings
   - Body content
   - Lists or bullet points
   - **Tables** (CRITICAL: Must be preserved and properly formatted)
   - Any data or key metrics
   - Call-to-action or conclusion

2. Determine appropriate formatting:
   - Which sections need emphasis (use accent colors)
   - Where to use bullets vs. numbered lists
   - Key phrases to highlight
   - **Table formatting** (headers, colors, borders)

#### Step 3: Create Document with Python-docx

Use the `python-docx` library to programmatically create the Word document:

```python
from docx import Document
from docx.shared import Inches, Pt, RGBColor
from docx.enum.text import WD_ALIGN_PARAGRAPH
from docx.oxml.ns import qn
from docx.oxml import OxmlElement

# DXC Color Palette (RGB values)
COLORS = {
    'canvas': RGBColor(246, 243, 240),
    'midnight_blue': RGBColor(14, 16, 32),
    'white': RGBColor(255, 255, 255),
    'true_blue': RGBColor(73, 149, 255),
    'royal': RGBColor(0, 74, 172),
    'sky': RGBColor(161, 230, 255),
    'peach': RGBColor(255, 201, 130),
    'gold': RGBColor(255, 174, 65),
    'melon': RGBColor(255, 126, 81),
    'red': RGBColor(209, 70, 0)
}

# Create document from template or new
doc = Document('/home/jwilkins25/Documents/BRANDING/Word Template/DXC Word_A4_DEC 25.dotx')
# OR if template doesn't work: doc = Document()

# Add logo (top right or centered)
logo_path = '/home/jwilkins25/Documents/BRANDING/DXC Logos/Brand Mark/Full Color/RGB/DXC-Full-Color.png'
header = doc.sections[0].header
header_para = header.paragraphs[0]
header_para.alignment = WD_ALIGN_PARAGRAPH.RIGHT
run = header_para.add_run()
run.add_picture(logo_path, width=Inches(1.5))

# Title (Midnight Blue, bold, large) - Use level=0 for title, won't appear in TOC
title = doc.add_heading('Document Title Here', level=0)
title.runs[0].font.color.rgb = COLORS['midnight_blue']
title.runs[0].font.size = Pt(28)
title.runs[0].font.bold = True

# CRITICAL: Use doc.add_heading() with levels 1-3 for section headings
# These will automatically use Heading 1, 2, 3 styles which TOC requires

# Major section heading (will appear in TOC)
heading1 = doc.add_heading('Executive Summary', level=1)
heading1.runs[0].font.color.rgb = COLORS['midnight_blue']
heading1.runs[0].font.size = Pt(20)

# Subsection heading (will appear in TOC)
heading2 = doc.add_heading('Core Principles', level=2)
heading2.runs[0].font.color.rgb = COLORS['midnight_blue']
heading2.runs[0].font.size = Pt(16)

# Body paragraphs (Midnight Blue on Canvas background)
para = doc.add_paragraph('Body text goes here...')
para.runs[0].font.color.rgb = COLORS['midnight_blue']
para.runs[0].font.size = Pt(11)

# Highlighted text (use accent color sparingly)
para = doc.add_paragraph()
para.add_run('This is regular text and ')
highlight = para.add_run('this is highlighted')
highlight.font.color.rgb = COLORS['royal']  # Accent color
highlight.bold = True
para.add_run(' text.')

# Bullet points with accent color bullets
# (Bullets themselves can be accent colored, but text should be Midnight Blue)

# TABLE OF CONTENTS - CRITICAL: Add TOC with page numbers for document and report formats
# TOC should appear after title page, before main content

def add_table_of_contents_with_field(doc):
    """
    Add a proper TOC field that Word will populate with page numbers.
    This creates a dynamic TOC that updates when the document is opened in Word.
    """
    from docx.oxml import OxmlElement
    from docx.oxml.ns import qn

    # Add TOC heading
    toc_heading = doc.add_paragraph()
    toc_run = toc_heading.add_run('Table of Contents')
    toc_run.font.size = Pt(20)
    toc_run.font.bold = True
    toc_run.font.color.rgb = COLORS['midnight_blue']
    toc_heading.alignment = WD_ALIGN_PARAGRAPH.CENTER
    toc_heading.space_after = Pt(12)

    # Add spacing
    doc.add_paragraph()

    # Create TOC field paragraph
    paragraph = doc.add_paragraph()
    run = paragraph.add_run()

    # Add the field beginning
    fldChar1 = OxmlElement('w:fldChar')
    fldChar1.set(qn('w:fldCharType'), 'begin')

    # Add instruction text for TOC
    instrText = OxmlElement('w:instrText')
    instrText.set(qn('xml:space'), 'preserve')
    instrText.text = 'TOC \\o "1-3" \\h \\z \\u'

    # Add field separator
    fldChar2 = OxmlElement('w:fldChar')
    fldChar2.set(qn('w:fldCharType'), 'separate')

    # Add placeholder text
    t = OxmlElement('w:t')
    t.text = 'Right-click to update field'

    # Add field end
    fldChar3 = OxmlElement('w:fldChar')
    fldChar3.set(qn('w:fldCharType'), 'end')

    # Append all elements to the run
    run._r.append(fldChar1)
    run._r.append(instrText)
    run._r.append(fldChar2)
    run._r.append(t)
    run._r.append(fldChar3)

    # Add instruction note
    doc.add_paragraph()
    note = doc.add_paragraph()
    note_run = note.add_run('Note: Open this document in Microsoft Word and right-click on the field above, then select "Update Field" to populate the table of contents with page numbers.')
    note_run.font.size = Pt(9)
    note_run.font.italic = True
    note_run.font.color.rgb = COLORS['royal']

    # Add page break after TOC
    doc.add_page_break()

# ALTERNATIVE: If TOC field causes issues, use this static version with page numbers
def add_table_of_contents_static(doc, sections_with_pages):
    """
    Add a static TOC with page numbers.
    sections_with_pages should be a list of tuples: [('Section Name', page_number), ...]
    """
    # Add TOC heading
    toc_heading = doc.add_paragraph()
    toc_run = toc_heading.add_run('Table of Contents')
    toc_run.font.size = Pt(20)
    toc_run.font.bold = True
    toc_run.font.color.rgb = COLORS['midnight_blue']
    toc_heading.alignment = WD_ALIGN_PARAGRAPH.CENTER
    toc_heading.space_after = Pt(12)

    doc.add_paragraph()

    # Add TOC entries with page numbers
    for section_name, page_num in sections_with_pages:
        toc_para = doc.add_paragraph()

        # Section name (left-aligned)
        name_run = toc_para.add_run(section_name)
        name_run.font.size = Pt(12)
        name_run.font.color.rgb = COLORS['midnight_blue']

        # Add dots (leaders)
        dots_run = toc_para.add_run(' ' + '.' * 80)
        dots_run.font.size = Pt(12)
        dots_run.font.color.rgb = RGBColor(200, 200, 200)

        # Page number (right-aligned)
        page_run = toc_para.add_run(' ' + str(page_num))
        page_run.font.size = Pt(12)
        page_run.font.color.rgb = COLORS['midnight_blue']
        page_run.font.bold = True

        # Set up tab stops for proper alignment
        toc_para.paragraph_format.tab_stops.add_tab_stop(Inches(6.0), 'RIGHT', '.')

    # Add page break after TOC
    doc.add_page_break()

# RECOMMENDED: Use add_table_of_contents_with_field(doc) for dynamic TOC
# Use add_table_of_contents_static(doc, sections) if field approach fails

# TABLES - CRITICAL: Always preserve and format tables from source documents
table = doc.add_table(rows=3, cols=3)
table.style = 'Light Grid Accent 1'

# Header row: Midnight Blue background with White text
header_cells = table.rows[0].cells
for cell in header_cells:
    cell.text = 'Header'
    # Set background color to Midnight Blue
    shading_elm = OxmlElement('w:shd')
    shading_elm.set(qn('w:fill'), '0E1020')  # Midnight Blue
    cell._element.get_or_add_tcPr().append(shading_elm)
    # Set text color to White
    for paragraph in cell.paragraphs:
        for run in paragraph.runs:
            run.font.color.rgb = COLORS['white']
            run.font.bold = True

# Body rows: Midnight Blue text on White/Canvas background
for row in table.rows[1:]:
    for cell in row.cells:
        cell.text = 'Data'
        for paragraph in cell.paragraphs:
            for run in paragraph.runs:
                run.font.color.rgb = COLORS['midnight_blue']

# Save document
doc.save('DXC_Document_Output.docx')
```

#### Step 4: Apply DXC Formatting Standards

**Typography**:
- **Title**: 24-28pt, Bold, Midnight Blue (use level=0 with add_heading())
- **Heading 1**: 18-20pt, Bold, Midnight Blue (use level=1 with add_heading() - REQUIRED for TOC)
- **Heading 2**: 14-16pt, Bold, Midnight Blue (use level=2 with add_heading() - REQUIRED for TOC)
- **Heading 3**: 12-14pt, Bold, Midnight Blue (use level=3 with add_heading() - REQUIRED for TOC)
- **Body**: 11-12pt, Regular, Midnight Blue (use add_paragraph())
- **Emphasis**: Bold or accent color (for key phrases only)

**CRITICAL**: Always use doc.add_heading() for headings, never just format regular paragraphs to look like headings. This ensures proper Word Heading styles are applied, which is required for TOC functionality.

**Spacing**:
- Adequate white space between sections
- 1.15-1.5 line spacing for body text
- Margins: 1 inch (or per template)

**Visual Elements**:
- Use accent colors for divider lines between sections
- Color bullets or numbers with accent colors
- Keep accent color usage contained and purposeful

**Page Background**:
- Default: Canvas (#F6F3F0) or White
- Optional: Midnight Blue for impact pages (ensure text is White/light)

#### Step 5: Document Type Variations

**For Multi-Page Documents**:
- Logo in header (top right, 1.5 inches wide)
- Title page with horizontal tagline lockup logo (centered)
- **Table of Contents page** (after title page, before main content)
- Consistent header/footer on subsequent pages
- Section breaks with accent color dividers
- Use heading styles (Heading 1, Heading 2) to ensure TOC auto-populates correctly

**For Single-Page Presentations/Slides**:
- Larger logo (2-3 inches, centered top or corner)
- Bigger fonts (Title: 32-36pt, Body: 14-16pt)
- More white space
- Prominent use of 1-2 accent colors for visual interest
- Bullet points or key messages only
- No dense paragraphs

**For Reports**:
- Professional title page with vertical tagline logo
- **Table of Contents page** (REQUIRED - after title page, before main content)
- Headers with section names
- Page numbers in footer
- Consistent heading hierarchy (use Heading 1, 2, 3 styles for TOC)
- TOC should list all major sections and subsections

**For Memos**:
- Compact logo (top left, 1 inch)
- To/From/Date/Subject header
- Concise formatting
- Less formal structure

### Quality Checklist

Before finalizing the document, verify:

- [ ] Logo is appropriate size and positioned correctly
- [ ] **Table of Contents page included for document and report formats** (after title page)
- [ ] **TOC field is properly inserted and will update when opened in Word**
- [ ] **ALL section headings created with doc.add_heading(level=1/2/3) - NOT manual formatting**
- [ ] **Verified headings use Word Heading styles (check in Word: select heading, look at style name)**
- [ ] Body text uses ONLY Midnight Blue or White (no accent colors for paragraphs)
- [ ] Accent colors used only for highlights, bullets, icons, dividers
- [ ] **ALL TABLES from source document are preserved and properly formatted**
- [ ] **Table headers use Midnight Blue background with White text**
- [ ] **Table body text uses Midnight Blue color for readability**
- [ ] Color combinations follow accessibility guidelines
- [ ] Background is Canvas or White (Midnight Blue only for impact)
- [ ] Typography hierarchy is clear and consistent
- [ ] Adequate white space and readability
- [ ] No generic placeholders - all content is final
- [ ] File saved with descriptive name (e.g., `DXC_ProjectName_Document.docx`)

### Output

1. **Create the Word document** using Python's `python-docx` library
2. **Save to current directory** with format: `DXC_[DocumentName]_[Date].docx`
3. **Inform the user**:
   - Document location
   - File size
   - Brief summary of applied branding (colors used, logo variant)
   - Any customization options available

### Error Handling

- If template file is unavailable, create document from scratch
- If logo files are inaccessible, proceed without logo but warn user
- If python-docx is not installed, provide installation instructions
- If content is too long for single-page presentation, warn user and suggest document format

### Dependencies

Required Python libraries:
```bash
pip install python-docx pillow
```

### Example Usage

**User invokes**: `/dxc-doc-converter presentation`
- Prompt user for presentation content
- Create single-page presentation with large fonts
- Use horizontal tagline logo centered at top
- Apply Canvas background with Midnight Blue text
- Use True Blue and Gold accents for key points
- Save as `DXC_Presentation_2026-02-17.docx`

**User invokes**: `/dxc-doc-converter document file:content.txt`
- Read content from content.txt
- Create multi-page document
- Use brand mark logo in header
- Structure content with proper headings
- Apply DXC color palette per guidelines
- Save as `DXC_Document_2026-02-17.docx`

### Important Notes

- **Always follow branding guidelines** - this maintains DXC's brand consistency
- **Ask before assuming** - if document type or formatting is unclear, use AskUserQuestion
- **Preserve readability** - don't overuse accent colors; keep it professional
- **Test accessibility** - ensure color contrasts meet guidelines
- **Be creative within constraints** - DXC branding is flexible while maintaining consistency

### Tips for Best Results

1. **Start with structure**: Outline the content hierarchy before formatting
2. **Use the template**: The DXC Word template has pre-configured styles
3. **Less is more with accent colors**: Use them strategically for impact
4. **Maintain visual balance**: Follow the 80/20 rule (80% Canvas/Midnight Blue, 20% accents)
5. **Consider the audience**: Internal docs can be less formal than client-facing
6. **Leverage white space**: It improves readability and looks professional

---

## Advanced Features (Optional)

If user requests advanced formatting:

### Add Data Visualizations
- Use accent colors in charts/graphs following color pairing logic
- Blues on edges, golds in middle
- Ensure accessibility on chosen background

### Add Tables
- Header row: Midnight Blue background with White text
- Alternating rows: Canvas and White for readability
- Borders: Thin lines in Midnight Blue or accent color

### Add Footer with Metadata
- Small text (8-9pt)
- Include: Document title, date, page numbers
- Light gray or Midnight Blue

### Multi-Column Layouts
- Useful for presentations or marketing materials
- Maintain adequate spacing between columns
- Use accent color dividers

### Background Shapes/Graphics
- Subtle geometric shapes in accent colors
- Must not interfere with readability
- Keep opacity low (10-20%) if behind text

---

Remember: The goal is to create professional, on-brand documents that are visually appealing while maintaining clarity and accessibility. When in doubt, err on the side of simplicity and let the content shine.
