# Icon System — Strict Specification

## ⛔ MANDATORY — Use Fello Icons, Not Lucide

**ALWAYS use icons from the Fello icon set in this file.** Do NOT default to Lucide React icons. Lucide is ONLY a last-resort fallback when a specific icon does not exist in the Fello set.

**Before using ANY icon, check the Quick Lookup table at the bottom of this file and the categorized lists above it.** If a Fello icon exists for your need — use it. If you are using a Lucide icon, you are probably wrong — check this file again.

**Priority order:**
1. ✅ Fello icon set (this file) — **ALWAYS check here first, ALWAYS**
2. ✅ Lucide React — ONLY when the exact icon does not exist in the Fello set
3. ❌ Heroicons, Font Awesome, Material Icons, or any other library — NEVER use

**Fello icons use `<i>` tags with CSS classes, NOT React components.** If you see `<Home />`, `<Heart />`, `<Users />` in your code — those are Lucide components and should be replaced with Fello equivalents like `<i className="fello-icon-home">`, `<i className="fello-icon-heart">`, `<i className="fello-icon-users">`.

> **TOKEN RULE:** Every color in this spec is expressed as a CSS variable token. In Tailwind classes use `bg-[var(--token)]` / `text-[var(--token)]` / `border-[var(--token)]`. In inline styles use `'var(--token)'`. In prose and tables, show `var(--token)` (#hex) for reference.

---

## Setup — Add to index.css (ONE TIME)

Add this import at the TOP of `src/index.css` (before any Tailwind directives):

```css
@import url('https://cdn.icomoon.io/231278/FelloIcons/style.css?ubpci2');
```

---

## How to Use a Fello Icon

Fello icons are a **web font**. Render them with an `<i>` element and a CSS class:

```jsx
{/* 16px icon inside a small button */}
<i className="fello-icon-add" style={{ fontSize: '16px' }} />

{/* 20px icon inside a medium button */}
<i className="fello-icon-settings-cog-6" style={{ fontSize: '20px' }} />

{/* 24px icon in navigation / list items */}
<i className="fello-icon-home" style={{ fontSize: '24px' }} />

{/* Icon with custom color */}
<i className="fello-icon-heart-fill text-[var(--text-brand-primary)]" style={{ fontSize: '20px' }} />
```

### Pattern

```
<i className="fello-icon-{name}" style={{ fontSize: '{size}px' }} />
```

- **Class:** `fello-icon-` followed by the icon name from the table below
- **Size:** Set via `fontSize` inline style — **match the size to the context** (see table below). Do NOT default everything to 24px.
- **Color:** Inherited from parent `color` / Tailwind `text-[#hex]` class
- **Line height:** Always renders at `line-height: 1` (built into the font)
- **Display:** Inline element — use inside flex containers, buttons, etc.

### ⚠️ Size Guide — Match Size to Context

| Context | fontSize | When to use |
|---------|----------|-------------|
| `16px` | Small buttons, inline text, badges, table actions | Compact UI, tight spaces |
| `20px` | Medium buttons, form field icons, toolbar icons | Most common for buttons |
| `24px` | Navigation items, list items, card headers | Sidebar nav, main navigation |
| `32px` | Page headers, section titles | Large emphasis areas |
| `48px` or `64px` | Featured icons, empty states, onboarding | Hero/decorative use only |

**Rule: When replacing a Lucide icon, match the original pixel size.** If the Lucide icon was `w-5 h-5` (20px), use `fontSize: '20px'`. If it was `w-4 h-4` (16px), use `fontSize: '16px'`.

---

## ⚠️ CRITICAL Rules

1. **ALWAYS check this icon list first** before using any Lucide icon. Most UI needs are covered here.
2. **Use `<i className="fello-icon-{name}">` not `<img>`, not `<svg>`, not a React component** — this is a font icon system.
3. **Size is controlled ONLY by `fontSize`** — do NOT use `width`/`height` on font icons.
4. **Color is inherited** from the parent CSS `color` property. Use Tailwind `text-[#hex]` on the `<i>` tag or its parent.
5. **Do NOT wrap icons in unnecessary containers** — they are inline elements that work directly inside flex rows, buttons, spans, etc.
6. **The `@import` line in `index.css` is required** — without it, icons will not render. Add it once at the top of the file.
7. **When using inside a button**, place the `<i>` tag directly as a sibling of the label `<span>`:
   ```jsx
   <button className="inline-flex items-center gap-1.5 ...">
     <i className="fello-icon-add" style={{ fontSize: '20px' }} />
     <span>Add Item</span>
   </button>
   ```
8. **For icon-only buttons**, use the icon alone with an `aria-label` on the button:
   ```jsx
   <button aria-label="Settings" className="...">
     <i className="fello-icon-settings-cog-6" style={{ fontSize: '24px' }} />
   </button>
   ```

---

## Complete Icon Reference

### AI & Smart Features
| Icon Name | CSS Class |
|-----------|-----------|
| ai-content-star | `fello-icon-ai-content-star` |
| ai-automation-star | `fello-icon-ai-automation-star` |
| ai-data-enrich-star | `fello-icon-ai-data-enrich-star` |
| ai-generate-star | `fello-icon-ai-generate-star` |
| ai-insights | `fello-icon-ai-insights` |
| ai-mail-star | `fello-icon-ai-mail-star` |
| ai-paragraph-summary-star | `fello-icon-ai-paragraph-summary-star` |
| ai-search-star | `fello-icon-ai-search-star` |
| ai-segment-star | `fello-icon-ai-segment-star` |
| ai-show-star | `fello-icon-ai-show-star` |
| ai-image-star | `fello-icon-ai-image-star` |
| ai-mail-filled-star | `fello-icon-ai-mail-filled-star` |
| ai-pencil | `fello-icon-ai-pencil` |
| ai-segment-filled-star | `fello-icon-ai-segment-filled-star` |
| ai-smart-send | `fello-icon-ai-smart-send` |
| ai-stars | `fello-icon-ai-stars` |
| ai-user-stars | `fello-icon-ai-user-stars` |
| ai-call-phone-star | `fello-icon-ai-call-phone-star` |
| ai-sms | `fello-icon-ai-sms` |
| ai-chat | `fello-icon-ai-chat` |
| conversational-ai | `fello-icon-conversational-ai` |
| fello-iq-icon | `fello-icon-fello-iq-icon` |

### Arrows & Direction
| Icon Name | CSS Class |
|-----------|-----------|
| arrow-circular-01 | `fello-icon-arrow-circular-01` |
| arrow-circular-02 | `fello-icon-arrow-circular-02` |
| arrow-circular-down | `fello-icon-arrow-circular-down` |
| arrow-circular-left | `fello-icon-arrow-circular-left` |
| arrow-circular-right | `fello-icon-arrow-circular-right` |
| arrow-circular-up | `fello-icon-arrow-circular-up` |
| arrow-curved-down | `fello-icon-arrow-curved-down` |
| arrow-curved-left | `fello-icon-arrow-curved-left` |
| arrow-curved-left-redo | `fello-icon-arrow-curved-left-redo` |
| arrow-curved-left-undo | `fello-icon-arrow-curved-left-undo` |
| arrow-curved-right | `fello-icon-arrow-curved-right` |
| arrow-curved-top | `fello-icon-arrow-curved-top` |
| arrow-double-horizontal | `fello-icon-arrow-double-horizontal` |
| arrow-double-vertical | `fello-icon-arrow-double-vertical` |
| arrow-narrow-down | `fello-icon-arrow-narrow-down` |
| arrow-narrow-left | `fello-icon-arrow-narrow-left` |
| arrow-narrow-right | `fello-icon-arrow-narrow-right` |
| arrow-narrow-up | `fello-icon-arrow-narrow-up` |
| arrow-straight-down | `fello-icon-arrow-straight-down` |
| arrow-straight-down-left | `fello-icon-arrow-straight-down-left` |
| arrow-straight-down-right | `fello-icon-arrow-straight-down-right` |
| arrow-straight-left | `fello-icon-arrow-straight-left` |
| arrow-straight-right | `fello-icon-arrow-straight-right` |
| arrow-straight-up | `fello-icon-arrow-straight-up` |
| arrow-straight-up-left | `fello-icon-arrow-straight-up-left` |
| arrow-straight-up-right | `fello-icon-arrow-straight-up-right` |
| arrow-trend-down | `fello-icon-arrow-trend-down` |
| arrow-trend-up | `fello-icon-arrow-trend-up` |
| arrow-up-down-triangle | `fello-icon-arrow-up-down-triangle` |
| arrow-right-fill | `fello-icon-arrow-right-fill` |
| double-arrow-horizontal | `fello-icon-double-arrow-horizontal` |
| double-arrow-left | `fello-icon-double-arrow-left` |
| double-arrow-right | `fello-icon-double-arrow-right` |
| double-arrow-vertical | `fello-icon-double-arrow-vertical` |

### Chevrons
| Icon Name | CSS Class |
|-----------|-----------|
| chevron-arrow-down | `fello-icon-chevron-arrow-down` |
| chevron-arrow-left | `fello-icon-chevron-arrow-left` |
| chevron-arrow-right | `fello-icon-chevron-arrow-right` |
| chevron-arrow-up | `fello-icon-chevron-arrow-up` |
| chevron-double-horizontal | `fello-icon-chevron-double-horizontal` |
| chevron-double-horizontal-circular | `fello-icon-chevron-double-horizontal-circular` |
| chevron-double-vertical | `fello-icon-chevron-double-vertical` |
| chevron-double-vertical-circular | `fello-icon-chevron-double-vertical-circular` |

### Add, Remove & Close
| Icon Name | CSS Class |
|-----------|-----------|
| add | `fello-icon-add` |
| add-circle | `fello-icon-add-circle` |
| add-square | `fello-icon-add-square` |
| add-fill | `fello-icon-add-fill` |
| minus | `fello-icon-minus` |
| minus-circle | `fello-icon-minus-circle` |
| minus-square | `fello-icon-minus-square` |
| minus-fill | `fello-icon-minus-fill` |
| close | `fello-icon-close` |
| close-circle | `fello-icon-close-circle` |
| close-fill | `fello-icon-close-fill` |
| ban | `fello-icon-ban` |

### Check & Status
| Icon Name | CSS Class |
|-----------|-----------|
| check | `fello-icon-check` |
| checkmark | `fello-icon-checkmark` |
| check-badge | `fello-icon-check-badge` |
| check-badge-fill | `fello-icon-check-badge-fill` |
| check-circle | `fello-icon-check-circle` |
| check-circle-fill | `fello-icon-check-circle-fill` |
| check-square | `fello-icon-check-square` |
| check-square-fill | `fello-icon-check-square-fill` |
| check-shield | `fello-icon-check-shield` |
| alert-circle | `fello-icon-alert-circle` |
| alert-circle-fill | `fello-icon-alert-circle-fill` |
| alert-triangle | `fello-icon-alert-triangle` |
| alert-triangle-fill | `fello-icon-alert-triangle-fill` |
| info | `fello-icon-info` |
| info-fill | `fello-icon-info-fill` |
| question-circle | `fello-icon-question-circle` |
| question-square | `fello-icon-question-square` |
| circle-fill | `fello-icon-circle-fill` |

### User & People
| Icon Name | CSS Class |
|-----------|-----------|
| user | `fello-icon-user` |
| users | `fello-icon-users` |
| user-add | `fello-icon-user-add` |
| user-ban | `fello-icon-user-ban` |
| user-change | `fello-icon-user-change` |
| user-circle | `fello-icon-user-circle` |
| user-edit | `fello-icon-user-edit` |
| user-favourite | `fello-icon-user-favourite` |
| user-group | `fello-icon-user-group` |
| user-group-community | `fello-icon-user-group-community` |
| user-key | `fello-icon-user-key` |
| user-lead | `fello-icon-user-lead` |
| user-profile | `fello-icon-user-profile` |
| user-profile-01 | `fello-icon-user-profile-01` |
| user-profile-card | `fello-icon-user-profile-card` |
| user-refresh | `fello-icon-user-refresh` |
| user-remove | `fello-icon-user-remove` |
| user-select | `fello-icon-user-select` |
| user-settings | `fello-icon-user-settings` |
| user-view | `fello-icon-user-view` |
| user-web-profile | `fello-icon-user-web-profile` |

### Email & Messaging
| Icon Name | CSS Class |
|-----------|-----------|
| email-check | `fello-icon-email-check` |
| email-cross | `fello-icon-email-cross` |
| email-domain | `fello-icon-email-domain` |
| email-engage | `fello-icon-email-engage` |
| email-folder | `fello-icon-email-folder` |
| email-hand-holding | `fello-icon-email-hand-holding` |
| email-home-real-estate | `fello-icon-email-home-real-estate` |
| email-list | `fello-icon-email-list` |
| email-lock | `fello-icon-email-lock` |
| email-multiple | `fello-icon-email-multiple` |
| email-received | `fello-icon-email-received` |
| email-search | `fello-icon-email-search` |
| email-sent | `fello-icon-email-sent` |
| email-single | `fello-icon-email-single` |
| email-user | `fello-icon-email-user` |
| emails-group | `fello-icon-emails-group` |
| emails-move | `fello-icon-emails-move` |
| chat-message-01 | `fello-icon-chat-message-01` |
| chat-message-02 | `fello-icon-chat-message-02` |
| send-message | `fello-icon-send-message` |
| smart-send | `fello-icon-smart-send` |
| inbox | `fello-icon-inbox` |

### File & Document
| Icon Name | CSS Class |
|-----------|-----------|
| file | `fello-icon-file` |
| file-add | `fello-icon-file-add` |
| file-blank | `fello-icon-file-blank` |
| file-certificate | `fello-icon-file-certificate` |
| file-checkmark | `fello-icon-file-checkmark` |
| file-close | `fello-icon-file-close` |
| file-contacts-user | `fello-icon-file-contacts-user` |
| file-error | `fello-icon-file-error` |
| file-export | `fello-icon-file-export` |
| file-favourite | `fello-icon-file-favourite` |
| file-import | `fello-icon-file-import` |
| file-locked | `fello-icon-file-locked` |
| file-pending | `fello-icon-file-pending` |
| file-settings | `fello-icon-file-settings` |
| file-star | `fello-icon-file-star` |
| file-view-preview | `fello-icon-file-view-preview` |
| attachment | `fello-icon-attachment` |

### Folder
| Icon Name | CSS Class |
|-----------|-----------|
| folder | `fello-icon-folder` |
| folder-add | `fello-icon-folder-add` |
| folder-check | `fello-icon-folder-check` |
| folder-close | `fello-icon-folder-close` |
| folder-list | `fello-icon-folder-list` |
| folder-minus | `fello-icon-folder-minus` |
| folder-open | `fello-icon-folder-open` |

### Calendar & Time
| Icon Name | CSS Class |
|-----------|-----------|
| calendar | `fello-icon-calendar` |
| calendar-check | `fello-icon-calendar-check` |
| calendar-clock | `fello-icon-calendar-clock` |
| calendar-clock-fill | `fello-icon-calendar-clock-fill` |
| calendar-star | `fello-icon-calendar-star` |
| calendar-video | `fello-icon-calendar-video` |
| clock | `fello-icon-clock` |
| clock-add | `fello-icon-clock-add` |
| clock-minus | `fello-icon-clock-minus` |
| clock-snooze | `fello-icon-clock-snooze` |
| hour-glass | `fello-icon-hour-glass` |

### Home & Real Estate
| Icon Name | CSS Class |
|-----------|-----------|
| home | `fello-icon-home` |
| home-02 | `fello-icon-home-02` |
| home-marketing | `fello-icon-home-marketing` |
| house | `fello-icon-house` |
| house-check | `fello-icon-house-check` |
| house-edit | `fello-icon-house-edit` |
| house-reject | `fello-icon-house-reject` |
| house-sale | `fello-icon-house-sale` |
| house-search | `fello-icon-house-search` |
| house-star | `fello-icon-house-star` |
| activity-house | `fello-icon-activity-house` |

### Location & Maps
| Icon Name | CSS Class |
|-----------|-----------|
| map | `fello-icon-map` |
| map-and-location-pin | `fello-icon-map-and-location-pin` |
| map-direction | `fello-icon-map-direction` |
| map-location-pin-off | `fello-icon-map-location-pin-off` |
| map-location-pin-only | `fello-icon-map-location-pin-only` |
| map-with-house | `fello-icon-map-with-house` |
| map-with-location-pin | `fello-icon-map-with-location-pin` |
| location-pin | `fello-icon-location-pin` |
| location-pin-off | `fello-icon-location-pin-off` |
| globe | `fello-icon-globe` |

### Navigation & Menu
| Icon Name | CSS Class |
|-----------|-----------|
| menu-burger | `fello-icon-menu-burger` |
| menu-burger-left | `fello-icon-menu-burger-left` |
| menu-burger-right | `fello-icon-menu-burger-right` |
| menu-section | `fello-icon-menu-section` |
| menu-section-alt | `fello-icon-menu-section-alt` |
| navigation-panel | `fello-icon-navigation-panel` |
| nav-left | `fello-icon-navLeft` |
| nav-right | `fello-icon-nav-right` |

### Dashboard & Analytics
| Icon Name | CSS Class |
|-----------|-----------|
| dashboard-circles | `fello-icon-dashboard-circles` |
| dashboard-meter-01 | `fello-icon-dashboard-meter-01` |
| dashboard-meter-02 | `fello-icon-dashboard-meter-02` |
| dashboard-plus-apps | `fello-icon-dashboard-plus-apps` |
| dashboard-square-round | `fello-icon-dashboard-square-round` |
| dashboard-tiles | `fello-icon-dashboard-tiles` |
| graph | `fello-icon-graph` |
| graph-bar | `fello-icon-graph-bar` |
| graph-line | `fello-icon-graph-line` |
| graph-presentation-bar | `fello-icon-graph-presentation-bar` |
| graph-presentation-line | `fello-icon-graph-presentation-line` |

### Settings & Configuration
| Icon Name | CSS Class |
|-----------|-----------|
| settings-cog-6 | `fello-icon-settings-cog-6` |
| settings-filter-horizontal | `fello-icon-settings-filter-horizontal` |
| settings-filter-vertical | `fello-icon-settings-filter-vertical` |
| settings-minimal-circle | `fello-icon-settings-minimal-circle` |

### Search & Filter
| Icon Name | CSS Class |
|-----------|-----------|
| search | `fello-icon-search` |
| search-square | `fello-icon-search-square` |
| filter | `fello-icon-filter` |
| filter-check | `fello-icon-filter-check` |
| filter-close | `fello-icon-filter-close` |
| filter-edit | `fello-icon-filter-edit` |
| filter-fill | `fello-icon-filter-fill` |

### Security & Lock
| Icon Name | CSS Class |
|-----------|-----------|
| lock-01 | `fello-icon-lock-01` |
| lock-02 | `fello-icon-lock-02` |
| lock-check | `fello-icon-lock-check` |
| lock-unlock | `fello-icon-lock-unlock` |
| key | `fello-icon-key` |
| key-owner | `fello-icon-key-owner` |

### Financial & Banking
| Icon Name | CSS Class |
|-----------|-----------|
| dollar | `fello-icon-dollar` |
| dollar-budget | `fello-icon-dollar-budget` |
| dollar-circle | `fello-icon-dollar-circle` |
| dollar-select | `fello-icon-dollar-select` |
| bank-ban | `fello-icon-bank-ban` |
| bank-check | `fello-icon-bank-check` |
| bank-money-lender | `fello-icon-bank-money-lender` |
| bank-pending | `fello-icon-bank-pending` |
| bank-user | `fello-icon-bank-user` |
| credit-icon-v2 | `fello-icon-credit-icon-v2` |
| credit-icon-v2-recurring | `fello-icon-credit-icon-v2-recurring` |

### Devices
| Icon Name | CSS Class |
|-----------|-----------|
| device-desktop | `fello-icon-device-desktop` |
| device-laptop | `fello-icon-device-laptop` |
| device-mobile | `fello-icon-device-mobile` |
| device-laptop-mobile | `fello-icon-device-laptop-mobile` |
| device-tablet | `fello-icon-device-tablet` |

### Communication & Media
| Icon Name | CSS Class |
|-----------|-----------|
| phone | `fello-icon-phone` |
| phone-cut | `fello-icon-phone-cut` |
| microphone | `fello-icon-microphone` |
| mute | `fello-icon-mute` |
| headphones | `fello-icon-headphones` |
| video-camera | `fello-icon-video-camera` |
| video-movies | `fello-icon-video-movies` |
| camera | `fello-icon-camera` |
| image | `fello-icon-image` |
| play | `fello-icon-play` |
| pause | `fello-icon-pause` |
| forward-10sec | `fello-icon-forward-10sec` |
| backward-10sec | `fello-icon-backward-10sec` |
| volume-active | `fello-icon-volume-active` |
| volume-mute | `fello-icon-volume-mute` |

### Text & Formatting
| Icon Name | CSS Class |
|-----------|-----------|
| paragraph-text | `fello-icon-paragraph-text` |
| paragraph-bold | `fello-icon-paragraph-bold` |
| paragraph-italics | `fello-icon-paragraph-italics` |
| paragraph-underline | `fello-icon-paragraph-underline` |
| paragraph-strikethrough | `fello-icon-paragraph-strikethrough` |
| paragraph-bullets | `fello-icon-paragraph-bullets` |
| paragraph-numbering | `fello-icon-paragraph-numbering` |
| paragraph-qr-code | `fello-icon-paragraph-qr-code` |
| text-align-left | `fello-icon-text-align-left` |
| text-align-center | `fello-icon-text-align-center` |
| text-align-right | `fello-icon-text-align-right` |
| text-align-justified | `fello-icon-text-align-justified` |
| text-size | `fello-icon-text-size` |
| heading | `fello-icon-heading` |

### Notifications
| Icon Name | CSS Class |
|-----------|-----------|
| notification-bell | `fello-icon-notification-bell` |
| notification-bell-off | `fello-icon-notification-bell-off` |
| notification-bell-list | `fello-icon-notification-bell-list` |
| notification-bell-with-circle | `fello-icon-notification-bell-with-circle` |

### Links & Sharing
| Icon Name | CSS Class |
|-----------|-----------|
| link | `fello-icon-link` |
| link-integration | `fello-icon-link-integration` |
| link-unlink | `fello-icon-link-unlink` |
| group-link | `fello-icon-group-link` |
| group-unlink | `fello-icon-group-unlink` |
| external-link | `fello-icon-external-link` |
| share | `fello-icon-share` |
| share-external | `fello-icon-share-external` |
| share-ios | `fello-icon-share-ios` |

### Bookmarks & Tags
| Icon Name | CSS Class |
|-----------|-----------|
| bookmark | `fello-icon-bookmark` |
| bookmark-add | `fello-icon-bookmark-add` |
| bookmark-minus | `fello-icon-bookmark-minus` |
| bookmark-multiple | `fello-icon-bookmark-multiple` |
| tag-add | `fello-icon-tag-add` |
| tag-remove | `fello-icon-tag-remove` |
| pin | `fello-icon-pin` |
| pin-off | `fello-icon-pin-off` |
| pin-fill | `fello-icon-pin-fill` |

### Social Media
| Icon Name | CSS Class |
|-----------|-----------|
| social-facebook | `fello-icon-social-facebook` |
| social-linkedin | `fello-icon-social-linkedin` |
| social-x | `fello-icon-social-x` |
| social-instagram | `fello-icon-social-instagram` |
| airbnb | `fello-icon-airbnb` |

### Grid, Table & Layout
| Icon Name | CSS Class |
|-----------|-----------|
| grid-column-one | `fello-icon-grid-column-one` |
| grid-column-two | `fello-icon-grid-column-two` |
| grid-column-three | `fello-icon-grid-column-three` |
| grid-row-three | `fello-icon-grid-row-three` |
| table | `fello-icon-table` |
| table-settings | `fello-icon-table-settings` |
| row-basic | `fello-icon-row-basic` |
| row-with-child | `fello-icon-row-with-child` |
| kanban-view | `fello-icon-kanban-view` |
| list-view | `fello-icon-list-view` |

### Actions & Editing
| Icon Name | CSS Class |
|-----------|-----------|
| edit | `fello-icon-edit` |
| edit-pencil | `fello-icon-edit-pencil` |
| copy | `fello-icon-copy` |
| print | `fello-icon-print` |
| save-floppy | `fello-icon-save-floppy` |
| delete-trash | `fello-icon-delete-trash` |
| delete-trash-remove | `fello-icon-delete-trash-remove` |
| archive | `fello-icon-archive` |
| archive-off | `fello-icon-archive-off` |
| download | `fello-icon-download` |
| upload | `fello-icon-upload` |
| refresh | `fello-icon-refresh` |
| refresh-off | `fello-icon-refresh-off` |
| reload-rotate-left | `fello-icon-reload-rotate-left` |
| reload-rotate-right | `fello-icon-reload-rotate-right` |
| rotate-left | `fello-icon-rotate-left` |
| rotate-right | `fello-icon-rotate-right` |
| transfer | `fello-icon-transfer` |
| sort-down | `fello-icon-sort-down` |
| sort-up | `fello-icon-sort-up` |
| shuffle | `fello-icon-shuffle` |
| dots-option-horizontal | `fello-icon-dots-option-horizontal` |
| dots-option-vertical | `fello-icon-dots-option-vertical` |
| draggable-v4 | `fello-icon-draggable-v4` |
| draggable-v4-horizontal | `fello-icon-draggable-v4-horizontal` |

### Clipboard
| Icon Name | CSS Class |
|-----------|-----------|
| clipboard | `fello-icon-clipboard` |
| clipboard-check | `fello-icon-clipboard-check` |
| clipboard-list | `fello-icon-clipboard-list` |

### Tools & Utility
| Icon Name | CSS Class |
|-----------|-----------|
| tool-wrench | `fello-icon-tool-wrench` |
| tools-pencil-ruler-drawing | `fello-icon-tools-pencil-ruler-drawing` |
| tools-wench-ruler | `fello-icon-tools-wench-ruler` |
| resize-diagonal-01 | `fello-icon-resize-diagonal-01` |
| resize-diagonal-02 | `fello-icon-resize-diagonal-02` |
| resize-horizontal-01 | `fello-icon-resize-horizontal-01` |
| resize-horizontal-02 | `fello-icon-resize-horizontal-02` |
| resize-horizontal-03 | `fello-icon-resize-horizontal-03` |
| cursor-button-action | `fello-icon-cursor-button-action` |
| cursor-button-action-02 | `fello-icon-cursor-button-action-02` |
| zoom-in | `fello-icon-zoom-in` |
| zoom-out | `fello-icon-zoom-out` |
| zoom-close | `fello-icon-zoom-close` |

### Stars, Hearts & Favorites
| Icon Name | CSS Class |
|-----------|-----------|
| heart | `fello-icon-heart` |
| heart-fill | `fello-icon-heart-fill` |
| star-round | `fello-icon-star-round` |
| star-sharp | `fello-icon-star-sharp` |
| star-sharp-circle | `fello-icon-star-sharp-circle` |
| star-enrich | `fello-icon-star-enrich` |
| thumbs-up | `fello-icon-thumbs-up` |
| thumbs-up-fill | `fello-icon-thumbs-up-fill` |
| thumbs-down | `fello-icon-thumbs-down` |
| thumb | `fello-icon-thumb` |
| award | `fello-icon-award` |
| certificate-medal | `fello-icon-certificate-medal` |

### Auth & Access
| Icon Name | CSS Class |
|-----------|-----------|
| login-signin | `fello-icon-login-signin` |
| login-signin-alt | `fello-icon-login-signin-alt` |
| logout-signout | `fello-icon-logout-signout` |
| logout-signout-alt | `fello-icon-logout-signout-alt` |
| shut-down | `fello-icon-shut-down` |

### Marketing & CRM
| Icon Name | CSS Class |
|-----------|-----------|
| marketing | `fello-icon-marketing` |
| marketing-alt | `fello-icon-marketing-alt` |
| marketing-off | `fello-icon-marketing-off` |
| lead-funnel | `fello-icon-lead-funnel` |
| segment-circles | `fello-icon-segment-circles` |
| segment-icon | `fello-icon-segment-icon` |
| automation | `fello-icon-automation` |
| automation-off | `fello-icon-automation-off` |
| automate | `fello-icon-automate` |
| workflows | `fello-icon-workflows` |
| postcard | `fello-icon-postcard` |
| landing-pages | `fello-icon-landing-pages` |
| form | `fello-icon-form` |
| affiliate-handshake | `fello-icon-affiliateHandshake` |
| referred-fill | `fello-icon-referred-fill` |
| unmark-as-referred | `fello-icon-unmark-as-referred` |

### Data & Database
| Icon Name | CSS Class |
|-----------|-----------|
| database-default | `fello-icon-database-default` |
| database-attributes | `fello-icon-database-attributes` |
| datasets | `fello-icon-datasets` |
| hash-number | `fello-icon-hashNumber` |
| multi-select | `fello-icon-multiSelect` |
| single-select | `fello-icon-singleSelect` |
| percentage | `fello-icon-percentage` |

### Misc
| Icon Name | CSS Class |
|-----------|-----------|
| activity-list | `fello-icon-activity-list` |
| bluetooth | `fello-icon-bluetooth` |
| branch | `fello-icon-branch` |
| branch-arrow | `fello-icon-branch-arrow` |
| bulb | `fello-icon-bulb` |
| car | `fello-icon-car` |
| code | `fello-icon-code` |
| code-circle | `fello-icon-code-circle` |
| copyright | `fello-icon-copyright` |
| emoji-happy | `fello-icon-emoji-happy` |
| eye | `fello-icon-eye` |
| eye-remove | `fello-icon-eye-remove` |
| view-preview-details | `fello-icon-view-preview-details` |
| cloud-check | `fello-icon-cloud-check` |
| cloud-close | `fello-icon-cloud-close` |
| cloud-upload | `fello-icon-cloud-upload` |
| hierarchy | `fello-icon-hierarchy` |
| layers | `fello-icon-layers` |
| layers-connected | `fello-icon-layers-connected` |
| lightening-thunder | `fello-icon-lightening-thunder` |
| loader | `fello-icon-loader` |
| palette-color-theme | `fello-icon-palette-color-theme` |
| puzzle-widget | `fello-icon-puzzleWidget` |
| rocket | `fello-icon-rocket` |
| shopping-bag | `fello-icon-shopping-bag` |
| sticker-notes | `fello-icon-stickerNotes` |
| swatch-rectangle | `fello-icon-swatch-rectangle` |
| wi-fi | `fello-icon-wi-fi` |
| wi-fi-remove | `fello-icon-wi-fi-remove` |
| re-scan | `fello-icon-reScan` |
| past-property | `fello-icon-pastProperty` |
| fello-logo | `fello-icon-fello-logo` |

### Page Sections (Editor)
| Icon Name | CSS Class |
|-----------|-----------|
| header-section | `fello-icon-header-section` |
| hero-section | `fello-icon-hero-section` |
| footer-section | `fello-icon-footer-section` |

---

## Quick Lookup — Common UI Needs

Use this table to quickly find the right Fello icon for common UI patterns:

| UI Need | Fello Icon | CSS Class |
|---------|-----------|-----------|
| Home / Dashboard | home | `fello-icon-home` |
| Settings | settings-cog-6 | `fello-icon-settings-cog-6` |
| Search | search | `fello-icon-search` |
| Close / Dismiss | close | `fello-icon-close` |
| Back arrow | chevron-arrow-left | `fello-icon-chevron-arrow-left` |
| Forward arrow | chevron-arrow-right | `fello-icon-chevron-arrow-right` |
| Add / Create | add | `fello-icon-add` |
| Delete / Remove | delete-trash | `fello-icon-delete-trash` |
| Edit / Modify | edit-pencil | `fello-icon-edit-pencil` |
| Save | save-floppy | `fello-icon-save-floppy` |
| Download | download | `fello-icon-download` |
| Upload | upload | `fello-icon-upload` |
| Filter | filter | `fello-icon-filter` |
| Sort ascending | sort-up | `fello-icon-sort-up` |
| Sort descending | sort-down | `fello-icon-sort-down` |
| More options (horizontal) | dots-option-horizontal | `fello-icon-dots-option-horizontal` |
| More options (vertical) | dots-option-vertical | `fello-icon-dots-option-vertical` |
| User / Profile | user | `fello-icon-user` |
| Users / Team | users | `fello-icon-users` |
| Notification | notification-bell | `fello-icon-notification-bell` |
| Calendar | calendar | `fello-icon-calendar` |
| Email / Mail | email-single | `fello-icon-email-single` |
| Phone | phone | `fello-icon-phone` |
| Link | link | `fello-icon-link` |
| External link | external-link | `fello-icon-external-link` |
| Copy | copy | `fello-icon-copy` |
| Share | share | `fello-icon-share` |
| Lock | lock-01 | `fello-icon-lock-01` |
| Unlock | lock-unlock | `fello-icon-lock-unlock` |
| Eye / View | eye | `fello-icon-eye` |
| Eye off / Hide | eye-remove | `fello-icon-eye-remove` |
| Info | info | `fello-icon-info` |
| Warning | alert-triangle | `fello-icon-alert-triangle` |
| Error | alert-circle | `fello-icon-alert-circle` |
| Success | check-circle | `fello-icon-check-circle` |
| Star / Favorite | star-round | `fello-icon-star-round` |
| Heart / Like | heart | `fello-icon-heart` |
| Refresh | refresh | `fello-icon-refresh` |
| Image | image | `fello-icon-image` |
| Attach file | attachment | `fello-icon-attachment` |
| Bookmark | bookmark | `fello-icon-bookmark` |
| Pin | pin | `fello-icon-pin` |
| Drag handle | draggable-v4 | `fello-icon-draggable-v4` |
| Expand | chevron-arrow-down | `fello-icon-chevron-arrow-down` |
| Collapse | chevron-arrow-up | `fello-icon-chevron-arrow-up` |
| Leads / Funnel | lead-funnel | `fello-icon-lead-funnel` |
| Database | database-default | `fello-icon-database-default` |
| Marketing | marketing | `fello-icon-marketing` |
| Workflows | workflows | `fello-icon-workflows` |
| App store / Marketplace | dashboard-plus-apps | `fello-icon-dashboard-plus-apps` |
| Logout | logout-signout | `fello-icon-logout-signout` |
