# 11ty Crash Course: Part 1

This video tutorial introduces Eleventy (11ty), a popular static site generator, emphasizing its "no batteries included" philosophy which gives developers more control over project setup compared to tools like Astro. The guide walks through the initial steps of setting up an 11ty project, including creating a package.json file and installing Eleventy as a development dependency. A significant portion of the crash course focuses on leveraging Nunjucks templating within HTML files, which enables features like defining layouts for consistent page structure and using includes to separate components like the head tag. Furthermore, the tutorial demonstrates advanced templating concepts such as Nunjucks blocks and extensions, which allow developers to override specific sections of a base layout for specialized pages, ensuring a scalable and efficient site architecture.

# 11ty Crash Course: Part 1 - Setting Up Your First Static Site

Welcome back to the channel! I'm Jaydan Urwin, and today we're diving into an 11ty crash course. This is part 1 of what will likely be a multi-part series, focusing on the fundamentals of setting up an 11ty project from scratch.

## What is 11ty?

If you haven't heard of 11ty (Eleventy), it's currently one of the biggest static site generators being used today. While there are other popular options like Jekyll and Hugo, 11ty has largely replaced them in the static site generator space.

You can find 11ty at [11ty.dev](https://11ty.dev) - a pretty cool and memorable name!

### 11ty vs Other Static Site Generators

11ty takes a "no batteries included" approach, which differs significantly from my previous series on Astro. While Astro is more "batteries included," 11ty gives you the freedom to decide:

- Which JavaScript bundler to use
- How to set up your CSS
- How CSS integrates with your templates

This means more decisions to make, but it can be beneficial for many different types of projects.

### Why Choose 11ty?

11ty has some compelling advantages:

- **Mature ecosystem**: It's been around longer than Astro
- **Stable API**: Currently at version 1.0, meaning it's in a stable phase
- **Extensive documentation**: Well-documented with lots of examples
- **Plugin ecosystem**: Great selection of useful plugins

If you're working on a large project that requires stability and an API that won't change frequently, 11ty might be the perfect choice.

## Getting Started: Project Setup

While 11ty offers starter projects you can clone, I prefer building from scratch to implement my own opinions and preferences.

### Initial Setup

Let's start by creating our project structure:

```bash
# Create project directory
mkdir 11ty-crash-course
cd 11ty-crash-course

# Initialize npm package
npm init -y

# Install 11ty as development dependency
npm install --save-dev @11ty/eleventy
```

### Project Structure

I like to organize my 11ty projects with a specific structure:

```
11ty-crash-course/
├── .eleventy.js          # Configuration file
├── package.json
└── src/                  # Source directory
    ├── index.html
    └── _includes/        # Templates and partials
        └── layouts/
```

### Configuration File

Create `.eleventy.js` in your project root:

```javascript
module.exports = function(eleventyConfig) {
  return {
    dir: {
      input: "src",
      includes: "_includes",
      output: "_site"
    },
    templateFormats: ["md", "njk", "html"],
    markdownTemplateEngine: "njk",
    htmlTemplateEngine: "njk",
    dataTemplateEngine: "njk"
  };
};
```

**Note**: 11ty currently uses CommonJS (CJS) for imports, though better ESM support is planned for the future.

### Package.json Scripts

Add a start script to your `package.json`:

```json
{
  "scripts": {
    "start": "npx @11ty/eleventy --serve"
  }
}
```

## Template Formats and Nunjucks

One unique feature of 11ty is its support for multiple template formats:

- **Markdown**: Great for content
- **Nunjucks**: Powerful templating with JavaScript logic (similar to Liquid)
- **HTML**: Standard HTML with template features

A recent discovery: you can use Nunjucks templates inside regular HTML files, which opens up powerful possibilities.

## Creating Your First Pages

### Basic HTML Page

Start with a simple `src/index.html`:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <h1>I am the home page</h1>
    <p>I am also on the home page</p>
</body>
</html>
```

Run `npm start` and visit `localhost:8080` to see your page in action!

## Layouts and Templates

### Creating a Base Layout

To avoid repeating HTML structure across pages, create `src/_includes/layouts/base.html`:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>{{ title }}</title>
</head>
<body>
    {{ content | safe }}
</body>
</html>
```

### Using Layouts in Pages

Update your `src/index.html` to use the layout:

```html
---
layout: layouts/base.html
title: "11ty Crash Course #1"
---

<h1>{{ title }}</h1>
<p>I am also on the home page</p>
```

The front matter (between the `---` lines) sets the layout and passes data to the template.

## Creating Additional Pages

Create `src/contact.html`:

```html
---
layout: layouts/base.html
title: "Contact Page"
---

<h1>I am the {{ title }}</h1>
<a href="/">Go to home</a>
```

Add navigation to your home page:

```html
---
layout: layouts/base.html
title: "11ty Crash Course #1"
---

<h1>{{ title }}</h1>
<p>I am also on the home page</p>
<a href="/contact">Go to the contact page</a>
```

## Includes and Components

### Extracting the Head Section

Create `src/_includes/base-head.html`:

```html
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>{{ title }}</title>
```

Update your base layout:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    {% include "base-head.html" %}
</head>
<body>
    {{ content | safe }}
</body>
</html>
```

## Advanced Layout Features: Blocks and Extends

### Using Blocks for Extensibility

Update `src/_includes/layouts/base.html`:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    {% include "base-head.html" %}
    {% block head %}{% endblock %}
</head>
<body>
    {{ content | safe }}
</body>
</html>
```

### Creating Extended Layouts

Create `src/_includes/layouts/contact.html`:

```html
{% extends "layouts/base.html" %}

{% block head %}
<meta name="description" content="{{ description }}">
{% endblock %}
```

Use the extended layout in your contact page:

```html
---
layout: layouts/contact.html
title: "Contact Page"
description: "Hey, I'm on the contact page only"
---

<h1>I am the {{ title }}</h1>
<a href="/">Go to home</a>
```

## Development Tools

### Recommended VS Code Extension

Install the "Nunjucks Snippets" extension for VS Code to speed up your development with helpful shortcuts and syntax highlighting.

### Live Reloading

11ty's development server includes automatic reloading, making development fast and efficient. The server is lightweight and provides excellent performance during development.

## What's Next?

This covers the fundamentals of 11ty setup and templating. In future parts of this series, we'll explore:

- Setting up styles and CSS processing
- Image optimization with 11ty's image plugin
- Working with markdown files
- Integrating with headless CMS solutions
- Advanced plugin usage

## Key Takeaways

11ty's power lies in its flexibility and the robust templating system provided by Nunjucks. The combination of:

- Layouts for consistent structure
- Includes for reusable components
- Blocks for extensible templates
- Front matter for data passing

Creates a powerful foundation that can scale from simple sites to complex, large-scale projects.

## Getting Help

If you have questions:

- Check out the extensive [11ty documentation](https://11ty.dev)
- Join the 11ty Discord community
- Leave questions in the comments below

The 11ty community is welcoming and helpful, making it easy to get support when you need it.

---

*This is part 1 of the 11ty crash course series. Stay tuned for more advanced topics and techniques!*
