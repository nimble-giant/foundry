---
name: ghostwriter
description: Interactively interview the user to capture their personal writing voice (tone, rhythm, vocabulary, formatting, quirks) and emit a ready-to-install SKILL.md that teaches other AI agents to write in that voice. Use this skill whenever the user wants to "make Claude write like me", "create a writing style skill", "stop sounding like AI", calibrate an agent's prose for emails/comments/posts/docs, or otherwise capture their voice for downstream use by another model. Trigger this even if the user only hints at the need ("my agent's comments don't sound like me", "AI writing for me sounds generic"); proactively offer to run the interview.
---

# Writing Style Builder

This skill runs a short interactive interview, then writes a personalized writing-style SKILL.md the user can install so future agents (or future Claude sessions) match their voice. The output is the artifact. The interview is the means.

The job has three phases: **interview**, **optional sample analysis**, **generate**. Don't skip phases, but do compress them when the user is moving fast.

## When this triggers

User-facing signals. Any of these should make this skill fire:

- "I want Claude / my agent to write like me"
- "Help me create a writing style skill"
- "My agent's [comments / emails / posts] don't sound like me"
- "Stop sounding like AI"
- "Capture my voice in a skill"
- "Build me a prompt for tone"

If the user asks for help writing *one* thing in their voice (a single email, a single post), that's a different task. Just help with the writing. This skill is for building a *reusable* style asset.

## Operating principles

- **Make it feel like a conversation, not a form.** Batch 2 to 4 questions per turn, not one at a time. Use any available interactive elicitation tool (e.g. `ask_user_input_v0`) when it fits, with clean multiple-choice options. Fall back to numbered options in prose when no such tool is available.
- **Default to multiple choice; ask for free text only when it adds signal.** Open-ended "describe your voice" questions tend to get vague answers. Concrete forced choices ("warm vs. neutral vs. blunt") get usable ones.
- **Skip what's already obvious from context.** If the user has been chatting with you and their voice is already visible (they curse, they write in fragments, they hate bullet points), you've already got data. Skip those questions or confirm rather than asking from scratch.
- **The output is a SKILL.md the user can install.** Not a summary, not a style guide for humans. Frame everything in language another agent will use.

## Process

### Step 1: Frame the conversation

Before launching into questions, tell the user briefly what's about to happen. Keep it to two or three sentences. Example framing (adapt the wording):

> I'll ask a handful of questions about how you write: tone, rhythm, words you love or hate, formatting habits. Takes about 3 to 5 minutes. Then I'll optionally look at a sample or two of your writing, and produce a SKILL.md you can drop into Claude (or any agent that reads skills) so it writes like you going forward.

Ask one upstream question first, because it changes everything else:

**Where will this writing style apply?** Options:

- All my writing (general voice)
- A specific channel (email, Slack, blog posts, social comments, technical docs, marketing copy, etc.)
- A mix; I'll specify

This anchors the rest of the interview. A "social comments" style and a "technical docs" style have different rules; the questions should be read against that context.

### Step 2: Interview

Run the interview in roughly these batches. Adapt order and skip items based on what's obviously already known from the conversation. Don't ask all of these mechanically. Read the room.

#### Batch A: Voice and tone (3 questions)

1. **Formality.** Very casual / conversational / professional / formal.
2. **Warmth.** Warm and personable / neutral / direct and blunt.
3. **Energy.** Playful and witty / even-keeled / serious.

#### Batch B: Stance and confidence (2 questions)

4. **How assertive do you sound?** Hedging and exploratory ("I think maybe...") / balanced / declarative ("Here's what's true...").
5. **First person comfort.** Avoid "I" / use it naturally / lean into it ("I think", "in my experience").

#### Batch C: Rhythm and structure (2 questions)

6. **Sentence length.** Short and punchy / medium and steady / long and flowing / deliberately varied.
7. **Fragments.** Love them, use often / Sometimes for emphasis / Avoid, complete sentences only.

#### Batch D: Vocabulary (3 questions)

8. **Register.** Plain everyday words / mixed / vocabulary-rich and precise.
9. **Jargon and technical terms.** Avoid / use when it adds precision / use freely (audience can keep up).
10. **Profanity.** Never / occasional for emphasis / part of my normal voice.

#### Batch E: Formatting (3 questions)

11. **Structure.** Prose-first, minimal formatting / mix prose and structure / structured and scannable (headers, bullets).
12. **Emphasis (bold, italics).** Rare / occasional / frequent.
13. **Emojis.** Never / very rare / occasional / part of my voice.

#### Batch F: Quirks (2 short text answers)

14. **Words, phrases, or moves you love and want to see.** Free text. Let them list 3 to 10 things.
15. **Words, phrases, or moves you hate and never want to see.** Free text. Same.

If the user can't think of items for 14 or 15, that's fine. Move on.

### Step 3: Optional sample analysis

After the interview, offer this:

> Want to paste 1 to 3 short samples of your own writing? I'll pull out additional patterns I notice (turns of phrase, sentence rhythms, signature moves) and fold them into the skill. Totally optional.

If they say yes:

- Accept whatever they paste. Don't insist on a specific format or length.
- **Read closely, not just thematically.** Look at: average sentence length, paragraph length, punctuation habits (em dashes? semicolons? parentheticals?), favorite sentence openers, transitions, contractions, signature words, where they break grammar rules on purpose, how they handle emphasis.
- **Reflect back 4 to 6 specific observations** before generating the skill. Concrete is the goal: "You use sentence fragments mostly at the end of a paragraph for emphasis" beats "Your writing has personality." Let the user correct or refine your observations.
- Fold confirmed observations into the output skill.

