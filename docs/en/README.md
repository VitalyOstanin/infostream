# Working with Information Streams and the Digital Environment

Short, practical guidance to keep control over information, security, and productivity.

## Table of Contents
- [Tooling Organization](#tooling-organization)
- [OS Choice and Security Hygiene](#os-choice-and-security-hygiene)
- [Smartphones: Security and Productivity](#smartphones-security-and-productivity)
- [Disk Encryption](#disk-encryption)
- [Personal Knowledge Base](#personal-knowledge-base)
- [Password Manager](#password-manager)
- [Working with Information Streams](#working-with-information-streams)
- [Quick Search Examples](#quick-search-examples)
- [Short Checklist](#short-checklist)
 - [Why Git Matters Beyond Developers](#why-git-matters-beyond-developers)

## Tooling Organization
- Primary device: laptop for maximum control; phone is auxiliary.

## OS Choice and Security Hygiene
- Linux: control and transparency; macOS: stable with limits; Windows: requires extra hardening.
- Always update, back up, and be careful with downloads and links.
- Antivirus: mandatory on Windows; by risk profile on Linux/macOS.

## Smartphones: Security and Productivity
- Keep system and security patches up to date.
- Minimize notifications; allow only critical channels and calls.
- Install apps from trusted sources only.

## Disk Encryption
- Protect laptops with full‑disk encryption: LUKS (Linux), FileVault (macOS), BitLocker (Windows). Store recovery keys offline.

## Personal Knowledge Base
- Keep a local Markdown knowledge base under Git; sync via private remotes and back it up.
- See also: [Productivity Tools Guide](./productivity-tools-guide.md).

## Password Manager
- Prefer KeePassXC (local database), strong master password, and offline backups.

## Working with Information Streams
- Archive inactive chats, limit notifications, protect deep‑work windows. Use profiles/containers in browsers and keyboard shortcuts.

## Quick Search Examples
- Short prefixes in the address bar let you search a service instantly.

How to set up:
1) Chrome → Settings → Search engine → Manage search engines and site search.  
2) Click “Add” and fill in Name, Keyword, and a URL with `%s` placeholder.

Examples (copy-friendly table):

| Name | Keyword | URL |
|---|---|---|
| Private Meet | pmt | https://meet.google.com/new |
| Private Mail | pm | https://mail.google.com/mail/u/0/#inbox |
| Private Calendar | pc | https://calendar.google.com/calendar/u/0/r |
| Yandex Mail | ym | https://mail.yandex.ru/ |
| GitHub Issues | gi | https://github.com/search?q=%s&type=issues |
| Stack Overflow | so | https://stackoverflow.com/search?q=%s |
| Reverso Conjugator | cj | https://conjugator.reverso.net/conjugation-english-verb-%s.html |
| Yandex Translate | tt | https://translate.yandex.ru/?text=%s |
| Yandex Search | yy | https://ya.ru/search/?text=%s |
| npms | ns | https://npms.io/search?q=%s |
| DevDocs | d | https://devdocs.io/#q=%s |
| Merriam‑Webster | mw | https://www.merriam-webster.com/dictionary/%s |
| Cambridge Dictionary | cd | https://dictionary.cambridge.org/dictionary/english-russian/%s |

## Short Checklist
- Local Git‑versioned knowledge base with a development plan.
- Updates and backups enabled ([3‑2‑1 rule](https://www.backblaze.com/blog/the-3-2-1-backup-strategy/)).
- Disk encryption enabled; recovery keys stored offline.
- Local password manager with strong master password and backup.

## Why Git Matters Beyond Developers
Git — https://git-scm.com — is the industry‑standard version control well beyond programming.

- System administrators: version configs, automation scripts, and infra docs; quick rollback to known‑good states.
- DevOps engineers: manage IaC (Terraform — https://www.terraform.io, Ansible — https://www.ansible.com), CI/CD pipelines, and container configs.
- Test engineers: store test cases, automation, datasets, and reports; track coverage in sync with dev work.
- Analysts/data scientists: version SQL, pipelines, models, and reports; ensure experiment reproducibility.
- Tech writers and product managers: docs, requirements, specs (Markdown) with transparent history and collaboration.
- Key benefits: change history, collaboration, backup, branching for experiments, and a standardized team workflow — Git is the lingua franca of IT.

At minimum for everyone: clone/commit/branch, open PR/MR, resolve conflicts; keep personal knowledge and configs in repositories.
