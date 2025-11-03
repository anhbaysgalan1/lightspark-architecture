# Setup Instructions

This repository contains the ARD Financial Group's cross-border remittance system documentation.

## Quick Setup

Follow these steps to publish your documentation to GitHub Pages:

### 1. Initialize Git Repository

```bash
cd /Users/jojo/lightspark

# Initialize git
git init

# Add all files
git add .

# Create initial commit
git commit -m "Initial commit: ARD remittance documentation V2.0"

# Set main branch
git branch -M main

# Add remote repository
git remote add origin https://github.com/anhbaysgalan1/lightspark-architecture.git

# Push to GitHub
git push -u origin main
```

### 2. Enable GitHub Pages

1. Go to https://github.com/anhbaysgalan1/lightspark-architecture
2. Click **Settings** (top right)
3. Scroll down to **Pages** (left sidebar)
4. Under **Source**, select:
   - Branch: `main`
   - Folder: `/ (root)`
5. Click **Save**

### 3. Access Your Documentation

After 1-2 minutes, your documentation will be live at:

**https://anhbaysgalan1.github.io/lightspark-architecture/**

## Local Preview

To preview locally before publishing:

```bash
# Install a simple HTTP server (if you don't have one)
# Option 1: Python 3
python3 -m http.server 3000

# Option 2: Python 2
python -m SimpleHTTPServer 3000

# Option 3: Node.js
npx http-server -p 3000

# Then open: http://localhost:3000
```

## Updating Documentation

When you make changes:

```bash
# Check what changed
git status

# Add modified files
git add .

# Commit with a message
git commit -m "Update documentation: describe your changes"

# Push to GitHub
git push
```

GitHub Pages will automatically rebuild and publish your changes within 1-2 minutes.

## Features

✅ **Beautiful UI** - Clean, professional Docsify theme
✅ **Search** - Full-text search across all documents
✅ **Mobile-friendly** - Responsive design
✅ **Copy code** - Easy code snippet copying
✅ **Navigation** - Sidebar with document structure
✅ **Pagination** - Previous/Next navigation
✅ **Emoji support** - 🚨 ✅ 💰 etc.

## Documentation Structure

- **Home**: README_V2.md
- **Quick Reference**: QUICK_REFERENCE_V2.md
- **Technical Architecture**: ARD_Cross_Border_Remittance_Technical_Architecture_V2.md
- **Process Flows**: ARD_Process_Flow_Diagrams_UPDATED.md
- **System Architecture**: ARD_System_Architecture_Diagrams_V2.md
- **Business Operations**: ARD_Business_Operations_Treasury_Management_V2.md
- **Implementation Roadmap**: ARD_Implementation_Roadmap_V2.md
- **Updates Summary**: UPDATES_SUMMARY_V2.md

## Support

For issues or questions, contact the ARD technical team.

---

**Version:** 2.0
**Last Updated:** November 2025
**Critical Updates:** Immediate iDAX execution model
