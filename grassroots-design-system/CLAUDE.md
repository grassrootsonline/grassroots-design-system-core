# Grassroots Design System

This file tells Claude (and any AI assistant) how to build UI for Project Grassroots correctly. Read it before writing any HTML, CSS, React, or component code for this project.

---

## What this system is

Grassroots is a feed-based social platform. The design system is **clean and minimal** — it exists to get out of the way of content. The aesthetic is warm-neutral, not cool. The personality lives in the typography contrast and the non-blue accent, not in decoration.

When in doubt: **remove something rather than add something.**

---

## File structure

```
grassroots-design-system/
├── index.css                   ← Import this. It pulls everything in order.
├── tokens/
│   ├── colors.css              ← All color custom properties + dark mode
│   ├── typography.css          ← Font families, scale, utility classes
│   └── spacing.css             ← Spacing, radius, borders, z-index, layout
└── components/
    └── components.css          ← All component classes
```

Always import `index.css`. Never import token files individually in production.

---

## Color

All colors are CSS custom properties. **Never hardcode a hex value in component code.** Use tokens.

| Token | Value | Use |
|---|---|---|
| `--color-ink` | `#1C2B1A` | Primary text, filled buttons, headings |
| `--color-ink-soft` | `#3D3D35` | Body copy, feed post text |
| `--color-accent` | `#6B8C6A` | The one interactive color. Focus rings, active links, following badges |
| `--color-accent-subtle` | `#D5E5D4` | Avatar backgrounds, accent badge fills |
| `--color-accent-mist` | `rgba(107,140,106,0.12)` | Focus ring glow only |
| `--color-canvas` | `#F7F6F4` | Page background |
| `--color-surface` | `#FAFAF8` | Cards, inputs, nav bar |
| `--color-border` | `#E2DDD7` | Default hairline borders |
| `--color-border-strong` | `#C5C0B8` | Input borders, dividers on hover |
| `--color-secondary` | `#8A897F` | Timestamps, secondary labels |
| `--color-muted` | `#A5A49A` | Placeholders, tertiary text |
| `--color-danger` | `#C17A5A` | Errors, destructive actions |
| `--color-danger-subtle` | `#F0EAE0` | Error background tints |

**The accent is sage green, not blue.** This is the system's primary personality choice. Do not introduce blue as an interactive color. Do not use `#0000ff`, `#3B82F6`, or any blue for hover, focus, or active states.

Dark mode is handled automatically via `@media (prefers-color-scheme: dark)` overrides in `colors.css`. Never write manual dark mode overrides in component code — trust the tokens.

---

## Typography

Two fonts. That's it.

| Role | Family | When to use |
|---|---|---|
| `--font-display` | DM Serif Display | Wordmark, hero headings, section titles. Use sparingly. |
| `--font-body` | Inter | Everything else — all UI, labels, body copy, buttons. |

**The display font is kept rare.** If it appears on every screen it loses meaning. Use `--font-display` for the wordmark and one or two headline moments per page at most.

### Type scale

| Token | Size | Weight | Usage |
|---|---|---|---|
| `--text-display` | 36px | — (font-display) | Hero / page title |
| `--text-title` | 24px | — (font-display) | Section heading |
| `--text-heading` | 16px | 500 | Card titles, modal headers |
| `--text-body` | 14px | 400 | Feed posts, descriptions |
| `--text-small` | 12px | 400 | Timestamps, metadata |
| `--text-label` | 11px | 500 | Eyebrows, section labels (always uppercase + tracked) |

Only two weights are used: **400 regular** and **500 medium**. Never use 600, 700, or bold. They feel heavy against the warm neutrals.

### Utility classes

```html
<h1 class="text-display">Something worth saying</h1>
<h2 class="text-title">Section heading</h2>
<h3 class="text-heading">Card title</h3>
<p class="text-body">Post content goes here.</p>
<span class="text-small">2 hours ago</span>
<span class="text-label">Community</span>
```

---

## Spacing

Use the scale. Don't invent values.

| Token | Value |
|---|---|
| `--space-xs` | 4px |
| `--space-sm` | 8px |
| `--space-md` | 12px |
| `--space-lg` | 20px |
| `--space-xl` | 32px |
| `--space-2xl` | 48px |
| `--space-3xl` | 72px |

---

## Border radius

| Token | Value | Use |
|---|---|---|
| `--radius-sm` | 4px | Badges, small tags |
| `--radius-md` | 6px | Buttons, inputs |
| `--radius-lg` | 10px | Cards, panels |
| `--radius-xl` | 16px | Modals, bottom sheets |
| `--radius-pill` | 999px | Avatars, pill badges |

---

## Borders

All borders are **0.5px**. Never 1px. This is a system-wide rule.

