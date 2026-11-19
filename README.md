[README (1).md](https://github.com/user-attachments/files/35063809/README.1.md)
# Evidence Sorter v2

**Turns one customer-discovery interview transcript into draft, quote-backed evidence tagged to your assumptions. You review and confirm every row before exporting it.**

**Intended user:** a graduate student or early-stage founder running sponsor-backed B2B discovery interviews. After each call they must pull exact quotes from a long transcript, match them to many overlapping assumptions, keep observation separate from interpretation, and update a tracker by hand.

> **AI status: simulated (Wizard-of-Oz).** Keyword rules stand in for a language model. Quote verification, review, confirmation, and export are fully functional. See [AI role and status](#ai-role-and-status).

## Try it

- **Replit (running prototype):** `[ADD REPLIT LINK]`. Open it and click **Run** if it isn't running already. No sign-in or keys are needed.
- **Without Replit:** download this repository and open `index.html` in any browser.
- **Steps:** click **Sort My Interview**. Paste your own transcript and assumptions (or click a fictional sample, such as *Typical 1*), then click **Analyze**. On each evidence card, click **Accept…**, read the surrounding lines, choose a signal and confirm. Then check the CSV preview.

All sample data is fictional. Nothing is sent anywhere or saved, and refreshing the page clears everything.

## Demo Video

`[ADD GOOGLE DRIVE LINK]`. It is shared with viewing access for Emelia Hughes and Samuel Huang. The narration script is in [Demo script](#demo-script).

## Assignment components

| Component | Where |
|---|---|
| Current prototype code and assets | [`index.html`](index.html) (built page), with source in [`src/`](src) |
| Priority improvements | [§1](#1-priority-improvements) |
| Response to peer feedback | [§2](#2-response-to-peer-feedback) |
| Five-plus test cases, before/after comparison, and interpretation | [§3](#3-evaluation), with raw output in [`tests/`](tests) |
| Risk response | [§4](#4-risk-response-false-confidence-from-the-tools-silence-or-labels) |
| Remaining limitations and next priorities | [§5–6](#5-remaining-limitations-ranked-by-effect-on-the-central-task) |
| Draft AI-use disclosure | [AI-use disclosure](#ai-use-disclosure-draft) |
| Demo video | [Demo Video](#demo-video) |

## What changed in v2

1. **Fewer missed and mislabeled quotes.** Better rules now handle negation ("never delayed"), "pushed … back", and backup-stock wording. The gaps panel warns that the rules can miss evidence worded differently.
2. **You set the signal.** Accept shows the surrounding lines and asks you to choose Supports, Challenges, Mixed, or Unclear. Only quotes found in your transcript are ever shown.
3. **Clear, simple flow.** Three screens (welcome, your interview, your evidence) with one action each. The page opens empty so you paste your own transcript, and each piece of evidence is a card instead of a wide table. Highlighted words show why each quote matched. A status line stays visible after each action, and answers to leading questions are flagged.

## AI role and status

| | |
|---|---|
| Task | Extract exact quotes, match each to an assumption, and suggest a signal |
| Input | Transcript with speaker labels, and assumptions as `ID: statement` |
| Output | Draft evidence rows, each quoting the transcript word for word |
| Works now | Exact-quote verification, evidence review cards, context view, confirm-to-accept, CSV export, empty-input messages |
| Simulated | Choosing quotes and suggesting signals (keyword rules in [`src/core.js`](src/core.js)) |
| Planned | A language model proposes quotes and signals with a short reason. The quote check and your confirmation stay |

## Test summary

Full table: [§3](#3-evaluation).

| Case | Result |
|---|---|
| Typical 1: OEM engineer | Pass |
| Typical 2: CDMO quality director | Partial. All 5 suggested signals were wrong or Unclear; 3 relevant lines were missed |
| Challenge A: messy transcript | Pass |
| Challenge B: paraphrased interview | Fail (1 of 6 answers found); nothing false shown |
| Invalid / empty | Pass |
| Risk: leading questions | Pass |

Re-run the tests with Node 18+ and Playwright: `node build.js && node tests/run-ui-tests.js && node tests/before-after.js`.

## Repository layout

```
index.html            built page (open this, or let Replit serve it)
src/                  core.js (rules + verification), samples.js (fictional), index.src.html (UI)
build.js              inlines src/ into index.html
tests/                inputs/, scripts, results.json, before-after-results.json, screenshots/, baseline/ (v1 rules)
docs/                 the same sections as separate files
```

## Privacy

The prototype runs entirely in the browser. It does not store or transmit transcripts, and there are no accounts, database, or API keys. Use fictional or de-identified text only.

---

## Prototype Revision: Evidence Sorter v2

All interview text in this document and in the prototype is fictional. Classmate names are left out.

### What the AI is for, and how it works today

| | |
|---|---|
| **Task** | Extraction and classification. The AI reads one interview transcript, picks the exact sentences that bear on the user's assumptions, matches each to an assumption, and suggests whether it supports or challenges it. |
| **Input** | Transcript text with speaker labels, and the user's assumptions in the format `ID: statement`. |
| **Output** | Draft evidence rows: line number, exact quote, assumption ID, suggested signal (Supports / Challenges / Mixed / Unclear), match strength, a leading-question flag, an observation, and a draft interpretation. Only sentences found in the transcript are shown. |
| **Value** | Pulling evidence out of a 3,000–6,000 word transcript and checking it against many overlapping assumptions is slow and easy to get wrong by hand. The tool gives the user a short list to check, with every quote traceable to the transcript. |
| **Status** | **Simulated (Wizard-of-Oz).** Keyword rules in `src/core.js` stand in for the model. They run for real, in the browser, on any text you paste, but they are not AI. **Planned:** a language model proposes the quotes and labels. The exact-quote check, context view, and user confirmation stay as they are. |
| **Functional now (not AI)** | Exact-quote verification, the review table, context view, confirm-to-accept, CSV export, empty-input handling. |
| **Blocker for live AI** | A live model in Replit needs an API key stored in the project and a server route to call it. I kept this milestone client-side so no transcript leaves the browser and no key goes near a public repo. Adding a server-side model call is my first priority for the final. |

### 1. Priority improvements

I picked three changes from the peer reviews and my own first-slice tests. Each one targets the central task: turning one transcript into evidence the user can defend.

1. **Fewer missed and mislabeled quotes.** The rules now handle negation scope ("never delayed" challenges; "delayed" supports), phrases like "pushed … back", and backup-stock wording ("extra lots on the shelf"). Statements that cut both ways are marked **Mixed**. The old "Still needs evidence" panel is now **No linked evidence yet**. It warns that the rules can miss evidence worded differently, and weak matches no longer count as coverage.
2. **Signals the user sets.** Accepting a row now opens the surrounding transcript lines and asks the user to **choose the signal themselves**. Only rows the user confirms can be exported.
3. **A simple, clear flow.** The page is now three screens: welcome, your interview, your evidence. It opens empty so users paste their own transcript, and each piece of evidence is a readable card instead of a wide table. The words that made a quote match are highlighted, so the user can see why it was suggested. A status line stays visible after each accept, reject, or edit (for example: "Confirmed line 4 as Supports for P1. 1 row ready to export."). This README and the test record are new.

A smaller addition, part of the risk response in section 4: rows that answer a yes/no or confirming question from the interviewer are flagged **Answer to a leading question (L#)**.

### 2. Response to peer feedback

Three classmates tested the first working slice.

| Feedback or observed issue | Decision | What changed, or why it was deferred |
|---|---|---|
| No README, test cases, or limitations were included with the file (all three reviewers). | Acted on | Added `README.md`, this file, an automated test script, and a test results file. |
| "Still needs evidence" can tell the user an assumption is untested when the matcher simply missed it. Suggestion: list unmatched interviewee lines so the user can assign them (top recommendation from two reviewers). | Adapted | I built the suggested list of unmatched lines with a manual Link control, then removed it. In my own review it made the flow harder to follow, and it mixed non-evidence (greetings, small talk) in with evidence. The tool now shows only quotes it finds in the transcript. Instead, I reworded the gaps panel to say plainly that the rules can miss evidence worded differently, stopped counting weak matches as coverage, and improved the rules so the specific lines reviewers named now appear. |
| Keyword matching misses evidence worded differently: "we keep extra lots on the shelf" (backup stock) and "pushed our validation schedule back" (all three reviewers). | Adapted | Added synonym groups and phrase cues, so both test lines now produce rows. True paraphrase handling is **deferred to the live model**: the paraphrase test (Challenge B) still fails. |
| Strong P1 evidence (a six-week lead time delaying a project) was labeled Unclear, and "Lead times have never delayed a single project" produced no row (two reviewers). | Acted on | Added the negation-scope rule and the Mixed label. Both lines now get rows with the right suggestion (Supports and Challenges). The user now chooses the final signal, so a wrong suggestion can't reach the export unless the user confirms it. |
| The confirmation disappeared right after accepting or rejecting, so it was hard to tell whether the action worked (one reviewer). | Acted on | Added a status line that stays until the next action, and a lasting "You confirmed" label on accepted rows. |
| Suggested test: paraphrase each sample answer and check whether the same rows appear (one reviewer). | Acted on | Added as Challenge B. It shows the main limit of keyword rules. |
| Improve phrase recognition in general (one reviewer). | Deferred | Hand-written rules don't scale. Each new phrase fixes one case and risks new false matches. This is the job the planned language model would do. |
| Keep the exact-quote verification gate (praised by all three reviewers). | Kept | Unchanged, and re-tested (see "Regression" below). |

### 3. Evaluation

**Method.** `tests/run-ui-tests.js` drives the real page in headless Chromium and saves what happened to `tests/results.json` and `tests/screenshots/`. All inputs are fictional and stored in `tests/inputs/`. Because the analysis is simulated, these tests check whether the **flow and safeguards** work and whether the example outputs are useful. They **do not measure model accuracy**, since there is no model yet. No live model was run, so the three-repeat variability check does not apply.

| Case | Input or action | Expected result / success criterion | Actual result | Pass, partial, or fail | Implication or next step |
|---|---|---|---|---|---|
| **Typical 1** | Sample "Typical 1": OEM process engineer, 9 answers. Confirm one row, reject another, then export. | A verified row for each assumption the interviewee discusses (P1, P2, U1, T1, E1). Every quote word for word. Nothing exportable until the user confirms a signal. | 7 rows, all quotes verified. P1 (L4, L6), P2, T1, and E1 suggested Supports; E1 flagged as an answer to a leading question. U1 suggested Unclear. One weak, irrelevant match (L2, the speaker's job title). Two relevant lines were not found: a P2 line (L9) and a Mixed clamp line (L5). CSV empty until confirmation. The status line persisted after confirm and reject, and the download worked. | **Pass** | The flow meets its criteria. Labels are still rough: U1 should lean Challenges ("quality and engineering decide"). |
| **Typical 2** | Sample "Typical 2": CDMO quality director. | Rows for T1, P2 (supports), U1 (challenges: quality has veto power), E2 (supports: buys through distributors), and P1 (challenges: lead times "mostly fine now"). | T1 row (Unclear). Two P2 rows, but L7 was suggested **Challenges** (the rule read "Rarely" as negation) and L8 Unclear. P1 got only a weak Unclear row. The U1 veto line, the E2 distributor line, and a second T1 line were **not found**, so U1 and E2 appear under "No linked evidence yet". | **Partial** | Suggested signals were wrong or Unclear on all 5 rows, and 3 relevant lines were missed. The confirm step keeps wrong labels out of the export, but this is the clearest evidence that the matching job needs a language model. |
| **Challenge A** | Sample "Messy": voicemail greeting, filler, and swapped speaker labels (Speaker A is the interviewee). | Detect Speaker A, skip noise, keep rows reviewable, and surface the "extra lots on the shelf" line for P3. | Detected Speaker A. Skipped 7 noise sentences. 4 rows, all verified; the backup-stock line became a P3 row (weak, Supports). A price line that bears on E1 was not found. P3 still shows under "No linked evidence yet" until confirmed, because weak matches don't count. | **Pass** | Fixes the reviewers' main miss. Weak-only assumptions now sit in the gaps list with an explanation rather than silently counting. |
| **Challenge B** | Typical 1 rewritten in different words (`tests/inputs/paraphrase.txt`), suggested by a reviewer. | The same assumptions get rows as in Typical 1. | **1 weak row** (T1, wrongly suggested Challenges because of "won't … without"). The other 5 answers were not found, so all 7 assumptions appeared under "No linked evidence yet", with the warning that the rules can miss evidence worded differently. | **Fail** | Keyword rules don't handle paraphrase. Nothing false was shown, but a user who skims could wrongly conclude "no evidence". This is the main case for the live model. |
| **Invalid / empty** | (a) Empty transcript. (b) Empty assumptions. (c) Transcript with no speaker labels, and assumptions with no IDs. | Clear message saying what to fix, and no invented rows. | (a) "Paste an interview transcript first…" (b) "Add at least one assumption, one per line…" (c) A warning that every line will be treated as the interviewee, IDs auto-assigned (A1, A2), and 1 verified row from the only matching sentence. | **Pass** | (c) can still pull in the interviewer's words when labels are missing. The warning says so. |
| **Risk** | Sample "Leading questions": the interviewer states the hypothesis, asks yes/no questions, and gets a one-word "Yeah." and a hedged "Sure, I guess…". | The interviewer's own statement never becomes evidence. "Yeah." is not evidence. Prompted answers are flagged. The user can't accept without choosing a signal. The CSV records the flag. | The interviewer's statement (which contains every P1 keyword) was not a row. "Yeah." was skipped. Both prompted answers were flagged (L3, L7). The hedged L3 was suggested Mixed. "Confirm and accept" stayed disabled until a signal was chosen. The CSV included "Possibly prompted by leading question: Yes (line 2)". | **Pass** | The hedge "I guess" in L7 is not detected, so that row is still suggested as plain Supports. The flag is a rule, not judgment. |
| Extra: off-topic | Sample "Off-topic": supplement manufacturer. | Few or no rows, and nothing invented. | 0 rows, and a message that no matching quotes were found. The gaps panel warns that the rules can miss evidence worded differently. | **Pass** | Nothing invented. |
| Regression: quote gate | Edit a confirmed row's quote into text not in the transcript. | The row turns unverified, can't be accepted, and leaves the export. | "✗ Not found word for word". The status line explained why. Accept was disabled. Export rows went to 0. | **Pass** | The gate the reviewers valued still holds. |

Layout was also checked at a 400px phone width: no sideways scrolling.

#### Before and after (same inputs, v1 rules vs v2 rules)

`tests/before-after.js` runs the v1 rules (`tests/baseline/core-v1.js`) and the v2 rules on identical inputs. Results are in `tests/before-after-results.json`.

| Issue from peer review | Input | v1 result | v2 result | Did it help? |
|---|---|---|---|---|
| Lead-time delay labeled Unclear | Typical 1, L6 ("six-week lead time … delaying a project") | Row for P1, signal **Unclear** | Row for P1, strong match, suggested **Supports** | Yes |
| "Pushed … back" not recognized | Same sentence ("pushed our validation schedule back") | Counted only as an Unclear row | Counted as a delay cue: **Supports** | Yes |
| Backup-stock line missed; P3 shown as needing evidence | Messy, L10 ("we keep extra lots on the shelf") | **No row, not shown anywhere**; P3 listed as needing evidence | Row for **P3** (weak, Supports). P3 stays in the gaps list until confirmed, with a note to check | Mostly. The line is now visible, but the match is weak |
| Counter-evidence lost | Typical 1 plus "Lead times have never delayed a single project for us." | Row for P1, signal **Unclear** | Row for P1, suggested **Challenges** | Yes |

**Caveat.** The v1 baseline here is my saved first-slice rule file. The reviewers tested the HTML file I submitted, and some of their results differ slightly. For example, one reviewer saw no row at all for the "never delayed" line, while my baseline gives an Unclear row. Their observations are summarized in section 2 as they reported them.

#### What the evidence tells me

The flow now works end to end, and the safeguards do what they're meant to. Every exported quote is verified. Nothing reaches the CSV without the user choosing its signal. Interviewer statements and one-word answers don't become evidence, and the tool never shows anything that isn't in the transcript. What stays unreliable is the matching and labeling itself: Typical 2 got a wrong or Unclear suggestion on all 5 of its rows and missed 3 relevant lines, and the paraphrase test found 1 of 6 answers. So the evidence supports the **workflow** value I claim (traceable, reviewable evidence, faster than starting from a blank tracker). It does **not** yet support the claim that the tool finds the evidence for you; that depends on the planned language model. These are seven fictional cases, not proof of real-world performance. I haven't timed a real user against manual coding. Next I would connect a live model and re-run these same inputs, including three repeats of Typical 2 to see whether its suggestions vary, and time one classmate using the tool against a hand-coded transcript.

### 4. Risk response: false confidence from the tool's silence or labels

**Who could be affected.** The student making a go/no-go or pivot call, the sponsor who acts on it, and interviewees whose words could be misrepresented.

**What could go wrong.** The tool can make an assumption look untested when the matcher simply missed the evidence. It can attach a confident-looking label that is wrong (Typical 2). It can treat an answer the interviewer led the person into as independent support. A user in a hurry could export the suggestions as fact. This is the **over-reliance on AI** problem from the course: the output looks like analysis even though the rules don't understand the conversation.

**What I changed.** The flow now makes the risky shortcut harder, not just labeled:

- The user must choose the signal on every row before it can be exported, with the surrounding transcript lines shown at that moment.
- Answers to leading questions are flagged in the table and in the CSV.
- The gaps panel says plainly that the rules can miss evidence worded differently, so "no linked evidence" is not proof the topic never came up. Weak matches don't count as coverage.

The Risk and Challenge B tests check these safeguards.

**What risk remains.**

- Missed evidence is the biggest remaining risk. The tool only shows what it finds, and in Challenge B it found 1 of 6 answers. The gaps warning is text, not a check, so a user who skims could still treat silence as "no evidence". A live model is the planned fix.
- A user can still pick the suggested signal without thinking.
- The leading-question flag is a word rule that misses hedges like "I guess".
- Weak or wrong suggestions still shape the user's first impression.
- On privacy, nothing is stored or sent anywhere, but a live model would mean sending transcripts to a provider. That needs de-identification and the sponsor's consent before real interviews are used.

### 5. Remaining limitations, ranked by effect on the central task

1. **Matching misses paraphrased evidence** (Challenge B: 1 of 6 answers found). Next: live language model, re-run on the same inputs.
2. **Suggested signals are often wrong or Unclear** (Typical 2: all 5 rows). Next: the model suggests signals with a one-line reason the user can check; keep the confirm step.
3. **One assumption per sentence.** A sentence that bears on two assumptions is matched to only one. Next: allow multiple tags.
4. **No timing data.** Next: time one user on a hand-coded transcript against the tool.
5. **Download is blocked inside the claude.ai preview.** Copy CSV works there, and Download works in Replit or a normal browser.

### 6. Priorities for the final project

1. Connect a live model behind a server route, with the API key kept in Replit Secrets, never in the repo. Repeat the Typical and Challenge tests three times each.
2. Show a short reason with every suggested signal, and track how often users change the suggestion.
3. Run a timed comparison with one classmate on a de-identified transcript.

---

## AI-Use Disclosure (draft)

> **Draft. Eva: edit this into your own words before submitting.** Lines marked **[confirm]** are things only you know.

### Tools used

- **Claude (Anthropic), in claude.ai.** Used to draft the problem-framing outline, write the prototype code and test scripts, and draft the documentation.
- **Replit Agent** **[confirm whether and how you used it]**. I wrote a build prompt for Replit Agent to create the first working slice.
- **No AI runs inside the product yet.** The analysis is simulated with keyword rules (see "AI inside the product").

### Development assistance

- **Writing and planning:** Claude turned the assignment brief and my capstone context into the problem-framing outline, which I then corrected.
- **Code:** Claude wrote the HTML/JavaScript prototype: the analysis rules, the review table, quote verification, and CSV export. For this revision it added confirm-to-accept, negation handling, and the leading-question flag.
- **Testing:** Claude wrote `tests/run-ui-tests.js` (a scripted browser run) and `tests/before-after.js`, ran them, and recorded the results.
- **Documentation:** Claude drafted the README, `revision.md`, and this disclosure from the test output and my peer reviews.

### Representative prompts

1. **"Prepare a concise problem-framing document addressing the sections below."** (Problem Framing.) This produced a one-page draft that skipped sections 2, 4, 5, and 6, because I had only pasted part of the brief. I pasted the full assignment and asked for "an outline… only 1–2 pages and kept very simple." It then covered all six sections.
2. **"You are my AI development partner. Help me build, test, and document a small Replit prototype… If you simulate analysis… clearly label that behavior as simulated… Never paraphrase text in the quote field."** (First Working Slice.) This produced the v1 page, three fictional samples, and a test run. The rules I set in this prompt (label the simulation, verify every quote, don't invent evidence) became the product's core safeguards.
3. **"For this project, iterate the product based on this assignment's instructions and make sure it hits all of the metrics and deliverables my professor is looking for,"** followed by my three peer reviews. This produced v2, five-plus test cases, the before/after comparison, and these documents.

### Human judgment

- **Corrected:** the first problem-framing outline left out four required sections. I supplied the full brief and had it redone.
- **Rejected:** I declined a change that would have published the prototype with an extra browser permission (a download capability). I kept it simple and used Copy CSV inside the preview instead. **[confirm your reason]**
- **Removed:** Claude built a "Not linked: review manually" list, suggested by two reviewers, where you could link unmatched lines by hand. I tried it and removed it because it made the tool confusing. I want the tool to show only evidence it actually finds in the transcript, so I reworded the gaps panel to warn about missed evidence instead.
- **Decided:** to keep the analysis simulated and client-side for this milestone, rather than adding an API key to a public project. I also decided that the user, not the tool, sets the final signal on every row.
- **Chose priorities from the peer feedback:** I acted on the "silently dropped lines" and "no README" feedback, adapted the phrase-matching feedback, and deferred general paraphrase handling to the live model.
- **[Add one more decision of your own:** for example, a label, wording, or sample you changed after reviewing the draft.]

### Verification

- **Done:** Scripted browser tests ran the real page on every fictional sample and saved the results (`tests/results.json`) and screenshots. The before/after script compares v1 and v2 rules on identical inputs. Layout was checked at phone width.
- **[confirm]:** I opened the prototype myself, ran each sample, and checked that the README claims match what the page does.
- **Still needs checking:** that the simulated rules behave sensibly on a real (de-identified) transcript. Live-model behavior is untested because no model is connected. The keyword lists were written by AI and only checked against these fictional samples.

### AI inside the product

| | |
|---|---|
| Task | Extract exact quotes from a transcript, match each to an assumption, and suggest a signal |
| Inputs | Transcript text with speaker labels, and assumptions as `ID: statement` |
| Outputs | Draft evidence rows quoting the transcript word for word; the user confirms each row before export |
| Current status | **Simulated (Wizard-of-Oz):** keyword rules in `src/core.js`. Verification, review, and export are fully functional |
| Planned | A language model proposes quotes and signals with a short reason. The exact-quote check and user confirmation remain |

No credentials, API keys, or real interview data are in this repository.

---

## Demo script

Use the fictional samples only. Zoom the browser to 110–125% so the text is readable. Close other windows and turn off notifications.

**0:00 – Who and why (20 s).** Open the page.
> "Evidence Sorter is for people running customer-discovery interviews, like me on my ITG capstone. After every call I have to pull exact quotes out of a long transcript and match them to my assumptions by hand. That's slow, and it's easy to mix up what someone said with what I think it means."

**0:20 – The central task (50 s).** Click **Sort My Interview**, then the **Typical 1** sample. (With real data you would paste your transcript and assumptions and click Analyze.)
> "I paste a transcript and my assumptions, and click Analyze. Each card is a draft: the exact quote, its line number, the assumption, and a suggested signal. The green check means the quote appears word for word in the transcript."

Click **Accept…** on the Line 4 card.
> "Accepting opens the lines around the quote, and I have to choose the signal myself. Confirm stays greyed out until I do."

Choose **Supports**, then click **Confirm and accept**. Point at the status line and the CSV preview.
> "The confirmation now stays on screen, which a reviewer asked for, and only rows I've confirmed go into the export."

**1:10 – One revision and the evidence behind it (40 s).** Click **Edit transcript or assumptions**, then the **Messy transcript** sample, and scroll to the P3 card ("extra lots on the shelf").
> "All three reviewers said the rules missed evidence worded differently. For example, 'we keep extra lots on the shelf' didn't count as backup stock, so it looked like I had no evidence for P3. I improved the rules, so that line now appears as a P3 card, and version 1 didn't show it at all. I also reworded the gaps panel so it warns that the rules can miss evidence. I tried a list of unmatched lines that you link by hand, but it made the tool confusing, so I kept it to quotes it actually finds."

**1:50 – Where AI fits (25 s).** Point at the yellow **Simulated** badge.
> "The analysis is simulated. These are keyword rules standing in for a language model; it's a Wizard-of-Oz prototype. The quote check, the review and the export work for real. The plan is for a model to suggest quotes and signals, while I still confirm every row."

**2:15 – Limitation and next step (20 s).** Optional: go back and click the **Leading questions** sample to show the flag.
> "The main limit: when I reworded an interview, the rules found only 1 of 6 answers, and they mislabeled most rows in my second typical case. That's the job a live model should do, so connecting one and re-running these same tests is my next priority."

Stop recording. Name the file `demo-draft.mp4`.
