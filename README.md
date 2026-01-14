# SmartHomeGuy - GitHub Pages Blog (CSS FIXED)

## 🔥 ISSUE FIXED

If your site loaded but had **no styling** (looked like plain text with blue links), this version fixes it!

**What was wrong:** CSS path issues with GitHub Pages
**What I fixed:** Moved CSS/JS to root folders with simpler paths

---

## 🚀 Setup Instructions

### Step 1: Delete Old Files (If you uploaded before)
1. Go to your repository on GitHub
2. Select all files → Delete
3. Commit the deletion

### Step 2: Upload This Fixed Version
1. Download and unzip this folder
2. In your repository, click "Add file" → "Upload files"
3. Drag **ALL** files from the unzipped folder
4. **IMPORTANT:** Make sure you see these folders:
   - `_layouts/`
   - `_posts/`
   - `css/` (NOT assets/css!)
   - `js/` (NOT assets/js!)
5. Commit changes

### Step 3: Enable GitHub Pages
1. Go to Settings → Pages
2. Source: `main` branch, `/ (root)` folder
3. Click Save

### Step 4: Wait & Visit
- Wait **2-5 minutes**
- Visit `https://yourusername.github.io`
- You should now see:
  - ✅ Dark background
  - ✅ Green neon accents
  - ✅ Styled post cards
  - ✅ Proper fonts

---

## ✅ Verify It's Working

Your site should look like this:
- **Dark background** (#0a0e14)
- **Neon green** accents (#00e676)
- **Monospace font** for body text
- **Styled cards** for blog posts
- **Navigation bar** with green underline hover effect

If you still see plain text with blue links, check:
1. Did you upload the `css/` folder?
2. Is it `css/style.css` not `assets/css/style.css`?
3. Wait 5 full minutes and hard refresh (Ctrl+Shift+R)

---

## 📁 Correct File Structure

```
yourusername.github.io/
├── _config.yml
├── _layouts/
│   ├── default.html
│   └── post.html
├── _posts/
│   └── (your markdown posts here)
├── css/                 ← CSS here, NOT in assets/css!
│   ├── style.css
│   └── syntax.css
├── js/                  ← JS here, NOT in assets/js!
│   └── main.js
├── index.html
├── archive.html
└── about.html
```

---

## ✍️ Adding New Posts

Create file: `_posts/2026-01-15-my-post.md`

```markdown
---
layout: post
title: "My Post Title"
date: 2026-01-15
category: Smart Home
---

Your content here...

```python
# Code with syntax highlighting
def hello():
    print("Hello!")
```
```

---

## 🎨 Customization

### Change Colors
Edit `css/style.css`:
```css
:root {
  --color-bg: #0a0e14;      /* Background */
  --color-accent: #00e676;  /* Accent (green) */
}
```

### Change Title
Edit `_config.yml`:
```yaml
title: Your Site Name
description: Your description
```

---

## 🆘 Still Having Issues?

**Site shows plain text (no colors/styling):**
- Make sure `css/` folder is at root level
- Check you uploaded `css/style.css`
- Wait 5 minutes and hard refresh
- Check browser console (F12) for 404 errors

**404 Error:**
- Repository name must be `yourusername.github.io`
- Must be exactly your GitHub username
- Repository must be Public

**Build Failed:**
- Go to repository → Actions tab
- Click the failed build to see error
- Usually means YAML syntax error in `_config.yml`

**Posts not showing:**
- Files must be in `_posts/` folder
- Filename must be `YYYY-MM-DD-title.md`
- Date must be today or earlier
- Must have proper frontmatter (see example above)

---

## 📱 Features

- Dark theme with neon green accents
- Syntax highlighting for code
- Copy buttons for code blocks
- Responsive design
- Fast static site
- Easy post management

---

## 🎯 Quick Test

After setup, visit your site. You should see:

✅ **Dark background** (not white)  
✅ **Green text and accents** (not just blue links)  
✅ **Monospace font** (looks like code editor)  
✅ **Styled blog post cards** (not just plain lists)  
✅ **Navigation bar** at top with hover effects  

If you see all of these, **it's working!** 🎉

---

Built for the smart home community 🏠⚡