```css
border: var(--border-default);  /* 0.5px solid --color-border */
border: var(--border-strong);   /* 0.5px solid --color-border-strong */
border: var(--border-accent);   /* 0.5px solid --color-accent */
border: var(--border-dashed);   /* 0.5px dashed --color-border-strong — empty states only */
```

Focus rings use box-shadow, not border:
```css
box-shadow: var(--focus-ring); /* 0 0 0 3px --color-accent-mist */
```

---

## Components

### Buttons

```html
<!-- Primary — one per view maximum -->
<button class="btn btn-primary">Follow</button>

<!-- Secondary — default for most actions -->
<button class="btn btn-secondary">Share</button>

<!-- Ghost — low-emphasis, tertiary actions -->
<button class="btn btn-ghost">Skip for now</button>

<!-- Danger — destructive only -->
<button class="btn btn-danger">Delete post</button>

<!-- Sizes -->
<button class="btn btn-primary btn-sm">Post</button>
<button class="btn btn-secondary btn-lg">Create account</button>

<!-- Icon only -->
<button class="btn btn-secondary btn-icon" aria-label="Notifications">
  <i class="ti ti-bell" aria-hidden="true"></i>
</button>
```

**Rules:**
- Sentence case always. "Create post", not "Create Post".
- Verb first: "Save changes", not "Changes saved".
- One `.btn-primary` per view. All siblings use `.btn-secondary` or `.btn-ghost`.
- Never disable a button unless there's a clear reason visible on screen.

---

### Badges

```html
<span class="badge badge-default">General</span>
<span class="badge badge-accent">Following</span>
<span class="badge badge-warm">New</span>
<span class="badge badge-muted">Draft</span>
<span class="badge badge-danger">Removed</span>
```

---

### Inputs

```html
<!-- Basic input -->
<input class="input" type="text" placeholder="What's on your mind?" />

<!-- With field wrapper -->
<div class="field">
  <label class="field-label">Display name</label>
  <input class="input" type="text" placeholder="e.g. Maya Rodriguez" />
  <span class="field-hint">Shown on your profile and posts.</span>
</div>

<!-- Error state -->
<div class="field">
  <label class="field-label">Email</label>
  <input class="input input-error" type="email" value="not-an-email" />
  <span class="field-error">Enter a valid email address.</span>
</div>

<!-- Textarea -->
<textarea class="input" rows="4" placeholder="Share something..."></textarea>
```

---

### Avatars

```html
<!-- Default (md) -->
<div class="avatar avatar-md">MR</div>

<!-- Sizes -->
<div class="avatar avatar-sm">MR</div>
<div class="avatar avatar-lg">MR</div>
<div class="avatar avatar-xl">MR</div>

<!-- With image -->
<div class="avatar avatar-md">
  <img src="/user/maya.jpg" alt="Maya Rodriguez" />
</div>

<!-- Stacked group -->
<div class="avatar-group">
  <div class="avatar avatar-sm">MR</div>
  <div class="avatar avatar-sm">JK</div>
  <div class="avatar avatar-sm">AL</div>
  <div class="avatar avatar-sm" style="background:var(--color-surface);color:var(--color-secondary);">+4</div>
</div>
```

Avatar initials are always 2 characters. Overflow count ("+4") uses muted colors, not accent.

---

### Feed card

```html
<article class="feed-card">
  <header class="feed-card-header">
    <div class="avatar avatar-md">MR</div>
    <div class="feed-card-meta">
      <div class="feed-card-name">Maya Rodriguez</div>
      <div class="feed-card-time">
        2 hours ago · <a href="/c/design" style="color:var(--color-accent);">Community Design</a>
      </div>
    </div>
  </header>

  <div class="feed-card-body">
    Post content goes here. Keep line-height at --leading-body (1.65) for readability.
  </div>

  <footer class="feed-card-actions">
    <button class="action-btn">
      <i class="ti ti-heart" aria-hidden="true"></i>
      48
    </button>
    <button class="action-btn">
      <i class="ti ti-message-circle" aria-hidden="true"></i>
      12
    </button>
    <button class="action-btn">
      <i class="ti ti-share" aria-hidden="true"></i>
      Share
    </button>
  </footer>
</article>
```

---

### Navigation bar

```html
<nav class="navbar">
  <a href="/" class="navbar-logo">Grassroots</a>

  <ul class="navbar-links">
    <li><a href="/feed" class="navbar-link active">Feed</a></li>
    <li><a href="/explore" class="navbar-link">Explore</a></li>
    <li><a href="/communities" class="navbar-link">Communities</a></li>
  </ul>

  <div class="navbar-actions">
    <button class="btn btn-secondary btn-icon" aria-label="Notifications">
      <i class="ti ti-bell" aria-hidden="true"></i>
    </button>
    <div class="avatar avatar-sm">MR</div>
  </div>
</nav>
```

The navbar is `position: sticky; top: 0`. It always sits above the feed on scroll.

---

### Empty states

