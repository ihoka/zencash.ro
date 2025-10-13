# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a Jekyll-based static site serving as a website for ZenCash, a crypto trading platform. The site uses Tailwind CSS for styling and is built with a minimalist, dark theme design.

## Development Commands

- `mise run install` - Install missing Ruby dependencies (runs automatically before dev/build)
- `mise run dev` - Start Jekyll development server with live reload and auto-open browser
- `mise run build` - Build Jekyll site for production
- **Note**: The dev server runs continuously in the background as a VSCode task - no need to start it manually

## Jekyll Plugins

The site uses the following Jekyll plugins (configured in Gemfile):

- `jekyll-tailwindcss` - Tailwind CSS integration for Jekyll
- `jekyll-seo-tag` - SEO metadata tags
- `jekyll-sitemap` - Automatic sitemap generation

## Deployment

The site is hosted on GitHub Pages and published using GitHub Actions

## Code Style Guidelines

- **NO HTML IN MARKDOWN**: NEVER add ANY HTML tags to markdown files (.md files). Use pure markdown only
- **CSS-ONLY STYLING**: All styling must be done through CSS only (_tailwind.css file). Use CSS selectors and pseudo-elements to style the generated HTML markup
- **Styling**: All styling should go through Tailwind CSS system
- **CSS Processing**: Use the Jekyll + Tailwind CSS pipeline, don't write raw CSS
- **Layouts**: Keep layouts minimal and focused on structure
- **Content**: Separate content (markdown) from presentation (layouts)
