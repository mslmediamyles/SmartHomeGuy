# IMPORTANT - FOR YOUR SPECIFIC REPOSITORY

## Your Repository Setup
- Repository name: `SmartHomeGuy`
- Your URL: `https://mslmediamyles.github.io/SmartHomeGuy/`
- Type: Project site (not user site)

## What This Means
Since your repository is named `SmartHomeGuy` (not `mslmediamyles.github.io`), it's a "project site" and needs special configuration.

---

## 🔧 UPDATED FILES FOR YOUR SETUP

I've updated the `_config.yml` file specifically for your repository. The key change:

```yaml
baseurl: "/SmartHomeGuy"
url: "https://mslmediamyles.github.io"
```

This tells Jekyll where to find your CSS, JS, and other files.

---

## 📋 UPLOAD INSTRUCTIONS

### Step 1: Delete Old Files
1. Go to https://github.com/mslmediamyles/SmartHomeGuy
2. Delete ALL existing files (select all → Delete)
3. Commit the deletion

### Step 2: Upload New Files
1. Download and unzip the `smarthomeguy-site-FINAL.zip`
2. In your repository, click "Add file" → "Upload files"
3. Drag **ALL** files from the unzipped folder
4. Make sure these folders are uploaded:
   - `_layouts/`
   - `_posts/`
   - `css/`
   - `js/`
5. Commit changes

### Step 3: Verify GitHub Pages Settings
1. Go to Settings → Pages
2. Source should be: `main` branch, `/ (root)` folder
3. Click Save (if not already saved)

### Step 4: Wait and Test
1. Wait **3-5 minutes** for GitHub to rebuild
2. Visit: https://mslmediamyles.github.io/SmartHomeGuy/
3. Hard refresh: `Ctrl + Shift + R` (Windows) or `Cmd + Shift + R` (Mac)

---

## ✅ What You Should See

After uploading and waiting, your site should display:

- ✅ **Dark background** (not white)
- ✅ **Neon green accents** (#00e676)
- ✅ **"SmartHomeGuy" logo** in navigation
- ✅ **Three blog post cards** on the homepage
- ✅ **Monospace font** for that tech aesthetic
- ✅ **Working navigation** (Posts, Archive, About)

---

## 🐛 If It Still Doesn't Work

### Check Build Status:
1. Go to your repository → **Actions** tab
2. Look for the latest "pages build and deployment"
3. If there's a **red X**, click it to see the error
4. If there's a **green checkmark**, the build succeeded

### Check Browser Console:
1. On your site, press `F12` to open developer tools
2. Go to the **Console** tab
3. Look for 404 errors (file not found)
4. If you see errors loading `style.css`, the baseurl might be wrong

### Common Issues:

**Still seeing plain text:**
- Wait 5 full minutes after upload
- Hard refresh with `Ctrl+Shift+R`
- Check that `_config.yml` has `baseurl: "/SmartHomeGuy"`

**404 errors:**
- Verify GitHub Pages is enabled in Settings
- Check you uploaded to the `main` branch
- Make sure all folders uploaded correctly

**Build failed:**
- Go to Actions tab to see the error
- Usually means a YAML syntax error
- Re-upload all files from the ZIP

---

## 📝 Adding New Posts

Once it's working, add posts by creating files in `_posts/`:

**Filename:** `2026-01-15-my-post-title.md`

**Content:**
```markdown
---
layout: post
title: "My Post Title"
date: 2026-01-15
category: Smart Home
description: "Brief description"
---

Your content here...

## Code Example

```python
def smart_home():
    print("Automating everything!")
```

More content...
```

Just create the file in `_posts/` folder, commit, and it will appear automatically!

---

## 🎨 Customization

### Change Site Name:
Edit `_config.yml`:
```yaml
title: Your New Name
description: Your new description
```

### Change Colors:
Edit `css/style.css`:
```css
:root {
  --color-accent: #00e676;  /* Change this to any color */
}
```

### Update About Page:
Edit `about.html` and change the content to your information.

---

## 🆘 Still Need Help?

If it's still not working after following these steps:

1. Screenshot what you see when you visit the site
2. Screenshot the Actions tab showing any errors
3. Check browser console (F12) and screenshot any red errors
4. Reply with these screenshots

The most common issue is just needing to wait 5 minutes and hard refresh!

---

Good luck! Your site should look awesome once it loads. 🚀
