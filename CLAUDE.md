# Thai Realty Website Pattern

This site is a multilingual Hugo-based Thai real estate website with internationalization support. It uses Tailwind CSS for responsive design and supports both Thai and English content.

## Hugo Installation

```bash
# macOS
brew install hugo

# Linux (Ubuntu/Debian)
sudo apt install hugo

# Windows
choco install hugo-extended
```

## Configuration (`config.toml`)

Multi-language configuration with Thai and English:

```yaml
defaultContentLanguage: 'th'
defaultContentLanguageInSubdir: true

[languages]
  [languages.th]
    languageName = 'ไทย'
    weight = 1
    title = 'บริษัท อสังหาริมทรัพย์ไทย'
    
  [languages.en]
    languageName = 'English'
    weight = 2
    title = 'Thai Realty Company'

[params]
  company = 'Thai Realty Co., Ltd.'
  phone = '+66-2-xxx-xxxx'
  email = 'info@thairealty.com'
  address = 'Bangkok, Thailand'
  facebook = 'https://facebook.com/thairealty'
  line = '@thairealty'
```

## Content Structure

### Homepage (`content/_index.th.md` / `content/_index.en.md`)
Multilingual homepage with property search and featured listings.

### Properties (`content/properties/*/index.md`)
Property listings with multilingual support:
- Image galleries with Hugo image processing
- Price in THB with currency formatting
- Location maps and amenities
- Contact forms for inquiries

### Pages (`content/about/`, `content/services/`, `content/contact/`)
Standard pages with language-specific content.

## Layout Templates

### Base Template (`layouts/_default/baseof.html`)
- Responsive HTML5 structure with Tailwind CSS classes
- Language switcher in navigation
- RTL support considerations for Thai script
- SEO-optimized meta tags

### Homepage (`layouts/index.html`)
- Hero section with property search form
- Featured properties grid
- Company information and testimonials
- Contact information with Thai phone formatting

### Property Template (`layouts/properties/single.html`)
- Image gallery with lightbox functionality
- Property details with Thai/English formatting
- Interactive map integration
- Contact agent form

## CSS Architecture

### Tailwind CSS Setup
- Configured for Thai typography and spacing
- Custom color palette for real estate branding
- Responsive utilities for mobile-first design
- Thai font support (Sarabun, Prompt, or Kanit)

### Key Tailwind Components
- Navigation with language switcher
- Property card components
- Form elements with Thai styling
- Map containers and overlays

## Internationalization Features

### Language Support
- Thai (primary) and English content
- Language-specific URLs (/th/, /en/)
- Automatic language detection
- Currency formatting (THB, USD)
- Date formatting for Thai locale

### Content Management
- Shared assets across languages
- Language-specific images when needed
- Consistent navigation structure
- Localized contact information

## Property Data Structure

### Front Matter Schema
```yaml
---
title: "Property Title"
price: 15000000
currency: "THB"
type: "condo"
bedrooms: 3
bathrooms: 2
area: 120
location: "Sukhumvit"
coordinates: [13.7563, 100.5018]
images: ["main.jpg", "bedroom.jpg", "kitchen.jpg"]
features: ["pool", "gym", "parking"]
---
```

## Key Features

- **Multilingual** - Thai/English with proper i18n
- **Mobile-First** - Responsive Tailwind CSS design
- **Property Search** - Filtered listings by criteria
- **Image Optimization** - Hugo image processing
- **Maps Integration** - Property location mapping
- **Contact Forms** - Lead generation with validation
- **SEO Optimized** - Meta tags and structured data

## Development Commands

```bash
hugo server -D                    # Local development
hugo server -D --bind 0.0.0.0     # Network accessible
hugo --minify                     # Production build
```

## Thai-Specific Considerations

1. **Typography** - Use web-safe Thai fonts (Sarabun, Prompt)
2. **Number Formatting** - Thai number separators and currency
3. **Address Format** - Thai address conventions
4. **Phone Numbers** - Thai mobile and landline formats
5. **Cultural Design** - Colors and imagery appropriate for Thai market
6. **Legal Content** - Property disclosure requirements in Thai