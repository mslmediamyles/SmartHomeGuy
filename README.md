# SmartHomeGuy - GitHub Pages Blog

A modern, tech-focused blog for smart home automation, IoT projects, and code snippets. Built with Jekyll and designed for GitHub Pages.

## Features

- 🎨 **Distinctive Design**: Dark theme with monospace aesthetics and neon green accents
- 📝 **Easy Post Management**: Simply add Markdown files to `_posts/` directory
- 💻 **Code Snippet Support**: Syntax highlighting with Monokai-inspired theme
- 📱 **Responsive**: Works perfectly on desktop, tablet, and mobile
- ⚡ **Fast**: Static site generation with Jekyll
- 🔍 **SEO-Friendly**: Proper meta tags and structured content

## Quick Start

### 1. Setup GitHub Repository

1. Create a new repository named `yourusername.github.io` (replace `yourusername` with your GitHub username)
2. Upload all files from this folder to your repository
3. Go to repository Settings → Pages
4. Under "Source", select the `main` branch and `/ (root)` folder
5. Click Save

Your site will be live at `https://yourusername.github.io` in a few minutes!

### 2. Customize Your Site

Edit `_config.yml` to update:

```yaml
title: SmartHomeGuy
description: Your description here
url: "https://yourusername.github.io"  # Update with your username
```

Update the GitHub link in `_layouts/default.html` and `about.html`.

### 3. Add Your First Post

Create a new file in the `_posts/` directory with this naming format:
```
YYYY-MM-DD-title-of-post.md
```

Example: `2026-01-15-my-first-post.md`

Use this template:

```markdown
---
layout: post
title: "Your Post Title"
date: 2026-01-15
category: Home Automation
description: "A brief description of your post"
---

Your content here...

## Using Code Snippets

```python
def hello_world():
    print("Hello, Smart Home!")
```

More content...
```

## Writing Posts

### Front Matter

Every post needs front matter at the top:

```yaml
---
layout: post
title: "Your Title"
date: YYYY-MM-DD
category: Home Automation  # Optional
description: "Brief description"  # Optional
---
```

### Adding Code Snippets

Use triple backticks with language specification:

````markdown
```python
def my_function():
    return "Code here"
```
````

Supported languages include: python, yaml, cpp, javascript, bash, and many more.

### Formatting

- **Headers**: Use `##` for H2, `###` for H3, etc.
- **Bold**: `**bold text**`
- **Italic**: `*italic text*`
- **Links**: `[text](url)`
- **Lists**: Use `-` or `1.` for bullet and numbered lists
- **Blockquotes**: Start line with `>`

### Example Post Structure

```markdown
---
layout: post
title: "My Awesome Project"
date: 2026-01-15
category: IoT
---

Introduction paragraph...

## Prerequisites

- Item 1
- Item 2

## Step 1: Setup

Explanation text...

```python
# Code example
print("Hello")
```

## Conclusion

Wrap up...
```

## Project Structure

```
smarthomeguy-site/
├── _config.yml           # Site configuration
├── _layouts/
│   ├── default.html      # Main layout template
│   └── post.html         # Post layout template
├── _posts/               # Your blog posts go here
│   ├── 2026-01-14-automating-smart-home-with-home-assistant.md
│   └── 2026-01-12-esp32-temperature-monitor.md
├── assets/
│   ├── css/
│   │   ├── style.css     # Main stylesheet
│   │   └── syntax.css    # Code syntax highlighting
│   └── js/
│       └── main.js       # JavaScript functionality
├── index.html            # Homepage
├── archive.html          # All posts archive
├── about.html            # About page
└── README.md             # This file
```

## Customization

### Colors

Edit CSS variables in `assets/css/style.css`:

```css
:root {
  --color-bg: #0a0e14;
  --color-accent: #00e676;
  /* ... more colors */
}
```

### Fonts

The site uses:
- **Sometype Mono** for body text (monospace)
- **Darker Grotesque** for headings

Change fonts in `_layouts/default.html` by updating the Google Fonts link.

### Navigation

Edit the navigation links in `_layouts/default.html`:

```html
<ul class="nav-links">
  <li><a href="/">Posts</a></li>
  <li><a href="/archive">Archive</a></li>
  <li><a href="/about">About</a></li>
</ul>
```

## Advanced Features

### Categories

Organize posts by category in the front matter:

```yaml
category: Home Automation
```

Categories appear as tags on post cards and in the post header.

### Copy Code Buttons

Code blocks automatically get "Copy" buttons thanks to `assets/js/main.js`.

### Syntax Highlighting

Uses Rouge syntax highlighter with a custom Monokai-inspired theme in `assets/css/syntax.css`.

## Local Development

To test locally before publishing:

1. Install Jekyll:
   ```bash
   gem install jekyll bundler
   ```

2. Create a Gemfile:
   ```ruby
   source 'https://rubygems.org'
   gem 'github-pages', group: :jekyll_plugins
   ```

3. Install dependencies:
   ```bash
   bundle install
   ```

4. Run local server:
   ```bash
   bundle exec jekyll serve
   ```

5. Visit `http://localhost:4000`

## Tips for Success

1. **Post Regularly**: Consistency is key for building an audience
2. **Use Descriptive Titles**: Help readers find what they're looking for
3. **Include Code Comments**: Make your snippets easy to understand
4. **Add Screenshots**: Visual content enhances tutorials
5. **Link to Resources**: Help readers learn more
6. **Test Your Code**: Verify all code snippets work before posting

## Troubleshooting

**Site not updating after push:**
- Wait 2-5 minutes for GitHub Pages to rebuild
- Check the Actions tab in your repository for build status
- Verify your `_config.yml` is valid YAML

**Code highlighting not working:**
- Ensure you're using triple backticks with language name
- Check that `_config.yml` has the highlighter setting

**Styles not loading:**
- Verify `baseurl` in `_config.yml` is correct
- Check browser console for 404 errors

## Resources

- [Jekyll Documentation](https://jekyllrb.com/docs/)
- [GitHub Pages Documentation](https://docs.github.com/en/pages)
- [Markdown Guide](https://www.markdownguide.org/)
- [Rouge Syntax Highlighter](https://github.com/rouge-ruby/rouge)

## License

This template is free to use and modify for your own projects.

---

Built with ❤️ for the smart home community
```
