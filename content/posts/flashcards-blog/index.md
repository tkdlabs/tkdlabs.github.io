+++
date = '2025-12-31'
draft = false
title = 'Anki Flashcards - Korean'
tags = ["project-blog"]
+++

I built a workflow to generate Korean language flashcards using AI—combining Claude Code for development, OpenAI's lightweight models for content generation, and Google Cloud TTS for pronunciation. This post covers the technical journey: what worked, what broke, and what I learned about AI-assisted coding along the way.

**TL;DR:** AI coding tools are remarkably productive with the right supervision. Claude Code helped me build 50k+ lines of tooling in two months (including tests, examples, and micro-architecture components). The key insight: treat it like a junior engineer who needs technical direction but can execute fast.

🔗 [Project technical overview](https://tkdlabs.com/projects/korean-flashcards/) | [Live demo](https://korean.tkdlabs.com)

---

## Background: Flashcards and spaced repetition

Flashcards are often used for memorization of new content. It's a simple but powerful concept. A flashcard has a question part, which you reveal and try to guess an answer. Then you'd review the answer and grade yourself how well you rendered it. Based on this grade, a spaced repetition algorithm tries to optimally schedule your next repetition of this question: not too far away that you forget it, but not too close so that this knowledge only remains in your short term memory.

It's a surprisingly powerful way to learn a lot of content in a short amount of time. It's especially useful for exams and language learning. However, you can decompose pretty much any piece of knowledge into Q&A flashcards.

## AI-assisted coding project

I took this as my first project for a couple of reasons:

- I was travelling to South Korea for 3 weeks and wanted to learn their alphabet, and then language.
- I had implemented some building blocks for local AI inference already on my sabbatical in the summer.
- I wanted to build a complete set of AI inference tools I could use for this workflow and reuse for future ones.

But most of all, I wanted to try out building a full project using AI tools. I picked Claude Code with their Pro subscription ($20 a month).

## Anki and schema

For the spaced repetition software, I picked Anki. It's a well-known product with the advanced FSRS spaced repetition algorithm and open architecture.

Anki uses the following schema:

- **Deck** is a collection of your flashcards.
- **Note** is a piece of information in your deck that will be displayed as one or more flashcards.
- **Card** is an instance of a note. One note can have many cards. Imagine a Polish↔English note for the word "car"—one variant presents the Polish word and your goal is to answer in English, the other is English→Polish. The content is the same (note) but displayed differently (cards).
- **Multimedia items** are blobs that belong to a collection and can be referenced from notes.

Additionally, as a deck creator you have full control of the schema of a note. You provide a set of fields that a note should have, and you can create multiple different templates. You also provide HTML-like templates and CSS to control how your cards are displayed.

And here comes the challenge: the templates for notes are sadly global in your Anki installation, so you need to make sure they don't clash with others (or even with your earlier versions of collections). More on that later.

## Building the collection

Anki packages decks into `.apkg` distribution files—essentially a zipped SQLite database along with multimedia blobs.

When Anki installs your `.apkg` file, it merges this into its own database, hence the opportunity for conflicts. It's a nice distribution format, but that merging adds some pain.

For implementation, this was 100% Claude Code effort. It initially used the AnkiGen package to write to a `.apkg` file. Soon enough, due to merging issues, I wasn't able to see updated CSS styles in my collections (the content was always displayed the old way). It took some effort and manual debugging to figure out the problem.

However, I used Claude Code again to write tools like dumping `.apkg` packages and identifying problems from them. In the end, it implemented a complete package editor tool (AnkiGen can only create collections from scratch—it doesn't support editing existing ones). That opened up options for patching existing decks and updating visuals, which was essential for the AI coding cycle.

Overall, coding with AI was extremely fast, but it needed supervision—just like a TL would provide—with code reviews and setting technical direction for what to work on next. With that scaffolding, it can build the right thing fast.

A good example was the package dump tool. AI was creating very hacky and limited ways to investigate problems, eating up time and tokens. I needed to provide a very clear instruction and definition of what an ".apkg dump tool" must provide. With that built, it was easy to direct it to run the tool to investigate issues, and suddenly most of the tough issues were identified and fixed quickly—often on the first attempt.

## Workflow: pick the right LLM for the job

With the tools to create Anki packages ready, it was time to build the content. I created a workflow that evolved into four steps:

1. **Create the JSON outline of notes.** This JSON file is a collection of notes with all the fields populated.
2. **Generate audio pronunciations** for each JSON note.
3. **Generate images** for each JSON note (ended up not using this for Korean flashcards).
4. **Assemble the `.apkg` file.**

Each step can use a different tool:

| Step | Tool |
|------|------|
| 1 | Text LLM |
| 2 | TTS |
| 3 | AI image generator |
| 4 | AnkiGen + my package editor |

## Running it in real life: updates

Running the workflow revealed a few weaknesses that were quickly fixed with Claude Code.

### Step 1: Batching and retries

You can't just generate 3,000 flashcards with an LLM at once—there's an optimal input/output length for each model. Picking the right LLM for this task was interesting:

- **Local LLMs** could struggle. About 80% of the time a 20B model would produce something correctly, but dealing with mistakes was tricky. I feel these smaller models need the shortest batches and simplest instructions.
- **OpenAI's lightweight models** were surprisingly good. While [gpt-5-nano](https://platform.openai.com/docs/models/gpt-5-nano) sometimes struggled, [gpt-5-mini](https://platform.openai.com/docs/models/gpt-5-mini) was reliable for the task. Both are very inexpensive models suited for specific tasks.

With the right prompting, gpt-5-nano—or even a local LLM—could probably work. But I hit the value-of-time issue. It was easier to spend a few cents more on gpt-5-mini than spend hours crafting the ideal prompt for a simpler model.

### Step 2: Sound generation

I switched between OpenAI, Google Cloud TTS, and local methods.

Google Cloud TTS was the winner—it supports many languages and was even good enough for Cantonese, which other providers don't handle well.

However, Google's ecosystem is convoluted. They have two APIs for the same thing: Vertex and Gemini, each supporting different features and authentication methods. It took some effort to direct Claude Code to use the right tools, but since Claude Code produces code quickly, changing things went smoothly.

### Resumability

Over time, learning that popular providers like OpenAI and Google have APIs that aren't 100% reliable, a resumable/retriable workflow became essential.

Adding support for checkpointing and resuming opened up new options—like easily replacing a single sound file that wasn't generated correctly.

Again, LLM coding was powerful here, but it needed guidance. It won't necessarily come up with the best architecture or high-level solution. But if you provide one, it will execute to spec.

## Final thoughts

I gave Claude Code a try and loved it. It built a lot of building blocks for this project that I'll reuse for other LLM integrations.

The productivity blew me away. It produced over 50k lines of Python (plus 20k lines of Markdown docs and 5k lines of frontend code) in the last two months.

I think coding with AI will change what one can build and how one works on projects. With technical guidance, these tools can do amazing things. I'm especially excited about the support tooling that AI can build in hours. Imagine the 20% project at Google that helps engineers have a cool unique dev environment—now open to solopreneurs and small teams thanks to the productivity AI unlocks.