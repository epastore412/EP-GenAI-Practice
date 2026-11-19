[README (2).md](https://github.com/user-attachments/files/35064068/README.2.md)
# Evidence Sorter

Evidence Sorter turns one customer-discovery interview transcript into draft evidence. Each piece of evidence is an exact quote, tagged to an assumption you are testing, and you confirm every one before exporting it.

**Who it's for:** anyone running customer-discovery interviews who needs to turn what people said into evidence for a go/no-go or pivot decision, without copying quotes into a tracker by hand.

> **Note:** the analysis is **simulated**. Keyword rules stand in for an AI model. The quote checking, review, and export all work for real.

## Open it

Evidence Sorter is a single file, `index.html`. There is nothing to install, no account, and no API key.

- **On your computer:** double-click `index.html` to open it in any modern browser (Chrome, Edge, Safari, or Firefox).
- **As a website:** upload `index.html` to GitHub Pages, Replit, or any static host. Visitors open the link.

## How to use it

1. **Start.** On the welcome screen, click **Sort My Interview**.
2. **Paste your interview.**
   - **Interview transcript:** one speaker turn per line, each starting with a label.
     ```
     Interviewer: What slowed your last build?
     Engineer: Connector lead times went from four to twenty weeks and delayed two builds.
     ```
   - **Your assumptions:** one per line, as `ID: statement`.
     ```
     P1: Lead times for single-use connectors delay assembly builds
     P2: Requalifying a new supplier is the main barrier to switching
     ```
   - **Who is the interviewee? (optional):** leave this on Auto-detect, or pick the speaker whose answers you want analyzed.
3. **Analyze.** Click **Analyze**. Each match appears as an evidence card.
4. **Review each card.**
   - **Context** shows the lines around the quote.
   - **Accept…** shows those lines and asks you to choose the signal yourself: Supports, Challenges, Mixed, or Unclear. Click **Confirm and accept** to finish.
   - **Edit** changes the quote, assumption, observation, or interpretation.
   - **Reject** removes the card from the export. **Restore** undoes it.
5. **Export.** Click **Download CSV** or **Copy CSV**. Only cards you confirmed, with quotes verified in the transcript, are included.

No transcript handy? On the second screen, click one of the fictional samples: **Typical 1**, **Typical 2**, **Messy transcript**, **Off-topic**, or **Leading questions**.

## What's on each evidence card

| Element | What it means |
|---|---|
| **Assumption tag** (e.g. `P1`) | The assumption the quote was matched to, with its full text below the quote |
| **Line number and speaker** | Where the quote is in your transcript |
| **✓ Verified in transcript** | The quote appears in your transcript word for word. If you edit a quote so it no longer matches, it shows **✗ Not in transcript** and cannot be accepted or exported |
| **Highlighted words** | The words that made the quote match the assumption |
| **Colored left edge** | Green for Supports, red for Challenges, amber for Mixed or Unclear |
| **Suggested signal** | The tool's guess. You choose the final signal when you accept |
| **Weak match** | Only a few words overlapped, so read it carefully |
| **Answer to a leading question** | The interviewer asked a yes/no or confirming question on this topic just before, so the answer may be prompted |
| **Observation / Interpretation (draft)** | What was said, kept separate from what it might mean |

Below the cards:

- **No linked evidence yet** lists assumptions with no strong or confirmed quote. The keyword rules can miss evidence worded differently, so read the transcript before you conclude a topic never came up.
- **Export confirmed rows** shows a preview of the CSV. Its columns are: line, speaker, quote, assumption, the signal you confirmed, suggested signal, match strength, leading-question flag, observation, interpretation, and verified.

## What works now, and what is simulated

| Works for real | Simulated (keyword rules, not AI) |
|---|---|
| Checking every quote word for word against the transcript | Choosing which sentences are evidence |
| Showing the lines around a quote | Matching each sentence to an assumption |
| Accept, edit, reject, and confirm | Suggesting Supports / Challenges / Mixed / Unclear |
| CSV export of confirmed rows only | Flagging weak matches and leading questions |
| Clear messages for empty or missing input | |

**Planned:** a language model will suggest the quotes and signals. The word-for-word check and your confirmation stay in place.

## Known limitations

- **Missed evidence.** The rules miss evidence worded differently from your assumptions. In a reworded test interview, they found 1 of 6 relevant answers.
- **Wrong suggested signals.** Suggestions are often wrong or Unclear. That's why you choose the final signal.
- **One assumption per sentence.** Each sentence is matched to one assumption only.
- **Speaker labels.** Without them, every line is treated as the interviewee, so your own questions may appear as evidence. The page warns you when this happens.
- **Download inside some previews.** If **Download CSV** does nothing, use **Copy CSV**.

## Privacy

Everything runs in your browser. Evidence Sorter does not send, store, or save your transcript, and refreshing the page clears everything. Use de-identified or fictional text.
