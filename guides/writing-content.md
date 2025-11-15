---
label: Writing Content
icon: pencil
order: 300
---

# Writing Content Guide

Learn how to create effective documentation with Retype's Markdown features.

## Markdown Basics

Retype uses GitHub Flavored Markdown (GFM) with additional enhancements.

### Headings

Use `#` symbols to create headings:

```markdown
# Heading 1
## Heading 2
### Heading 3
#### Heading 4
##### Heading 5
###### Heading 6
```

!!! Note Heading Best Practices
- Use only one H1 (`#`) per page
- Maintain a logical heading hierarchy
- Keep headings descriptive and concise
!!!

### Paragraphs and Line Breaks

Write paragraphs as continuous text. Leave a blank line between paragraphs.

```markdown
This is the first paragraph.

This is the second paragraph.
```

For a line break within a paragraph, end the line with two spaces or use `<br>`.

### Emphasis

```markdown
*Italic text* or _italic text_
**Bold text** or __bold text__
***Bold and italic*** or ___bold and italic___
~~Strikethrough text~~
```

Result:
- *Italic text*
- **Bold text**
- ***Bold and italic***
- ~~Strikethrough text~~

## Lists

### Unordered Lists

```markdown
- Item 1
- Item 2
  - Nested item 2.1
  - Nested item 2.2
- Item 3
```

Result:
- Item 1
- Item 2
  - Nested item 2.1
  - Nested item 2.2
- Item 3

### Ordered Lists

```markdown
1. First item
2. Second item
   1. Nested item 2.1
   2. Nested item 2.2
3. Third item
```

Result:
1. First item
2. Second item
   1. Nested item 2.1
   2. Nested item 2.2
3. Third item

### Task Lists

```markdown
- [x] Completed task
- [ ] Incomplete task
- [ ] Another incomplete task
```

Result:
- [x] Completed task
- [ ] Incomplete task
- [ ] Another incomplete task

## Links

### Internal Links

Link to other pages in your site:

```markdown
[Getting Started](/getting-started/)
[Components](/components-demo/)
```

### External Links

```markdown
[Retype](https://retype.com)
[GitHub](https://github.com)
```

### Link with Title

```markdown
[Retype](https://retype.com "Official Retype Site")
```

## Images

### Basic Image

```markdown
![Alt text](path/to/image.png)
```

### Image with Link

```markdown
[![Alt text](image.png)](https://link.com)
```

### Image Sizing

```markdown
![Alt text](image.png){ width="300" }
![Alt text](image.png){ height="200" }
```

## Code

### Inline Code

Use backticks for `inline code`:

```markdown
Use the `retype build` command to build your site.
```

### Code Blocks

Use triple backticks with language specification:

````markdown
```javascript
function hello() {
  console.log("Hello, World!");
}
```
````

Result:
```javascript
function hello() {
  console.log("Hello, World!");
}
```

### Code Block with Line Numbers

Add `#` after the language:

````markdown
```javascript #
const a = 1;
const b = 2;
console.log(a + b);
```
````

## Tables

### Basic Table

```markdown
| Header 1 | Header 2 | Header 3 |
| --- | --- | --- |
| Row 1, Col 1 | Row 1, Col 2 | Row 1, Col 3 |
| Row 2, Col 1 | Row 2, Col 2 | Row 2, Col 3 |
```

Result:

| Header 1 | Header 2 | Header 3 |
| --- | --- | --- |
| Row 1, Col 1 | Row 1, Col 2 | Row 1, Col 3 |
| Row 2, Col 1 | Row 2, Col 2 | Row 2, Col 3 |

### Aligned Tables

```markdown
| Left | Center | Right |
| :--- | :---: | ---: |
| Text | Text | Text |
```

Result:

| Left | Center | Right |
| :--- | :---: | ---: |
| Text | Text | Text |

## Blockquotes

```markdown
> This is a blockquote.
> It can span multiple lines.
>
> — Author Name
```

Result:

> This is a blockquote.
> It can span multiple lines.
>
> — Author Name

## Alerts

Retype provides several alert types:

```markdown
!!! Info
This is an info alert.
!!!

!!! Success
This is a success alert.
!!!

!!! Warning
This is a warning alert.
!!!

!!! Danger
This is a danger alert.
!!!
```

Results:

!!! Info
This is an info alert.
!!!

!!! Success
This is a success alert.
!!!

!!! Warning
This is a warning alert.
!!!

!!! Danger
This is a danger alert.
!!!

## Panels

Collapsible panels for organizing content:

```markdown
+++ Panel Title
Content inside the panel.
+++
```

Result:

+++ Example Panel
This is content inside a collapsible panel.

You can include any Markdown content here!
+++

## Tabs

Organize content in tabs:

```markdown
=== Tab 1
Content for tab 1
===

=== Tab 2
Content for tab 2
===
```

Result:

=== JavaScript
```javascript
console.log("JavaScript example");
```
===

=== Python
```python
print("Python example")
```
===

## Front Matter

Add metadata to your pages:

```yaml
---
label: Page Title
icon: star
order: 100
tags: [guide, documentation]
---
```

Common properties:

Property | Description | Example
--- | --- | ---
`label` | Page title in navigation | `Getting Started`
`icon` | Icon (emoji or name) | `rocket` or `:rocket:`
`order` | Sort order (higher first) | `1000`
`tags` | List of tags | `[guide, tutorial]`
`description` | Page description | `Learn the basics`
`hidden` | Hide from navigation | `true` or `false`

## Emojis

Use emoji codes or Unicode emojis:

```markdown
:rocket: :sparkles: :book:
```

Result: :rocket: :sparkles: :book:

## Horizontal Rules

Create horizontal rules with three or more dashes, asterisks, or underscores:

```markdown
---
***
___
```

---

## Best Practices

!!! Success Writing Tips
1. **Be concise** - Keep sentences and paragraphs short
2. **Use headings** - Break content into logical sections
3. **Add examples** - Show, don't just tell
4. **Include visuals** - Use images, diagrams, and code samples
5. **Stay consistent** - Use consistent terminology and formatting
6. **Test links** - Ensure all links work correctly
7. **Proofread** - Check for typos and grammar errors
!!!

## Content Organization

### File Structure

Organize your content logically:

```
docs/
├── README.md              # Homepage
├── getting-started.md     # Getting started guide
├── guides/
│   ├── README.md         # Guides overview
│   ├── guide-1.md
│   └── guide-2.md
└── api/
    ├── README.md         # API overview
    └── reference.md      # API reference
```

### Navigation Order

Control navigation order with the `order` property:

- Higher numbers appear first
- Default order is 0
- Negative numbers appear last

## Resources

- [Markdown Guide](https://www.markdownguide.org/)
- [GitHub Flavored Markdown](https://github.github.com/gfm/)
- [Retype Components](/components-demo/)

---

Continue to [Customization](customization.md) to learn how to personalize your Retype site.
