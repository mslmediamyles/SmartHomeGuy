# Quick Setup Guide for GitHub Pages

## Step 1: Create GitHub Repository

1. Go to GitHub and create a new repository
2. Name it: `yourusername.github.io` (replace `yourusername` with your actual GitHub username)
3. Make it public
4. Don't initialize with README (you already have one)

## Step 2: Upload Files

Option A - Using GitHub Web Interface:
1. Go to your new repository
2. Click "uploading an existing file"
3. Drag and drop all the files from the `smarthomeguy-site` folder
4. Commit the changes

Option B - Using Git Command Line:
```bash
cd smarthomeguy-site
git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin https://github.com/yourusername/yourusername.github.io.git
git push -u origin main
```

## Step 3: Enable GitHub Pages

1. Go to your repository Settings
2. Click on "Pages" in the left sidebar
3. Under "Source", select branch: `main` and folder: `/ (root)`
4. Click Save

## Step 4: Customize Your Site

1. Edit `_config.yml`:
   - Change `url` to your GitHub Pages URL
   - Update title and description

2. Update links in files:
   - `_layouts/default.html` - Update GitHub link in footer
   - `about.html` - Update your contact information

3. Customize colors (optional):
   - Edit `assets/css/style.css` to change color variables

## Step 5: Add Your First Post

1. Create a new file in `_posts/` folder
2. Name it: `2026-01-15-my-first-post.md` (use today's date)
3. Use this template:

```markdown
---
layout: post
title: "My First Post"
date: 2026-01-15
category: Smart Home
description: "Getting started with my smart home blog"
---

Your content here...

## Code Example

```python
print("Hello, world!")
```

More content...
```

## Your Site is Live!

Visit: `https://yourusername.github.io`

It may take 2-5 minutes for changes to appear after you push updates.

## Adding New Posts

Just create new markdown files in `_posts/` with the format:
- Filename: `YYYY-MM-DD-title.md`
- Include front matter at the top
- Write in Markdown
- Push to GitHub

That's it! Your posts will automatically appear on your site.

## Need Help?

Check the full README.md for detailed documentation.
