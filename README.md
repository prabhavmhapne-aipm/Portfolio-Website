# Prabhav Mhapne - AI Product Manager Portfolio

Professional portfolio website showcasing product management work and technical product leadership.

## 🚀 Live Website
https://www.prabhavmhapne.com/

## 📁 Structure
- **Root** - Production portfolio website (single-page HTML app)
  - `index.html` - Main portfolio page
  - `robots.txt` - SEO configuration
  - `sitemap.xml` - Search engine sitemap
  - `career-logos/` - Company logos (6 files)
  - `skill-icons/` - Technical skill icons (10 files)
  - `product-images/` - Product screenshots (4 files)
- `Portfolio/` - Detailed case studies and project documentation

## 🛠️ Tech Stack
- **Frontend:** HTML5, CSS3, JavaScript (vanilla)
- **Hosting:** Netlify (CDN, continuous deployment from GitHub)
- **Backend:** Supabase (Postgres database, REST API)
- **Design:** Responsive design, mobile-first approach
- **SEO:** Open Graph meta tags, Twitter Cards, structured data
- **Performance:** Progressive loading with asset preloading, optimized images
- **Accessibility:** Semantic HTML, ARIA labels, keyboard navigation

## 🏗️ Architecture

### Overview

Single-file web application — all HTML, CSS, and JavaScript are co-located in `index.html` to eliminate network round-trips and enable zero-build-step deployment. The site is hosted on Netlify with continuous deployment from this GitHub repository, and uses Supabase as a managed backend for form submissions, analytics, and GDPR audit logging.

```
  GitHub Repository
  (source of truth)
         │
         │ git push → triggers deploy
         ▼
  ┌─────────────────────┐
  │   Netlify           │
  │   CDN + CI/CD       │
  │   (global edge)     │
  └──────────┬──────────┘
             │ serves files
             ▼
┌─────────────────────────────────────────────────────────────┐
│                          Browser                            │
│                                                             │
│  ┌──────────────┐   ┌─────────────────┐    ┌──────────────┐ │
│  │ Asset        │   │ Single-Page App │    │ i18n Engine  │ │
│  │ Preloader    │─ ▶  index.html      │──▶│ EN / DE      │ │
│  │ (progress %) │   │  (4,100 lines)  │    │ localStorage │ │
│  └──────────────┘   └────────┬────────┘    └──────────────┘ │
│                              │                              │
│          ┌───────────────────┼───────────────────┐          │
│          ▼                   ▼                   ▼          │
│   ┌─────────────┐   ┌──────────────┐   ┌──────────────┐     │
│   │ Scroll      │   │ Product      │   │ GDPR Consent │     │
│   │ Reveal      │   │ Modals       │   │ Manager      │     │
│   │ (Intersect. │   │ (dynamic     │   │              │     │
│   │  Observer)  │   │  rendering)  │   └──────┬───────┘     │
│   └─────────────┘   └──────────────┘          │             │
│                                               ▼             │
│                                      ┌──────────────┐       │
│                                      │  Analytics   │       │
│                                      │  (consent-   │       │
│                                      │   gated)     │       │
│                                      └──────┬───────┘       │
└─────────────────────────────────────────────┼───────────────┘
                                              │ HTTPS API
                                              ▼
                            ┌─────────────────────────────┐
                            │       Supabase (BaaS)       │
                            │                             │
                            │  ┌───────────────────────┐  │
                            │  │ contact_submissions   │  │
                            │  │ (contact form data)   │  │
                            │  └───────────────────────┘  │
                            │  ┌───────────────────────┐  │
                            │  │ analytics_events      │  │
                            │  │ (page interactions)   │  │
                            │  └───────────────────────┘  │
                            │  ┌───────────────────────┐  │
                            │  │ consent_decisions     │  │
                            │  │ (GDPR legal record)   │  │
                            │  └───────────────────────┘  │
                            └─────────────────────────────┘

  Static Assets
  ├── career-logos/   (6 SVG/PNG company logos)
  ├── skill-icons/    (10 SVG tool icons)
  └── product-images/ (4 product screenshots)
```

### Key Design Decisions

