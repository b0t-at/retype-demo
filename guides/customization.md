---
label: Customization
icon: paintbrush
order: 200
---

# Customization Guide

Learn how to customize your Retype site to match your brand and preferences.

## Configuration File

All customization starts with the `retype.yml` configuration file in your project root.

### Basic Structure

```yaml
input: .
output: .retype

branding:
  title: My Project
  label: Docs
  
links:
  - text: Home
    link: /

footer:
  copyright: "&copy; Copyright {{ year }}. All rights reserved."
```

## Branding

### Site Title and Label

```yaml
branding:
  title: My Documentation
  label: v2.0
```

- `title`: Main site title
- `label`: Optional label/version displayed with the title

### Logo

Add a custom logo:

```yaml
branding:
  logo: /static/logo.png
  logoAlign: left  # left or right
```

Logo recommendations:
- PNG or SVG format
- Transparent background
- Height: 40-60px
- Keep it simple and clear

### Colors

Customize your site colors:

```yaml
branding:
  colors:
    label:
      text: "#ffffff"
      background: "#0ea5e9"
```

## Navigation

### Top Links

Add links to the top navigation bar:

```yaml
links:
  - text: Documentation
    link: /docs/
  - text: API
    link: /api/
  - text: GitHub
    icon: mark-github
    link: https://github.com/username/repo
  - text: Website
    icon: home
    link: https://example.com
```

Link properties:
- `text`: Link text
- `link`: URL (internal or external)
- `icon`: Optional icon name
- `target`: `_blank` to open in new tab

### Icons

Use GitHub Octicons or emojis:

```yaml
links:
  - text: Home
    icon: home        # Octicon name
    link: /
  - text: Rocket
    icon: ":rocket:"  # Emoji
    link: /features/
```

Popular Octicons:
- `home`
- `book`
- `code`
- `gear`
- `mark-github`
- `heart`
- `rocket`

## Footer

### Copyright

```yaml
footer:
  copyright: "&copy; Copyright {{ year }}. [My Company](https://example.com)"
```

The `{{ year }}` variable automatically displays the current year.

### Footer Links

```yaml
footer:
  links:
    - text: Privacy Policy
      link: /privacy/
    - text: Terms of Service
      link: /terms/
    - text: Contact
      link: mailto:info@example.com
```

## Meta Tags

### SEO and Social

```yaml
meta:
  title: " - My Documentation"  # Appended to page titles
  description: "Comprehensive documentation for My Project"
  
og:
  image: /static/og-image.png
  title: My Documentation
  description: Learn how to use My Project
  
twitter:
  card: summary_large_image
  site: "@username"
```

## Search

### Configure Search

```yaml
search:
  placeholder: "Search documentation..."
  hotkeys:
    - "/"
    - "ctrl+k"
  maxResults: 20
```

Search is enabled by default and indexes all your content automatically.

## Editing

### Edit This Page

Enable "Edit this page" links:

```yaml
edit:
  repo: "https://github.com/username/repo"
  base: /docs
  branch: main
  label: "Edit on GitHub"
```

This adds an edit link to each page that opens the source file on GitHub.

## Themes

### Dark Mode

Configure dark mode behavior:

```yaml
theme:
  mode: auto  # auto, light, or dark
```

Options:
- `auto`: Respects user's system preference
- `light`: Always light mode
- `dark`: Always dark mode

### Custom CSS

Add custom styles:

1. Create a CSS file (e.g., `custom.css`)
2. Reference it in `retype.yml`:

```yaml
head:
  - <link rel="stylesheet" href="/static/custom.css" />
```

Example custom CSS:

```css
/* custom.css */
:root {
  --color-primary: #0ea5e9;
  --color-secondary: #64748b;
}

.content {
  max-width: 900px;
}

h1 {
  color: var(--color-primary);
}
```

## Favicon

Set a custom favicon:

```yaml
favicon: /static/favicon.ico
```

Supported formats:
- `.ico`
- `.png`
- `.svg`

## Integration

### Google Analytics

```yaml
integrations:
  googleAnalytics:
    id: G-XXXXXXXXXX
```

### Google Tag Manager

```yaml
integrations:
  googleTagManager:
    id: GTM-XXXXXXX
```

### Plausible Analytics

```yaml
integrations:
  plausible:
    domain: docs.example.com
```

## Advanced Configuration

### Custom Head Content

Add custom HTML to the `<head>`:

```yaml
head:
  - <meta name="author" content="Your Name" />
  - <link rel="preconnect" href="https://fonts.googleapis.com" />
```

### Custom Scripts

```yaml
scripts:
  - /static/custom.js
```

### Redirects

Set up URL redirects:

```yaml
redirects:
  /old-page: /new-page
  /legacy/guide: /guides/new-guide
```

## Page-Level Configuration

Create a `.yml` file next to your `.md` file for page-specific settings:

**my-page.md** + **my-page.yml**

```yaml
# my-page.yml
label: Custom Page Title
icon: star
order: 100
tags:
  - guide
  - important
description: A custom description for this page
```

## Templating

### Variables

Use variables in your content:

```yaml
# retype.yml
variables:
  version: "2.0.0"
  company: "My Company"
```

Then in your Markdown:

```markdown
Welcome to {{ version }} of {{ company }}'s documentation!
```

## Examples

### Minimal Configuration

```yaml
input: .
output: .retype

branding:
  title: My Docs
```

### Complete Configuration

```yaml
input: .
output: .retype
url: docs.example.com

branding:
  title: My Project
  label: v2.0
  logo: /static/logo.png
  colors:
    label:
      text: "#ffffff"
      background: "#0ea5e9"

links:
  - text: Home
    icon: home
    link: /
  - text: GitHub
    icon: mark-github
    link: https://github.com/username/repo

footer:
  copyright: "&copy; Copyright {{ year }}. [My Company](https://example.com)"
  links:
    - text: Privacy
      link: /privacy/
    - text: Terms
      link: /terms/

meta:
  title: " - My Documentation"
  description: "Official documentation for My Project"

search:
  placeholder: "Search docs..."
  hotkeys:
    - "/"

edit:
  repo: "https://github.com/username/repo"
  branch: main
  label: "Edit this page"

integrations:
  googleAnalytics:
    id: G-XXXXXXXXXX
```

## Tips and Best Practices

!!! Success Customization Tips
1. **Start simple** - Begin with basic branding
2. **Test changes** - Use `retype watch` to see changes live
3. **Be consistent** - Maintain consistent styling
4. **Optimize images** - Keep logos and images small
5. **Use variables** - Centralize repeated values
6. **Document changes** - Keep track of customizations
!!!

## Resources

- [Retype Configuration Reference](https://retype.com/configuration/project/)
- [GitHub Octicons](https://primer.style/octicons/)
- [Emoji Cheat Sheet](https://github.com/ikatyang/emoji-cheat-sheet)

---

Next: [Deployment](deployment.md) - Learn how to deploy your customized site.
