# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a Hugo static site using the PaperMod theme. The site is built for personal blogging and portfolio purposes, currently configured to deploy to Cloudflare Pages at `https://my-site-c8h.pages.dev/`.

## Key Commands

### Development and Building
- `hugo server` - Start development server with live reload
- `hugo server -D` - Start development server including draft posts
- `hugo` - Build the static site (outputs to `public/` directory)
- `hugo --minify` - Build with minification for production

### Content Management
- `hugo new posts/post-name.md` - Create a new blog post
- `hugo new content/page-name.md` - Create a new page

## Architecture

### Directory Structure
- `content/` - Markdown content files
  - `_index.md` - Homepage content and profile information
  - `posts/` - Blog posts directory
- `static/` - Static assets (images, CSS, etc.)
- `layouts/` - Custom Hugo templates (overrides theme templates)
- `themes/PaperMod/` - Git submodule containing the PaperMod theme
- `public/` - Generated static site output (auto-generated, do not edit)

### Theme System
The site uses Hugo PaperMod theme as a Git submodule. Custom modifications should be made in the root `layouts/` directory to override theme templates, and in `static/` for custom CSS/assets.

### Configuration
Site configuration is in `hugo.toml` with:
- Base URL set for Cloudflare Pages deployment
- PaperMod theme configuration
- Site metadata (title, language)

### Content Format
- Posts use Hugo front matter (TOML format with `+++` delimiters)
- Homepage uses YAML front matter (with `---` delimiters) and includes profile configuration
- All content written in Markdown

## Development Notes

### Theme Customization
- Custom CSS should go in `static/css/`
- Template overrides should go in `layouts/` following Hugo's lookup order
- The theme documentation is available at: https://github.com/adityatelange/hugo-PaperMod/wiki

### Deployment
The site is configured to deploy to Cloudflare Pages. The `public/` directory contains the built site and should not be manually edited.

### Git Submodules
The PaperMod theme is included as a Git submodule. When cloning the repository, use:
```bash
git clone --recurse-submodules [repo-url]
```
Or after cloning:
```bash
git submodule update --init --recursive
```