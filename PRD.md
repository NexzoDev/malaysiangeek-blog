# Product Requirements Document (PRD)

# MalaysianGeek.com Homepage Design

**Version:** 1.0
**Product Type:** Personal Technology Blog
**Platform:** Responsive Web Application
**Primary Audience:** Malaysian technology enthusiasts
**Design Direction:** Industrial Tech Magazine

---

# 1. Product Vision

## Overview

MalaysianGeek.com is a personal technology blog focused on documenting the creator's journey through technology in Malaysia.

The platform combines:

* Technology reviews
* Tutorials
* DIY projects
* Tech event coverage
* Gaming and movie-related technology discussions
* Personal opinions and experiences

The website should feel like entering a modern technology workshop: professional enough to establish credibility while maintaining the personality of a passionate tech enthusiast.

---

# 2. Design Philosophy

## Core Identity

**"A Malaysian tech enthusiast's digital workshop."**

The design should communicate:

| Attribute    | Expression                                                     |
| ------------ | -------------------------------------------------------------- |
| Technical    | Structured layouts, code-friendly typography, clean components |
| Industrial   | Brick texture, dark surfaces, metallic feel                    |
| Modern       | Smooth animations, responsive layouts, polished interactions   |
| Personal     | Author presence, events, projects, community comments          |
| Professional | Magazine-style content hierarchy                               |

---

# 3. Visual Theme

## Background

### Primary Background

A tiled brick wall texture covering the entire website.

Requirements:

* Seamless repeating pattern
* Darkened using overlay layer
* Low contrast to prevent distraction
* Fixed background while scrolling

Example layering:

```
Brick Wall Image
        ↓
Dark Overlay (70-80%)
        ↓
Content Surface
```

---

## Edge Treatment

Left and right edges fade into black.

Implementation:

```
Black gradient
←────── Website Content ──────→
Black gradient
```

Purpose:

* Creates depth
* Makes content appear like a physical display wall
* Improves readability

---

# 4. Color System

## Primary Accent

### MalaysianGeek Purple

```
#9b1cb6
```

Usage:

* Buttons
* Active navigation
* Category labels
* Hover states
* Links
* Highlights

---

## Secondary Accent

### Tech Red

```
#fe0001
```

Usage:

Limited use only:

* Live event badges
* Trending indicators
* Important notifications
* Featured labels

---

## Neutral Palette

| Usage             | Color   |
| ----------------- | ------- |
| Page background   | #050505 |
| Surface panels    | #111111 |
| Card background   | #161616 |
| Elevated surfaces | #202020 |
| Primary text      | #FFFFFF |
| Secondary text    | #A0A0A0 |
| Borders           | #333333 |

---

# 5. Typography

## Style Direction

**Premium Magazine**

The website should combine:

* Editorial personality
* Technical readability

---

# Font Recommendation

## Headings

Playfair Display style editorial serif OR modern display font.

Purpose:

* Gives articles personality
* Makes featured stories feel important

---

## Body

Inter

Purpose:

* Excellent readability
* Modern software/product feeling
* Works well with technical content

---

## Code

JetBrains Mono

Usage:

* Code blocks
* Technical snippets
* Terminal-style UI elements

---

# Typography Scale

| Element       | Size    |
| ------------- | ------- |
| Hero title    | 48-64px |
| Page heading  | 36-44px |
| Article title | 28-32px |
| Card title    | 20-24px |
| Body          | 16-18px |
| Metadata      | 13-14px |

---

# 6. Header

## Structure

```
------------------------------------------------
 Logo          Reviews Tutorials DIY News Movies Games
                                   Search  User  Dark
------------------------------------------------
```

---

## Logo

Requirements:

* Image-based logo uploaded by owner
* Clicking logo returns homepage
* Desktop:

  * Left aligned
* Mobile:

  * Center aligned

---

## Navigation Items

Primary navigation:

* Reviews
* Tutorials
* DIY Projects
* News
* Movies
* Games

---

## Header Behavior

No sticky header.

Reason:

* Keeps magazine feel
* Allows content immersion

---

## Header Features

Right side:

* Search icon
* Dark mode toggle
* User account icon

---

# 7. Homepage Structure

```
HEADER

↓


FEATURED ARTICLE GALLERY


↓


TRENDING / LATEST SWITCHER


↓


ARTICLE FEED + SIDEBAR


↓


COMMUNITY INFORMATION SECTION


↓


FOOTER
```

---

# 8. Hero Article Gallery

## Purpose

Immediately showcase the newest and most important content.

---

## Layout

Magazine-style carousel:

```
┌──────────┬──────────────────────┬──────────┐
│          │                      │          │
│ Article  │                      │ Article  │
│ Small    │   Main Article       │ Small    │
│          │                      │          │
└──────────┴──────────────────────┴──────────┘
```

