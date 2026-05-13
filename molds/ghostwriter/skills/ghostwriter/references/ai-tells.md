# AI tells to avoid

This is the verbatim block to embed in the output SKILL.md under "## AI tells to avoid" when the user opts in. Copy it through as-is — the specific phrasing is what makes it actionable for an agent.

---

## AI tells to avoid

These are patterns that make AI-generated prose sound like AI even when the rest of the voice is dialed in. Avoid them.

### Punctuation

- **No em dashes (`—` or `--`).** Use a comma, parentheses, a colon, or a new sentence. This is the single most recognizable AI tell.
- **No "elegant" semicolons strung together** to glue independent clauses; if a sentence wants a break, give it a period.
- **Don't bold whole phrases inside a sentence for "emphasis"** — write the sentence so the emphasis lives in the word order.

### Forbidden phrases (do not emit, in any variant)

- "It's not X, it's Y."
- "It's not just X — it's Y." (or any em-dash variant)
- "A isn't about X, it's about Y."
- "Not only X, but Y."
- "It's not merely / simply / just X."
- "You're absolutely right." (or "absolutely correct", "spot on", or any sycophantic confirmation opener)
- "There's a kind of X that isn't Y. It doesn't come with..."
- "One of the most important things to consider is..."
- "In today's fast-paced world..."
- "It's worth noting that..."
- "In conclusion," / "In summary," / "To sum up,"
- "Let's dive in." / "Let's delve into..."
- "I hope this helps!" (as a closer)
- "Feel free to..." (as a closer)
- "Great question!" (or any preamble that compliments the prompt)

### Forbidden vocabulary

These words appear far more often in AI text than in natural human writing. Reach for the everyday alternative.

- delve, delving
- etched
- tapestry (especially "rich tapestry of...")
- intricate
- pivotal
- underscore (as a verb)
- landscape (as a metaphor: "the AI landscape")
- foster (as a verb, unless literally about fostering)
- testament ("a testament to...")
- enhance, enhancing
- crucial (especially "crucially")
- robust (especially "robust solution")
- leverage (as a verb)
- seamless, seamlessly
- navigate (as a metaphor: "navigate the complexities")
- realm, realm of
- paradigm, paradigm shift
- ever-evolving, ever-changing
- multifaceted
- nuanced (when not adding actual nuance)

### Structural tells

- **No bullets where the bold prefix just restates the next sentence.** This pattern:
  > - **Scalability:** The system scales easily across use cases.
  > - **Flexibility:** It is flexible for different needs.

  …is a giveaway. Either drop the bold prefix or drop the bullet entirely and write prose.
- **No triplets.** AI defaults to "innovative, transformative, and groundbreaking" — three adjectives, three benefits, three takeaways. Use one strong word, or two if needed, almost never three.
- **Vary paragraph length.** AI produces uniform 3–4 sentence blocks. Mix in one-sentence paragraphs. Let some run longer.
- **Vary sentence length.** Same idea at the sentence level.
- **No "On one hand... on the other hand" both-sides padding** when you actually have a view.
- **Don't open every paragraph with a topic sentence that previews what's coming.** Sometimes start with the thing itself.

### Tone tells

- **No reflexive admiration.** Words like "famous", "iconic", "renowned", "celebrated", "esteemed" attached to anything — a person, a building, a company — are almost always AI reverence. Drop them.
- **No fluff that takes a position on the importance of the topic** instead of taking a position on the topic. "X is an important and complex issue" says nothing; say what you think about X.
- **No hedging stack.** "It could potentially be argued that perhaps..." — pick one hedge if you need one, or drop them all.
- **Specifics over abstractions.** Replace "a wide range of stakeholders" with the actual stakeholders. Replace "various challenges" with the actual challenges.

### Elements of Style discipline

Apply Strunk & White's old rules ruthlessly:

- **Omit needless words.** If a sentence reads fine with a word removed, remove it.
- **Use the active voice.** "We shipped the feature" not "the feature was shipped."
- **Use definite, specific, concrete language.** Concrete nouns beat abstract ones. Specific numbers beat "many".
- **Put statements in positive form.** "He usually came late" not "he was not very often on time."
- **Don't explain too much.** Trust the reader to follow.

### Self-check before emitting

Before sending any drafted prose, scan the output for: em dashes, the words above, the "It's not X, it's Y" pattern, sycophantic openers, and bolded bullet-prefix lists. If any are present, rewrite.
