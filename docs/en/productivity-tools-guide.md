# Productivity Tools for Working with Information

## Table of Contents

- [Tools Overview Table](#tools-overview-table)
- [Important Data Storage Recommendation](#important-data-storage-recommendation)
- [Short Overviews](#short-overviews)
- [Obsidian — Knowledge Base as a Graph](#obsidian--knowledge-base-as-a-graph)
- [VS Code + Markdown — Knowledge Base as Code](#vs-code--markdown--knowledge-base-as-code)
- [Todoist — Task and Project Management](#todoist--task-and-project-management)
- [General Recommendations for Context Switching](#general-recommendations-for-context-switching)
- [Methodologies for Knowledge and Task Management](#methodologies-for-knowledge-and-task-management)
- [Developing Your Own Methodology](#developing-your-own-methodology)
- [Study Checklist (Printable)](#study-checklist-printable)
- [Obsidian Settings (Settings and Plugins)](#obsidian-settings-settings-and-plugins)
- [VSCode Extensions (Hotkeys and Extensions)](#vscode-extensions-hotkeys-and-extensions)
- [Todoist Features (Functions and Integrations)](#todoist-features-functions-and-integrations)

## Tools Overview Table

| Tool | Official site | Popularity | Knowledge type | Storage | Structuring | Context switching | Primary purpose |
|------|----------------|------------|----------------|---------|-------------|-------------------|-----------------|
| **Obsidian** | [obsidian.md](https://obsidian.md/) | ⭐⭐⭐⭐⭐ | Knowledge graph | Local .md files | Links, tags, graph | Search, backlinks, graph | Long‑term knowledge base |
| **VS Code + Markdown** | [code.visualstudio.com](https://code.visualstudio.com/) | ⭐⭐⭐⭐⭐ | File architecture | Folders + Git versioning | Folders, TODO Tree, bookmarks | Hotkeys, navigation | Technical documentation |
| **Todoist** | [todoist.com](https://todoist.com/) | ⭐⭐⭐⭐⭐ | Task management | Cloud + local cache | Projects, labels, filters | Quick add, notifications | Task management |

Popularity rationale:
- **Obsidian** (⭐⭐⭐⭐⭐): Very popular among professionals building personal knowledge bases; large community and rich plugin ecosystem.
- **VS Code** (⭐⭐⭐⭐⭐): The most popular code editor with a huge developer community and continuous evolution.
- **Todoist** (⭐⭐⭐⭐⭐): A leading task manager used by millions worldwide.

## Important Data Storage Recommendation

Do not use cloud apps as the primary store for your knowledge base.

You can use Todoist, Telegram notes, and other cloud services for quick capture, but always consolidate into your main knowledge base which:

- Lives locally on your computer.
- Is versioned by Git.
- Has backups in private repositories on GitHub — https://github.com or GitLab.com — https://gitlab.com.

Why this matters: cloud content can be deleted or become inaccessible; policies change, accounts are blocked, services shut down. Your knowledge is your capital — keep it under your control.

Recommended workflow:
1. Capture ideas in convenient mobile apps (Todoist, Telegram).
2. Regularly move important notes to the local base (Obsidian, VS Code).
3. Commit changes in Git and push to private remotes.
4. Sync across devices via Syncthing — https://syncthing.net — cloud‑free with versioning.

## Short Overviews

### Obsidian — Knowledge Base as a Graph

Official site: [https://obsidian.md/](https://obsidian.md/)

Storage and structure: Obsidian stores local Markdown files, ensuring platform independence and durability. Its core feature is linking notes with `[[...]]`, forming a knowledge graph.

Context switching: Instant full‑text search, backlinks navigation, and an interactive graph. Quick switch panel among open notes.

Russian resources:
- [Telegram channel @obsidianrus](https://t.me/obsidianrus) — settings, plugins, news.
- [Official Russian help](https://publish.obsidian.md/help-ru/)
- [Habr article series](https://habr.com/ru/articles/710508/) on knowledge management in Obsidian.

---

### VS Code + Markdown — Knowledge Base as Code

Official site: [https://code.visualstudio.com/](https://code.visualstudio.com/)

Storage and structure: Hierarchical folders, Git versioning, and naming conventions. Treat knowledge “as code”: each note is a file, each folder a topic.

Context switching: Powerful hotkeys, multi‑file workflows, integrated terminal. The TODO Tree extension gives quick access to tasks across the project.

Russian resources:
- [28 VS Code extensions for documentation](https://habr.com/ru/articles/698702/)
- [Top 10 VS Code extensions](https://proglib.io/p/10-vscode-extensions)

---

### Todoist — Task and Project Management

Official site: [https://todoist.com/](https://todoist.com/)

Storage and structure: Cloud with local caching and offline mode. Organized using projects, labels, and filters. Natural‑language date parsing.

Context switching: Quick add, time and geolocation reminders, filters for instant context changes. Gamification via the karma system.

Russian resources:
- [Sky.pro overview](https://sky.pro/wiki/lifestyle/obzor-todoist-kak-organizovat-svoi-zadachi/)
- [Ways to organize work in Todoist](https://vc.ru/u/660293-julia-cores/185637-upravlenie-zadachami-sposoby-organizacii-zadach-i-raboty-v-todoist)

## General Recommendations for Context Switching

### Local‑first principle

Data safety: keep the primary knowledge base local — cloud services can disappear or lock you out. Use cloud only for syncing copies.

Git for versioning: keep your main base under Git with backups in private repos (GitHub — https://github.com / GitLab.com — https://gitlab.com). This gives history and backups.

### Sync across devices

Syncthing: use Syncthing — https://syncthing.net to sync laptop, server, tablet, and phone. It provides versioning and works without cloud services.

Practical setup example:
- Todoist — quick notes and small tasks.
- Obsidian — detailed, primary knowledge base.
- Syncthing — sync Obsidian across all devices (laptop, server, tablet, phone) with automatic versioning.
- Result — full access from any device without relying on the internet.

Mobile access: capture in Todoist on the go, then move important items into the main base.

### Strategies for switching contexts

Tool hierarchy:
1. Todoist — fast capture of ideas and tasks.
2. Main knowledge base (Obsidian/VS Code) — structured storage.
3. Git — versioning and long‑term storage.

Emotional posting: jot ideas immediately when they appear. “Here and now”: after an event, course, or insight — create a note.

Quick notes: keep small notes in Todoist or a personal Telegram chat, but regularly move important items into the main base.

---

## Obsidian Settings (Settings and Plugins)

### Communities and resources

Telegram channels:
- [@obsidianrus](https://t.me/obsidianrus) — settings, plugins, tips, news.
- [@Second_brain_ru](https://t.me/second_brain_ru) — methods for building personal knowledge bases.

Learning materials:
- [Official Russian help](https://publish.obsidian.md/help-ru/Начните+здесь)
- [Habr series](https://habr.com/ru/articles/710508/) — knowledge management in Obsidian.
- [From simple to complex structure](https://habr.com/ru/articles/796899/) — practical experience.

### Key features

Knowledge graph: visualize links between notes via an interactive graph to surface non‑obvious connections.

Backlinks: automatically track reverse links — see all notes referring to the current one.

Local storage: all data lives as .md files on your computer. Settings are in `.obsidian/`.

Plugins: a large community builds extensions for everything from calendars to external integrations.

### Recommended plugins

- [Templater](https://github.com/SilentVoid13/Templater) — templates for fast note creation.
- [Calendar](https://github.com/liamcain/obsidian-calendar-plugin) — calendar view of notes.
- [Tag Wrangler](https://github.com/pjeby/tag-wrangler) — convenient tag management.
- [Advanced Tables](https://github.com/tgrosinger/advanced-tables-obsidian) — better Markdown tables.

---

## VSCode Extensions (Hotkeys and Extensions)

### Navigation

Core hotkeys:
- `Ctrl+B` — Toggle primary side bar visibility.
- `Ctrl+Shift+P` — Go to parent fold.
- `Alt+Shift+-` — Go back in navigation history.
- `Alt+Shift+=` — Go forward in navigation history.
- `Shift+F10` — Debug: Run to cursor.
- `Alt+Shift+F12` — Find all references.
- `Ctrl+J` — Toggle bottom panel.
- `Ctrl+I` — Toggle maximized panel.
- `Ctrl+L` — Clear terminal (when focus is in terminal).

Code editing:
- `Ctrl+Shift+[` — Fold code block.
- `Ctrl+Shift+]` — Unfold code block.
- `Alt+↑` — Move line up.
- `Alt+↓` — Move line down.

Panels:
- `Alt+Shift+L` — Focus Source Control view.
- `Alt+Shift+D` — Show Run and Debug.
- `Alt+Shift+G` — Show GitHub view.
- `Alt+Shift+E` — Show Explorer.
- `Alt+Shift+F` — Show Search.
- `Alt+Shift+O` — Focus Outline view.

### Recommended extensions for Markdown

Core:
- Markdown All in One — hotkeys, TOC, formulas.
- GitLens — advanced Git tooling.
- Bookmarks — in‑code bookmarks.

Project work:
- GitHub Pull Requests — work with PRs.
- ESLint — code linting.
- Search node_modules — search dependencies.

Documentation:
- SQL Formatter — SQL formatting.
- Todo Tree — project‑wide TODO tracking.
- YAML — YAML support.

### Commands

Useful commands:
- Open file on remote — open a file on a remote host.
- Copy line down — duplicate the current line.

---

## Todoist Features (Functions and Integrations)

### Core capabilities

GTD methodology: Todoist is designed for [Getting Things Done](https://gettingthingsdone.com/) — clear task organization frees up mental resources.

Smart input: natural‑language date parsing such as “tomorrow 3pm”, “every Monday”.

Gamification: karma system rewards completed tasks; daily goals and achievements sustain usage.

### Structuring

Projects: group tasks by areas of life or work.

Labels: context organization — @calls, @computer, @home.

Filters: quick switching between views — “today”, “overdue”, “priority”.

### Integrations and sync

Multiplatform: iOS, Android, Windows, macOS, web.

Offline mode: work without internet and sync when connected.

Notifications: by time, geolocation, across devices.

Files: attachments via Google Drive, Dropbox.

### Using with other tools

Idea capture: quickly jot ideas, then move them into the main knowledge base.

Context control: filters for instant switching between work, personal, and projects.

Emotional posting: capture fleeting thoughts to later structure in Obsidian or VS Code.

---

## Methodologies for Knowledge and Task Management

### Getting Things Done (GTD)
- Official site: [https://gettingthingsdone.com/](https://gettingthingsdone.com/)
- Russian resources: [GTD on Habr](https://habr.com/ru/articles/50821/)
- Description: a productivity system that frees your head from remembering tasks.

### Kanban
- Official site: [https://kanbanflow.com/](https://kanbanflow.com/)
- Russian resources: [Kanban on Habr](https://habr.com/ru/articles/488304/)
- Description: visual task management with boards and cards.

### Zettelkasten (Card system)
- Official site: [https://zettelkasten.de/](https://zettelkasten.de/)
- Russian resources: [Zettelkasten on Habr](https://habr.com/ru/articles/508672/)
- Description: linked‑note method for accumulating and developing knowledge.

## Developing Your Own Methodology

### Stating your needs

Core idea: don’t fit yourself to tools; articulate your needs and find tools for them.

Iteration loop:

1. Identify problems
   - What exactly fails in your current process?
   - Where is time or information lost?
   - Which tasks cause the most resistance?

2. Formulate the need
   - “I need to quickly find related project notes.”
   - “I need to track progress on long‑term goals.”
   - “I need to sync tasks across devices without the cloud.”

3. Search for solutions
   - First in existing tools (settings, plugins).
   - Then in alternative approaches.
   - As a last resort — build your own.

4. Iterate and adapt
   - Test for 2–4 weeks.
   - Analyze effectiveness.
   - Adjust or change approach.

### Examples of methodology evolution

Need: “Fast switching between projects”
- Solution 1: VS Code hotkeys to open project folders.
- Solution 2: Obsidian templates per project type.
- Solution 3: Todoist labels to filter contexts.

Need: “Tracking learning and progress”
- Solution 1: a dedicated Todoist “Learning” project with subtasks.
- Solution 2: a learning log in Obsidian with links between topics.
- Solution 3: combo: goals in Todoist, detailed notes in Obsidian.

### Criteria of an effective methodology

Ease of adoption: minimal effort to get started.  
Natural fit: matches your thinking style.  
Scalability: keeps working as information grows.  
Reliability: minimal dependence on external services and tech.

### Document your own experience

Keep an experiment log:
- Start date.
- Problem statement.
- Chosen solution.
- Result after 2–4 weeks.
- Decision: continue/modify/abandon.

This becomes your personal knowledge base of what works for you.

---

## Study Checklist (Printable)

### Core tools (must‑learn)

□ Obsidian — Knowledge base
- [ ] Install and configure (30 min)
- [ ] [Official Russian help](https://publish.obsidian.md/help-ru/) (2 h)
- [ ] [Habr series](https://habr.com/ru/articles/710508/) (1 h)
- [ ] Practice: first notes (1 h)
- Total: 4.5 h

□ VS Code + Markdown — Technical docs
- [ ] Install VS Code (15 min)
- [ ] Install Markdown extensions (15 min)
- [ ] [28 VS Code extensions for docs](https://habr.com/ru/articles/698702/) (30 min)
- [ ] Configure hotkeys (30 min)
- [ ] Practice: create a docs project (1 h)
- Total: 2.5 h

□ Todoist — Task management
- [ ] Sign up and install (15 min)
- [ ] [Sky.pro overview](https://sky.pro/wiki/lifestyle/obzor-todoist-kak-organizovat-svoi-zadachi/) (30 min)
- [ ] [Ways to organize in Todoist](https://vc.ru/u/660293-julia-cores/185637-upravlenie-zadachami-sposoby-organizacii-zadach-i-raboty-v-todoist) (30 min)
- [ ] Configure projects and labels (30 min)
- [ ] Practice: build your task system (45 min)
- Total: 2.5 h

### Methodologies (pick to study)

□ Getting Things Done (GTD)
- [ ] [GTD on Habr](https://habr.com/ru/articles/50821/) (45 min)
- [ ] Practice: apply GTD principles (1 h)
- Total: 1.75 h

□ Kanban
- [ ] [Kanban on Habr](https://habr.com/ru/articles/488304/) (30 min)
- [ ] Practice: build a kanban board (30 min)
- Total: 1 h

□ Zettelkasten
- [ ] [Zettelkasten on Habr](https://habr.com/ru/articles/508672/) (45 min)
- [ ] Practice: create linked notes (1 h)
- Total: 1.75 h

### Extra setup (as needed)

□ Syncthing — Cloud‑free sync
- [ ] [Syncthing.net](https://syncthing.net/) — install (30 min)
- [ ] Configure device sync (45 min)
- Total: 1.25 h

□ Git + private repos
- [ ] Configure Git for docs (30 min)
- [ ] Create a private repo (15 min)
- [ ] First commit of the knowledge base (15 min)
- Total: 1 h

---

Total time for full mastery: 15–20 hours.  
Minimal set (Obsidian + Todoist): 7 hours.

---
