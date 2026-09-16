---
title: Styling
weight: 40
---

CM Live Deal 4.0 draws its front end with one stylesheet and a set of CSS variables. You can change most of the look — colours, spacing, corners, type sizes — by setting a few variables in your template, without copying a single PHP file.

## Where the CSS comes from

| File | What it is |
| ---- | ---------- |
| `media/com_cmlivedeal/css/site-styles.css` | Every front-end page and module. |
| `media/com_cmlivedeal/css/site-styles-rtl.css` | Loaded on top of it for a right-to-left language. |
| `media/com_cmlivedeal/css/admin-styles.css` | The back end only. |

**Never edit these files.** They are built from source and an update overwrites them. Put your CSS in your template instead.

In Cassiopeia, create `media/templates/site/cassiopeia/css/user.css`. Cassiopeia loads it last on every page, after CM Live Deal, so your rules win. Another template will have its own place for custom CSS; any stylesheet loaded after CM Live Deal's works.

## The `.cmlivedeal` wrapper

Every CM Live Deal view and module is inside one element with the class `cmlivedeal`:

```html
<div class="cmlivedeal" data-cmld-theme="auto">
```

Two things follow from this:

* All of the extension's own CSS is written inside `.cmlivedeal`, so nothing leaks into the rest of your page.
* Write your own rules inside `.cmlivedeal` too. That keeps them out of the rest of the site, and it gives them the same weight as the rule you are replacing, so they win simply by coming later.

```css
.cmlivedeal .cmld-card {
    border-width: 2px;
}
```

## The variables

All of the variables are set on `.cmlivedeal`, and they all start with `--cmld-`. Set the ones you want in your own stylesheet and everything that uses them follows:

```css
.cmlivedeal {
    --cmld-radius: 4px;
    --cmld-accent: #6b3fa0;
}
```

### Colours

| Variable | Light value | What it colours |
| -------- | ----------- | --------------- |
| `--cmld-bg` | `#fcfaf7` | The page background behind the deal list. Only painted in dark mode. |
| `--cmld-surface` | `#ffffff` | Cards, modals, tables. |
| `--cmld-surface-2` | `#f6f2ed` | Quieter panels: card headers, custom field boxes, form controls in dark mode. |
| `--cmld-text` | `#241e1a` | Normal text. |
| `--cmld-muted` | `#6f6761` | Secondary text: dates, distances, the crossed-out price. |
| `--cmld-border` | `#e1ddd8` | Every border and divider. |
| `--cmld-accent` | your template's primary colour | The small accents CM Live Deal draws itself: a ticked checkbox, the "You are here" marker on the map. |
| `--cmld-accent-hover` | your template's hover colour | The same, hovered. |
| `--cmld-accent-ink` | `#ffffff` | Text and icons drawn *on* the accent colour. |
| `--cmld-accent-text` | `var(--cmld-accent)` | The accent used as text: links, a hovered deal title, the focus ring. Lightened in dark mode so it stays readable. |
| `--cmld-sale` | Bootstrap's danger | The sale price and the "hurry" countdown. |
| `--cmld-sale-text`, `--cmld-sale-bg` | Bootstrap's danger emphasis / subtle | The countdown bar when a deal is nearly over. |
| `--cmld-live` | Bootstrap's success | A deal that is running. |
| `--cmld-live-text`, `--cmld-live-bg` | Bootstrap's success emphasis / subtle | The countdown bar in its normal state. |
| `--cmld-warn` | Bootstrap's warning | A deal ending later today. |
| `--cmld-warn-text`, `--cmld-warn-bg` | Bootstrap's warning emphasis / subtle | The countdown bar in that state. |
| `--cmld-overlay` | `#000000` | The "please wait" cover over the scanner. |
| `--cmld-overlay-ink` | `#ffffff` | Its spinner. |
| `--cmld-field-border` | `#8f877f` | Inputs and selects, dark enough to pass the contrast rules. |

