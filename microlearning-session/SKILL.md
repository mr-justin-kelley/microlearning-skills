---
name: microlearning-session
description: Start a microlearning session. Ask for topic, optional source, optional lens, depth, and teaching or testing, then run it (Feynman, Socratic, case study, or rest-set drills). Use when the user asks to start a microlearning or learning session.
---

# Microlearning session

A short guided learning session. It always starts with a quick intake so the session fits what the user wants, then runs one of four methods.

## 1. Intake

Collect five settings. Skip any the user already gave in their first message. Never ask for something you already have.

| # | Setting | Required | Default |
|---|---|---|---|
| 1 | **Primary topic** | Yes | none |
| 2 | **Source**: a book, author or framework to anchor on (e.g. *Thinking in Systems* by Donella Meadows, *The Mom Test* by Rob Fitzpatrick) | No | none: draw on well-established thinking in the field |
| 3 | **Secondary lens**: another field or framework to view the topic through (e.g. product discovery seen through systems thinking) | No | none |
| 4 | **Depth**: broad overview, or a deep dive into one element | Yes | broad |
| 5 | **Learning type**: Teaching (Feynman, Socratic, case study) or Testing (drills) | Yes | ask |

**Round 1: settings 1–4, in one plain-text message.** Number the questions so the user can answer in one reply. Mark 2 and 3 as optional with a short example each. For 4, say they can name an element or ask for suggestions. Keep the whole message to a phone screen.

**Deep-dive suggestions.** If the user wants a deep dive but hasn't named an element, or asks for suggestions, give 4–6 distinct elements of the topic. Each gets a few words on what it covers. If a source is set, draw the elements from it, using its own concepts and chapters. If a lens is set, include at least one element where the lens fits naturally. Let them pick one, or name their own.

**Round 2: learning type.** If the AskUserQuestion tool is available, ask with these four options. If it isn't, use a numbered list.
- Teaching: Feynman (simple explanation, no jargon)
- Teaching: Socratic (I ask, you discover)
- Teaching: Case study (Challenge, Solution, Outcome)
- Testing: Rest-set drills (tap-to-answer quiz between sets)

You can combine the deep-dive pick and the learning type into one AskUserQuestion call (two questions).

**Confirm, then start.** Before starting, give one line summing up the setup:
`Topic · Source · Lens · Depth · Method`, leaving out anything unset. Then start the session in the same message. Don't ask for confirmation.

## 2. Build the subject line

Combine the settings into one subject. Every method below uses it in place of `[topic]`:
- Base: the primary topic, narrowed to the deep-dive element if one was picked.
- Source: "…as taught in *[source]*." Stay faithful to the source's real concepts and terms, and don't invent quotes or page numbers. If you're unsure whether something is in the source, say it's general knowledge rather than attributing it.
- Lens: "…viewed through the lens of [lens]." Use the lens throughout, not as a footnote. Show where the two connect, and where the lens adds something the topic alone misses.

Example: *"Customer interview questioning, as taught in The Mom Test, viewed through the lens of systems thinking."*

## 3. Run the method

### Teaching: Feynman technique
Follow this prompt with the subject line:
> Act as a world-class educator. Explain [topic] using the Feynman Technique. Break it down so a middle-school student could understand it. Avoid all technical jargon. If you must use a complex term, define it immediately with a simple sentence. Use short sentences and a conversational tone.

Session shape:
- Answer in the chat, not in an artifact. Aim for about a 3–5 minute read: several short paragraphs and one concrete everyday analogy.
- If a source is set, explain its core ideas this way, and name the source once at the start.
- End with three takeaways, then ask the user to explain the topic back in their own words. That's the real final step of the Feynman technique. When they do, point out any gaps or fuzzy parts kindly and specifically.

### Teaching: Socratic method
Follow this prompt with the subject line:
> I want to learn about [topic]. Do not give me an explanation yet. Instead, ask me a series of leading questions, one at a time, that will guide me to discover the fundamental principles of this topic on my own. Wait for my response after each question.

Session shape:
- Ask exactly one question per message, then stop and wait. Never answer your own question.
- Start from what the user likely already knows, such as everyday experience, and build toward the core principles of the topic (and source, if set).
- After each answer, acknowledge what's right in a sentence. If they're off track, ask a narrower question to steer them. Don't correct them with a lecture.
- After roughly 5–8 questions, or once they've reached the main principles, end the questioning. Sum up the principles they discovered, crediting their own words where you can. Name the source's terms for those principles if a source is set.

### Teaching: Case study
Follow this prompt with the subject line:
> Provide a detailed case study of a real or hypothetical company that successfully implemented [topic]. Describe the problem they faced, how they applied this concept, and the measurable results they achieved. Break the case study into 'The Challenge,' 'The Solution,' and 'The Outcome.' Inform the user if the case study is fictional or sourced and if sourced, provide the source.

Session shape:
- **Label it first.** Start with "Real case" or "Fictional case."
- **Real cases:** use web search to find a documented example and check the facts. Cite the sources with links. Don't present an unverified detail or number as fact. If nothing solid turns up, switch to a fictional case and say so.
- **Fictional cases:** make the company and numbers plausible, and say clearly that they're illustrative.
- Use the three headings exactly. In The Solution, show how the concept and the source's specific ideas were applied, step by step. In The Outcome, give measurable results, plus one thing that didn't go as planned.
- End with "What to steal": 2–3 lessons the user can apply.

### Testing: Rest-set drills
Load the `rest-set-drills` skill with the Skill tool and follow it. Pass it the topic, source, lens and depth from the intake, and its defaults (6 exercises × 3 sets) unless the user said otherwise. Use the lens and depth to shape what the drills cover. If that skill isn't available, tell the user it needs to be added, and offer one of the teaching methods instead.

## 4. Close

After any method, offer one natural next step in one line, keeping the same settings. Teaching usually leads to testing ("Want drills on this for your next workout?"). Testing can lead to teaching on the items marked Review. Don't recap the session.