If they say no, skip and move on.

### Step 4: Best practices opt-in

Offer the AI-tells block:

> One more thing: I can include a "common AI tells to avoid" section in the skill. No em dashes, avoid phrases like "It's not X, it's Y", "delve", "you're absolutely right", and so on. Most people want this because it's what makes AI-written text sound like AI even when the rest of the voice is dialed in. Include it?

If yes: load `references/ai-tells.md` and embed its full content as a section in the output SKILL.md. Don't summarize it. The verbatim list is what makes it actionable for an agent.

If no: skip the section entirely. Don't argue. The user may have their own list or may want em dashes (some people genuinely do).

### Step 5: Generate the SKILL.md

Use the template in the next section. Fill it in from the interview answers, sample observations, and the optional AI-tells block.

**Naming.** Default to `<user-handle-or-firstname>-writing-style` (e.g. `kris-writing-style`). If that's awkward, propose a sensible alternative and let the user pick.

**Writing the description (frontmatter).** This is what makes the skill trigger reliably. Write it so it pulls in:

- What the skill does ("apply [Name]'s personal writing voice")
- When it should fire. List concrete contexts from the user's Batch A answer ("emails, Slack messages, blog comments" or "all written output")
- Pushy triggering language: "use this skill whenever you're drafting any prose on behalf of [Name], even if they don't explicitly ask for their voice"

**Body.** Use imperative voice aimed at the agent that will read the skill ("Write short sentences. Avoid em dashes. Open with..."), not descriptive voice aimed at a human ("Kris likes short sentences"). The agent is the audience.

**Include 2 to 3 short before/after examples** if the interview surfaced specific moves (e.g. "instead of 'It's important to note that X' the agent should write 'X.'"). Examples teach faster than rules.

**Length.** Aim for under ~200 lines. Voice skills don't need to be long. They need to be specific.

### Step 6: Save, present, explain installation

Save the finished file to `/mnt/user-data/outputs/<skill-name>.md` (or, if creating a folder skill with assets, `/mnt/user-data/outputs/<skill-name>/SKILL.md`). Use `present_files` so the user can download it.

Tell the user briefly how to install:

> Drop this file into your Claude skills directory (or wherever your agent loads skills from). The frontmatter handles triggering. You shouldn't need to invoke it by name; it'll fire automatically when you ask Claude to draft something.

Offer one round of revision: "Want me to adjust anything before you install it?"

## Output SKILL.md template

Use this as the scaffold. Fill in the bracketed sections from interview answers. Drop sections that don't apply rather than leaving them empty or vague.

```markdown
---
name: [skill-name]
description: Apply [Name]'s personal writing voice to [contexts]. Use this skill whenever drafting [list channels/contexts] on [Name]'s behalf, or any time output will be read as if [Name] wrote it. [Add 1 to 2 sentences of pushy triggering language so the skill fires reliably even when not explicitly invoked.]
---

# [Name]'s Writing Voice

[1 to 2 sentence summary of the voice. E.g. "Direct, conversational, dry. Reads like a thoughtful colleague typing fast, not a brand."]

## When to apply this voice

[Bullet list of contexts. Be specific: channels, audiences, types of content.]

## Voice and tone

- **Formality:** [answer + 1-line elaboration]
- **Warmth:** [answer + 1-line elaboration]
- **Energy:** [answer + 1-line elaboration]
- **Stance:** [hedging / balanced / declarative, with how it shows up]
- **First person:** [how I/me/my are used]

## Sentence rhythm

- **Length:** [preference]
- **Fragments:** [policy and when they're earned]
- **Openings:** [if sample analysis revealed favorite sentence starters, list them]

## Vocabulary

**Favor:**

- [specific words/phrases the user loves]
- [register notes]

**Avoid:**

- [specific words/phrases the user hates]
- [register notes]

[If profanity tolerance was set: one line on policy.]

## Formatting

- **Structure:** [prose-first / mixed / scannable]
- **Bold/italics:** [frequency and when earned]
- **Lists and bullets:** [policy]
- **Emoji:** [policy]
- **Other:** [Oxford comma, en/em dashes, semicolons, etc.]

## Quirks and signatures

[Observed signature moves from sample analysis, if any. E.g. "Tends to land paragraphs on a fragment for emphasis." "Uses parentheticals as asides, not for citations." "Opens push-back with 'Honestly,' or 'Look,'."]

## Examples

[2 to 3 short before/after pairs, each one illustrating a specific rule above. These are the highest-leverage part of the skill. Keep them concrete.]

**Generic AI-flavored:**
> [bland version]

**In voice:**
> [voiced version]

## AI tells to avoid

[Verbatim contents of references/ai-tells.md from this skill, IF the user opted into best practices. Otherwise omit this section entirely.]
```

## Reference files

- `references/ai-tells.md`: Comprehensive list of AI writing tells (vocabulary, phrases, structural patterns) to embed verbatim into the output skill when the user opts into best practices. Don't paraphrase this list when copying it through. The specific wording is what makes it usable by an agent.

## A note on tone for the interview itself

This skill is about helping someone capture their voice. The interview itself should not feel AI-generic. Skip the "Great question!" preambles. Don't summarize back what the user just said before each new question. Don't say "I'd be happy to..." or "Let me know if you'd like...". Just ask the next thing.

And: practice what you preach. While conducting the interview, follow the rules in `references/ai-tells.md` yourself. No em dashes. No "delve". No "It's not X, it's Y". If the skill is going to teach an agent to write like a human, it shouldn't sound like an AI while it's doing the teaching.
