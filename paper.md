# Autonomy in Three Files
## Transforming AI-Powered IDEs into Self-Directing Agent Platforms

**Authors:** Mike Luan, Claude Opus  
**Repository:** [github.com/luan007/autonomy-in-3-files](https://github.com/luan007/autonomy-in-3-files)

---

## Abstract

Modern AI-powered integrated development environments (IDEs) such as Cursor, Windsurf, and VS Code with GitHub Copilot possess capabilities that extend well beyond code editing. These systems can read and write arbitrary files, execute terminal commands, browse the web, and maintain conversational context across sessions. Collectively, these capabilities constitute the essential components of an autonomous agent: perception, action, and memory.

This paper introduces the **Path-Way Protocol**, a lightweight methodology that enables any file-capable IDE to function as a self-directing autonomous agent for arbitrary tasks—including research, content creation, and data analysis—using only three Markdown files. The protocol externalizes agent state into human-readable documents, enabling recursive self-prompting without custom code or external frameworks.

We present the complete protocol specification, demonstrate its application through a detailed case study in social media market research, and provide reusable templates. The approach offers two distinctive properties: (1) **transparent interaction**, where all agent reasoning and state transitions are visible and editable by the human operator, and (2) **writing as coordination**, where natural language documents serve as the primary mechanism for human-agent and agent-self coordination.

**Keywords:** Autonomous Agents, Integrated Development Environments, Meta-Prompting, File-Based State Management, Extended Cognition, Human-AI Collaboration

---

## 1. Introduction

### 1.1 Background: The Emergence of File-Capable AI Assistants

Recent advances in AI-powered development tools have produced a new class of software: integrated development environments with embedded large language models (LLMs) capable of reading, writing, and executing code within a project workspace. Tools such as Cursor, Windsurf, and Visual Studio Code with GitHub Copilot represent this category.

While these tools are designed primarily for software development, their underlying capabilities are domain-agnostic:

| Capability | Technical Mechanism | General Application |
|------------|---------------------|---------------------|
| File reading | Workspace indexing, `@file` references | Access research notes, data, prior work |
| File writing | Direct file creation and modification | Generate reports, update logs, create content |
| Command execution | Integrated terminal with approval flow | Run data processing scripts, API calls, scrapers |
| Web browsing | Built-in browser or search capabilities | Conduct research, verify facts, gather information |
| Context maintenance | Conversation history, project awareness | Maintain task continuity across sessions |

This combination of capabilities—perception through file reading, action through file writing and command execution, and memory through persistent files—constitutes the functional requirements for an autonomous agent system.

### 1.2 The Observation

The central observation motivating this work is straightforward: **if an AI assistant can read files, write files, and execute commands, it already possesses the infrastructure required for autonomous multi-step task execution.** What it lacks is not capability, but *structure*—a framework for knowing what to do, how to do it, and how to track progress across iterations.

This leads to a natural question: rather than building autonomous agents as separate systems with complex orchestration code, can we leverage existing AI-powered IDEs as autonomous agent platforms by providing them with appropriate structural guidance?

### 1.3 Contributions

This paper makes the following contributions:

1. **Protocol Specification:** We define the Path-Way Protocol, a minimal three-file architecture that enables recursive autonomous behavior in any file-capable AI assistant.

2. **Transparency Mechanism:** We demonstrate how file-based state externalization creates inherently transparent agent operation, where all reasoning and decisions are visible and modifiable by the human operator.

3. **Writing as Coordination:** We articulate the principle that natural language documents can serve as the primary coordination mechanism between human and agent, as well as between an agent's successive iterations.

4. **Case Study:** We provide a complete, reproducible example of the protocol applied to market research, including all input files, execution traces, and output.

5. **Reusable Templates:** We offer templates that practitioners can adapt for their own autonomous workflows.

---

## 2. Related Work

### 2.1 LLM Agent Architectures

The development of autonomous LLM agents has produced several notable frameworks. ReAct (Yao et al., 2022) introduced the pattern of interleaving reasoning ("Thought") with action ("Act") and observation, establishing a foundation for tool-using agents. Reflexion (Shinn et al., 2023) extended this with verbal self-reflection, allowing agents to learn from their own outputs. Meta-Prompting (Suzgun & Kalai, 2024) demonstrated that LLMs can generate prompts for themselves or other LLMs, enabling task decomposition without explicit programming.

Toolformer (Schick et al., 2023) showed that LLMs can learn to use external tools through self-supervised training, while frameworks such as LangChain and AutoGPT provide programmatic orchestration for multi-step agent workflows.

### 2.2 Extended Mind and Cognitive Offloading

The Extended Mind thesis (Clark & Chalmers, 1998) proposes that cognitive processes can extend beyond the brain to include external tools and representations when those tools are reliably available and routinely used. This philosophical framework has been applied to human-AI interaction, where AI systems function as "cognitive extensions" that offload mental tasks.

The Path-Way Protocol instantiates this concept directly: the agent's "memory" is externalized to files, its "planning" is documented in natural language, and its "self-awareness" of past actions is mediated through readable logs. The file system becomes, in effect, an extended cognitive workspace shared between human and AI.

### 2.3 Project Documentation for AI Agents

Recent developments include standardized documentation formats designed specifically for AI agents. AGENTS.md provides machine-readable project instructions for coding agents, while CLAUDE.md (Anthropic) offers project-specific context for the Claude assistant. The Path-Way Protocol extends this concept from static documentation to dynamic, recursive state management.

### 2.4 Markdown-Based Agent Definition

Emerging work demonstrates growing interest in minimalist approaches to agent construction. EvolvingAgentsLabs' framework-core elegantly demonstrates that agent behavior can be defined entirely through structured Markdown files, treating natural language as a programming medium. Their approach uses three files (`SystemAgent.md`, `SmartLibrary.md`, `SmartMemory.md`) to define agent behavior declaratively.

Our work shares this appreciation for simplicity but differs in focus: rather than defining a new agent specification format, we explore how existing AI-powered IDEs—and by extension, any chat interface with file access—can be recognized as **already-capable agent platforms** requiring only structural guidance to exhibit autonomous behavior. Where framework-core creates a new abstraction layer, we leverage existing infrastructure.

---

## 3. The Path-Way Protocol

### 3.1 Overview

The Path-Way Protocol consists of three Markdown files that together define an autonomous agent's objective, capabilities, and state:

| File | Purpose | Mutability | Author |
|------|---------|------------|--------|
| `goal.md` | Defines the objective, constraints, methodology, and termination condition | Immutable during execution | Human |
| `how-to.md` | Documents available tools and their usage | Growable (can be updated when new tools are created) | Human and/or Agent |
| `path_way.md` | Records execution history and specifies next actions | Updated each iteration | Agent |

### 3.2 File Specifications

#### 3.2.1 `goal.md` — The Directive

The goal file establishes the strategic context for the agent. It specifies:

- **Background:** Domain context, persona, and constraints
- **Objective:** What the agent should accomplish
- **Methodology:** How the agent should approach the task (e.g., breadth-first exploration, iterative refinement)
- **Termination condition:** When the task is complete (e.g., "at least 5 rounds of research")
- **Output specification:** Expected deliverables

This file remains unchanged during execution, providing a stable reference that prevents goal drift across iterations.

#### 3.2.2 `how-to.md` — The Capability Reference

The how-to file documents available tools and their usage patterns. This may include:

- Custom scripts (e.g., data scrapers, API clients)
- Command-line tools and their arguments
- External services and how to access them
- Best practices and common patterns

This file can grow during execution. If the agent creates a new tool (through vibe coding or otherwise), it can document that tool in `how-to.md` for future iterations.

#### 3.2.3 `path_way.md` — The Working Memory

The path_way file is the agent's mutable state. After each iteration, the agent records:

- **Round identifier:** Sequential numbering
- **Action taken:** What was done in this iteration
- **Observations:** What was learned or discovered
- **Analysis:** Interpretation of the observations
- **Next step:** What should be done in the subsequent iteration

The "Next step" field is critical: it serves as the agent's self-authored prompt for the next iteration. In this way, **the output of iteration N becomes the input instruction for iteration N+1**, implementing recursive meta-prompting without code.

### 3.3 The Execution Cycle

The protocol operates through a simple cycle:

1. **Read:** The agent reads `goal.md`, `how-to.md`, and `path_way.md` to understand the current context.
2. **Plan:** Based on the "Next step" from the previous round (or initial instructions from `goal.md`), the agent determines what action to take.
3. **Execute:** The agent performs the action—running commands, browsing the web, analyzing data, etc.
4. **Write:** The agent appends a new round to `path_way.md`, documenting what was done and what should happen next.
5. **Repeat:** The human sends a continuation signal (in current IDE implementations), and the cycle repeats.

### 3.4 Transparency Through Externalized State

A distinctive property of this protocol is **radical transparency**. Because all agent state is externalized to human-readable files:

- The human operator can inspect the agent's "reasoning" at any point
- The human can edit `path_way.md` to redirect the agent
- Execution history is automatically preserved and auditable
- Debugging is straightforward: simply read the files

This contrasts with opaque agent frameworks where internal state is hidden in memory or logs that require specialized tools to interpret.

### 3.5 Writing as a Coordination Mechanism

The protocol embodies a principle we term **writing as coordination**: natural language documents serve as the primary interface for coordinating activity between human and agent, as well as between the agent's own iterations.

This approach has several advantages:

1. **Human-in-the-loop by default:** The state is always accessible and modifiable
2. **No serialization complexity:** State is stored in the same format it is reasoned about
3. **Graceful degradation:** If the system fails, the work-in-progress is preserved in readable form
4. **Collaborative editing:** Human and agent can both contribute to the same documents

---

## 4. Case Study: Market Research on Xiaohongshu

### 4.1 Task Description

We applied the Path-Way Protocol to conduct market research on Xiaohongshu (小红书), a Chinese social media platform similar to Instagram and Pinterest. The objective was to identify trending content related to AI-generated images and produce recommendations for content creation.

### 4.2 Setup

#### `goal.md`

```markdown
## Background
I am Nanobanana, an AIGC content creator focusing on AI-generated visualizations.
We will research trending content to generate recommendations for creative topics.

## Starting Theme
Fun, viral AI images (entertaining content)

## Methodology
- Breadth-first: Explore multiple related keywords
- During search, refine understanding of trends and expand keywords
- For high-engagement content, analyze structure and comments
- If images are available, download and analyze them

## Requirements
- Record all searches and plans in path_way.md
- Review path_way.md and develop new keywords based on findings
- Complete at least 5 rounds of research

## Output
- Data listing
- Summary of findings
- Research report
- Recommended creative topics with justifications
```

#### `how-to.md`

```markdown
# XHS Fetch CLI - Usage Guide

## Search Command
```bash
node xhs-fetch.js search <keyword> <limit> --session <name> --deep
```

### Parameters
- `keyword`: Search term
- `limit`: Number of items for deep fetching
- `--session <name>`: Saves data to session file for deduplication
- `--deep`: Enables detailed scraping (comments, full text, tags)

## Example Workflow
1. Broad search: `node xhs-fetch.js search "AI图" 1 --session research`
2. Deep dive: `node xhs-fetch.js search "AI图" 5 --session research --deep`
```

#### `meta-prompt.md` (Ignition Prompt)

```markdown
As a social media content research specialist:

- Study and use the search tool in the xhs folder
- Follow objectives defined in @goal.md
- Record all progress to @path_way.md

Strictly follow @goal.md.
```

### 4.3 Execution Trace

The agent executed 5 rounds of autonomous research. The following table summarizes each round:

| Round | Keywords Explored | Key Findings | Self-Generated Next Step |
|-------|-------------------|--------------|--------------------------|
| 1 | "AI图", "AI绘画" | Identified "NanoBanana Pro" as trending tool; found high engagement on prompt tutorials | "Explore application scenarios: AI头像, AI图片生成" |
| 2 | "AI头像", "AI图片生成" | LinkedIn-style headshots show strong demand; scientific figure generation is a niche | "Pivot to entertainment content: 趣味AI, AI表情包" |
| 3 | "趣味AI", "AI表情包" | AI memes receive 4000+ likes; philosophical content achieves 100k+ engagement | "Synthesize findings into user personas" |
| 4 | (Synthesis phase) | Identified 4 distinct user personas: designers, researchers, professionals, entertainment seekers | "Generate content recommendations" |
| 5 | (Output phase) | Produced 5 content ideas with engagement predictions and justifications | Task complete |

### 4.4 Sample `path_way.md` Entry (Round 3)

```markdown
## Round 3 - Entertainment Content Exploration

**Keywords:** "趣味AI", "AI表情包"

**Rationale:** Rounds 1-2 focused on practical applications. Now exploring 
entertainment content to understand viral potential.

**Findings:**
- AI memes and sticker packs show extremely high engagement:
  - "AI jokes collection (3)" — 4,049 likes
  - "Deepseek sticker pack" — 4,479 likes
  - "GPT memes" — 2,146 likes
- Philosophical/reflective content achieves breakthrough engagement:
  - "If the world is really a program, would you continue as an NPC?" — 107,000 likes
- Content exposing AI scams also performs well:
  - "Debunking AI sticker pack income claims" — 1,960 likes

**Key Insight:** Entertainment content succeeds through either "surprise" 
(unexpected AI capabilities) or "resonance" (expressing shared feelings about AI).

**Next Step:** Synthesize all findings to define distinct user personas, analyzing 
their pain points, needs, and content preferences.
```

### 4.5 Outcome

The agent produced a comprehensive research report including:

- Quantitative summary of engagement by content type
- Four reader personas with detailed characterization
- Five content recommendations with justifications
- Trend analysis and strategic recommendations

**Total human intervention during execution: 0** (after initial setup and between-round continuation signals)

---

## 5. Properties of the Protocol

### 5.1 Minimal Dependencies

The protocol requires only:
- An AI-powered IDE with file read/write and terminal access
- Three Markdown files
- (Optional) Custom tools for specific tasks

No external frameworks, databases, or orchestration code are needed.

### 5.2 Transparency and Debuggability

Because all state is externalized to readable files:
- Agent "reasoning" is always visible
- Errors can be diagnosed by reading `path_way.md`
- Recovery is possible by editing files and resuming

### 5.3 Human Control

The human operator retains full control:
- Edit `goal.md` to change objectives
- Edit `how-to.md` to add or modify capabilities
- Edit `path_way.md` to redirect the agent or correct errors
- Decline to send continuation signals to pause execution

### 5.4 Tool Extensibility

New tools can be added during execution through vibe coding: the agent generates a script to meet a need, then documents it in `how-to.md` for future use. The capability set grows organically with the task.

---

## 6. Limitations

### 6.1 Semi-Autonomous Operation

Current IDE implementations require the human to send a message to trigger each iteration. Fully autonomous looping would require additional automation.

### 6.2 IDE-Specific Requirements

The protocol assumes an IDE with:
- LLM integration with file access
- Terminal execution capability
- File reference mechanisms (e.g., `@file.md` syntax)

It does not work with web-based chat interfaces or text editors without AI integration.

### 6.3 Scale Limitations

We have validated the protocol on tasks requiring 5-10 iterations. Performance with 50+ iterations or very large `path_way.md` files has not been systematically tested.

### 6.4 No Quantitative Benchmarks

This paper describes a methodology rather than reporting experimental results. We do not provide comparisons with other agent frameworks on standardized benchmarks. Such evaluation is valuable future work.

### 6.5 LLM Capability Dependency

The protocol assumes the LLM can reliably:
- Read and parse Markdown files
- Follow multi-step instructions
- Maintain consistency across iterations

Less capable models may exhibit drift or failures.

---

## 7. Discussion

### 7.1 IDE as Agent Runtime

The emergence of AI-powered IDEs represents an underappreciated development in autonomous agent infrastructure. Rather than building agent capabilities from scratch with orchestration frameworks, practitioners can leverage the file handling, terminal access, and conversational capabilities that already exist in modern development environments.

### 7.2 The Role of Writing

The protocol foregrounds writing as a cognitive and coordinative activity. The agent "thinks" by writing; it "remembers" by reading what it wrote; it "plans" by articulating next steps in prose. This mirrors human knowledge work practices and creates natural alignment between human and agent workflows.

### 7.3 Implications for Human-AI Collaboration

The transparent, file-based nature of the protocol suggests a mode of human-AI collaboration where:
- The AI contributes sustained attention and execution
- The human contributes judgment and redirection
- Both parties work within a shared, readable workspace

This contrasts with delegation models where the human specifies a task and waits for a result from an opaque system.

---

## 8. Conclusion

This paper has presented the Path-Way Protocol, a minimal methodology for transforming AI-powered IDEs into autonomous agent platforms. The protocol requires only three Markdown files and no custom code, yet enables multi-step autonomous execution with full transparency and human control.

The key insights are:
1. Modern IDEs already possess the capabilities required for autonomous agent operation
2. Externalizing state to files creates inherent transparency and debuggability
3. Natural language documents can serve as effective coordination mechanisms
4. The structure of files, not the complexity of frameworks, determines agent capability

We hope this work encourages practitioners to explore the autonomous potential of the tools they already use, and to consider file-based, transparent approaches to human-AI collaboration.

---

## Appendix A: File Templates

### A.1 `goal.md` Template

```markdown
## Background
[Describe the context, persona, and constraints]

## Objective
[State what should be accomplished]

## Methodology
- [Approach step 1]
- [Approach step 2]
- Record all progress in path_way.md
- Complete at least N rounds

## Output
[Describe expected deliverables]
```

### A.2 `path_way.md` Template (Empty at Start)

```markdown
# Execution Log

## Round 1
**Action:** [What was done]
**Observations:** [What was learned]
**Analysis:** [Interpretation of findings]
**Next Step:** [What to do in Round 2]
```

### A.3 Ignition Prompt Template

```markdown
[Role description]

- Study and use tools documented in @how-to.md
- Follow objectives in @goal.md
- Record all progress to @path_way.md

Execute according to @goal.md.
```

---

## References

1. Clark, A., & Chalmers, D. (1998). The Extended Mind. *Analysis*, 58(1), 7-19.

2. Yao, S., Zhao, J., Yu, D., Du, N., Shafran, I., Narasimhan, K., & Cao, Y. (2022). ReAct: Synergizing Reasoning and Acting in Language Models. *arXiv:2210.03629*.

3. Shinn, N., Cassano, F., Gopinath, A., Narasimhan, K., & Yao, S. (2023). Reflexion: Language Agents with Verbal Reinforcement Learning. *arXiv:2303.11366*.

4. Schick, T., Dwivedi-Yu, J., Dessì, R., Raileanu, R., Lomeli, M., Zettlemoyer, L., Cancedda, N., & Scialom, T. (2023). Toolformer: Language Models Can Teach Themselves to Use Tools. *arXiv:2302.04761*.

5. Suzgun, M., & Kalai, A. T. (2024). Meta-Prompting: Enhancing Language Models with Task-Agnostic Scaffolding. *arXiv:2401.12954*.

6. Anthropic. (2024). Building Effective Agents. *Anthropic Documentation*.

7. Wei, J., Wang, X., Schuurmans, D., Bosma, M., Ichter, B., Xia, F., Chi, E., Le, Q., & Zhou, D. (2022). Chain-of-Thought Prompting Elicits Reasoning in Large Language Models. *arXiv:2201.11903*.

8. Park, J. S., O'Brien, J. C., Cai, C. J., Morris, M. R., Liang, P., & Bernstein, M. S. (2023). Generative Agents: Interactive Simulacra of Human Behavior. *arXiv:2304.03442*.

---

*This paper describes a workflow methodology developed through practical experimentation. The authors share it in the spirit of contributing to the emerging practice of human-AI collaboration. No claims are made regarding optimality; the approach is offered as one viable pattern among many.*
