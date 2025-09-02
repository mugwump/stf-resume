# WARP.md

This file provides guidance to WARP (warp.dev) when working with code in this repository.

## Project Overview

This is an Astro-based resume builder that generates both web and PDF versions of a resume from Markdown content. The project uses Tailwind CSS for styling and Playwright for PDF generation.

## Key Architecture

- **Single-page Astro app**: The resume content lives in `src/pages/index.md` which uses the `Minimalist.astro` layout
- **Markdown-driven content**: All resume information is written in Markdown with frontmatter metadata
- **PDF generation**: Uses Playwright to generate PDFs at build time by screenshotting the rendered HTML
- **Theme support**: Built-in dark/light mode support with optional theme forcing via `FORCE_THEME` environment variable
- **Static deployment**: Configured for Netlify deployment with automatic PDF generation during build

## Development Commands

```bash
# Install dependencies and Playwright browsers
yarn

# Start development server (localhost:4321)
yarn dev

# Type checking and build preparation
yarn prebuild

# Full production build (includes PDF generation)
yarn build

# Build with specific theme
yarn build:light    # Force light theme
yarn build:dark     # Force dark theme

# Preview production build locally
yarn preview

# Generate PDF manually (requires dev server running)
yarn generate-pdf
```

## Content Management

### Resume Content
- **Main content**: Edit `src/pages/index.md` to update resume information
- **Profile image**: Place image in `public/` directory and reference in frontmatter
- **PDF filename**: Set via `pdfLink` in frontmatter

### Frontmatter Structure
```yaml
---
title: "Name - Resume"          # Page title and used for initials
description: "Resume description"
layout: ../layouts/Minimalist.astro
pdfLink: resume-filename.pdf    # Generated PDF filename
---
```

## Build Process

1. **Pre-build**: Runs Astro type checking and TypeScript compilation
2. **PDF Generation**: Starts development server and uses Playwright to generate PDF
3. **Static Build**: Astro builds the static site to `dist/`

The PDF generation process:
- Starts local server on port 4321
- Playwright launches Chromium and screenshots the page
- PDF saved to `public/` directory with specified filename
- Build includes the PDF in the final static output

## Environment Variables

- `PDF_VIEW`: When true, hides the download button (used during PDF generation)
- `FORCE_THEME`: Force specific theme ("light" or "dark") instead of system preference

## Project Structure

```
src/
├── pages/index.md              # Main resume content
├── layouts/Minimalist.astro    # Resume layout template
├── components/Download.astro   # PDF download button
└── styles.css                 # Global styles

scripts/
└── generate-pdf.js            # Playwright PDF generation script

public/                        # Static assets and generated PDFs
```

## Styling System

- **Tailwind CSS**: Primary styling framework with typography plugin
- **Dark mode**: Automatic based on system preference or forced via environment
- **Responsive**: Mobile-first responsive design
- **Print optimization**: Special handling for PDF generation

## Development Tips

- Changes to `index.md` require server restart for PDF regeneration
- Use `yarn dev` for quick content iteration (PDF not auto-generated)
- PDF generation requires the development server to be running
- Theme forcing is useful for generating PDFs in specific themes
- The initials badge is auto-generated from the title frontmatter
