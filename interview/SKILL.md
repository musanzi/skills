---
name: interview
description: Interview you one question at a time, then write an article from your answers.
disable-model-invocation: true
---

Interview the user one question at a time, then turn their answers into an article. The material is the user's own experience: your job is to draw it out, so every fact in the article comes from something they said.

1. **Settle the goal.** Open on the goal before any story or detail: the article type (a story-driven technical post, an opinion piece, or a tutorial or how-to), the reader, the one point, and the takeaway. Done when you can write the **goal statement**, "For <reader>, show <the one point> so they can <takeaway>.", from the user's words, and the user confirms it.

2. **Fill the checklist.** Ask adaptive questions, following the Asking rules below, until every item on the Checklist is backed by something the user said. Each question serves the goal statement; dig into an answer when it is rich, and return to an uncovered item when a thread runs dry. When the user says "write it", go to step 4 with the uncovered items as gaps. Done when every Checklist item is backed by the user's answers.

3. **Confirm the goal.** Show the goal statement again, sharpened by what the interview uncovered. Done when the user confirms it.

4. **Outline.** Propose a title, the angle, and a section outline. If any Checklist item is still thin, list it as a **gap** beside the outline, so the user can fill in or skip each one in a single reply. Skipped gaps stay out of the article. Done when the user approves the outline.

5. **Draft.** Write the article in the language the user answered in, unless they name another, using only the facts they gave. For a tutorial or how-to, call the Skill tool with `technical-writing:technical-writing` and apply it to the draft. Then call the Skill tool with `unslop:unslop` and run its review pass over the draft; in a language it does not cover, apply the rules that carry over. Done when every section of the approved outline is written and the review passes are applied.

6. **Save.** Write the article to `<slug>.md` in the current directory, slug from the title. Report the path with a two-line summary of the article. Done when the file exists.

## Asking rules

- Put exactly one question in each message, headed by a counter: `Question <n> of ~<estimate>`. Revise the estimate as the interview moves.
- Ask the question bare, with no suggested answer. Offer a nudge (an example angle, or what a reader would want to know) only when the user asks for one or gives a thin answer.
- Follow one thread for at most 2 follow-ups, then move to the next uncovered Checklist item.
- **Check in** at question 10 and every 5 questions after: list what is still unclear, and ask "keep going, or write now?". "Write now" goes to step 4 with those items as gaps.

## Checklist

- **Audience**: who reads it, and what they already know.
- **Core point**: the one thing the article argues or teaches.
- **Story or context**: what happened, or the situation that makes the point matter.
- **Specifics**: numbers, code, names, failures, decisions and their reasons.
- **Counterpoints**: where the point does not hold, and what the user would say to a skeptic.
- **Takeaway**: what the reader does or believes differently afterward.
