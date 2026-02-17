# DXC Branding Assets & Claude Skill

This repository contains DXC branding assets and a custom Claude Code skill for converting text into professionally formatted, DXC-branded Word documents.

## Contents

### Branding Assets

- **Word Template**: `Word Template/DXC Word_A4_DEC 25.dotx` - Official DXC Word template
- **Color Palette**: `Color Palette/Colour Palette Guidance.docx` - Official color palette and usage guidelines
- **Logos**: `DXC Logos/` - Complete set of DXC logos in various formats:
  - Brand Mark (logo only)
  - Tagline Lockup (logo with "Leading Enterprises Forward")
  - Horizontal and Vertical orientations
  - Full Color, 1 Color Dark, 1 Color Light variants
  - RGB and CMYK formats
  - PNG, SVG, and JPG files

### Claude Code Skill

**File**: `dxc-doc-converter-SKILL.md`

A custom skill for Claude Code that converts any text content into branded Word documents following DXC's official branding guidelines.

## Installing the Skill

### Prerequisites

1. **Claude Code CLI** must be installed ([Installation Guide](https://github.com/anthropics/claude-code))
2. **Python 3** with required libraries:
   ```bash
   pip install python-docx pillow
   ```

### Installation Steps

1. **Clone or download this repository** to your local machine

2. **Create the skill directory** (if it doesn't exist):
   ```bash
   mkdir -p ~/.claude/skills/dxc-doc-converter
   ```

3. **Copy the skill file**:
   ```bash
   cp dxc-doc-converter-SKILL.md ~/.claude/skills/dxc-doc-converter/SKILL.md
   ```

4. **Update paths in the skill file** (if your branding assets are in a different location):
   - Open `~/.claude/skills/dxc-doc-converter/SKILL.md`
   - Find and replace `/home/jwilkins25/Documents/BRANDING/` with your actual path
   - Save the file

5. **Restart Claude Code** or start a new conversation to load the skill

## Using the DXC Doc Converter Skill

### Basic Usage

In Claude Code, invoke the skill using the slash command:

```
/dxc-doc-converter
```

Claude will prompt you for:
- Document type (document, presentation, report, memo)
- Content to convert
- Any specific formatting preferences

### Advanced Usage

**Create a presentation from inline text**:
```
/dxc-doc-converter presentation
[Then provide your content when prompted]
```

**Convert a text file to a document**:
```
/dxc-doc-converter document file:path/to/content.txt
```

**Create a formal report**:
```
/dxc-doc-converter report
```

### What the Skill Does

1. ✅ Applies DXC color palette correctly (Canvas, Midnight Blue, accent colors)
2. ✅ Follows color usage guidelines (body text only in Midnight Blue/White)
3. ✅ Inserts appropriate DXC logo (Full Color, Brand Mark, or Tagline Lockup)
4. ✅ Uses the official Word template
5. ✅ Formats documents professionally with proper hierarchy
6. ✅ Ensures accessibility standards are met
7. ✅ Creates single-page presentations or multi-page documents
8. ✅ Saves with descriptive filename: `DXC_[Name]_[Date].docx`

## DXC Color Palette Quick Reference

### Primary Colors (Backgrounds & Body Text)
- **Canvas**: `#F6F3F0` - Primary background color
- **Midnight Blue**: `#0E1020` - Body text and alternate background
- **White**: `#FFFFFF`

### Accent Colors (Highlights, Icons, Visual Elements)
- **True Blue**: `#4995FF`
- **Royal**: `#004AAC`
- **Sky**: `#A1E6FF`
- **Peach**: `#FFC982`
- **Gold**: `#FFAE41`
- **Melon**: `#FF7E51`
- **Red**: `#D14600`

### Usage Rules

⚠️ **IMPORTANT**:
- Body copy must ONLY use Midnight Blue or White
- Accent colors are for icons, bullets, highlights, and visual elements ONLY
- Never use accent colors for paragraphs or large text blocks

## Customizing the Skill

You can modify `~/.claude/skills/dxc-doc-converter/SKILL.md` to:

- Change default logo selection
- Adjust typography preferences
- Add custom document types
- Modify color usage patterns
- Add company-specific formatting rules

After editing, restart Claude Code or start a new conversation for changes to take effect.

## Making it Portable

To share this skill with your team:

1. **Ensure branding assets are in a shared location** (network drive, shared repository)
2. **Update paths** in `dxc-doc-converter-SKILL.md` to point to the shared location
3. **Document the shared path** in team instructions
4. **Share the skill file** via GitLab, email, or internal documentation
5. **Provide installation instructions** (link to this README)

### Team Installation Quick Start

Share these instructions with your team:

```bash
# 1. Install dependencies
pip install python-docx pillow

# 2. Create skill directory
mkdir -p ~/.claude/skills/dxc-doc-converter

# 3. Download skill file from GitLab
git clone https://github.com/Cenarkion/dxc-doc-converter-skill.git

# 4. Copy to skills directory
cp dxc-doc-converter-SKILL.md ~/.claude/skills/dxc-doc-converter/SKILL.md

# 5. Update branding assets path in the skill file if needed

# 6. Restart Claude Code
```

## Troubleshooting

### Skill not showing up
- Verify file is at: `~/.claude/skills/dxc-doc-converter/SKILL.md`
- Restart Claude Code or start a new conversation
- Check Claude Code version (requires recent version with skills support)

### Python dependencies missing
```bash
pip install python-docx pillow
```

### Logo/template files not found
- Update paths in the SKILL.md file to match your branding assets location
- Ensure all files are accessible from the location specified

### Document formatting issues
- Ensure python-docx is up to date: `pip install --upgrade python-docx`
- Check that the Word template file is not corrupted
- Try creating a document without the template first

## License & Usage

These branding assets and the custom skill are for internal DXC use only. Do not share branding assets outside of authorized personnel.

## Support

For issues or questions:
- Check the Claude Code documentation: https://github.com/anthropics/claude-code
- Review the skill file for detailed usage instructions
- Contact your internal IT/branding team for branding asset questions

---

**Version**: 1.0
**Last Updated**: 2026-02-17
**Compatible with**: Claude Code (latest)