---

## Main Card

Contains:

* Large image
* Category
* Title
* Date
* Short excerpt

---

## Side Cards

Contains:

* Smaller image
* Category
* Title
* Date

---

## Behaviour

Auto rotation:

* Every 5 seconds
* Smooth slide/fade animation

Controls:

* Pagination dots

---

# 9. Article Feed

## Layout

Desktop:

```
-------------------------------------------------
Articles                         Trending Topics
-------------------------------------------------

[ Article Card ]

[ Article Card ]

[ Article Card ]

```

---

## Article Cards

Magazine style.

Desktop:

Horizontal cards:

```
┌──────────┬──────────────────────────┐
│ Image    │ Category                 │
│          │ Title                    │
│          │ Description              │
│          │ Date                     │
└──────────┴──────────────────────────┘
```

---

## Card Information

Required:

* Featured image
* Category
* Published date
* Title
* Short excerpt

---

# 10. Trending / Latest Filter

At top of article list:

```
[ Trending ] [ Latest ]
```

---

## Trending

Sorted by:

* Views
* Engagement
* Comments

---

## Latest

Sorted by:

* Publication date

---

Animation:

* Smooth content transition
* Fade + slide

---

# 11. Sidebar

## Trending Topics

Purpose:

Provide quick discovery.

Contains:

* Popular articles
* Popular categories
* Popular tags

Example:

```
Trending Topics

#AI
#MalaysiaTech
#GamingPC
#DIYBuild
```

---

# 12. Categories & Tags

## Categories

Every article has:

One primary category.

Examples:

* Reviews
* Tutorials
* DIY Projects
* News
* Movies
* Games

---

## Tags

Optional.

Examples:

```
#RTX5090
#RaspberryPi
#Linux
#AI
#MechanicalKeyboard
```

---

# 13. Article Page Requirements

Articles support:

## Markdown

Required.

---

## Code Blocks

Features:

* Syntax highlighting
* Copy button
* Language labels

---

## Embedded Media

Supported:

* YouTube videos
* Image galleries

---

# 14. User System

## Authentication Required

Users must register before commenting.

---

# Comment System

Community style.

Features:

## User Profile

Contains:

* Avatar
* Username
* Join date

---

## Comments

Features:

* Comment likes/upvotes
* Replies
* User badges
* Profile pages

---

# 15. Community Information Section

Position:

Before footer.

Three-column layout.

---

# Column 1: Recent Comments

Purpose:

Encourage community engagement.

Displays:

* User avatar
* Username
* Comment preview
* Article link

Example:

```
John:
"Great build! I tried this too..."

on Raspberry Pi Project
```

---

# Column 2: Random News

Displays:

Random articles.

Purpose:

Increase content discovery.

---

# Column 3: Geeky Information

Contains:

## Links

* About Us
* Contact Us
* Privacy Policy
* Terms of Service

---

## Newsletter

Component:

```
Stay Updated

[ Email Address ]

[ Subscribe ]
```

---

# 16. Footer

Contains:

* Logo
* Social links
* Copyright
* Legal links

Style:

Minimal.

---

# 17. Dark Mode

Default:

Dark mode.

---

Theme:

Industrial dark.

---

Toggle:

Header icon.

Transition:

300ms smooth fade.

---

# 18. Responsive Design

## Desktop

1200px+

Layout:

* Three-column hero
* Article + sidebar
* Three-column community section

---

## Tablet

768px-1199px

Changes:

* Hero becomes stacked
* Sidebar moves below articles
* Two-column community section

---

## Mobile

<768px

Changes:

Hero:

```
Main article
↓
Small articles
```

Feed:

Vertical cards.

Navigation:

Hamburger menu.

Community section:

Single column.

---

# 19. Animation System

Style:

Modern, subtle.

---

## Required animations

### Card Hover

* Lift 8px
* Shadow increase
* Image zoom

---

### Buttons

* Purple glow
* Smooth color transition

---

### Carousel

* Slide transition
* Fade transition

---

### Page

* Smooth scrolling
* Content reveal animation

---

# 20. UX Goals

Visitors should:

1. Immediately understand this is a Malaysian tech creator's blog.
2. Discover interesting articles within seconds.
3. Feel encouraged to explore projects and events.
4. Participate through comments.
5. Subscribe for future updates.

---

# 21. Homepage Success Criteria

The homepage is successful if:

✅ Users immediately see featured content
✅ The personality of the creator is obvious
✅ Articles are easy to discover
✅ Community interaction is visible
✅ Design feels modern and technical
✅ The website feels unique compared to generic tech blogs
