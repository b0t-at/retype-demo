---
label: Getting Started
icon: rocket
order: 1000
---

# Getting Started with Retype

Welcome! This guide will help you get up and running with Retype quickly.

## Installation

### Prerequisites

Before installing Retype, ensure you have:

- [Node.js](https://nodejs.org/) (version 14 or higher)
- npm or yarn package manager

### Install Retype

Install Retype globally using npm:

```bash
npm install retypeapp --global
```

Or using yarn:

```bash
yarn global add retypeapp
```

Verify the installation:

```bash
retype --version
```

## Creating Your First Project

### Initialize a New Project

Navigate to your project folder and run:

```bash
retype init
```

This creates a basic `retype.yml` configuration file.

### Project Structure

A typical Retype project looks like this:

```
my-project/
├── retype.yml          # Configuration file
├── README.md           # Homepage
├── guides/             # Documentation pages
│   ├── guide-1.md
│   └── guide-2.md
└── api/                # API documentation
    └── reference.md
```

## Writing Content

### Basic Markdown

Retype uses standard Markdown syntax:

```markdown
# Heading 1
## Heading 2
### Heading 3

This is a paragraph with **bold** and *italic* text.

- Bullet point 1
- Bullet point 2

1. Numbered item 1
2. Numbered item 2
```

### Front Matter

Add metadata to your pages using YAML front matter:

```yaml
---
label: My Page
icon: star
order: 100
---
```

Common front matter properties:

Property | Description
--- | ---
`label` | Navigation label
`icon` | Icon name (GitHub Octicons or emoji)
`order` | Sort order (higher appears first)
`tags` | List of tags

## Development Workflow

### Start the Development Server

Run the local development server:

```bash
retype watch
```

This command:
- Builds your site
- Starts a local web server
- Watches for file changes
- Auto-reloads your browser

Access your site at `http://localhost:5000`

### Building for Production

When ready to deploy:

```bash
retype build
```

This generates static files in the `.retype` folder (or your configured output directory).

## Configuration

### Basic Configuration

The `retype.yml` file controls your site settings:

```yaml
input: .
output: .retype

branding:
  title: My Project
  label: Docs

links:
  - text: Home
    link: /
  - text: GitHub
    icon: mark-github
    link: https://github.com/username/repo

footer:
  copyright: "&copy; Copyright {{ year }}. All rights reserved."
```

### Common Settings

Setting | Purpose | Example
--- | --- | ---
`input` | Source folder | `.` (current directory)
`output` | Build output folder | `.retype`
`url` | Production URL | `docs.example.com`
`branding.title` | Site title | `My Documentation`
`branding.logo` | Logo image path | `/static/logo.png`

## Next Steps

!!!info What's Next?
Now that you have the basics down, explore:
- [Components Demo](/components-demo/) - Learn about rich components
- [Guides](/guides/) - Detailed tutorials
- [API Reference](/api/) - API documentation patterns
!!!

## Troubleshooting

### Common Issues

**Issue**: Command not found
```bash
retype: command not found
```
**Solution**: Ensure Retype is installed globally and your PATH is configured correctly.

---

**Issue**: Port already in use
```
Port 5000 is already in use
```
**Solution**: Stop other processes using port 5000 or specify a different port:
```bash
retype watch --port 5001
```

---

**Issue**: Build fails
**Solution**: Check your `retype.yml` syntax and ensure all referenced files exist.

## Resources

- [Official Documentation](https://retype.com)
- [GitHub Repository](https://github.com/retypeapp/retype)
- [Community Discussions](https://github.com/retypeapp/retype/discussions)
- [Examples](https://github.com/retypeapp/retype/tree/main/samples)

---

Need help? Check out the [components demo](/components-demo/) or visit the [official docs](https://retype.com).
