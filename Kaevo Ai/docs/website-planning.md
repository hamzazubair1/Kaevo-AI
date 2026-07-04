# Kaevo AI — Website Planning Document

> Architecture, pages, SEO strategy, and technical specifications for the Kaevo AI website.

---

## Website Goals

1. **Establish credibility** — Present Kaevo AI as a professional, trustworthy AI agency
2. **Generate leads** — Convert visitors into consultation bookings
3. **Showcase services** — Clearly communicate what we offer and who it's for
4. **Demonstrate expertise** — Portfolio, case studies, and thought leadership
5. **Enable self-service** — AI chatbot for instant engagement

---

## Tech Stack

| Layer | Technology | Reason |
|-------|-----------|--------|
| **Framework** | Next.js 14+ (App Router) | SSR, SEO, performance, React ecosystem |
| **Styling** | Tailwind CSS | Rapid development, consistent design |
| **Animation** | Framer Motion | Smooth, professional animations |
| **CMS** | Notion API or MDX | Blog and case study content |
| **Forms** | React Hook Form + Zod | Validation, type safety |
| **Analytics** | Vercel Analytics + GA4 | Performance and visitor tracking |
| **Hosting** | Vercel | Next.js native, global CDN, fast deploys |
| **AI Chat** | Custom chatbot widget | Lead capture, instant support |

---

## Site Architecture

```
kaevo.ai
├── / (Home)
├── /services
│   ├── /ai-chatbots
│   ├── /workflow-automation
│   ├── /ai-voice-agents
│   └── /custom-ai-solutions
├── /portfolio
│   └── /[case-study-slug]
├── /about
├── /blog
│   └── /[post-slug]
├── /contact
├── /pricing
├── /book-call
└── /legal
    ├── /privacy-policy
    └── /terms-of-service
```

---

## Page Specifications

### Home Page (`/`)

| Section | Content |
|---------|---------|
| Hero | Bold headline, subheadline, CTA button, animated visual |
| Social Proof | Client logos, testimonials, metrics |
| Services Overview | Card grid of core services |
| How It Works | 3-step process (Discover → Build → Launch) |
| Case Studies | Featured portfolio items |
| AI Demo | Interactive chatbot demonstration |
| CTA | Book a free consultation |
| Footer | Navigation, social links, legal |

### Services Page (`/services`)

| Section | Content |
|---------|---------|
| Hero | Services overview with visual |
| Service Cards | Each service with description, features, pricing |
| Comparison Table | Service tiers side-by-side |
| FAQ | Common questions per service |
| CTA | Get started with a service |

### Portfolio Page (`/portfolio`)

| Section | Content |
|---------|---------|
| Hero | Portfolio showcase |
| Case Study Grid | Filterable grid of completed projects |
| Case Study Detail | Problem, solution, results, testimonial, tech used |

### About Page (`/about`)

| Section | Content |
|---------|---------|
| Story | Company origin and mission |
| Team | Team member profiles |
| Values | Core values with visuals |
| Tech Stack | Technologies we use |

### Contact Page (`/contact`)

| Section | Content |
|---------|---------|
| Contact Form | Name, email, company, message, service interest |
| Calendar Embed | Calendly or Cal.com integration |
| Contact Info | Email, social media links |
| Map | Office location (if applicable) |

---

## SEO Strategy

### On-Page SEO
- Unique meta titles and descriptions for every page
- Structured heading hierarchy (H1 → H2 → H3)
- Schema.org markup (Organization, Service, FAQ)
- Alt text on all images
- Internal linking strategy
- Mobile-first responsive design

### Content Strategy
- Weekly blog posts on AI automation topics
- Case studies for every completed project
- Service-specific landing pages for SEO
- Thought leadership content

### Technical SEO
- Server-side rendering (Next.js SSR/SSG)
- Core Web Vitals optimization (LCP, CLS, INP)
- Sitemap.xml and robots.txt
- Canonical URLs
- Open Graph and Twitter Card meta tags

### Target Keywords
| Category | Keywords |
|----------|----------|
| Primary | ai automation agency, ai chatbot development, workflow automation |
| Secondary | ai voice agent, custom ai solutions, business ai consulting |
| Long-tail | hire ai automation company, ai chatbot for small business |

---

## Design Requirements

### Visual Identity
- Dark-mode primary design with light mode option
- Brand gradient accents (Blue → Violet)
- Clean, modern typography (Inter)
- Glassmorphism and subtle depth effects
- Smooth page transitions and micro-animations

### Performance Targets
| Metric | Target |
|--------|--------|
| Lighthouse Performance | > 95 |
| Lighthouse Accessibility | > 95 |
| Lighthouse SEO | > 95 |
| First Contentful Paint | < 1.2s |
| Largest Contentful Paint | < 2.5s |
| Cumulative Layout Shift | < 0.1 |

---

## Development Phases

| Phase | Scope | Timeline |
|-------|-------|----------|
| Phase 1 | Home, Services, Contact, About | Week 1–2 |
| Phase 2 | Portfolio, Blog, Pricing | Week 3–4 |
| Phase 3 | AI Chatbot, Analytics, SEO | Week 5–6 |
| Phase 4 | Optimization, A/B Testing | Ongoing |

---

> **File Location:** Website source code → `website/`
