# Working with AI: a short recap

A summary of the AI part of [Session 6](lectures/06-model-evaluation/). *As of October 2026: model names, plans and limits change fast.*

## 1. Which models to use

- **Top models only: ChatGPT or Claude.** A weaker model saves you money and costs you more in time. For code I prefer Claude.
- **Match the model and the reasoning level to the task.** My default right now is Opus 5.5 with high reasoning, for everything; I don't even find myself needing Fable 5.1. There is no golden rule: there are your limits and a feel for it that comes with practice.

## 2. How to state a task

**Write "context + task".** Without context the model produces nonsense. Explain what you have, why you need it and what result you expect.

**Show an example.** One sample of the result you want often works better than a paragraph of explanation.

**Not sure how to phrase it? Write your stream of thought.** Dump everything into the chat as it is, even if your fifth sentence contradicts the first: the model will sort it out. Then ask it to critically check and structure your thoughts. Read the result and add whatever is missing. AI is excellent at turning a chaotic pile of ideas into a plan, a diagram and a sequence of steps.

**Ask the model to ask you questions.** Let it keep asking until it is sure you both understand the task the same way. The questions also help you shape and solve the task yourself. This works great with Claude. GPT tends to fire off 50 points at once, so ask it for 3–5 questions at a time.

**Don't argue with the model in circles.** If after 2–3 corrections it keeps going the wrong way, start a new chat with a sharper prompt.

## 3. How to check the result

**Check in a new chat, ideally with a different model.** A new chat means a clean context: the model looks at the work as someone else's and is more critical of it, especially if you directly ask it to find mistakes. Checking in the same chat is often useless, because the model skips its own errors. GPT and Claude are great at catching each other's slips.

**Remember that the context window is limited.** Keep everything in one chat and the model starts to get dumb. New subtask, new chat. When a chat has grown long, ask the model for a summary and start a new chat with it.

**Check facts and sources.** Models invent references to papers, numbers and quotes, and Deep Research makes mistakes too. For a thesis this is critical: open and check every reference.

**Think about how to check the result, not about every nut and bolt.** The key question: what metric or check will confirm the result behaves the way you expect? Once you know what to validate and how, the speed at which you build new things grows many times over.

> A head of development usually doesn't know exactly how a senior developer with ten years of experience wrote their piece of code. But they know how to check the work without reading every line and still be 99% sure of the result. I'm convinced this is exactly the right way to work with AI.

A caveat: this is mostly about code. Understanding the details is useful, and for learning it is essential, so while you are studying, ask AI to explain rather than to solve things for you. But over time the focus will shift towards building the right way to evaluate.

## 4. Tools: move beyond the web chat

**Agents: Claude Code / Codex.** An app where an agent works directly with your files. It has dozens of times more capabilities than the basic chat.

**Claude Cowork / ChatGPT Work.** The same idea for tasks without code: connect a folder with your files, refer to them, and the agent finds what it needs.

**Claude Design: presentations.** Open it from the Claude web app.

1. Drop in screenshots and examples of what you like.
2. Connect a folder with your materials from your computer.
3. Ask it to build the presentation, and ask for several design variants at once so you can pick the best.
4. Edit. Be a demanding client: say everything you don't like and how you want it. It will do it; you only have to say so.
5. Take the result in whatever format suits you, ideally PDF or HTML.

**Deep Research.** Cheaper in ChatGPT: in the web app on the $20 plan it is almost unlimited.

**Skills.** Turn recurring routine into a Claude Code / Codex skill:

1. Go through the whole flow by hand: send a document, ask the model to look at it, tell it what came out wrong, and repeat "answer → question → answer" five times.
2. Once the final result satisfies you, ask it to turn this conversation into a skill.
3. If you use it all the time, make the skill global (available in every project). If you only need it here, make it local.

Examples: checking lectures, a weekly report, taking apart a problematic plan, reviewing a thesis chapter.

**Browser use and computer use.** Agents are great at working with a browser. Need to check a website or a UI design? Ask it to start a local Python server and click through every button. For debugging a desktop app, computer use in ChatGPT is more convenient: you can give it access to a single app, and it will click anything there.

**Subagents.** Parallelize work where it makes sense (mind your limits), and tell Claude/GPT so directly. Ask for weaker models on simple subtasks, Sonnet is usually enough. To take apart an unfamiliar repository with thousands of lines, keep Opus. The number of subagents is limited only by your enthusiasm and your limits.

**Your own tools.** Cut or merge PDFs, pull the audio out of a video, trim audio, find something across 592 PDFs: don't do it by hand. Ask for a tool and keep using it. Any feature can be added; you only need to describe what you want.

## 5. What to install

VS Code + Python + Git make Claude Code / Codex much stronger. The [setup guide](lectures/00-precourse/00-setup.md) walks through the first two.

- **VS Code.** A convenient way to see the whole project. Installs in a couple of clicks, and YouTube has plenty of videos on setting it up.
- **Python.** Download it from python.org. On macOS it is easier to install through Homebrew. On Windows, make sure Python is added to PATH (the installer has an "Add python.exe to PATH" checkbox). If something doesn't work, ask an LLM.
- **Git.** Version control. Of everything it can do, you need two things: commits, and pushing to a private GitHub repository so you never lose your code. Commit before you let an agent loose on your files: Git then works as an undo button if the agent breaks something. Ask Claude/GPT to explain it and help you set it up.

## 6. Be careful

- **Don't upload to AI** personal data, your employer's confidential data, or anything under NDA.

## 7. Use cases

**Anything that feels hard or unclear** ("I need some kind of server", "I'd have to code something", "I need to show it somehow"): bring it to AI and ask for help figuring it out. A large share of your tasks can be done at the same quality, just many times faster.

**Stock price data** adjusted for splits, dividend gaps and exchange data errors:

1. Claude Code: ask it to collect the data.
2. A new chat: ask it to design a validation and run it.
3. Another chat: run the validation results through subagents and take apart every anomaly.
4. Claude Design: build the report.
5. Claude Code: write a project that rebuilds this report daily or weekly.

What used to take a year now takes an evening.

**A thesis, but no idea where to start:**

1. In a regular chat, dump all your thoughts on the topic and ask it to ask you questions. Answer a few.
2. Ask it to turn this into a Deep Research prompt: find all the questions and directions on your topic.
3. Run the research in a separate chat.

**And anything else you like:** to-do notes, a converter, a data collector, picking a fridge.
