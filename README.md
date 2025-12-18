# Autonomy in Three Files

**Transforming AI-Powered IDEs into Self-Directing Agent Platforms**

*A file-based, minimalist meta-prompt structure for autonomous agent behavior*

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.17979699.svg)](https://doi.org/10.5281/zenodo.17979699)


---

## The Core Idea

Modern AI-powered IDEs (Cursor, Windsurf, VS Code + Copilot) already have everything an autonomous agent needs:
- **Perception:** Can read any file
- **Action:** Can write files, run commands, browse the web
- **Memory:** Files persist indefinitely

They're not just code editors. **They're autonomous agent platforms.** You just need to give them structure.

## The Three Files = Cognitive Architecture

| File | Role | What It Does |
|------|------|--------------|
| `goal.md` | **Long-term Goal Memory** | Defines what you're trying to achieve, how to approach it, when to stop |
| `how-to.md` | **Procedural Knowledge** | Documents available tools and how to use them (grows as new tools are created) |
| `path_way.md` | **Working Memory** | Agent writes its progress here, including "Next Step" which becomes the next prompt |

**The magic:** The "Next Step" from round N becomes the instruction for round N+1. The agent writes its own future prompts.

## How It Works

1. Human creates `goal.md` and `how-to.md`
2. Human sends an ignition prompt: "Follow goal.md, use tools from how-to.md, record progress in path_way.md"
3. Agent executes, writes findings and next step to `path_way.md`
4. Human sends continuation signal
5. Agent reads its own "next step" and continues
6. Repeat until task complete

The magic: **the output of iteration N becomes the instructions for iteration N+1**. The agent writes its own todo list and follows it.

## Key Properties

- **Transparent:** All reasoning is visible in files you can read and edit
- **Controllable:** Edit any file to redirect the agent
- **Minimal:** No dependencies beyond your IDE
- **Generalizable:** Works for any chat interface with file access

## Quick Start

```bash
# Copy templates to your project
cp templates/* your-project/

# Edit goal.md with your objective
# Edit how-to.md with your tools (or let the agent document them)

# In your AI-powered IDE, send:
# "Follow @goal.md, use tools from @how-to.md, record progress in @path_way.md"
```

## Repository Structure

```
autonomy-in-3-files/
├── README.md                 # This file
├── paper.md                  # Full paper/documentation
├── templates/
│   ├── goal.md               # Template for objectives
│   ├── how-to.md             # Template for tool documentation
│   ├── path_way.md           # Template for execution log
│   └── ignition-prompt.md    # Template for starting the loop
└── examples/
    └── xhs-research/         # Real case study: market research
        ├── goal.md
        ├── how-to.md
        └── path_way.md
```

## Tested Platforms

**IDEs:**
- Cursor
- Antigravity (VS Code)

**Models:**
- Gemini series (Flash, Pro)
- Claude series (Sonnet, Opus)

## Related Work

Emerging work demonstrates growing interest in minimalist approaches to agent construction. [EvolvingAgentsLabs' framework-core](https://github.com/EvolvingAgentsLabs/framework-core) elegantly demonstrates that agent behavior can be defined entirely through structured Markdown files, treating natural language as a programming medium.

Our work shares this appreciation for simplicity but differs in focus: rather than defining a new agent specification format, we explore how existing AI-powered IDEs—and by extension, any chat interface with file access—can be recognized as **already-capable agent platforms** requiring only structural guidance to exhibit autonomous behavior.

## Citation

```bibtex
@misc{luan2024autonomy,
  title={Autonomy in Three Files: Transforming AI-Powered IDEs into Self-Directing Agent Platforms},
  author={Luan, Mike and Opus, Claude},
  year={2024},
  howpublished={\url{https://github.com/luan007/autonomy-in-3-files}}
}
```

## License

MIT
