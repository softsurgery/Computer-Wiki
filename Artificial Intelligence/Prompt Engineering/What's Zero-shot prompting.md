**Zero-shot prompting** is a technique where you ask an AI model to perform a task **without providing any examples** of how to do it. The model relies only on the instructions in your prompt and the knowledge it learned during training.

### Example

**Prompt:**

> Translate this sentence into French: "Good morning, how are you?"

**AI response:**

> Bonjour, comment allez-vous ?

No examples of translations were given beforehand, so this is **zero-shot** prompting.

### Comparison with Other Prompting Methods

|Method|What you provide|Example|
|---|---|---|
|**Zero-shot**|Instructions only|"Classify this review as positive or negative."|
|**One-shot**|One example|Show one labeled review, then ask for another classification.|
|**Few-shot**|Several examples|Provide multiple examples before asking the model to do the task.|

### Another Example

**Zero-shot sentiment analysis**

Prompt:

> Determine whether this review is positive or negative:  
> "The movie was beautifully filmed, but the story was boring."

Possible response:

> Negative

### Why use zero-shot prompting?

- **Simple and fast** — no need to create examples.
- **Flexible** — works for many tasks (summarization, translation, classification, coding, brainstorming, etc.).
- **Efficient** — useful when you don't have labeled examples available.

### Tips for effective zero-shot prompts

- Be clear and specific.
- State the desired format.
- Include constraints if needed.

For example:

> Summarize the following article in three bullet points written for a high school audience.

This is still zero-shot because you're giving instructions, not examples.