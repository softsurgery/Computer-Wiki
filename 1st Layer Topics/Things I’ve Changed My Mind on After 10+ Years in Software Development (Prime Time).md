Four years ago, I wrote about how my views on software had evolved. A recent email reminded me to check back in. So here we go—lessons learned, opinions evolved, and beliefs still held tight after over a decade in the industry.

---

## 🧠 **Simplicity vs. Complexity: Still a Tough One**

- **Simplicity takes effort.** It's not the default. Writing "simple" code demands discipline and constant pruning.
    
- The **definition of simplicity** still eludes me. Rich Hickey (creator of Clojure) talks about it a lot, but honestly? It often feels abstract.
    
- **Locality of behavior** is my current working model: if I can understand 10 lines without jumping through hoops, it’s simple—even if it's doing something hard (e.g., implementing Mulberry32, a PRNG).
    
- That said, **sometimes abstraction helps**—even if it adds complexity. My own abstraction for Twitch interactions made things testable and extensible. So, was that overengineering? I don't think so.
    
- **Real clarity often comes in hindsight.** Foresight in software design is incredibly hard.
    

---

## 🧑‍💻 **Types, Tooling, and Language Preferences**

- **Typed languages are essential** on teams with varying experience levels—_but only_ with good tooling (like modern LSPs).
    
- Once, I thought JavaScript was fine. Now, I see TypeScript as a big productivity booster—thanks to its tooling.
    
- **Python works too**, even for teams with juniors, but it's a different trade-off—type hints aren't full types.
    
- **Java? Better than people think.** Yes, the “enterprise” abstraction obsession can get in the way, but it’s not the language’s fault.
    

---

## 🧪 **Design, Code, and Testing**

- **Exploration is design.** The idea that you should fully design software _before_ writing a line of code doesn’t hold up. Especially for experienced devs, writing code is often part of the thinking process.
    
- **Formal planning helps junior devs.** Encouraging mental modeling before diving in can be a great learning tool.
    
- **REPLs are valid design tools.** Period.
    

---

## 🧱 **Front-End, Frameworks, and Fatigue**

- Yes, front-end has become **Kafkaesque**. But that might be a tooling problem.
    
- If you hate it, **try something different**: use Web Components, go old-school with HTMX, avoid the React-Redux boilerplate mess.
    
- Not all front-end has to be terrible.
    

---

## 🧰 **Tools, Databases, and Abstractions**

- **DynamoDB is insanely fast**, but also overkill for most.
    
- **SQLite is underrated.** Most projects don’t need the scale they think they do.
    
- **ORMs? Mixed bag.** They’re okay for simple CRUD. But the moment you do joins or anything non-trivial—you'll wish you wrote SQL directly.
    
- **Microservices should not be the default.** They require strong justification. Many teams cargo-cult them and suffer for it.
    

---

## 🛠️ **Code Quality, Comments, and Coverage**

- **You can't comment too much... in test code?** I disagree. Comments should be rare, precise, and high-signal. Document APIs, sure. But don’t comment every line.
    
- **Elegance is not a real metric.** What does it even mean? Clarity and purpose matter more.
    
- **Code coverage is not code quality.** Some coverage is useful. Zero coverage is dangerous. But chasing 100% often leads to waste.
    
- **Consistency in code style?** Meh. Not the hill I’d die on. Just write readable code and move on.
    

---

## 💼 **Teams, Management, and Soft Skills**

- **Good managers are rare—and invaluable.** Most of my career, I worked without strong leadership. But when you have a good one, everything improves.
    
- **Soft skills matter.** Public speaking, clear communication, mentoring—all game-changers. Take a course if you need to.
    
- **Most project managers?** Not adding value. A few good ones are critical on massive projects. The rest... probably expendable.
    

---

## 💡 **Beliefs That Still Hold Strong**

- **Monoliths are still great.**
    
- **Code coverage != quality.**
    
- **You can build great software in many ways.**
    
- **Focus on business value, not perfect code.**
    
- **Most people don’t care about code quality—cherish the ones who do.**
    
- **You don’t need 50 services and 20 DBs. Just write what you need.**
    

---

## 🧵 Final Thoughts

After 10+ years, my biggest learning is this:

> **“Most truths in software are only visible in hindsight.”**

You can’t plan your way into perfect architecture. You iterate. You screw up. You learn. Then, you look back and go, “Oh, _that’s_ what simple means.”

And that’s okay.

---