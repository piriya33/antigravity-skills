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
   - Use pure, clean HTML/CSS/JS (like the CDC-branded framework).
   - Enforce smooth, 800ms ease-in-out crossfade transitions.
   - Strict typography: Massive, elegant serif headers mixed with clean sans-serif subtext.
   - Inject the JavaScript `WakeLock` API to force the browser to keep the screen on while presenting:
     ```javascript
     let wakeLock = null;
     const requestWakeLock = async () => {
       try { wakeLock = await navigator.wakeLock.request('screen'); } catch (err) {}
     };
     document.addEventListener('visibilitychange', () => { if (wakeLock !== null && document.visibilityState === 'visible') requestWakeLock(); });
     requestWakeLock();
     ```

4. **Aesthetic Constraints**
   - Rely on generous padding and margin (negative space) so the content feels expensive and uncrowded.
   - Ensure high contrast.

## Artifact Integration
- The script should write the generated presentation file directly to the workspace (e.g., `slides.md` or `presentation.html`).
- Provide instructions to the user on how to preview/export it (e.g., using the Marp for VS Code extension's export tools).

## Files & Resources
- None