| Decision | Approach | Rationale |
|---|---|---|
| **No framework** | Vanilla HTML/CSS/JS | Zero dependencies, no build step, instant deploy, no supply-chain risk |
| **Single file** | All code in `index.html` | Eliminates extra HTTP requests; fully portable and self-contained |
| **Bilingual (EN/DE)** | `data-i18n` attributes + `localStorage` | Targets German job market without duplicate pages; language state persists across reloads |
| **Progressive loading** | Asset preloader with % progress ring | Prevents layout shift on slow connections; ensures images and fonts are ready before content reveals |
| **Scroll animations** | Intersection Observer API | GPU-accelerated; avoids scroll event polling which degrades performance on low-end devices |
| **GDPR consent** | Custom consent manager | Legally required for EU visitors; analytics are blocked until explicit opt-in |
| **SEO** | OG tags, hreflang, sitemap.xml, robots.txt | Rich social previews; signals bilingual content to search engines for EN and DE indexing |
| **Hosting** | Netlify (CDN + CI/CD) | Push to GitHub automatically triggers a deploy; global CDN ensures fast load times worldwide with no server management |
| **Backend** | Supabase (BaaS) via JS SDK | Provides a managed Postgres database and REST API with no server to maintain; anon key is safe to expose client-side due to row-level security |
| **GDPR backend logging** | Consent decisions written to Supabase | Creates a verifiable audit trail of user consent as required under Art. 28 DSGVO; Supabase operates under a data processing agreement |

### Key Modules

- **Asset Preloader** — Tracks image and font loading in parallel, renders an animated SVG progress ring, and reveals the page only when all assets are ready
- **i18n Engine** — Scans all DOM elements for `data-i18n` attributes on load and language toggle, swaps text content, and persists the user's preference to `localStorage`
- **Scroll Reveal** — An Intersection Observer watches each `<section>`; when it enters the viewport, a `.visible` class triggers a CSS fade-in transition
- **Product Modals** — Case study content is stored as a JavaScript data object and dynamically injected into a single reusable modal template, avoiding repeated HTML markup
- **Consent Manager** — GDPR-compliant banner with granular category toggles; all analytics calls are gated behind a consent check before firing
- **Analytics** — Custom event tracking for key interactions (CTA clicks, modal opens, language switches); fires only after consent is granted
- **Supabase Integration** — Backend-as-a-Service layer handling three data flows: contact form submissions → `contact_submissions` table, page interaction events → `analytics_events` table, and consent decisions → `consent_decisions` table for GDPR audit trail

## 📊 Featured Projects

### 1. MyPorsche GenAI CarPlay Assistant (Porsche Digital)
- Shipped GenAI feature beta to 500+ users
- Built RAG pipeline with 85% relevance accuracy
- Reduced support queries by 30% through AI-powered assistance

### 2. Innovation Funnel Process (Porsche Digital)
- Designed and implemented end-to-end innovation management system
- Processed 60+ ideas, validated top 3 for production
- Achieved 40% faster feature iteration cycles

### 3. Local RAG Chatbot (AI PM Certification)
- Built production-grade RAG system from scratch
- Established comprehensive eval framework (RAGAS metrics)
- Demonstrated technical product decision-making

## 👤 About
Product Manager with 4+ years of engineering experience transitioning into product leadership. Unique blend of:
- **Technical Depth:** Can build production RAG systems and evaluate AI/ML models
- **Product Mindset:** Shipped 0→1 GenAI features, designed discovery processes
- **Cross-Functional Leadership:** Led teams across engineering, design, and business stakeholders

**Certifications:**
- SAFe® 6 Product Owner/Product Manager
- AI Product Management Certification with Capstone Project (Maven)
- Product Management Bootcamp (Digitale Leute School)

**Location:** Düsseldorf, Germany
**Languages:** German (C1), English (Fluent)

## 📫 Contact
- **LinkedIn:** https://www.linkedin.com/in/prabhavmhapne/
- **Email:** prabhavmhapne@gmx.de
- **GitHub:** https://github.com/prabhavmhapne-aipm/

---

**Last Updated:** February 2026
