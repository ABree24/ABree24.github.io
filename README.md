# Brender Akinyi Portfolio Website.

# 🧱 Portfolio Overview

This repository contains the source code and structure for my **cybersecurity portfolio website**, built using **Jekyll** and hosted on **GitHub Pages**.

The goal of this portfolio is to document my hands-on work in **detection engineering, SIEM operations, and digital forensics**, while also experimenting with automation and cloud-based SOC tools.

---

## 🛠️ Tech Stack

| Category | Tools / Technologies |
|-----------|----------------------|
| **Framework** | [Jekyll](https://jekyllrb.com/) |
| **Hosting** | [GitHub Pages](https://pages.github.com/) |
| **Version Control** | Git & GitHub |
| **Languages** | HTML · CSS · Markdown |
| **Layout Theme** | Minimal Mistakes Jekyll Theme |
| **Markdown Renderer** | kramdown |

---

## 🧩 Portfolio Structure

```plaintext
/
├── index.md              → Homepage (About + Projects Overview)
├── projects.md           → Case studies and detailed project documentation
├── resume.md             → Experience, education, and skills
├── _config.yml           → Jekyll site configuration
├── assets/               → Images, icons, and media used across pages
└── _layouts/             → Page templates and custom layout tweaks
```
---
🚀 Local Development
To build and preview the site locally:

# 1. Clone the repository
git clone https://github.com/abree24/abree24.github.io

# 2. Move into the project folder
cd abree24.github.io

# 3. Install Jekyll and Bundler (if not installed)
gem install jekyll bundler

# 4. Serve the site locally
bundle exec jekyll serve

# 5. Open your browser
http://localhost:4000
---
🧠 Key Features
Responsive design using the Minimal Mistakes theme

Organized Markdown-based pages for easy content updates

Syntax highlighting for code blocks and PowerShell/KQL snippets

Clean directory structure and consistent visual layout

Integrated assets folder for screenshots and lab visuals
---
🧪 Current Focus
Adding new cybersecurity case studies (SOC detections, forensics investigations).

Improving visual presentation of images and dashboards.

Exploring automation and cloud integration for incident detection workflows.
---
📜 License
This project is licensed under the MIT License.


---

## Troubleshooting

If you have a question about using Jekyll, start a discussion on the [Jekyll Forum](https://talk.jekyllrb.com/) or [StackOverflow](https://stackoverflow.com/questions/tagged/jekyll). Other resources:

- [Ruby 101](https://jekyllrb.com/docs/ruby-101/)
- [Setting up a Jekyll site with GitHub Pages](https://jekyllrb.com/docs/github-pages/)
- [Configuring GitHub Metadata](https://github.com/jekyll/github-metadata/blob/master/docs/configuration.md#configuration) to work properly when developing locally and avoid `No GitHub API authentication could be found. Some fields may be missing or have incorrect data.` warnings.
