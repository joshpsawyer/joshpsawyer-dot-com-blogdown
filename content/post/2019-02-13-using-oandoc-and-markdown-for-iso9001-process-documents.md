---
title: Using Markdown, Github and Pandoc for Document control
author: Josh P. Sawyer
date: '2019-02-13'
slug: markdown-github-pandoc-for-document-control
categories:
  - Markdown
tags:
  - markdown
  - pandoc
  - latex
image:
  caption: ''
  focal_point: ''
draft: true
---

# My Personal Document Control Nightmare

At this point in my life, I've written a lot of process documents. I've tried Word, Google Docs, DokuWiki - it's possible to write an SOP in any of these systems, and at work we use _all_ of these systems. We also have standalone PDFs produced years ago that nobody has the original file for - presumably, a doc file. It's a hell of a situation:

- Documents describing one procedure live simultaneously in multiple systems without clear understanding of which document supercedes the other.
- Collaborative tools (Google Docs, DokuWiki) make team editing easy but allow for anybody to change an SOP on the whim without a manager approving it. Locking these changes down discourages user buy-in on process design.
- Some tools are just ugly: Google Docs doesn't allow natively for legal-style numbered headings (1, 1.1, 1.1.1, 1.1.2, ...) and dokuwiki has a weird flavor of syntax that is just close enough to Markdown for me to always screw up the code.
- Aesthetically, the documents don't look similar and a useful SOP might not be trusted because of how poorly it's styled. Similarly, if a document is visually clean and easy to read
- Dokuwiki is the only tool with some sort of transparent version control, but none of them allow for staged approvals of changes, allowing a manager to sign off on a new SOP.

The above concerns aren't issues for a lot of companies. If your team is small with low turnover and you aren't performing complex actions, you don't really need this. If you don't care about ISO9001 as a guiding principal (regardless of whether you get certified), then you can live comfortably in this nightmare... but I can't.

If only there were a free tool that would allow me to version everything, compare documents in plain text and see exactly what had changed, approve or reject changes, track issues, and have a consistent aesthetic output.

# Git & Markdown to the answer

This shouldn't surprise you. Many of my desired features are native to software version control. I can create a branch for a new section of a document and create a pull request to have it merged in. It's totally transparent as to who created what and who merged it. I can roll back to earlier versions and see the full history of a document. 

If you've used github, you've probably encountered a markdown file and are at least generally familiar with Markdown syntax. If not, there are a lot of websites arguing the benefit of Markdown, but I'll summarize it thusly:

- Simple, plain text format - open that file on its own, and you can undertand it.

Okay, this is all great, but most of our users do not have or want github accounts and while Markdown allows for excellent version control, it's missing some important things like standalone files which we can disseminate or legal-stye numbering

# Pandoc for Sanity

It's Pandoc which makes all of this possible. With Pandoc, I can take a Markdown file and trivially create HTML, PDF, Word and OpenOffice files. Practically speaking, I don't want anybody touching the finished document so I output to PDF and then stash these PDFs into the wiki. Pandoc also has its own YAML (YAML Ain't Markup Language) front-matter so I can get really specific about the outputs, and I can change the template of how the document is output.

# Examples

# How to sell this idea at work

"But the way we're doing documents is fine!" cry the CEO and COO who write documents so rarely they don't see the problem.

Selling this idea at work is tough, because there's a learning curve to Markdown, and you need a tech-savvy user to manage the pandoc yaml and templates.

If you're just getting started with document control and you have the authority to do it, _implement this immediately_. Once you trivially change and republish a collection of documents, this method will speak for itself. You'll need to show people how to use github, but you should be doing that anyway, right?

If you're entrenched in document hell like I am, one strategy is to let things get really, really bad.

It also might be easier to approach this on a departmental level. HR and Admin should be under document control, but some departments want flexibility to change processes readily. With a department like that, you could allow them to edit google docs to their hearts content and then https://github.com/mangini/gdocs2md use a script to convert the doc into markdown and version it quarterly.