```html
<div class="empty-state">
  <div class="empty-state-icon">
    <i class="ti ti-photo" aria-hidden="true"></i>
  </div>
  <div class="empty-state-title">Your feed is quiet</div>
  <div class="empty-state-body">
    Follow people and communities to see their posts here.
  </div>
  <button class="btn btn-primary btn-sm">Explore communities</button>
</div>
```

Empty states are invitations, not apologies. The title names the space. The body says what to do. The CTA is a verb.

---

### Notifications

```html
<div class="notif">
  <div class="notif-dot"></div>           <!-- Unread: filled sage -->
  <div>
    <div class="notif-text">
      <strong style="font-weight:500;">Jonah Kim</strong> liked your post.
    </div>
    <div class="notif-time">5 min ago</div>
  </div>
</div>

<div class="notif">
  <div class="notif-dot read"></div>      <!-- Read: hollow dot -->
  <div>
    <div class="notif-text">3 people started following you.</div>
    <div class="notif-time">Yesterday</div>
  </div>
</div>
```

---

### Toasts

```html
<div class="toast">Post published.</div>
<div class="toast toast-success">Changes saved.</div>
<div class="toast toast-danger">Couldn't connect. Try again.</div>
```

Toasts are brief, past-tense, no punctuation unless needed. "Post published", not "Your post was published successfully!".

---

## Icons

This system uses [Tabler Icons](https://tabler.io/icons) (outline only).

```html
<!-- Load via CDN in <head> -->
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/@tabler/icons-webfont@latest/tabler-icons.min.css" />

<!-- Usage -->
<i class="ti ti-heart" aria-hidden="true"></i>
<i class="ti ti-message-circle" aria-hidden="true"></i>
<i class="ti ti-share" aria-hidden="true"></i>
<i class="ti ti-bell" aria-hidden="true"></i>
<i class="ti ti-search" aria-hidden="true"></i>
<i class="ti ti-user" aria-hidden="true"></i>
<i class="ti ti-settings" aria-hidden="true"></i>
<i class="ti ti-home" aria-hidden="true"></i>
```

- Always **outline** variants. Never `-filled` suffixes.
- Decorative icons get `aria-hidden="true"`.
- Icon-only buttons get `aria-label` on the button element.
- Size via `font-size`: 14px inline, 16–20px standard UI, 24px max.

---

## Layout

```css
--feed-max-width:    640px;  /* Single-column feed container */
--sidebar-width:     240px;  /* Left nav / sidebar */
--content-max-width: 960px;  /* Outer page wrapper */
```

The feed is single-column, centered, max 640px. This is not negotiable — wider feeds hurt readability and feel like a desktop product, not a social platform.

Standard page shell:

```html
<body>
  <nav class="navbar">…</nav>

  <main style="max-width: var(--content-max-width); margin: 0 auto; padding: var(--space-xl);">
    <div style="max-width: var(--feed-max-width); margin: 0 auto;">
      <!-- Feed content -->
    </div>
  </main>
</body>
```

---

## Writing style

- **Sentence case everywhere.** Buttons, headings, labels, nav links. Never Title Case in UI.
- **Verb-first on CTAs.** "Create post", "Edit profile", "Follow community".
- **No "successfully".** "Post published", not "Your post was published successfully."
- **No "please".** "Enter a display name", not "Please enter a display name."
- **No exclamation marks** on system copy. "Changes saved." not "Changes saved!"
- **Errors say what happened and what to do.** "That username's taken. Try another." Not "Error 409."
- **Empty states are invitations.** "Your feed is quiet" + "Follow people to see their posts here." Not "Nothing here yet."

---

## What not to do

| Don't | Do instead |
|---|---|
| Hardcode hex values | Use `--color-*` tokens |
| Use blue for interactive states | Use `--color-accent` (sage green) |
| Use `font-weight: 600` or `700` | Max is `500` |
| Add drop shadows to cards | Hairline borders only (`--border-default`) |
| Use `border: 1px` | Always `0.5px` |
| Use DM Serif Display for body text | Only for wordmark and display headings |
| Use more than one `.btn-primary` per view | Demote extras to `.btn-secondary` |
| Write "successfully" in toast copy | Just state what happened |
| Create new spacing values outside the scale | Use the closest `--space-*` token |
| Introduce a second accent color | Sage green is the only interactive color |

---

## Checklist before shipping a screen

- [ ] All colors reference `--color-*` tokens
- [ ] No hardcoded hex, rgb, or rgba values in component styles
- [ ] All borders are 0.5px
- [ ] Focus states use `--focus-ring` box-shadow
- [ ] One `.btn-primary` maximum per view
- [ ] DM Serif Display used only for display/title roles
- [ ] All text is sentence case
- [ ] Icons have `aria-hidden="true"` or `aria-label` on parent
- [ ] Dark mode works (tokens flip automatically — just don't override them)
- [ ] Feed content is constrained to `--feed-max-width` (640px)
