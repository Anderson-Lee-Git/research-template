# App Design Reference (from Apple's HIG)

Applies to every app in `apps/` (the review tool, the related-work gallery). Each app's own spec lives in
`docs/<app>/SPEC.md`; where this document says "the spec", read that app's spec. Guidance about controls an
app does not have (for example verdicts, tags or edits in a read-only app) does not apply to it.

Source: Apple Human Interface Guidelines, retrieved 2026-10-07, paraphrased. HIG points (pt) are read as CSS px; both are abstract units ([images](https://developer.apple.com/design/human-interface-guidelines/images)). Tags: **HIG** = Apple's rule. **Web** = our browser translation. **Team default** = a value the HIG doesn't set; the closest HIG page is cited.

---

## 1. How to use this document

1. Read §2 first. Settle design trade-offs with these principles.
2. Build design tokens (color, type, spacing, motion) and copy rules only from §3.
3. Before using a pattern or control, check its entry in §4–§5.
4. Run §6 in every design review and PR review. An unchecked item blocks the change until it is fixed or explicitly waived.
5. If this document and the linked HIG page disagree, the HIG page wins. For web-only questions, follow the **Web** and **Team default** notes.

---

## 2. Core principles

> The current [design-principles](https://developer.apple.com/design/human-interface-guidelines/design-principles) page names eight principles: Purpose, Agency, Responsibility, Familiarity, Flexibility, Simplicity, Craft and Delight. Hierarchy and consistency survive inside them, as "Establish hierarchy" under Simplicity and "Keep visuals and interactions consistent" under Familiarity. Harmony is no longer a named principle.

1. **Go straight to the work.** The HIG says good design should "stay out of the way". The app opens on the next unreviewed item with no intermediate screens. An extra click per item adds up to thousands per session. ([design-principles](https://developer.apple.com/design/human-interface-guidelines/design-principles))
2. **Build in forgiveness.** Recovering from a mistake shouldn't cost time or work. Every verdict, tag, keep/drop and edit is undoable, and nothing needs a Save. A wrong keypress should cost one Cmd/Ctrl-Z. ([design-principles](https://developer.apple.com/design/human-interface-guidelines/design-principles), [undo-and-redo](https://developer.apple.com/design/human-interface-guidelines/undo-and-redo), [file-management](https://developer.apple.com/design/human-interface-guidelines/file-management))
3. **Scale feedback to importance.** Show routine status passively, next to its object. Interrupt only to prevent an unexpected, irreversible loss. Frequent interruptions wear down focus over long sessions. ([feedback](https://developer.apple.com/design/human-interface-guidelines/feedback))
4. **Be consistent.** Once a behavior or appearance is set, use it everywhere, and keep standard shortcuts. Reviewers work from muscle memory, so inconsistencies turn into errors. ([design-principles](https://developer.apple.com/design/human-interface-guidelines/design-principles), [keyboards](https://developer.apple.com/design/human-interface-guidelines/keyboards))
5. **Establish hierarchy.** Put the most important content at the top and leading edge. Show structure with alignment, indentation and grouping. Photo, request and grading description always appear in the same order. ([layout](https://developer.apple.com/design/human-interface-guidelines/layout))
6. **Keyboard first, every input supported.** Design for a variety of inputs, and make the whole UI work from the keyboard alone. Annotators work by keyboard, and tablets need touch. ([design-principles](https://developer.apple.com/design/human-interface-guidelines/design-principles), [accessibility](https://developer.apple.com/design/human-interface-guidelines/accessibility))
7. **Keep the tone calm.** The HIG asks you to identify the emotion you want to evoke and not to "mistake delight for decoration". Our target is calm focus: neutral surfaces, little color, minimal motion and neutral wording. The content is distressing, so the UI must not add stress. ([design-principles](https://developer.apple.com/design/human-interface-guidelines/design-principles), [writing](https://developer.apple.com/design/human-interface-guidelines/writing))
8. **Minimize modality.** On large displays, show more in fewer levels with fewer modal views. The audit form stays in place and never opens in a dialog. ([designing-for-macos](https://developer.apple.com/design/human-interface-guidelines/designing-for-macos), [modality](https://developer.apple.com/design/human-interface-guidelines/modality))
9. **Never rely on color alone.** Verdicts and states must be readable without color, in both light and dark mode. ([accessibility](https://developer.apple.com/design/human-interface-guidelines/accessibility), [color](https://developer.apple.com/design/human-interface-guidelines/color))
10. **Progressive disclosure.** Show what every review needs. Put rare options behind disclosure controls. ([layout](https://developer.apple.com/design/human-interface-guidelines/layout), [disclosure-controls](https://developer.apple.com/design/human-interface-guidelines/disclosure-controls))
11. **Preserve context.** Keep the selection highlighted and controls in stable positions. On reload, restore the item, scroll position and expanded groups. ([design-principles](https://developer.apple.com/design/human-interface-guidelines/design-principles), [launching](https://developer.apple.com/design/human-interface-guidelines/launching), [split-views](https://developer.apple.com/design/human-interface-guidelines/split-views))
12. **Act responsibly with data.** Be transparent about what is recorded, and protect its integrity. The judgments are the research output. ([design-principles](https://developer.apple.com/design/human-interface-guidelines/design-principles))

---

## 3. Foundations

### 3.1 Accessibility — [accessibility](https://developer.apple.com/design/human-interface-guidelines/accessibility), [voiceover](https://developer.apple.com/design/human-interface-guidelines/voiceover)

- **HIG contrast (WCAG AA):** text up to 17pt needs **≥ 4.5:1**. Text 18pt or larger, or bold, needs **≥ 3:1**. Check light and dark.
- **HIG color:** never signal with color alone; add shape, icon or text. Red–green and blue–orange pairs are the riskiest.
- **HIG text size:** text must scale to at least **200%**. **Web:** use `rem` units; at 200% zoom nothing clips and the content pane doesn't scroll sideways.
- **HIG targets:** macOS controls are 28×28 by default (minimum 20×20). iPadOS controls are 44×44 (minimum 28×28). Leave about 12pt around bezeled controls and 24pt around unbezeled ones. **Web:** 28px controls for `(pointer: fine)`, 44px hit areas for `(pointer: coarse)`.
- **HIG keyboard:** everything works from the keyboard alone (Full Keyboard Access). Don't override system shortcuts.
- **HIG gestures:** every swipe or drag needs an on-screen alternative.
- **HIG timing:** minimize anything that dismisses itself on a timer. A disappearing "Undo" toast can't be the only way to undo.
- **HIG motion:** with Reduce Motion on, remove zoom, scale and peripheral motion, use fades instead of slides, and don't animate blur.
- **HIG VoiceOver:** label every control, describe meaningful images and hide decorative ones. Give each view a unique title and real headings. Expose grouping. Announce content changes.
- **Web for VoiceOver:** `aria-label`, landmarks, `h1`–`h3`, `role="group"`, an `aria-live="polite"` region for "Saved" and "Item 41 of 300", and a per-view `<title>`.
- **Web system-setting mapping:**
  - Increase Contrast → `prefers-contrast: more`
  - Reduce Motion → `prefers-reduced-motion`
  - Reduce Transparency → `prefers-reduced-transparency`
  - Dark Mode → `prefers-color-scheme`

### 3.2 Color — [color](https://developer.apple.com/design/human-interface-guidelines/color)

- **HIG:** one color, one meaning. If the accent color means "interactive", don't also use it for static text.
- **HIG:** name colors by purpose, never reuse one for another purpose (no separator color for text), and don't hard-code values. **Web:** use CSS custom properties modeled on the macOS dynamic colors. Each has a light, dark and increased-contrast value.

| Token | Purpose (HIG macOS dynamic color) |
|---|---|
| `--label`, `--label-2`, `--label-3`, `--label-4` | Primary text, then supplementary, unavailable and watermark text ([labels](https://developer.apple.com/design/human-interface-guidelines/labels)) |
| `--bg-window`, `--bg-content`, `--bg-content-alt` | Window background, list/table background, alternating rows |
| `--separator`, `--grid` | Section dividers, table gridlines |
| `--accent` | Interactive elements, primary action |
| `--sel-bg`, `--sel-bg-unemph`, `--sel-text` | Selection in the focused pane, selection in unfocused panes, text on selection |
| `--focus-ring`, `--find-highlight` | Keyboard focus, search matches |
| `--destructive` | Destructive actions (system red, per [buttons](https://developer.apple.com/design/human-interface-guidelines/buttons)) |

- **HIG:** use color sparingly, for status and primary actions. Put emphasis color on a control's background, and never tint several controls at once.
- **HIG:** color meanings vary by culture. Pair every verdict color with an icon and a word.

### 3.3 Dark Mode — [dark-mode](https://developer.apple.com/design/human-interface-guidelines/dark-mode)

- **HIG:** follow the system appearance. Apple advises *against* an app-specific appearance setting. **Web:** follow `prefers-color-scheme`. If you add a manual override, record it as a deliberate deviation.
- **HIG:** dark palettes are not inversions. Backgrounds get dimmer and foregrounds brighter.
- **HIG:** contrast never drops below 4.5:1. Aim for **7:1** on custom colors, especially small text.
- **HIG:** content images with white backgrounds glare in dark mode, so dim them slightly. Only do this if it doesn't change what's being judged.
- **HIG:** test with Increase Contrast and Reduce Transparency, separately and together.

### 3.4 Typography — [typography](https://developer.apple.com/design/human-interface-guidelines/typography)

- **HIG:** use system fonts; don't embed them. **Web:** `system-ui, -apple-system, "Segoe UI", Roboto, sans-serif`, plus a monospace stack for IDs. **Team default:** these two families only.
- **HIG:** use Regular, Medium, Semibold or Bold. Avoid Ultralight, Thin and Light.
- **HIG:** build hierarchy with size, weight and color, and keep it when text scales up. Use looser leading for long passages and tighter leading in constrained rows.
- **HIG:** keep truncation to a minimum. When text grows, stack elements rather than truncate.
- **HIG sizes:** macOS defaults to 13pt (minimum 10pt). iPadOS defaults to 17pt (minimum 11pt), using the
  Dynamic Type "Large" scale. **Team default:** follow the iPadOS scale, because reviewers read for
  hours and need comfortable text, and nothing smaller than **14px** anywhere.

| HIG iPadOS style, Dynamic Type Large (size/line, pt) | Web token (Team default) | Use |
|---|---|---|
| Large Title 34/41 | `--t-display` 34/41 | Empty states, rare emphasis |
| Title 1 28/34 | `--t-title1` 28/34 | View titles |
| Title 2 22/28 | `--t-title2` 22/28 semibold | Pane headers |
| Title 3 20/25 | `--t-title3` 20/25 semibold | Section headers |
| Body 17/22 | `--t-content` 17/26 | Reading text: requests, grading descriptions, scenarios, notes |
| Callout 16/21 | `--t-ui` 16/22 | Controls, table rows, labels |
| Subheadline 15/20 | `--t-secondary` 15/20 | Metadata |
| Footnote 13/18 | `--t-caption` 14/18 (raised to the 14px floor) | Captions, counts |

- **Line length:** the HIG points to layout guides that "restrict the width of text for optimal readability" but gives no number ([layout](https://developer.apple.com/design/human-interface-guidelines/layout)). **Team default:** `max-width: 70ch`.

### 3.5 Layout and spacing — [layout](https://developer.apple.com/design/human-interface-guidelines/layout)

- **HIG:** order content by importance, starting top and leading. Align elements so they scan easily; indent to show subordination. Group related items with space, containers or separators.
- **HIG:** keep controls visibly distinct from content. **Web:** Liquid Glass doesn't apply; use solid, slightly tinted toolbar and sidebar surfaces.
- **HIG:** base layout on the space available (size classes), not on the device or orientation. Keep the same features at every size; only how much is visible changes. **Web:** use width breakpoints or container queries, never user-agent sniffing.
- **Spacing:** the HIG has no spacing scale beyond the 12/24pt padding in §3.1. **Team default:** a 4px base (4/8/12/16/24/32/48). Space inside a group is always smaller than space between groups.
- **Adaptivity ([split-views](https://developer.apple.com/design/human-interface-guidelines/split-views), [sidebars](https://developer.apple.com/design/human-interface-guidelines/sidebars)):** desktop shows sidebar | list | detail. On tablet landscape the sidebar collapses. On tablet portrait, list and detail alternate.

### 3.6 Motion — [motion](https://developer.apple.com/design/human-interface-guidelines/motion)

- **HIG:** motion needs a purpose and never carries information alone. Keep feedback short and precise.
- **HIG:** avoid motion on frequent interactions. Moving between items is the most frequent action here, so it should be instant.
- **HIG:** never make people wait for an animation to finish.
- **Team default:**
  - Opacity and color transitions only, 150ms or less.
  - No slides, bounce or parallax.
  - No transitions at all under `prefers-reduced-motion`. Spinners still run.

### 3.7 Writing — [writing](https://developer.apple.com/design/human-interface-guidelines/writing)

- **HIG:** keep a glossary and use one term per concept. **Team default:** use the glossary in the app's spec exactly (§3 of [review_app/SPEC.md](../review_app/SPEC.md#3-glossary), [gallery_app/SPEC.md](../gallery_app/SPEC.md#3-glossary)). No synonyms.
- **HIG:** label buttons with verbs ("Keep Group"). Avoid "Click here" and cute labels.
- **HIG:** use few possessives and no "we". Write "Unable to load item".
- **HIG:** show errors next to the problem, never blame, and say how to fix it. No "Oops".
- **HIG:** every empty state names a next step, with a button if possible.
- **HIG:** pick a capitalization style per element type and use it consistently. Component rules:

| Element | Style | Source |
|---|---|---|
| Buttons, menu items, tab and segment labels, column headings, panel titles | Title-style ("Add Issue Tag") | [buttons](https://developer.apple.com/design/human-interface-guidelines/buttons), [menus](https://developer.apple.com/design/human-interface-guidelines/menus), [tab-views](https://developer.apple.com/design/human-interface-guidelines/tab-views), [segmented-controls](https://developer.apple.com/design/human-interface-guidelines/segmented-controls), [lists-and-tables](https://developer.apple.com/design/human-interface-guidelines/lists-and-tables), [panels](https://developer.apple.com/design/human-interface-guidelines/panels) |
| Alert title | Title-style for a fragment (no period); sentence-style with punctuation for a full sentence | [alerts](https://developer.apple.com/design/human-interface-guidelines/alerts) |
| Alert body, tooltips, group-box titles | Sentence-style | [alerts](https://developer.apple.com/design/human-interface-guidelines/alerts), [offering-help](https://developer.apple.com/design/human-interface-guidelines/offering-help), [boxes](https://developer.apple.com/design/human-interface-guidelines/boxes) |

- **HIG:** an ellipsis ("Export…") means more input follows. Column headings take no punctuation. Use device-appropriate verbs (click vs. tap), or neutral ones such as "choose".
- **Tone:** describe harmful requests literally and neutrally. No humor, idioms or undefined jargon ([inclusion](https://developer.apple.com/design/human-interface-guidelines/inclusion)).

### 3.8 Images and icons — [images](https://developer.apple.com/design/human-interface-guidelines/images), [icons](https://developer.apple.com/design/human-interface-guidelines/icons)

- **HIG:** ship @1x and @2x rasters. Use JPEG or HEIC for photos and SVG or PDF for icons. Embed a color profile; sRGB is safe. **Web:** `srcset` plus SVG icons.
- **HIG:** all icons share size, stroke weight and detail level, match the weight of nearby text, and have text alternatives.
- **HIG standard icons:** undo `arrow.uturn.backward`, redo `arrow.uturn.forward`, delete `trash`, add `plus`, more `ellipsis`, done `checkmark`. **Web:** SF Symbols are an Apple-platform asset. Use an open icon set with the same metaphors.
- **HIG:** avoid text over photos. Item photos get no overlays.

### 3.9 Inclusion — [inclusion](https://developer.apple.com/design/human-interface-guidelines/inclusion)

- **HIG:** use plain language. Say "you", not "the user". Define specialist terms (for example, a glossary popover for the taxonomy). Avoid gendered wording.
- **HIG:** give people a clear path to learn the app over time.

---

## 4. Patterns

**Entering data** — [entering-data](https://developer.apple.com/design/human-interface-guidelines/entering-data)
- Offer choices instead of typing: verdict and tags are selections. Free text only for the note and the rewrite.
- Prefill sensible defaults; the rewrite field starts with the original text.
- Validate as people go. If a verdict is required, keep "Next" unavailable and say why.

**Feedback** — [feedback](https://developer.apple.com/design/human-interface-guidelines/feedback)
- Show status next to its object: a "Saved" state on the item, progress counts in the toolbar.
- Use alerts only for critical, actionable issues. Don't warn when data loss is the expected result.
- Confirm only significant completions, such as finishing a batch. If a command can't run, say why.

**Loading** — [loading](https://developer.apple.com/design/human-interface-guidelines/loading)
- Show placeholders immediately and keep the rest of the UI usable.
- Prefetch the next items in the background.
- Use determinate progress whenever the total is known.

**Managing content and selection** — [focus-and-selection](https://developer.apple.com/design/human-interface-guidelines/focus-and-selection)
- Highlight the full row for selection in lists. Use focus rings only for text fields.
- Selection in the focused pane uses the accent color. Selection in other panes uses neutral gray.
- Tab moves between focus groups (panes) and arrow keys move within one. **Web:** a roving `tabindex` per pane.
- Never move focus without user action. Exception: if the focused item disappears, focus its nearest neighbor.

**Modality** — [modality](https://developer.apple.com/design/human-interface-guidelines/modality)
- Use a modal only when it clearly helps. Keep it short, title it with its task, and make dismissal obvious.
- One modal at a time.
- Confirm before closing a modal that holds unsaved input.

**Searching and filtering** — [searching](https://developer.apple.com/design/human-interface-guidelines/searching), [search-fields](https://developer.apple.com/design/human-interface-guidelines/search-fields)
- Put search in one place, the toolbar. Search as people type.
- Always show the current scope ("Unreviewed in Batch 3").
- Use a scope bar for fixed categories (All / Unreviewed / Flagged), defaulting to the broadest.
- Use tokens with suggestions for attribute filters (verdict, tag).

**Undo and redo** — [undo-and-redo](https://developer.apple.com/design/human-interface-guidelines/undo-and-redo)
- Undo has no artificial depth limit within a session. Use Cmd/Ctrl-Z and Shift-Cmd/Ctrl-Z.
- Name the action ("Undo Verdict", "Undo Keep Group").
- If the change is off-screen, navigate to it and highlight it.
- Consider a batch "Revert Item".
- Add on-screen undo/redo buttons only if needed. If you do, put them in the toolbar with the standard icons.

**Saving and restoring state** — [file-management](https://developer.apple.com/design/human-interface-guidelines/file-management), [launching](https://developer.apple.com/design/human-interface-guidelines/launching)
- Auto-save continuously; there is no Save button.
- If changes can be pending (offline), show it and prevent silent loss on close. **Web:** use `beforeunload` only then.
- Restore the item, scroll position, filters and expansions on return.

**Drag and drop** (moving items between groups) — [drag-and-drop](https://developer.apple.com/design/human-interface-guidelines/drag-and-drop)
- Always offer a non-drag alternative: a "Move to Group…" command with a shortcut.
- Highlight a valid target only while the item hovers over it. Show a count badge for multi-item drags.
- Animate failed drops back to their source. Every drop is undoable.

**Onboarding and help** — [onboarding](https://developer.apple.com/design/human-interface-guidelines/onboarding), [offering-help](https://developer.apple.com/design/human-interface-guidelines/offering-help)
- Prefer contextual tips to an up-front tour. A tip is one or two sentences, for features of three steps or fewer.
- Any tutorial is optional and can be reopened from Help.
- Every icon-only control has a tooltip: starts with a verb, sentence case, **60–75 characters at most**, doesn't repeat the control's name.
- **Web:** `?` opens the shortcut reference.

**Going full screen / focus mode** — [going-full-screen](https://developer.apple.com/design/human-interface-guidelines/going-full-screen)
- Offer a distraction-free audit mode that hides the sidebar but keeps the essentials: verdict, previous/next, progress.
- People choose when to enter and exit it; it never exits on its own.
- **Web:** call the Fullscreen API only from a user gesture. The browser reserves Esc to exit fullscreen, so also bind cancel to Cmd/Ctrl-.

**Keyboard shortcuts and commands** — [keyboards](https://developer.apple.com/design/human-interface-guidelines/keyboards), [the-menu-bar](https://developer.apple.com/design/human-interface-guidelines/the-menu-bar)
- **HIG:** keep standard meanings:
  - ⌘Z undo, ⇧⌘Z redo
  - ⌘F find, ⌘A select all
  - Esc and ⌘. cancel
  - ⌘, settings, ⌘? help
  - Tab / ⇧Tab move between controls
- **HIG:** don't add a modifier to an existing shortcut to make an unrelated command.
- **HIG:** custom shortcuts are only for the most frequent commands. Command is the main modifier, Shift secondary, Option rare, Control avoided. List modifiers as Control, Option, Shift, Command.
- **Web:** use Ctrl in place of ⌘ on Windows and Linux. Never bind browser-reserved keys (Cmd/Ctrl-W/T/N/L/R/Q, Ctrl-Tab).
- **Web single-key bindings:** the HIG mentions single-key bindings only for games. We use them for verdicts (1–n), tags and J/K navigation:
  - active only when focus is outside a text field
  - Esc leaves a field
  - every binding is listed in its tooltip and in the `?` sheet
- **HIG → Web, menu bar:** every toolbar command also appears elsewhere. Provide a command menu or palette with every command and its shortcut. Unavailable commands appear dimmed, not hidden.

**Settings** — [settings](https://developer.apple.com/design/human-interface-guidelines/settings)
- Keep settings few; open them with ⌘/Ctrl-,.
- Task options (filters, sort) live in their view.
- Never duplicate system settings such as appearance or text size.

---

## 5. Components

**Split view** (the main frame) — [split-views](https://developer.apple.com/design/human-interface-guidelines/split-views)
- Sidebar (queues) | list (groups or items) | detail (audit).
- Keep the selection highlighted in every pane on the path to the detail.
- 1px dividers, resizable within min/max limits so a divider never disappears.
- Panes can be hidden from a toolbar button, a command and a shortcut.

**Sidebar** — [sidebars](https://developer.apple.com/design/human-interface-guidelines/sidebars)
- Holds top-level areas (Group Review, Item Audit) and saved queues.
- Two levels at most, short labels, consistent icons.
- Collapsible; collapses automatically when narrow.
- No critical actions at the bottom.

**Tab bar / tab view** — [tab-bars](https://developer.apple.com/design/human-interface-guidelines/tab-bars), [tab-views](https://developer.apple.com/design/human-interface-guidelines/tab-views)
- Tabs navigate; they never perform actions.
- Six tabs at most, with title-style noun labels.
- Never hide or disable a tab. An empty tab explains why it's empty.
- **Web:** on desktop the sidebar replaces a tab bar. Use tabs inside the detail pane only (for example, Item / History), with `role="tablist"` and arrow keys.

**Toolbar** — [toolbars](https://developer.apple.com/design/human-interface-guidelines/toolbars)
- Leading: sidebar toggle, then a title under 15 characters that is not the app name.
- Center: common actions.
- Trailing: search, inspector toggle, and **one** prominent primary action.
- Three groups at most. Icon-only buttons only for familiar metaphors, spaced apart from text buttons. Decide which items move into "More" when the window narrows.

**Lists and tables** — [lists-and-tables](https://developer.apple.com/design/human-interface-guidelines/lists-and-tables)
- Use tables for text.
- Column headings are title-style nouns with no punctuation.
- Click a header to sort; click again to reverse.
- Columns are resizable. Alternate row colors in wide tables.
- Truncate IDs in the middle. Keep row text short; the detail pane shows the full text.
- **Web:** a native `<table>` or `role="grid"`, with `aria-selected` on the selected row.

**Outline view** (groups → member items, for merge comparison) — [outline-views](https://developer.apple.com/design/human-interface-guidelines/outline-views)
- Show hierarchy in the first column only; attributes go in the other columns.
- Disclosure triangles expand rows; Alt/Option-click expands all.
- Remember expansion state.

**Collection** — [collections](https://developer.apple.com/design/human-interface-guidelines/collections)
- Only for photo-first browsing, in a standard grid.
- Leave enough padding that focus and hover states stay visible.

**Buttons** — [buttons](https://developer.apple.com/design/human-interface-guidelines/buttons)
- One or two prominent buttons per view at most.
- Signal the preferred option with style, not size.
- Custom buttons need hover, pressed, focus and disabled states.
- Use an ellipsis when a button opens another view. Return activates the primary button in a dialog.
- **A destructive action is never the primary button.**

**Toggles, checkboxes, radio buttons** — [toggles](https://developer.apple.com/design/human-interface-guidelines/toggles)
- Checkboxes for issue tags, including a mixed state for a partially checked parent.
- Radio buttons for 2–5 exclusive options. Beyond that, use a pop-up button.
- Switches only for emphasized settings.
- States differ in shape, not only color.
- Keep these out of toolbars.

**Segmented control** (verdict picker) — [segmented-controls](https://developer.apple.com/design/human-interface-guidelines/segmented-controls)
- One choice from a small fixed set: 7 segments at most, equal widths, title-style text labels, no mix of icons and text.
- Never mix action segments with selection segments. Don't use it to switch views in the main area.
- **Web:** `role="radiogroup"` with arrow keys. Show each segment's number key as a hint.

**Menus, context menus, pop-up and pull-down buttons** — [menus](https://developer.apple.com/design/human-interface-guidelines/menus), [context-menus](https://developer.apple.com/design/human-interface-guidelines/context-menus), [pop-up-buttons](https://developer.apple.com/design/human-interface-guidelines/pop-up-buttons), [pull-down-buttons](https://developer.apple.com/design/human-interface-guidelines/pull-down-buttons)
- **Menu items:** title-style verbs without articles, frequent items first, grouped with separators. Submenus one level deep with about five items at most.
- **Unavailable items:** dimmed in regular menus, hidden in context menus.
- **Context menus:** relevant actions only, about three groups at most, no shortcut hints. Every action also exists in the main UI.
- **Pop-up button:** a flat list of exclusive options (such as sort order) with a sensible default.
- **Pull-down button:** three or more related actions. Never hide a view's primary actions in one.
- **Web:** `role="menu"`, arrow keys, Esc closes, focus returns to the trigger.

**Text fields and text views** — [text-fields](https://developer.apple.com/design/human-interface-guidelines/text-fields), [text-views](https://developer.apple.com/design/human-interface-guidelines/text-views)
- Text fields for short input; text views for the note and the rewrite.
- Every field has a visible label. Placeholder text is a hint, not a label.
- Size fields to their expected content, and keep a logical tab order.
- Make useful read-only text selectable (IDs, request text).

**Token field** (optional, for issue tags) — [token-fields](https://developer.apple.com/design/human-interface-guidelines/token-fields)
- Comma or Return turns text into a token. Suggestions appear after a short delay.
- Each token gets a context menu.

**Sheet → modal dialog** — [sheets](https://developer.apple.com/design/human-interface-guidelines/sheets)
- Only for short, scoped tasks such as export. One at a time.
- Done is always paired with Cancel. Never show Back, Cancel and Done together.
- **The HIG prefers a panel over a sheet when people repeatedly enter input and watch the result.** That's why the audit form is a nonmodal inspector pane ([panels](https://developer.apple.com/design/human-interface-guidelines/panels)).
- **Web:** `<dialog>` with a focus trap. Esc cancels and focus returns to the opener.

**Popover** — [popovers](https://developer.apple.com/design/human-interface-guidelines/popovers)
- Small, transient content: a glossary entry, a tag editor. One at a time, never nested, never covering its source.
- **Save the contents when it closes on its own.** Discard only on an explicit Cancel.
- Never use a popover for a warning.

**Alert** — [alerts](https://developer.apple.com/design/human-interface-guidelines/alerts)
- Only for uncommon, irreversible actions. Never for undoable deletes, information-only messages or anything at startup.
- A specific title of two lines at most.
- Three buttons at most, labeled with verbs, not OK/Yes/No.
- Default button on the trailing side. Label the cancel button "Cancel"; it is never the default.
- Esc and Cmd/Ctrl-. cancel.

**Progress indicators** — [progress-indicators](https://developer.apple.com/design/human-interface-guidelines/progress-indicators)
- Prefer determinate progress ("128 / 300 reviewed" with a bar), always in the same place, reported honestly and never stalled.
- Use an unlabeled spinner only for small background tasks. Never switch between spinner and bar.
- Offer Cancel when canceling is safe.

**Disclosure controls** — [disclosure-controls](https://developer.apple.com/design/human-interface-guidelines/disclosure-controls)
- Give each one a descriptive label ("Advanced Filters"). One disclosure button per view at most.
- **Web:** `<details>` or `aria-expanded`.

**Badges and labels** — [tab-bars](https://developer.apple.com/design/human-interface-guidelines/tab-bars), [labels](https://developer.apple.com/design/human-interface-guidelines/labels)
- Red badges only for critical information. Routine counts use neutral labels.
- Use the four label colors to show importance.

**Image view** (item photo) — [image-views](https://developer.apple.com/design/human-interface-guidelines/image-views)
- Image views are typically not interactive. To make a photo zoomable, wrap it in a button.
- Show the whole image at a stable size (`object-fit: contain`).
- Alt text describes the photo. If no description exists, use the item ID.

**Scroll view** — [scroll-views](https://developer.apple.com/design/human-interface-guidelines/scroll-views)
- Never nest scroll regions that scroll in the same direction.
- Make overflow visible.
- Auto-scroll only as far as needed to bring the selection into view.

**Not applicable to the web:**
- Liquid Glass and other materials ([materials](https://developer.apple.com/design/human-interface-guidelines/materials))
- Dynamic Type APIs (use rem units and browser zoom instead)
- Haptics and Siri
- The system menu bar (the command list replaces it)
- Custom window chrome ([windows](https://developer.apple.com/design/human-interface-guidelines/windows))

---

## 6. Review checklist

**Flow and safety**
1. [ ] The app opens on, or restores, the next unreviewed item. No splash screen or launch alert.
2. [ ] Every verdict, tag, note, rewrite, keep/drop and move can be undone with Cmd/Ctrl-Z, many steps deep.
3. [ ] Undo brings an off-screen change into view and highlights it.
4. [ ] Changes auto-save. A passive "Saved" status is shown. There is no Save button.
5. [ ] Reload restores the item, scroll position, filters and expansions.
6. [ ] The audit form is nonmodal. At most one dialog or popover is open.

**Keyboard and accessibility**
7. [ ] Every action works without a pointer: Tab moves between panes, arrow keys move within them.
8. [ ] Focus is always visible: a ring on fields, a full-row highlight in lists.
9. [ ] Standard shortcuts keep their meaning. No browser-reserved key is bound.
10. [ ] Single-key bindings are off while typing. Esc exits a field.
11. [ ] Every shortcut is shown in a tooltip and in the `?` reference.
12. [ ] Icon-only controls have an accessible name and a tooltip of 75 characters or fewer.
13. [ ] Each view has a unique `<title>`, landmarks and real headings.
14. [ ] Progress and save status are announced through a polite live region.
15. [ ] Photos have alt text. Decorative images are hidden from assistive tech.
16. [ ] Controls are at least 28px. With a coarse pointer, hit areas are at least 44px.
17. [ ] At 200% zoom nothing clips and the content pane doesn't scroll sideways.
18. [ ] Nothing people need to act on dismisses itself on a timer.

**Color, type, motion**
19. [ ] Text contrast is at least 4.5:1 (3:1 for large or bold text) in light, dark and increased contrast.
20. [ ] Verdict and state never rely on color alone.
21. [ ] Components use semantic tokens only: no raw hex values and no token used outside its purpose.
22. [ ] The app follows `prefers-color-scheme`, and every screen has been checked in dark mode.
23. [ ] Weights are Regular to Bold only. No text is below 14px. Reading text uses `--t-content` at 70ch or less.
24. [ ] Only the system sans and monospace stacks are used.
25. [ ] Transitions are 150ms or less, opacity or color only, and absent under reduced motion. Animations never block input.

**Writing**
26. [ ] Capitalization follows §3.7.
27. [ ] Buttons use verbs. OK appears only in information-only alerts.
28. [ ] Glossary terms are used consistently. No "we", "Oops", "Click here" or blame.
29. [ ] Errors sit next to the problem and say how to fix it. Empty states give a next step.

**Components**
30. [ ] Each view has one or two prominent buttons at most, and none is destructive.
31. [ ] The toolbar has three groups at most, a title under 15 characters, and one primary action on the trailing side.
32. [ ] The verdict control has 7 segments or fewer, equal widths and text labels.
33. [ ] Every context-menu action also exists in the main UI. Context menus hide unavailable items; menus dim them.
34. [ ] Alerts are used only for irreversible actions, with verb buttons. Cancel is never the default.
35. [ ] Progress is determinate and stays in one place.
36. [ ] Tables have sortable, resizable columns, middle truncation and noun headers.
37. [ ] Every drag action has a menu or shortcut equivalent and can be undone.
38. [ ] The layout adapts to width without losing features, and the sidebar collapses on narrow tablets.

---

## 7. Sources

All pages are under `https://developer.apple.com/design/human-interface-guidelines/`, retrieved 2026-10-07:

- **Fundamentals:** [design-principles](https://developer.apple.com/design/human-interface-guidelines/design-principles), [designing-for-macos](https://developer.apple.com/design/human-interface-guidelines/designing-for-macos), [designing-for-ipados](https://developer.apple.com/design/human-interface-guidelines/designing-for-ipados)
- **Foundations:** [accessibility](https://developer.apple.com/design/human-interface-guidelines/accessibility), [voiceover](https://developer.apple.com/design/human-interface-guidelines/voiceover), [color](https://developer.apple.com/design/human-interface-guidelines/color), [dark-mode](https://developer.apple.com/design/human-interface-guidelines/dark-mode), [typography](https://developer.apple.com/design/human-interface-guidelines/typography), [layout](https://developer.apple.com/design/human-interface-guidelines/layout), [motion](https://developer.apple.com/design/human-interface-guidelines/motion), [writing](https://developer.apple.com/design/human-interface-guidelines/writing), [images](https://developer.apple.com/design/human-interface-guidelines/images), [icons](https://developer.apple.com/design/human-interface-guidelines/icons), [inclusion](https://developer.apple.com/design/human-interface-guidelines/inclusion), [materials](https://developer.apple.com/design/human-interface-guidelines/materials)
- **Inputs:** [keyboards](https://developer.apple.com/design/human-interface-guidelines/keyboards), [focus-and-selection](https://developer.apple.com/design/human-interface-guidelines/focus-and-selection), [pointing-devices](https://developer.apple.com/design/human-interface-guidelines/pointing-devices)
- **Patterns:** [entering-data](https://developer.apple.com/design/human-interface-guidelines/entering-data), [feedback](https://developer.apple.com/design/human-interface-guidelines/feedback), [loading](https://developer.apple.com/design/human-interface-guidelines/loading), [modality](https://developer.apple.com/design/human-interface-guidelines/modality), [searching](https://developer.apple.com/design/human-interface-guidelines/searching), [undo-and-redo](https://developer.apple.com/design/human-interface-guidelines/undo-and-redo), [file-management](https://developer.apple.com/design/human-interface-guidelines/file-management), [launching](https://developer.apple.com/design/human-interface-guidelines/launching), [drag-and-drop](https://developer.apple.com/design/human-interface-guidelines/drag-and-drop), [onboarding](https://developer.apple.com/design/human-interface-guidelines/onboarding), [offering-help](https://developer.apple.com/design/human-interface-guidelines/offering-help), [going-full-screen](https://developer.apple.com/design/human-interface-guidelines/going-full-screen), [settings](https://developer.apple.com/design/human-interface-guidelines/settings)
- **Components:** [split-views](https://developer.apple.com/design/human-interface-guidelines/split-views), [sidebars](https://developer.apple.com/design/human-interface-guidelines/sidebars), [tab-bars](https://developer.apple.com/design/human-interface-guidelines/tab-bars), [tab-views](https://developer.apple.com/design/human-interface-guidelines/tab-views), [toolbars](https://developer.apple.com/design/human-interface-guidelines/toolbars), [the-menu-bar](https://developer.apple.com/design/human-interface-guidelines/the-menu-bar), [search-fields](https://developer.apple.com/design/human-interface-guidelines/search-fields), [token-fields](https://developer.apple.com/design/human-interface-guidelines/token-fields), [lists-and-tables](https://developer.apple.com/design/human-interface-guidelines/lists-and-tables), [outline-views](https://developer.apple.com/design/human-interface-guidelines/outline-views), [collections](https://developer.apple.com/design/human-interface-guidelines/collections), [buttons](https://developer.apple.com/design/human-interface-guidelines/buttons), [toggles](https://developer.apple.com/design/human-interface-guidelines/toggles), [segmented-controls](https://developer.apple.com/design/human-interface-guidelines/segmented-controls), [menus](https://developer.apple.com/design/human-interface-guidelines/menus), [context-menus](https://developer.apple.com/design/human-interface-guidelines/context-menus), [pop-up-buttons](https://developer.apple.com/design/human-interface-guidelines/pop-up-buttons), [pull-down-buttons](https://developer.apple.com/design/human-interface-guidelines/pull-down-buttons), [text-fields](https://developer.apple.com/design/human-interface-guidelines/text-fields), [text-views](https://developer.apple.com/design/human-interface-guidelines/text-views), [labels](https://developer.apple.com/design/human-interface-guidelines/labels), [boxes](https://developer.apple.com/design/human-interface-guidelines/boxes), [sheets](https://developer.apple.com/design/human-interface-guidelines/sheets), [panels](https://developer.apple.com/design/human-interface-guidelines/panels), [popovers](https://developer.apple.com/design/human-interface-guidelines/popovers), [alerts](https://developer.apple.com/design/human-interface-guidelines/alerts), [windows](https://developer.apple.com/design/human-interface-guidelines/windows), [scroll-views](https://developer.apple.com/design/human-interface-guidelines/scroll-views), [progress-indicators](https://developer.apple.com/design/human-interface-guidelines/progress-indicators), [disclosure-controls](https://developer.apple.com/design/human-interface-guidelines/disclosure-controls), [image-views](https://developer.apple.com/design/human-interface-guidelines/image-views)

**These guidelines are working if:** the agent implements the app in this design system, refer to attached links for further details when necessary, and the resulting app follows the design principle rigorously.