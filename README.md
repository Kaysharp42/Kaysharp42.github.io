# Project Documentation Hub

This repository hosts the GitHub Pages site for my project documentation.

## Structure

```
/_config.yml          # Jekyll + Just the Docs configuration
/index.md             # Portal landing page listing projects
/MapVoter/            # MapVoter plugin documentation (Markdown + legacy HTML guide)
	Config-Guide.html   # Legacy full configuration guide (to be incrementally migrated)
	index.md            # MapVoter landing page
	installation.md
	configuration.md
	commands.md
	wipe-scheduling.md
	discord.md
	changelog.md
```

## MapVoter
- Rust server plugin for automated map voting, wipe scheduling, and Discord integration.
- Purchase: https://codefling.com/plugins/map-voter-and-auto-wipe-script

## Adding New Projects
1. Create a new top-level folder (e.g. `NewProject/`).
2. Add an `index.md` with front matter:
	 ```
	 ---
	 layout: default
	 title: NewProject
	 nav_order: 1
	 ---
	 ```
3. Add additional markdown pages; control order with `nav_order`.
4. Update portal `index.md` table to link the new project.

## Local Preview (Optional)
Create a Gemfile:
```
source 'https://rubygems.org'
gem 'github-pages', group: :jekyll_plugins
```
Then:
```
bundle install
bundle exec jekyll serve
```

## License
Content license: as per each project. Plugin source/licensing stays in its respective repository or marketplace page.

---
Generated structure prepared on October 6, 2025.