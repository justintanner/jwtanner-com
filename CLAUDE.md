# Hugo Personal Blog Site Pattern

This site is a minimal Hugo blog focused on technical writing and personal projects. It uses Bootstrap for responsive design and Charter font for readable typography.

## Configuration (`config.toml`)

```toml
baseURL = 'https://example.com/'
languageCode = 'en-us'
title = 'Your Name'
pygmentsUseClassic = false
pygmentsCodeFences = true
pygmentsCodeFencesGuessSyntax = true
pygmentsUseClasses = true
disableKinds = ['taxonomy', 'term']

[params]
  author = 'Your Name'
  github = 'https://github.com/username'
  og_image = '/og.jpg'
  sitename = 'Your Name: Description'
```

## Content Structure

### Homepage (`content/_index.md`)
```yaml
---
title: "Profile"
description: "Your contact info and blog"
image: "profile.jpg"
---
```

### Posts (`content/posts/*/index.md`)
```yaml
---
title: Post Title
description: Brief description for cards and meta
author: Your Name
date: 2024-01-01
image: "cover.jpg"
---
```

Posts use page bundles with `index.md` and assets (cover.jpg, other images) in same folder.

## Layout Templates

### Base Template (`layouts/_default/baseof.html`)
- Minimal HTML5 structure with Bootstrap classes
- Uses partial for `<head>` section
- Block template for main content

### Homepage (`layouts/index.html`)
- Profile section with rotated image and shadow effect
- Posts grid with cover images and descriptions
- Footer with social links

### Post Template (`layouts/posts/single.html`)
- Responsive cover images (desktop/mobile variants)
- Charter font for body text
- Back to posts navigation

## CSS Architecture

### Main Styles (`assets/css/main.css`)
- Bootstrap icon alignment fix
- Code block styling with rounded borders
- Charter font family definitions
- Responsive design with breakpoint-specific styles
- Custom classes: `.gray-bg`, `.post`, `.text-decoration-onhover`

### Typography (`assets/css/charter.css`)
- Charter font face definitions for all weights/styles
- Covers multiple character sets (Latin, Cyrillic, Vietnamese)

### Asset Pipeline (`layouts/partials/head.html`)
- Conditional Charter font loading for posts section
- CSS bundling and minification with fingerprinting
- Bootstrap + main + Pygments CSS bundle

## Custom Shortcodes

### Bootstrap Figure (`layouts/shortcodes/bootstrap_figure.html`)
- Responsive image with srcset generation
- Multiple width variants for optimal loading
- Figure/figcaption structure with center alignment

### Bootstrap YouTube (`layouts/shortcodes/bootstrap_youtube.html`)
- Responsive YouTube embeds

## Image Handling

- Profile images with 2x retina support
- Post cover images with multiple responsive variants
- Automatic cropping and resizing using Hugo image processing
- Images stored as page resources in post bundles

## Key Features

- **No JavaScript** - Pure HTML/CSS site
- **Responsive** - Mobile-first Bootstrap grid
- **Performance** - Image optimization, CSS bundling
- **Typography** - Charter font for readability
- **Social** - Email, GitHub, Twitter links
- **Code Highlighting** - Pygments with custom styling

## Development Commands

```bash
hugo server -D     # Local development
hugo               # Build for production
```

## Customization Tips

1. Replace social links in homepage and footer templates
2. Update `params` in config.toml for site metadata
3. Add new shortcodes for custom content types
4. Modify CSS variables in main.css for color scheme
5. Use page bundles for posts with multiple images