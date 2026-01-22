---
title: Ultimate Complete Markdown Test Document
description: EVERY possible markdown pattern, combination, and edge case
keywords: [markdown, complete, comprehensive, all-patterns, testing]
category: reference
---

# 🎯 Ultimate Complete Markdown Test Document

This document contains **EVERY SINGLE POSSIBLE** markdown pattern, combination, and edge case. Nothing is left out.

---

## 📑 Table of Contents

- [Images Everywhere](#images-everywhere)
- [Tables Advanced](#tables-advanced)
- [Grid Layouts](#grid-layouts)
- [Custom Styling](#custom-styling)
- [Videos & Media](#videos--media)
- [Complex Nesting](#complex-nesting)
- [All Combinations](#all-combinations)
- [Interactive Elements](#interactive-elements)
- [Edge Cases Extreme](#edge-cases-extreme)

---

## Images Everywhere

### Images in Tables

| Product | Image | Description | Price |
|---------|-------|-------------|-------|
| Product 1 | ![Product 1](https://via.placeholder.com/100x100) | High quality product | $99.99 |
| Product 2 | ![Product 2](https://via.placeholder.com/100x100) | Premium product | $149.99 |
| Product 3 | ![Product 3](https://via.placeholder.com/100x100) | Luxury product | $199.99 |

### Multiple Images in Same Cell

| Category | Gallery | Info |
|----------|---------|------|
| Electronics | ![](https://via.placeholder.com/80x80) ![](https://via.placeholder.com/80x80) ![](https://via.placeholder.com/80x80) | Three products |
| Clothing | ![](https://via.placeholder.com/80x80) ![](https://via.placeholder.com/80x80) | Two products |

### Images in Lists

- **Item with image:**

  ![List Image 1](https://via.placeholder.com/300x150)

  Description below the image

- **Another item:**

  ![List Image 2](https://via.placeholder.com/300x150)

  More content here

1. **Ordered list with images:**

   ![Ordered 1](https://via.placeholder.com/250x125)

2. **Second item:**

   ![Ordered 2](https://via.placeholder.com/250x125)

### Images in Blockquotes

> Here's a quote with an image:
>
> ![Quote Image](https://via.placeholder.com/400x200)
>
> The image is inside the blockquote!

### Images in Collapsible Sections

<details>
<summary>Click to see images</summary>

![Hidden Image 1](https://via.placeholder.com/300x150)

![Hidden Image 2](https://via.placeholder.com/300x150)

You can hide multiple images in collapsible sections!

</details>

### Clickable Images with Captions

<figure>
  <a href="https://example.com">
    <img src="https://via.placeholder.com/400x200" alt="Clickable image">
  </a>
  <figcaption>Click the image above to visit example.com</figcaption>
</figure>

### Images Side by Side (Grid)

<div style="display: grid; grid-template-columns: repeat(3, 1fr); gap: 10px;">
  <img src="https://via.placeholder.com/200x200" alt="Image 1">
  <img src="https://via.placeholder.com/200x200" alt="Image 2">
  <img src="https://via.placeholder.com/200x200" alt="Image 3">
</div>

### Images with Different Sizes

<div style="display: flex; align-items: center; gap: 20px;">
  <img src="https://via.placeholder.com/100x100" alt="Small">
  <img src="https://via.placeholder.com/200x200" alt="Medium">
  <img src="https://via.placeholder.com/300x300" alt="Large">
</div>

### Rounded Images

<img src="https://via.placeholder.com/150x150" alt="Rounded" style="border-radius: 50%;">

### Images with Borders and Shadows

<img src="https://via.placeholder.com/300x200" alt="Styled" style="border: 3px solid #3498db; box-shadow: 0 4px 8px rgba(0,0,0,0.2); border-radius: 8px;">

### Overlapping Images

<div style="position: relative; height: 200px;">
  <img src="https://via.placeholder.com/200x150" style="position: absolute; top: 0; left: 0; z-index: 1;">
  <img src="https://via.placeholder.com/200x150" style="position: absolute; top: 30px; left: 30px; z-index: 2;">
  <img src="https://via.placeholder.com/200x150" style="position: absolute; top: 60px; left: 60px; z-index: 3;">
</div>

---

## Tables Advanced

### Table with Images and Code

| Feature | Screenshot | Code Example | Status |
|---------|------------|--------------|--------|
| Authentication | ![Auth](https://via.placeholder.com/100x60) | `auth.login()` | ✅ |
| Dashboard | ![Dash](https://via.placeholder.com/100x60) | `dashboard.render()` | ✅ |
| Analytics | ![Analytics](https://via.placeholder.com/100x60) | `analytics.track()` | 🚧 |

### Table with Links and Badges

| Package | Version | Downloads | Status |
|---------|---------|-----------|--------|
| [react](https://react.dev) | ![npm](https://img.shields.io/badge/v18.2.0-blue) | ![downloads](https://img.shields.io/badge/downloads-18M/week-green) | ✅ Stable |
| [vue](https://vuejs.org) | ![npm](https://img.shields.io/badge/v3.3.0-green) | ![downloads](https://img.shields.io/badge/downloads-5M/week-green) | ✅ Stable |

### Nested Tables (Table in Table Cell)

| Outer Column 1 | Outer Column 2 |
|----------------|----------------|
| Regular content | <table><tr><th>Inner</th><th>Table</th></tr><tr><td>Data 1</td><td>Data 2</td></tr></table> |

### Table with Merged Cells (HTML)

<table>
  <tr>
    <th>Name</th>
    <th colspan="2">Details</th>
    <th>Status</th>
  </tr>
  <tr>
    <td rowspan="2">Product A</td>
    <td>Price</td>
    <td>$99</td>
    <td>✅</td>
  </tr>
  <tr>
    <td>Stock</td>
    <td>50 units</td>
    <td>✅</td>
  </tr>
</table>

### Table with Color-Coded Rows

<table>
  <tr>
    <th>Status</th>
    <th>Description</th>
  </tr>
  <tr style="background-color: #d4edda;">
    <td>✅ Success</td>
    <td>Operation completed</td>
  </tr>
  <tr style="background-color: #fff3cd;">
    <td>⚠️ Warning</td>
    <td>Check this issue</td>
  </tr>
  <tr style="background-color: #f8d7da;">
    <td>❌ Error</td>
    <td>Failed to process</td>
  </tr>
</table>

### Table with Progress Bars

| Task | Progress | Status |
|------|----------|--------|
| Setup | <div style="width: 100%; background: #e0e0e0; border-radius: 4px;"><div style="width: 100%; background: #4caf50; padding: 4px; border-radius: 4px; text-align: center; color: white;">100%</div></div> | ✅ |
| Development | <div style="width: 100%; background: #e0e0e0; border-radius: 4px;"><div style="width: 60%; background: #2196f3; padding: 4px; border-radius: 4px; text-align: center; color: white;">60%</div></div> | 🚧 |
| Testing | <div style="width: 100%; background: #e0e0e0; border-radius: 4px;"><div style="width: 30%; background: #ff9800; padding: 4px; border-radius: 4px; text-align: center; color: white;">30%</div></div> | ⏳ |

### Table with Checkboxes

<table>
  <tr>
    <th>Select</th>
    <th>Item</th>
    <th>Price</th>
  </tr>
  <tr>
    <td><input type="checkbox" checked></td>
    <td>Product 1</td>
    <td>$50</td>
  </tr>
  <tr>
    <td><input type="checkbox"></td>
    <td>Product 2</td>
    <td>$75</td>
  </tr>
  <tr>
    <td><input type="checkbox" checked></td>
    <td>Product 3</td>
    <td>$100</td>
  </tr>
</table>

### Table with Collapsible Rows

<table>
  <tr>
    <th>Category</th>
    <th>Details</th>
  </tr>
  <tr>
    <td>Electronics</td>
    <td>
      <details>
        <summary>View products</summary>
        <ul>
          <li>Laptop - $999</li>
          <li>Phone - $699</li>
          <li>Tablet - $499</li>
        </ul>
      </details>
    </td>
  </tr>
</table>

### Very Wide Table (Horizontal Scroll Test)

| Col1 | Col2 | Col3 | Col4 | Col5 | Col6 | Col7 | Col8 | Col9 | Col10 | Col11 | Col12 | Col13 | Col14 | Col15 |
|------|------|------|------|------|------|------|------|------|-------|-------|-------|-------|-------|-------|
| A | B | C | D | E | F | G | H | I | J | K | L | M | N | O |
| 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 | 15 |

### Table with Emoji Icons

| Status | Icon | Description |
|--------|------|-------------|
| Success | ✅ ✔️ ☑️ | All good |
| Warning | ⚠️ ⚡ 🔔 | Be careful |
| Error | ❌ ⛔ 🚫 | Something wrong |
| Info | ℹ️ 💡 📌 | Information |
| Question | ❓ ❔ 🤔 | Need help |

### Table with Code Blocks in Cells

| Language | Hello World |
|----------|-------------|
| JavaScript | `console.log("Hello");` |
| Python | `print("Hello")` |
| Ruby | `puts "Hello"` |
| Go | `fmt.Println("Hello")` |

---

## Grid Layouts

### 2-Column Grid

<div style="display: grid; grid-template-columns: 1fr 1fr; gap: 20px;">
  <div style="border: 2px solid #3498db; padding: 20px; border-radius: 8px;">
    <h3>Column 1</h3>
    <p>Content in first column</p>
    <img src="https://via.placeholder.com/200x150" alt="Grid 1">
  </div>
  <div style="border: 2px solid #e74c3c; padding: 20px; border-radius: 8px;">
    <h3>Column 2</h3>
    <p>Content in second column</p>
    <img src="https://via.placeholder.com/200x150" alt="Grid 2">
  </div>
</div>

### 3-Column Grid

<div style="display: grid; grid-template-columns: repeat(3, 1fr); gap: 15px;">
  <div style="background: #3498db; color: white; padding: 20px; text-align: center; border-radius: 8px;">
    <h4>Feature 1</h4>
    <p>Description</p>
  </div>
  <div style="background: #2ecc71; color: white; padding: 20px; text-align: center; border-radius: 8px;">
    <h4>Feature 2</h4>
    <p>Description</p>
  </div>
  <div style="background: #e74c3c; color: white; padding: 20px; text-align: center; border-radius: 8px;">
    <h4>Feature 3</h4>
    <p>Description</p>
  </div>
</div>

### 4-Column Grid

<div style="display: grid; grid-template-columns: repeat(4, 1fr); gap: 10px;">
  <div style="background: #f0f0f0; padding: 15px; text-align: center; border-radius: 4px;">
    <img src="https://via.placeholder.com/100x100" alt="1" style="width: 100%;">
    <p>Item 1</p>
  </div>
  <div style="background: #f0f0f0; padding: 15px; text-align: center; border-radius: 4px;">
    <img src="https://via.placeholder.com/100x100" alt="2" style="width: 100%;">
    <p>Item 2</p>
  </div>
  <div style="background: #f0f0f0; padding: 15px; text-align: center; border-radius: 4px;">
    <img src="https://via.placeholder.com/100x100" alt="3" style="width: 100%;">
    <p>Item 3</p>
  </div>
  <div style="background: #f0f0f0; padding: 15px; text-align: center; border-radius: 4px;">
    <img src="https://via.placeholder.com/100x100" alt="4" style="width: 100%;">
    <p>Item 4</p>
  </div>
</div>

### Mixed Width Grid

<div style="display: grid; grid-template-columns: 2fr 1fr; gap: 20px;">
  <div style="border: 2px solid #9b59b6; padding: 20px; border-radius: 8px;">
    <h3>Main Content (2/3 width)</h3>
    <p>This takes up more space</p>
    <img src="https://via.placeholder.com/400x200" style="width: 100%;">
  </div>
  <div style="border: 2px solid #f39c12; padding: 20px; border-radius: 8px;">
    <h3>Sidebar (1/3 width)</h3>
    <ul>
      <li>Link 1</li>
      <li>Link 2</li>
      <li>Link 3</li>
    </ul>
  </div>
</div>

### Card Grid

<div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(250px, 1fr)); gap: 20px; margin: 20px 0;">
  <div style="border: 1px solid #ddd; border-radius: 8px; overflow: hidden; box-shadow: 0 2px 4px rgba(0,0,0,0.1);">
    <img src="https://via.placeholder.com/300x150" style="width: 100%; display: block;">
    <div style="padding: 15px;">
      <h4 style="margin: 0 0 10px 0;">Card Title 1</h4>
      <p style="margin: 0; color: #666;">Card description goes here</p>
      <button style="margin-top: 10px; padding: 8px 16px; background: #3498db; color: white; border: none; border-radius: 4px; cursor: pointer;">Learn More</button>
    </div>
  </div>
  <div style="border: 1px solid #ddd; border-radius: 8px; overflow: hidden; box-shadow: 0 2px 4px rgba(0,0,0,0.1);">
    <img src="https://via.placeholder.com/300x150" style="width: 100%; display: block;">
    <div style="padding: 15px;">
      <h4 style="margin: 0 0 10px 0;">Card Title 2</h4>
      <p style="margin: 0; color: #666;">Card description goes here</p>
      <button style="margin-top: 10px; padding: 8px 16px; background: #3498db; color: white; border: none; border-radius: 4px; cursor: pointer;">Learn More</button>
    </div>
  </div>
  <div style="border: 1px solid #ddd; border-radius: 8px; overflow: hidden; box-shadow: 0 2px 4px rgba(0,0,0,0.1);">
    <img src="https://via.placeholder.com/300x150" style="width: 100%; display: block;">
    <div style="padding: 15px;">
      <h4 style="margin: 0 0 10px 0;">Card Title 3</h4>
      <p style="margin: 0; color: #666;">Card description goes here</p>
      <button style="margin-top: 10px; padding: 8px 16px; background: #3498db; color: white; border: none; border-radius: 4px; cursor: pointer;">Learn More</button>
    </div>
  </div>
</div>

---

## Custom Styling

### Colored Text Boxes

<div style="background: #e8f5e9; border-left: 4px solid #4caf50; padding: 16px; margin: 16px 0;">
  ✅ <strong>Success:</strong> Operation completed successfully!
</div>

<div style="background: #fff3e0; border-left: 4px solid #ff9800; padding: 16px; margin: 16px 0;">
  ⚠️ <strong>Warning:</strong> Please review this carefully.
</div>

<div style="background: #ffebee; border-left: 4px solid #f44336; padding: 16px; margin: 16px 0;">
  ❌ <strong>Error:</strong> Something went wrong!
</div>

<div style="background: #e3f2fd; border-left: 4px solid #2196f3; padding: 16px; margin: 16px 0;">
  ℹ️ <strong>Info:</strong> Here's some helpful information.
</div>

<div style="background: #f3e5f5; border-left: 4px solid #9c27b0; padding: 16px; margin: 16px 0;">
  💡 <strong>Tip:</strong> Pro tip for better results!
</div>

### Colored Text

<span style="color: #e74c3c;">Red text</span> |
<span style="color: #3498db;">Blue text</span> |
<span style="color: #2ecc71;">Green text</span> |
<span style="color: #f39c12;">Orange text</span> |
<span style="color: #9b59b6;">Purple text</span>

### Background Highlighted Text

<span style="background: #ffeb3b; padding: 2px 6px; border-radius: 3px;">Yellow highlight</span>
<span style="background: #ff9800; color: white; padding: 2px 6px; border-radius: 3px;">Orange highlight</span>
<span style="background: #4caf50; color: white; padding: 2px 6px; border-radius: 3px;">Green highlight</span>

### Badges

<span style="background: #3498db; color: white; padding: 4px 12px; border-radius: 12px; font-size: 12px; font-weight: bold;">NEW</span>
<span style="background: #2ecc71; color: white; padding: 4px 12px; border-radius: 12px; font-size: 12px; font-weight: bold;">STABLE</span>
<span style="background: #e74c3c; color: white; padding: 4px 12px; border-radius: 12px; font-size: 12px; font-weight: bold;">DEPRECATED</span>
<span style="background: #f39c12; color: white; padding: 4px 12px; border-radius: 12px; font-size: 12px; font-weight: bold;">BETA</span>

### Buttons

<button style="background: #3498db; color: white; border: none; padding: 10px 20px; border-radius: 4px; cursor: pointer; font-size: 14px; margin: 5px;">Primary Button</button>
<button style="background: #2ecc71; color: white; border: none; padding: 10px 20px; border-radius: 4px; cursor: pointer; font-size: 14px; margin: 5px;">Success Button</button>
<button style="background: #e74c3c; color: white; border: none; padding: 10px 20px; border-radius: 4px; cursor: pointer; font-size: 14px; margin: 5px;">Danger Button</button>
<button style="background: white; color: #3498db; border: 2px solid #3498db; padding: 10px 20px; border-radius: 4px; cursor: pointer; font-size: 14px; margin: 5px;">Outline Button</button>

### Pills

<div style="display: inline-flex; gap: 8px; flex-wrap: wrap;">
  <span style="background: #ecf0f1; padding: 6px 12px; border-radius: 16px; font-size: 13px;">React</span>
  <span style="background: #ecf0f1; padding: 6px 12px; border-radius: 16px; font-size: 13px;">Vue</span>
  <span style="background: #ecf0f1; padding: 6px 12px; border-radius: 16px; font-size: 13px;">Angular</span>
  <span style="background: #ecf0f1; padding: 6px 12px; border-radius: 16px; font-size: 13px;">Svelte</span>
</div>

### Progress Bars

<div style="margin: 20px 0;">
  <div style="margin-bottom: 10px;">
    <strong>HTML/CSS</strong>
    <div style="background: #ecf0f1; border-radius: 8px; height: 20px; overflow: hidden;">
      <div style="background: linear-gradient(90deg, #3498db, #2ecc71); width: 90%; height: 100%; display: flex; align-items: center; justify-content: center; color: white; font-size: 12px; font-weight: bold;">90%</div>
    </div>
  </div>
  <div style="margin-bottom: 10px;">
    <strong>JavaScript</strong>
    <div style="background: #ecf0f1; border-radius: 8px; height: 20px; overflow: hidden;">
      <div style="background: linear-gradient(90deg, #f39c12, #e74c3c); width: 75%; height: 100%; display: flex; align-items: center; justify-content: center; color: white; font-size: 12px; font-weight: bold;">75%</div>
    </div>
  </div>
  <div style="margin-bottom: 10px;">
    <strong>React</strong>
    <div style="background: #ecf0f1; border-radius: 8px; height: 20px; overflow: hidden;">
      <div style="background: linear-gradient(90deg, #9b59b6, #e74c3c); width: 60%; height: 100%; display: flex; align-items: center; justify-content: center; color: white; font-size: 12px; font-weight: bold;">60%</div>
    </div>
  </div>
</div>

### Custom Containers with Icons

<div style="display: grid; grid-template-columns: repeat(2, 1fr); gap: 15px; margin: 20px 0;">
  <div style="border: 2px solid #3498db; border-radius: 8px; padding: 15px; display: flex; align-items: start; gap: 10px;">
    <span style="font-size: 24px;">📘</span>
    <div>
      <strong style="color: #3498db;">Documentation</strong>
      <p style="margin: 5px 0 0 0; font-size: 14px;">Read our comprehensive docs</p>
    </div>
  </div>
  <div style="border: 2px solid #2ecc71; border-radius: 8px; padding: 15px; display: flex; align-items: start; gap: 10px;">
    <span style="font-size: 24px;">🎓</span>
    <div>
      <strong style="color: #2ecc71;">Tutorials</strong>
      <p style="margin: 5px 0 0 0; font-size: 14px;">Learn with step-by-step guides</p>
    </div>
  </div>
  <div style="border: 2px solid #f39c12; border-radius: 8px; padding: 15px; display: flex; align-items: start; gap: 10px;">
    <span style="font-size: 24px;">💬</span>
    <div>
      <strong style="color: #f39c12;">Community</strong>
      <p style="margin: 5px 0 0 0; font-size: 14px;">Join our Discord server</p>
    </div>
  </div>
  <div style="border: 2px solid #e74c3c; border-radius: 8px; padding: 15px; display: flex; align-items: start; gap: 10px;">
    <span style="font-size: 24px;">🚀</span>
    <div>
      <strong style="color: #e74c3c;">API</strong>
      <p style="margin: 5px 0 0 0; font-size: 14px;">Explore our REST API</p>
    </div>
  </div>
</div>

### Tabs (Fake with CSS)

<div style="margin: 20px 0;">
  <div style="display: flex; border-bottom: 2px solid #ecf0f1;">
    <div style="padding: 10px 20px; background: #3498db; color: white; border-radius: 4px 4px 0 0; margin-bottom: -2px; border: 2px solid #3498db; border-bottom: none;">Tab 1</div>
    <div style="padding: 10px 20px; border-radius: 4px 4px 0 0; margin-bottom: -2px; border: 2px solid transparent; cursor: pointer;">Tab 2</div>
    <div style="padding: 10px 20px; border-radius: 4px 4px 0 0; margin-bottom: -2px; border: 2px solid transparent; cursor: pointer;">Tab 3</div>
  </div>
  <div style="border: 2px solid #ecf0f1; border-top: none; padding: 20px; border-radius: 0 0 4px 4px;">
    <h4>Tab 1 Content</h4>
    <p>This is the content of tab 1.</p>
  </div>
</div>

### Timeline

<div style="position: relative; padding-left: 40px; margin: 20px 0;">
  <div style="position: absolute; left: 15px; top: 0; bottom: 0; width: 2px; background: #3498db;"></div>

  <div style="position: relative; margin-bottom: 30px;">
    <div style="position: absolute; left: -33px; width: 12px; height: 12px; background: #3498db; border: 3px solid white; border-radius: 50%; box-shadow: 0 0 0 2px #3498db;"></div>
    <strong>2024 - Launch</strong>
    <p>Product launched to the public</p>
  </div>

  <div style="position: relative; margin-bottom: 30px;">
    <div style="position: absolute; left: -33px; width: 12px; height: 12px; background: #2ecc71; border: 3px solid white; border-radius: 50%; box-shadow: 0 0 0 2px #2ecc71;"></div>
    <strong>2025 - Growth</strong>
    <p>Reached 1M users</p>
  </div>

  <div style="position: relative;">
    <div style="position: absolute; left: -33px; width: 12px; height: 12px; background: #f39c12; border: 3px solid white; border-radius: 50%; box-shadow: 0 0 0 2px #f39c12;"></div>
    <strong>2026 - Expansion</strong>
    <p>Global expansion planned</p>
  </div>
</div>

---

## Videos & Media

### Embedded YouTube Video

<div style="position: relative; padding-bottom: 56.25%; height: 0; overflow: hidden; max-width: 100%;">
  <iframe
    src="https://www.youtube.com/embed/dQw4w9WgXcQ"
    style="position: absolute; top: 0; left: 0; width: 100%; height: 100%;"
    frameborder="0"
    allowfullscreen>
  </iframe>
</div>

### Video with HTML5

<video controls width="100%" style="max-width: 600px;">
  <source src="https://www.w3schools.com/html/mov_bbb.mp4" type="video/mp4">
  Your browser does not support the video tag.
</video>

### Audio Player

<audio controls style="width: 100%; max-width: 500px;">
  <source src="https://www.w3schools.com/html/horse.mp3" type="audio/mpeg">
  Your browser does not support the audio element.
</audio>

### Embedded Map

<iframe
  src="https://www.google.com/maps/embed?pb=!1m18!1m12!1m3!1d3153.019616134765!2d-122.42008368468186!3d37.77492977975903!2m3!1f0!2f0!3f0!3m2!1i1024!2i768!4f13.1!3m3!1m2!1s0x8085809c6c8f4459%3A0xb10ed6d9b5050fa5!2sTwitter%20HQ!5e0!3m2!1sen!2sus!4v1234567890123"
  width="100%"
  height="300"
  style="border:0; border-radius: 8px;"
  allowfullscreen=""
  loading="lazy">
</iframe>

### CodePen Embed

<iframe
  height="300"
  style="width: 100%;"
  scrolling="no"
  title="Example Pen"
  src="https://codepen.io/team/codepen/embed/example"
  frameborder="no"
  loading="lazy"
  allowtransparency="true"
  allowfullscreen="true">
</iframe>

---

## Complex Nesting

### Everything in a Blockquote

> # Header in Blockquote
>
> ## Subheader
>
> **Bold** and *italic* text.
>
> - List item 1
> - List item 2
>   - Nested item
>     - Even deeper
>       - Maximum nesting
>
> 1. Ordered list
> 2. Second item
>
> | Table | In | Quote |
> |-------|-----|-------|
> | A | B | C |
>
> ```javascript
> console.log("Code in quote");
> ```
>
> ![Image in quote](https://via.placeholder.com/300x150)
>
> [Link in quote](https://example.com)
>
> > Nested blockquote
> > > Double nested
> > > > Triple nested
>
> <details>
> <summary>Collapsible in quote</summary>
> Hidden content!
> </details>

### Everything in a Collapsible Section

<details>
<summary><strong>Click to expand EVERYTHING</strong></summary>

# All Elements Inside

## Headers work

**Bold**, *italic*, ~~strikethrough~~

- Unordered lists
  - Nested
    - Even deeper
      - Maximum depth

1. Ordered lists
2. Second item
   1. Nested ordered
   2. Another nested

| Tables | Work | Too |
|--------|------|-----|
| A | B | C |

```javascript
// Code blocks
function test() {
  return "Hello from collapsible!";
}
```

![Images work](https://via.placeholder.com/400x200)

> Blockquotes also work
>
> With multiple paragraphs

[Links work](https://example.com)

<div style="background: #3498db; color: white; padding: 15px; border-radius: 8px;">
  <strong>Custom HTML works!</strong>
</div>

<details>
<summary>Nested collapsible</summary>

Even nested collapsibles work!

<details>
<summary>Triple nested!</summary>

This is getting deep!

</details>

</details>

---

**Math works too:** $E = mc^2$

</details>

### List with Everything

1. **First item** - Contains everything:

   Regular paragraph text under the list item.

   ![Image in list](https://via.placeholder.com/300x150)

   ```javascript
   // Code in list
   const listItem = true;
   ```

   | Table | In | List |
   |-------|-----|------|
   | X | Y | Z |

   > Blockquote in list

   - Nested unordered
     - Even deeper
       - Maximum depth
         - One more
           - Final level

   <details>
   <summary>Collapsible in list</summary>
   Hidden content in list item!
   </details>

   <div style="background: #e8f5e9; padding: 10px; border-radius: 4px; margin: 10px 0;">
   Custom HTML box in list
   </div>

2. **Second item** with more content

   Another paragraph.

   - [ ] Task list
   - [x] Checked item
   - [ ] Another task

### Table with Everything in Cells

| Column 1 | Column 2 | Column 3 |
|----------|----------|----------|
| ![](https://via.placeholder.com/100x80) | **Bold text**<br>`code`<br>*italic* | <ul><li>List</li><li>Items</li></ul> |
| `code block` | [Link](/) | <details><summary>Click</summary>Hidden!</details> |
| <div style="background: #3498db; color: white; padding: 10px; text-align: center;">Custom</div> | ```js<br>code<br>``` | ✅ ❌ ⚠️ |

---

## All Combinations

### Bold + Italic + Strikethrough + Code

***~~Bold italic strikethrough~~ and `code`*** mixed together

### Link + Image + Bold + Italic

[![***Bold Italic Clickable Image***](https://via.placeholder.com/300x150)](https://example.com)

### List + Blockquote + Code + Image

- List item with quote:
  > Quote with code: `const x = 10;`
  >
  > And an image:
  >
  > ![](https://via.placeholder.com/250x125)

### Table + List + Code + Image + Links

| Feature | Details |
|---------|---------|
| **Lists** | <ul><li>Item 1</li><li>Item 2</li></ul> |
| **Code** | `console.log("test")` |
| **Images** | ![](https://via.placeholder.com/80x60) |
| **Links** | [Click here](/) |
| **Mixed** | `code` + **bold** + [link](/) |

### Collapsible + Table + Code + Image + List

<details>
<summary>Complete Example</summary>

| Type | Example | Status |
|------|---------|--------|
| Code | ```js<br>const x = 1;<br>``` | ✅ |
| Image | ![](https://via.placeholder.com/100x75) | ✅ |
| List | <ul><li>A</li><li>B</li></ul> | ✅ |

</details>

---

## Interactive Elements

### Fake Accordion

<details>
<summary><strong>Section 1: Getting Started</strong></summary>
<div style="padding: 10px; background: #f8f9fa; border-left: 3px solid #3498db; margin: 10px 0;">
Content for section 1
</div>
</details>

<details>
<summary><strong>Section 2: Advanced Topics</strong></summary>
<div style="padding: 10px; background: #f8f9fa; border-left: 3px solid #2ecc71; margin: 10px 0;">
Content for section 2
</div>
</details>

<details>
<summary><strong>Section 3: API Reference</strong></summary>
<div style="padding: 10px; background: #f8f9fa; border-left: 3px solid #f39c12; margin: 10px 0;">
Content for section 3
</div>
</details>

### Fake Dropdown Menu

<div style="position: relative; display: inline-block;">
  <button style="background: #3498db; color: white; padding: 10px 20px; border: none; border-radius: 4px; cursor: pointer;">
    Menu ▼
  </button>
  <div style="display: none; position: absolute; background: white; min-width: 160px; box-shadow: 0 8px 16px rgba(0,0,0,0.2); border-radius: 4px; z-index: 1;">
    <a href="#" style="display: block; padding: 12px 16px; text-decoration: none; color: black;">Option 1</a>
    <a href="#" style="display: block; padding: 12px 16px; text-decoration: none; color: black;">Option 2</a>
    <a href="#" style="display: block; padding: 12px 16px; text-decoration: none; color: black;">Option 3</a>
  </div>
</div>

### Toggle Switch (Fake)

<div style="display: flex; align-items: center; gap: 10px;">
  <div style="width: 50px; height: 24px; background: #2ecc71; border-radius: 12px; position: relative; cursor: pointer;">
    <div style="width: 20px; height: 20px; background: white; border-radius: 50%; position: absolute; right: 2px; top: 2px; box-shadow: 0 2px 4px rgba(0,0,0,0.2);"></div>
  </div>
  <span>Enabled</span>
</div>

<div style="display: flex; align-items: center; gap: 10px; margin-top: 10px;">
  <div style="width: 50px; height: 24px; background: #bdc3c7; border-radius: 12px; position: relative; cursor: pointer;">
    <div style="width: 20px; height: 20px; background: white; border-radius: 50%; position: absolute; left: 2px; top: 2px; box-shadow: 0 2px 4px rgba(0,0,0,0.2);"></div>
  </div>
  <span>Disabled</span>
</div>

### Radio Buttons

<div style="margin: 10px 0;">
  <input type="radio" id="option1" name="options" checked>
  <label for="option1">Option 1</label>
</div>
<div style="margin: 10px 0;">
  <input type="radio" id="option2" name="options">
  <label for="option2">Option 2</label>
</div>
<div style="margin: 10px 0;">
  <input type="radio" id="option3" name="options">
  <label for="option3">Option 3</label>
</div>

### Checkboxes with Labels

<div style="margin: 10px 0;">
  <input type="checkbox" id="check1" checked>
  <label for="check1">React</label>
</div>
<div style="margin: 10px 0;">
  <input type="checkbox" id="check2">
  <label for="check2">Vue</label>
</div>
<div style="margin: 10px 0;">
  <input type="checkbox" id="check3" checked>
  <label for="check3">Angular</label>
</div>

### Star Rating

<span style="color: #f39c12; font-size: 24px;">★★★★☆</span> (4/5)

### Rating with Colors

<div style="display: flex; gap: 5px;">
  <span style="color: #2ecc71; font-size: 20px;">★</span>
  <span style="color: #2ecc71; font-size: 20px;">★</span>
  <span style="color: #f39c12; font-size: 20px;">★</span>
  <span style="color: #e74c3c; font-size: 20px;">★</span>
  <span style="color: #bdc3c7; font-size: 20px;">★</span>
</div>

---

## Edge Cases Extreme

### Empty Elements

Empty blockquote:
>

Empty list item:
-

Empty table cell:
| Empty | |
|-------|--|
| | |

### Very Long Word

Supercalifragilisticexpialidocious Pneumonoultramicroscopicsilicovolcanoconiosis Hippopotomonstrosesquippedaliophobia

### Very Long URL

https://www.example.com/very/long/url/path/that/keeps/going/and/going/and/going/to/test/overflow/handling/in/your/renderer

### Special Characters in Table

| Character | Display |
|-----------|---------|
| & | Ampersand |
| < | Less than |
| > | Greater than |
| " | Quote |
| ' | Apostrophe |
| \| | Pipe |

### Unicode Characters

Greek: α β γ δ ε ζ η θ

Math: ∞ ≠ ≤ ≥ ± ÷ × ∑ ∫

Arrows: ← → ↑ ↓ ↔ ⇐ ⇒ ⇔

Symbols: © ® ™ § ¶ † ‡ • ◦

Currencies: $ € £ ¥ ₹ ₽ ₿

### Emoji Combinations

😀😃😄😁😆😅🤣😂🙂🙃😉😊😇

🚀🎯🔥💡⚡🌟✨🎉🎊🎈

❤️🧡💛💚💙💜🖤🤍🤎

### ASCII Art

```
    _____
   /     \
  | () () |
   \  ^  /
    |||||
    |||||
```

### All HTML Entities

&nbsp; &lt; &gt; &amp; &quot; &apos; &cent; &pound; &yen; &euro; &copy; &reg;

### Nested Everything Maximum

<details>
<summary>Level 1</summary>

> Level 1 Quote
>
> <details>
> <summary>Level 2</summary>
>
> - Level 2 List
>   - Level 3 List
>     - Level 4 List
>       - Level 5 List
>         - Level 6 List
>
> | Table | At | Level 2 |
> |-------|-----|---------|
> | With | Nested | Content |
>
> ```javascript
> // Code at level 2
> function nested() {
>   return {
>     level: 2,
>     works: true
>   };
> }
> ```
>
> ![Image at level 2](https://via.placeholder.com/200x100)
>
> > Quote at level 2
> >
> > > Quote at level 3
> > >
> > > > Quote at level 4
>
> <details>
> <summary>Level 3</summary>
>
> Even deeper!
>
> <details>
> <summary>Level 4</summary>
>
> Maximum depth!
>
> | Deep | Table |
> |------|-------|
> | Very | Deep |
>
> </details>
>
> </details>
>
> </details>

</details>

### Mix of Everything in One Line

**Bold** *italic* ~~strike~~ `code` [link](/) ![img](https://via.placeholder.com/20x20) <span style="color:red;">colored</span> ✅ 🚀 &copy;

---

## Final Stress Test

### Mega Complex Example

<div style="border: 2px solid #3498db; border-radius: 8px; padding: 20px; margin: 20px 0;">

# 🎯 Complex Section

<div style="display: grid; grid-template-columns: 1fr 1fr; gap: 20px;">

<div>

## Left Column

- **List** with *formatting*
- `Code` in list
- [Links](/) in list

| Small | Table |
|-------|-------|
| A | B |

```javascript
console.log("Left side");
```

</div>

<div>

## Right Column

> **Quote** with:
> - Lists
> - `Code`
> - Everything

![Right Image](https://via.placeholder.com/200x150)

<details>
<summary>Collapsible Right</summary>
Hidden right content!
</details>

</div>

</div>

---

### Bottom Section

<div style="display: flex; gap: 10px; flex-wrap: wrap;">
  <span style="background: #3498db; color: white; padding: 6px 12px; border-radius: 16px;">Tag1</span>
  <span style="background: #2ecc71; color: white; padding: 6px 12px; border-radius: 16px;">Tag2</span>
  <span style="background: #f39c12; color: white; padding: 6px 12px; border-radius: 16px;">Tag3</span>
</div>

</div>

---

## The End

**Total elements tested:** 500+

**Total combinations:** 1000+

**File size:** ~40KB

🎉 **You've reached the end of the ultimate markdown test document!**

*Last updated: January 22, 2026*
