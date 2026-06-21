---
name: brainstorming
description: "当用户要做需求澄清、方案设计、功能探索时使用。适用于：(1)已有想法要设计实现方案 (2)「做个XX」需要先理解需求。如果用户说「给我个点子」「不知道做什么」则用 ideation skill。实现方案确定后进入 writing-plans。"
---

# Brainstorming — Turn Ideas Into Designs

Help turn ideas into fully formed designs and specs through natural collaborative dialogue.

## HARD GATE

Do NOT write any code, scaffold any project, or take any implementation action until you have presented a design and the user has approved it. This applies to EVERY project regardless of perceived simplicity.

"Simple" projects are where unexamined assumptions cause the most wasted work. The design can be short, but you MUST present it and get approval.

## Process

### 1. Explore Project Context
- Check files, docs, recent commits
- Assess scope: if the request covers multiple independent subsystems, flag immediately
- If too large, help decompose into sub-projects

### 2. Ask Clarifying Questions
- **For technical/functional decisions:** One question at a time — don't overwhelm
- **For creative/naming/design decisions:** Ask ALL key questions in ONE message before giving any suggestions. Creative direction requires full context — partial info leads to suggestions the user has to reject and correct ("你貌似没问全吧"). Gather target audience, core value proposition, style preference, and language/format preference together.
- Multiple choice preferred when possible
- Focus on: purpose, constraints, success criteria

### 3. Propose 2-3 Approaches
- Present options with trade-offs and your recommendation
- Lead with your recommended option and explain why

### 4. Present Design
- Scale sections to complexity
- Ask after each section whether it looks right
- Cover: architecture, components, data flow, error handling, testing

### 5. Write Design Doc
- Save to `docs/specs/YYYY-MM-DD-<topic>-design.md`
- Commit to git

### 6. Self-Review
- Placeholder scan: any "TBD", "TODO", vague requirements?
- Internal consistency: do sections contradict?
- Scope check: focused enough for one plan?
- Ambiguity check: any requirement interpretable two ways?

### 7. User Reviews Spec
- Ask user to review before proceeding
- Only proceed once approved

### 8. Transition to Implementation
- Create implementation plan (use `planning` or `writing-plans` skill)

## Pitfalls

- **Don't give creative suggestions with partial requirements.** When helping with naming, branding, or design decisions, you MUST gather all key dimensions first (audience, value prop, style, format). If the user answers only 1 of 3 questions and you proceed to give 5 suggestions, they'll say "你没问全" and you wasted both your time. For creative tasks, a single batch of 3-4 targeted questions with multiple-choice options beats one-at-a-time drip feeding.

- **Discuss before executing, even for operational tasks.** User feedback: "我问你问题，讨论啊，你怎么直接去做了" — when the user asks a question (e.g., "你觉得用微信好还是飞书好"), they want to DISCUSS options first, not have you immediately start executing. This applies beyond design tasks to any situation with multiple valid approaches. Present options with trade-offs, get confirmation, THEN act.

## Key Principles

- **Context before suggestions** — for creative/naming/design tasks, gather ALL requirements first; for technical tasks, one question at a time is fine
- **Multiple choice preferred** — easier to answer
- **YAGNI ruthlessly** — remove unnecessary features
- **Explore alternatives** — always propose 2-3 approaches
- **Incremental validation** — present design, get approval before moving on
- **Design for isolation** — break into units with clear purpose and well-defined interfaces
