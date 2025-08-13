# RoomsWriteups Repository Instructions

**ALWAYS reference these instructions first and fallback to search or bash commands only when you encounter unexpected information that does not match the info here.**

## Repository Overview
RoomsWriteups is a documentation repository containing cybersecurity challenge writeups and walkthroughs. Each challenge gets its own folder with a detailed README.md and supporting images showing the solution steps.

## Working Effectively

### Repository Structure
- Each writeup gets its own folder (e.g., `aklaaX/`)
- Each folder contains:
  - `README.md` - Detailed walkthrough of the challenge
  - `img/` directory - Screenshots and images supporting the writeup
- Main `README.md` - Simple repository description

### Bootstrap and Setup
This repository requires no build tools, dependencies, or compilation. It's purely documentation.
- Clone the repository and you're ready to work
- No installation steps required - everything runs on basic system tools
- Validation tools use Python 3 and markdown-it-py (pre-installed)

### Validation Workflow
Always validate changes before committing:
```bash
# Validate all markdown files and image links (takes ~0.1 seconds)
python3 -c "
import markdown_it
from pathlib import Path
import re
import sys

def validate_markdown_files():
    '''Validate all markdown files in the repository'''
    md = markdown_it.MarkdownIt()
    errors = []
    
    md_files = list(Path('.').glob('**/*.md'))
    print(f'Found {len(md_files)} markdown files')
    
    for md_file in md_files:
        print(f'Validating {md_file}...')
        try:
            content = md_file.read_text(encoding='utf-8')
            result = md.parse(content)
            print(f'  ✓ {len(result)} tokens parsed successfully')
            
            # Check image links
            img_refs = re.findall(r'!\[.*?\]\((.*?)\)', content)
            for img_ref in img_refs:
                img_path = (md_file.parent / img_ref).resolve()
                if not img_path.exists():
                    errors.append(f'{md_file}: Missing image {img_ref}')
                    print(f'  ✗ Missing image: {img_ref}')
                else:
                    print(f'  ✓ Image exists: {img_ref}')
                    
        except Exception as e:
            errors.append(f'{md_file}: {str(e)}')
            print(f'  ✗ Error: {e}')
    
    return errors

errors = validate_markdown_files()
print(f'\n{len(errors)} errors found:')
for error in errors:
    print(f'  - {error}')

sys.exit(len(errors))
"
```

### Creating New Writeups
Follow this structure for consistency:
1. Create a new folder with the challenge name: `mkdir challengeName/`
2. Create the main writeup: `challengeName/README.md`
3. Create images directory: `mkdir challengeName/img/`
4. Add screenshots as you document steps
5. Use relative paths for images in markdown format

### Image Management
- Store all images in the `img/` subdirectory of each writeup
- Use descriptive filenames (e.g., `nmap-results.png`, `dashboard-view.png`)
- CRITICAL: Avoid spaces in filenames - use hyphens or underscores instead
- If spaces exist in filenames, reference them with exact spacing in markdown (not URL-encoded)

## Validation Requirements

### Before Committing Changes
ALWAYS run the validation script above. Common issues to avoid:
- Broken image links (most frequent issue)
- Invalid markdown syntax
- Missing image files
- Incorrect relative paths

### File Naming Best Practices
- Use lowercase for folder names
- Use hyphens instead of spaces in filenames
- Keep image filenames descriptive but concise
- Use standard image formats: PNG, JPG, JPEG

## Common Tasks

### Repository Status Check
```bash
# Check current repository state
git status
find . -name "*.md" | wc -l  # Count markdown files
find . -name "*.png" -o -name "*.jpg" -o -name "*.jpeg" | wc -l  # Count images
```

### Quick File Structure View
Current repository structure:
```
RoomsWriteups/
├── README.md
├── .github/
│   └── copilot-instructions.md
└── aklaaX/
    ├── README.md
    └── img/
        ├── Dashboard.png
        ├── a little bit of OSINT.png
        ├── exiftool.png
        ├── jwt-edited.png
        ├── jwt.png
        ├── localstorage.png
        ├── login.png
        ├── new-beat.png
        ├── nmapresult.png
        ├── register-error.png
        ├── sonic-visualiser.png
        ├── sonic-visualiser2.png
        └── steganography-doc.png
```

## Troubleshooting

### Image Link Issues
If validation shows missing images:
1. Check the exact filename in the filesystem: `ls -la folder/img/`
2. Verify the markdown uses the exact filename (case-sensitive)
3. Ensure relative path is correct from the markdown file location
4. Fix URL encoding issues (use actual spaces, not %20)

### Markdown Parsing Errors
If markdown validation fails:
1. Check for unbalanced quotes or brackets
2. Verify code block syntax (use triple backticks)
3. Ensure proper heading hierarchy
4. Check for special characters that need escaping

## Performance Notes
- Validation script runs in ~0.1 seconds for typical repository size
- No build or compilation steps - changes are immediately viewable
- Image files are typically 200KB-1MB each (normal for screenshot documentation)

## Repository Patterns
Based on existing writeups, follow this pattern:
1. **Reconnaissance** - Network scanning, initial discovery
2. **Registration/Login** - Account creation and authentication steps  
3. **Dashboard/Interface** - Main application exploration
4. **Exploitation** - Vulnerability discovery and exploitation
5. **Flag** - Final objective achievement

Always include screenshots at each major step to help other security researchers follow your methodology.