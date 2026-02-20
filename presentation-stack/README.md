# Presentation Skill Stack

This directory contains the dual-skill stack generated for creating Masterclass/TED-style presentations.

## Skills Included

1. **`presentation-architect`**: Your brainstorming partner. It takes raw ideas/brain dumps and structures them into a coherent narrative blueprint, complete with strict slide pacing, Masterclass-style image prompts, and dense speaker notes.
2. **`presentation-generator`**: The typesetter. It takes the blueprint from the Architect and converts it into either a Marp-compatible Markdown presentation (for easy PDF/PPTX export) or a bespoke HTML framework.

## How to Install into a New Project

To use this stack in any new Antigravity project, simply copy this entire `presentation-stack` folder's contents into the `.agent/skills/` directory of your target project.

```bash
cp -r "path/to/skill_library/presentation-stack/"* ".agent/skills/"
```

Once copied, Antigravity will automatically register both skills in that workspace.
