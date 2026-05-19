# Thai Realty Website - Hugo CLI Commands

## Installation
```bash
# macOS
brew install hugo

# Linux (Ubuntu/Debian)
sudo apt install hugo

# Windows
choco install hugo-extended
```

## Site Setup
```bash
# Create new site
hugo new site thai-realty

# Navigate to site
cd thai-realty

# Initialize as git repo
git init

# Create content files
hugo new content _index.th.md
hugo new content _index.en.md
hugo new content properties/condo-sukhumvit/index.md
hugo new content about/_index.th.md
hugo new content contact/_index.th.md
```

## Development
```bash
# Start development server
hugo server -D

# With draft content
hugo server -D --buildDrafts

# Open browser automatically
hugo server -D -O

# Bind to all interfaces
hugo server -D --bind 0.0.0.0

# Custom port
hugo server -D -p 8080

# Live reload disabled
hugo server -D --disableLiveReload
```

## Building
```bash
# Build for production
hugo

# Build with minification
hugo --minify

# Build including drafts
hugo -D

# Build to custom directory
hugo --destination dist/

# Clean public directory first
hugo --cleanDestinationDir
```

## Content Management
```bash
# List all content
hugo list all

# List drafts only
hugo list drafts

# List published content
hugo list published

# List future posts
hugo list future
```

## Configuration
```bash
# View current config
hugo config

# View config in specific format
hugo config --format yaml
hugo config --format json
```

## Deployment
```bash
# Deploy to configured provider
hugo deploy

# Deploy with confirmation
hugo deploy --confirm

# Dry run deployment
hugo deploy --dryRun
```

## Module Management
```bash
# Initialize as Hugo module
hugo mod init github.com/youruser/thai-realty

# Add theme as module
hugo mod get github.com/themename/theme

# Update modules
hugo mod get -u

# View dependency graph
hugo mod graph

# Clean module cache
hugo mod clean
```

## Utilities
```bash
# Check Hugo version
hugo version

# View environment info
hugo env

# Generate completion scripts
hugo completion bash > hugo_completion.sh
```

## Thai Realty Specific Workflow
```bash
# Create new property listing
hugo new content properties/luxury-condo-asok/index.md

# Start development with Thai locale
hugo server -D --baseURL http://localhost:1313/th/

# Build multilingual site
hugo --minify

# Deploy to staging
hugo deploy --target staging
```