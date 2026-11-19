---
# Leave the homepage title empty to use the site title
title: ''
date: 2025-12-26
type: landing

design:
  # Default section spacing
  spacing: '3rem'

sections:
  - block: resume-biography-3
    content:
      # Choose a user profile to display (a folder name within `content/authors/`)
      username: me
      text: ''
      # Restrained entry to the work archive; no CV download / job-seeking CTA
      button:
        text: Work Overview
        url: /projects/
      headings:
        about: ''
        education: ''
        interests: ''
    design:
      # De-CV: plain first screen, no decorative gradient mesh
      background:
        gradient_mesh:
          enable: false

      # Smaller portrait: the work, not the person, leads
      name:
        size: md # Options: xs, sm, md, lg (default), xl
      avatar:
        size: small # Options: small (150px), medium (200px), large (320px), xl (400px), xxl (500px)
        shape: circle # Options: circle (default), square, rounded
  - block: markdown
    content:
      title: 'Working Agenda'
      subtitle: 'Foundations, public inclusion, and counter-leverage'
      text: |-
        > 一切技术探索与工程实践，皆是对最高指导思想的创造性贯彻落实。
        >
        > All technical exploration and engineering practice is a creative implementation of the highest guiding thought.

        The work is organized along three parallel tracks:

        1. **Foundational Infrastructure（科技底层）** — Build self-controlled foundational technologies and the computing / data infrastructure that other work depends on.
        2. **Public Inclusion（技术济弱）** — Ship low-threshold, general-purpose tools that extend real capability to ordinary and underserved groups.
        3. **Technical & Institutional Counter-Leverage（规则与技术反制）** — Use transparent engineering and empirical analysis to break monopoly and unfair barriers.
    design:
      columns: '1'
  - block: collection
    id: projects
    content:
      title: Selected Work
      text: >-
        Projects and field reports. The standalone empirical report —
        [Older Adults' Digital Survival](/viz2.html) — is a self-contained
        HTML document, visually isolated from this site.
      filters:
        folders:
          - projects
    design:
      view: article-grid
      columns: 2
      fill_image: false
      show_date: false
      show_read_time: false
      show_read_more: false
  - block: collection
    id: papers
    content:
      title: Featured Publications
      filters:
        folders:
          - publications
        featured_only: true
    design:
      view: article-grid
      columns: 2
  - block: collection
    content:
      title: Recent Publications
      text: ''
      filters:
        folders:
          - publications
        exclude_featured: false
    design:
      view: citation
  - block: collection
    id: news
    content:
      title: Recent News
      subtitle: ''
      text: ''
      # Page type to display. E.g. post, talk, publication...
      page_type: blog
      # Choose how many pages you would like to display (0 = all pages)
      count: 10
      # Filter on criteria
      filters:
        author: ''
        category: ''
        tag: ''
        exclude_featured: false
        exclude_future: false
        exclude_past: false
        publication_type: ''
      # Choose how many pages you would like to offset by
      offset: 0
      # Page order: descending (desc) or ascending (asc) date.
      order: desc
    design:
      # Choose a layout view
      view: card
      # Reduce spacing
      spacing:
        padding: [0, 0, 0, 0]
---