`--cmld-accent` is taken from your template, in this order: Cassiopeia's `--cassiopeia-color-primary`, then Bootstrap's `--bs-primary`, then `#0d6efd`. So on Cassiopeia, changing the template's colour in `System > Site Templates Styles` also changes CM Live Deal. The buttons are a separate matter: they are your template's Bootstrap buttons — see [What your template still owns](#what-your-template-still-owns).

The sale, live and warn colours follow Bootstrap's own variables the same way, so a template with its own Bootstrap palette is followed too.

### Spacing, corners and type

| Variable | Value | Used for |
| -------- | ----- | -------- |
| `--cmld-space-1` … `--cmld-space-6` | `0.25rem`, `0.5rem`, `0.75rem`, `1rem`, `1.5rem`, `2rem` | Every gap and padding. |
| `--cmld-radius-sm` | `7px` | Small boxes: the countdown bar, a custom field. |
| `--cmld-radius` | `12px` | Cards and modals. |
| `--cmld-radius-pill` | `999px` | Pills and the countdown track. |
| `--cmld-font-xs` … `--cmld-font-2xl` | `0.75rem`, `0.8125rem`, `0.9375rem`, `1.0625rem`, `1.5rem`, `1.875rem` | Type sizes, in order. |
| `--cmld-weight-medium` | `600` | Emphasised text. |
| `--cmld-weight-strong` | `650` | Deal titles and the button on a card. |
| `--cmld-shadow` | soft double shadow | Cards. |
| `--cmld-shadow-up` | upward shadow | The buy bar and the sticky modal footer. |

Two more variables are set by JavaScript, not by you: `--cmld-countdown-left` (how much of a deal's time is left, `0` to `1`) and `--cmld-scrollbar-width`.

## Dark mode

Dark mode is a component setting, not a template one: `Components > CM Live Deal > Options > Dark mode`. CM Live Deal cannot ask your template whether the page is dark, so you tell it which rule to follow:

| Setting | What happens | Selector |
| ------- | ------------ | -------- |
| Disabled | Always light. No attribute is written. | — |
| Follow visitor's system setting | Dark when the device is in dark mode. | `@media (prefers-color-scheme: dark) .cmlivedeal[data-cmld-theme="auto"]` |
| Always dark | Always dark, for a template that is always dark. | `.cmlivedeal[data-cmld-theme="dark"]` |
| Follow template (`data-bs-theme`) | Dark when an ancestor has `data-bs-theme="dark"`. | `[data-bs-theme="dark"] .cmlivedeal[data-cmld-theme="template"]` |

The setting is written onto the wrapper as `data-cmld-theme`. In dark mode CM Live Deal also gives its wrapper a background and padding, so the block reads as a dark panel even on a light page:

![/images/styling-4-0-dark.png](/images/styling-4-0-dark.png)

**Write your colours for both themes.** The dark rules set the same variables again on a more specific selector, so a colour you set on `.cmlivedeal` alone is used in light mode and then overruled in dark mode. Set it in both places:

```css
/* Light. */
.cmlivedeal {
    --cmld-surface: #fffdf8;
}

/* Dark: all three ways of turning it on. */
.cmlivedeal[data-cmld-theme="dark"],
[data-bs-theme="dark"] .cmlivedeal[data-cmld-theme="template"] {
    --cmld-surface: #1d1a16;
}

@media (prefers-color-scheme: dark) {
    .cmlivedeal[data-cmld-theme="auto"] {
        --cmld-surface: #1d1a16;
    }
}
```

Variables that dark mode does not touch — the spacing, the corners, the type sizes, `--cmld-accent` — only need to be set once, on `.cmlivedeal`.

## A worked example

Say you want squarer cards, a little more air between the deals, and a purple accent that the "View deal" button follows too. Create `media/templates/site/cassiopeia/css/user.css` with:

```css
.cmlivedeal {
    --cmld-radius: 4px;
    --cmld-radius-sm: 3px;
    --cmld-accent: #6b3fa0;
    --cmld-space-5: 2rem;
}

.cmlivedeal .btn-primary {
    background-color: var(--cmld-accent);
    border-color: var(--cmld-accent);
    color: var(--cmld-accent-ink);
}
```

Save it and reload the deal list:

![/images/styling-4-0-tokens.png](/images/styling-4-0-tokens.png)

The four variables were enough for the cards, the popup, the countdown and the corners everywhere, because all of those were built from them. The last rule is only needed because the button is a Bootstrap `btn btn-primary`, which belongs to your template, not to CM Live Deal.

No PHP file was copied, so a CM Live Deal update cannot undo any of it.

## What your template still owns

CM Live Deal uses Bootstrap 5 classes for buttons, badges, tables, alerts, modals and forms. Those keep your template's look on purpose — a `btn btn-primary` in a deal should be the same button as everywhere else on your site. The discount tag is a `badge text-bg-danger` and the "Featured" ribbon is a `badge text-bg-warning`, so they follow your template too.

If you want those to look different inside CM Live Deal only, style them under the wrapper:

```css
.cmlivedeal .badge {
    border-radius: var(--cmld-radius-pill);
}
```

## The pieces you are most likely to style

| Class | What it is |
| ----- | ---------- |
| `.deal-list-container`, `.deal-list` | The grid of deals. `data-columns` on `.deal-list` holds the column count from the menu item. |
| `.deal-container`, `.cmld-card` | One deal card, and the box around it. |
| `.cmld-card-media`, `.cmld-card-body`, `.cmld-card-cta` | Its image, its text and its button. |
| `.cmld-card-badges`, `.value-tag`, `.featured-ribbon` | The tags over the image. |
| `.deal-name`, `.deal-info`, `.cmld-meta` | The title, the small print under it, and one line of it with its icon. |
| `.values`, `.sale-price`, `.original-price` | The prices. |
| `.cmld-countdown-bar`, `.cmld-countdown-panel`, `.cmld-countdown-track`, `.cmld-countdown-fill` | The countdown on a deal page. |
| `.cmld-modal-header`, `.cmld-modal-body`, `.cmld-modal-summary`, `.cmld-modal-section` | The quick-view popup. |
| `.cmld-buy-bar` | The bar that sticks to the bottom of a deal on a phone. |
| `.cmld-share`, `.cmld-share-link` | The share buttons and the copy-link box. |

### The countdown states

The countdown sets `data-state` on its `<cmld-countdown>` element as the clock runs down, so you can colour each stage:

| `data-state` | When |
| ------------ | ---- |
| `live` | More than 12 hours left. |
| `soon` | Less than 12 hours left. |
| `urgent` | Less than 2 hours left. |
| `expired` | The deal has ended. |

```css
.cmlivedeal cmld-countdown[data-state="urgent"] .cmld-countdown-panel {
    animation: pulse 2s infinite;
}
```

## The cards resize by their container, not the window

The deal list, each card and the quick-view popup are CSS containers, named `cmld-list`, `cmld-card` and `cmld-modal`. The layout changes with the width of the container, not the width of the browser, so a deal list in a narrow sidebar module shows narrow cards on a wide screen. If you add your own breakpoints, use `@container` the same way:

```css
@container cmld-card (min-width: 520px) {
    .cmlivedeal .deal-name {
        font-size: var(--cmld-font-xl);
    }
}
```

## Icons

The icons are Font Awesome 6, loaded from a CDN. If your template already ships Font Awesome, turn CM Live Deal's copy off in `Options > General > Font Awesome` so the page does not load it twice.

## Accessibility

Two rules are there for people who do not use a mouse. Please keep them if you restyle:

* **The focus ring.** Anything focused with the keyboard gets `outline: 2px solid var(--cmld-accent-text)` with a 2px offset. If you dislike it, change the colour — never set `outline: none`.
* **Touch targets.** Buttons, pagination links and the small meta links are kept at a minimum size so they can be tapped.

The colours above were picked to pass WCAG AA contrast in both themes. If you change `--cmld-muted`, `--cmld-field-border` or any of the `-text` colours, check them against their background.

## Right to left

CM Live Deal's stylesheet uses logical properties (`padding-inline`, `inset-inline`, `margin-inline-start`), so it flips by itself for Arabic, Hebrew and Persian. `site-styles-rtl.css` is loaded after it for anything that still needs a hand. Write your own rules the same way and you will not need an RTL override at all.

## See also

* [JavaScript](/developers/javascript/): the scripts, their options and the `<cmld-countdown>` element.
* [Template overrides](/developers/template-overrides/): change the HTML when CSS is not enough.
* [General options](/configuration/general/): where the Dark mode and Font Awesome settings are.
