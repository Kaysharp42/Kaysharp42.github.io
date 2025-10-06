# Quick Deployment Guide

## Push Changes to GitHub

```powershell
# Check what changed
git status

# Stage all changes
git add .

# Commit with message
git commit -m "Fix site structure and navigation for multi-project hub"

# Push to GitHub
git push origin main
```

## GitHub Pages will automatically:
1. Detect the changes
2. Build the Jekyll site
3. Deploy to https://kaysharp42.github.io/

**Build time:** 2-3 minutes

## Test These URLs After Deploy:

### Main Portal
- https://kaysharp42.github.io/

### MapVoter Documentation
- https://kaysharp42.github.io/MapVoter/
- https://kaysharp42.github.io/MapVoter/installation
- https://kaysharp42.github.io/MapVoter/configuration
- https://kaysharp42.github.io/MapVoter/commands
- https://kaysharp42.github.io/MapVoter/wipe-scheduling
- https://kaysharp42.github.io/MapVoter/discord
- https://kaysharp42.github.io/MapVoter/changelog

### Legacy Guide (preserved)
- https://kaysharp42.github.io/MapVoter/Config-Guide.html

## Local Testing (Optional)

If you want to test locally before pushing:

```powershell
# Install dependencies (first time only)
bundle install

# Start local server
bundle exec jekyll serve

# Open browser to:
# http://127.0.0.1:4000
```

Press `Ctrl+C` to stop the local server.

## Troubleshooting

### If navigation doesn't appear:
- Check that `MapVoter/index.md` has `has_children: true`
- Check that child pages have `parent: MapVoter`

### If search doesn't work:
- Wait for full GitHub Pages build (3-5 minutes)
- Clear browser cache

### If styles look wrong:
- Verify `_config.yml` has `remote_theme: just-the-docs/just-the-docs`
- Check that front matter uses `layout: default`

## Changes Made:
✅ Deleted orphaned `/docs/` folder  
✅ Moved all pages to `/MapVoter/`  
✅ Fixed navigation hierarchy (parent/child)  
✅ Updated layouts and styling  
✅ Added Gemfile for local development  
✅ Added .gitignore for build artifacts  
✅ Created multi-project portal structure  

**You're ready to deploy!**
