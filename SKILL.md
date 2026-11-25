---
name: showme
description: Draw how something in this project works, on one page in the browser.
argument-hint: "[question]"
disable-model-invocation: true
allowed-tools: Write(//tmp/showme/**) Edit(//tmp/showme/**) Bash(showme *)
effort: low
---

Question: $ARGUMENTS

If the question is empty, use what we last discussed.

Pick one concrete run that answers it: one request, one frame, one message. Follow it through the code, from what starts it to where it ends.

Use what you already know from this conversation first. Read only what is missing, several files per turn. Do not use subagents.

Then, in one response, write `/tmp/showme/${CLAUDE_SESSION_ID}/<short-name>.txt` with Write and run `showme "${CLAUDE_PROJECT_DIR}" "<that path>"` with Bash. The file:

<the question, copied word for word as the user typed it, on one line>
<the short answer, at most 160 characters>
<optional note, at most 160 characters: what you did not check, or what depends on a setting. Leave the line out if there is nothing to say>

1 <place>
<what happens, at most 28 characters>
<how or why, at most 140 characters>
<file path from the project root>
<code copied from one line of that file>

2 <place>
...

- 2 to 9 steps, numbered in the order they happen. One blank line between steps, none inside a step.
- A place is where the step runs: a process, a module, a service. 1 to 4 places, at most 18 characters. Repeat the exact name.
- The code is the line that does the step or hands it on. The part you copy must be on only one line of the file.
- A step leads to the next one. If it does not, or it can also jump elsewhere, add a line for each: `> 3`, `> 7 cache miss`. A jump needs words that say when, at most 20 characters. A lone `>` ends the run.
- Link two steps only if the second really happens after the first. If the question asks for a list, a count or a comparison, end every step with `>`. Then each step is one item, and the places are the things you list or compare.
- At most one jump per step and three in all.
- Leave out a step if the answer is the same without it.

## How to word it

A person reads this page. Picture a friend at a bar who asks how this thing works. There is no laptop on the table. After each bit the friend says "ok, and then?", and the next step is your reply. Write those replies.

- Say who does what, with everyday verbs: takes, sends, checks, saves, waits, skips. Use "you" for the person who uses the project.
- Use contractions. Keep sentences to 15 words at most, and let some be much shorter.
- Use only words the friend already knows. When a technical word matters, say what the thing does instead. Place names follow this rule too.
- Keep names of functions, variables, files and maps out of the sentences. The code is right beside them.
- Every number in a sentence must come from that step's code. The page shows the line you copy and the 3 after it, so copy the line with the number. Changing units is fine.
- Keep the facts: the real number, the real condition, what happens when it fails. Simple wording must stay true to the code.
- Give a fact in every sentence. No summaries, comparisons, jokes, "not X but Y", colons, semicolons or dashes.

Stiff: The scheduler debounces input events prior to invoking the search handler.
Said: It waits until you stop typing for 0.3 seconds. Then it searches.

Stiff: Playback resumes from the persisted offset, with a fallback to zero when absent.
Said: It starts where you stopped last time. If it's your first listen, it starts at zero.

Titles work the same way. "Resolve track metadata" becomes "Looks up the song name". These show the tone only. Take your words from this project.

Before you write the file, say each line in your head to that friend. If it sounds like documentation, say it again.

If `showme` prints problems, fix those lines with Edit and run it again. Your whole reply is the path it printed.
