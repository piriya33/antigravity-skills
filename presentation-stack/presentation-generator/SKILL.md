---
name: presentation-generator
description: Generates a flawless Marp Markdown presentation or custom HTML deck from a presentation blueprint. Use when the user wants to convert an outline or blueprint into actual presentation slides (Marp, HTML, PDF, PPTX).
---

# Presentation Generator

## Triggering Contexts
- When the user has a finalized presentation blueprint (from the `presentation-architect` skill).
- When the user asks to turn their outline into Marp markdown, HTML slides, PDF, or PowerPoint.

## Workflow Checklist
```markdown
- [ ] Ask the user for their preferred output format (Marp Markdown or Custom HTML)
- [ ] If Marp: Create the target `.md` file with Marp frontmatter and directives
- [ ] If HTML: Generate the custom CDC-branded HTML framework with WakeLock API
- [ ] Translate the blueprint's slide copy, speaker notes, and image placements into the target format
- [ ] Apply elegant, minimalist CSS/theme styling
```

## Instructions

You are a silent, elegant coder. Your job is to take a validated presentation blueprint and typeset it into a flawless, functional presentation file with premium aesthetics.

1. **Determine the Format**
   - Ask the user if they want **Marp Markdown** (for easy PDF/PPTX export) or **Custom HTML** (for presenting in browser). Default to Marp if they want a clean, printable deck.

2. **Marp Generation Rules**
   - Files should end in `.md`.
   - Start with Marp YAML frontmatter:
     ```yaml
     ---
     marp: true
     theme: default
     class: invert
     ---
     ```
   - Use `---` to separate slides.
   - Use `![bg right:40%](image-url)` for elegant split-screen layouts.
   - Use `![bg](image-url)` for full-bleed Masterclass backgrounds.
   - Put speaker notes at the bottom of the slide under `<!--` and `-->` HTML comments.
   - Apply restraint: No jarring animations, just simple clean layouts.

3. **HTML Generation Rules (If requested instead of Marp)**
   - Files should end in `.html`.
   - Take a clear, **intentional aesthetic stance** (e.g., Luxury Minimal, Editorial Brutalism, Glass & Light). Do not output generic, safe "AI UI".
   - **Typography & Accessibility**: 
     - Never use system default fonts. Choose 1 highly expressive display font and 1 clean body font.
     - Enforce minimum 4.5:1 color contrast.
     - Limit text line-lengths to 65-75 characters for readability, even on massive 4K screens.
     - Use structural typography scaling (massive headers, tight tracking) and line-heights of 1.5 - 1.75 for body text.
   - **Design Mechanics**:
     - Use CSS variables exclusively for the color system (1 dominant, 1 accent, 1 neutral).
     - **No emojis**; if icons are needed, use clean inline SVGs.
     - Ensure Glassmorphism components have sufficient opacity (e.g., `bg-white/80` for light mode, `rgba(40,40,40,0.6)` for dark) and visible borders.
   - **Motion & Interaction**:
     - Motion must be purposeful and sparse. Prefer one strong entrance sequence over decorative micro-motion.
     - Enforce smooth, 800-1200ms ease-in-out crossfade transitions between slides.
     - Any interactive elements must have `cursor: pointer` and smooth 150-300ms hover transitions that do NOT cause layout shifts.
   - Inject the JavaScript `WakeLock` API to force the browser to keep the screen on while presenting:
     ```javascript
     let wakeLock = null;
     const requestWakeLock = async () => {
       try { if ('wakeLock' in navigator) wakeLock = await navigator.wakeLock.request('screen'); } catch (err) {}
     };
     document.addEventListener('visibilitychange', () => { if (wakeLock !== null && document.visibilityState === 'visible') requestWakeLock(); });
     requestWakeLock();
     ```

4. **Aesthetic Constraints & Spatial Composition**
   - Break the grid intentionally. Use asymmetry, overlap, and massive negative space.
   - White space is a structural design element, not an absence of content. Rely on generous padding and margin so the content feels expensive and uncrowded.

## Artifact Integration
- The script should write the generated presentation file directly to the workspace (e.g., `slides.md` or `presentation.html`).
- Provide instructions to the user on how to preview/export it (e.g., using the Marp for VS Code extension's export tools).

## Files & Resources
- None
