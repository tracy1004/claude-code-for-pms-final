# 03 · Rewind — prompts

**Context:** You joined Rook two weeks ago as PM on Dispatch.
Release 4.2 shipped on 12 August, just before you arrived, and
landed badly.

Last session you read four conversations and every support ticket
since 4.2 — and found the two piles did not agree.

At the end of the session, type wrap up, and Claude Code saves the
prompts you wrote yourself below — not the starter prompt — updates
CLAUDE.md, and saves your work to GitHub. By Module 6 this file is a
prompt library built from your own questions.

---

### 1.

Use the rook-database connector to rebuild our weekly metrics export as 00-rook/data/callout-history.csv. One row per responder per week, for every week starting Monday 29 June through the week starting Monday 31 August 2026. Columns, in this order: week_starting, responder, handler, pings_sent, pings_taken. Use the pings table: pings_sent counts every ping that week, pings_taken counts the ones whose outcome is taken. Get each responder's handler from the responders table. Include weeks where a responder got no pings at all, with 0 in both columns. Sort by week, then responder. Save only the file, don't analyse it, and tell me how many rows you wrote.

### 2.

Use the rook-wiki connector. Read every page and database under the Company page, including the comments people left on them. Save each one as a plain text file in 00-rook/company/: about-rook.txt, rook-dispatch.txt, rook-supply.txt, glossary.txt, team-directory.txt, releases.txt and q3-roadmap.txt. Save the comments on the 4.2 page in the Releases database as 00-rook/company/notes/dispatch-slack-thread.txt. Copy the text exactly; don't summarise, reword or add anything. Leave handoff-from-priya.docx where it is. Tell me which files you made.

### 3.

Use the rook-wiki connector to open the Customer interviews database under the Research page. Save each interview's full page text as its own file in 00-rook/feedback/interviews/, named after the handler: aunt-dot.txt, ambrose.txt, halloran.txt and kip.txt. Copy the text exactly as it is; don't summarise or reword anything. Tell me when the four files are saved.

### 4.

Use the rook-database connector to read every row of the support_tickets table. Save each ticket as its own text file in 00-rook/feedback/tickets/, named t-<ticket_number>.txt. In each file put the ticket number, the date it was filed, who filed it, the responder it's about, the subject, the status, and then the full body. Copy everything exactly; don't summarise or reword anything. Tell me how many files you made.

### 5.

Use the rook-wiki connector to read every page under Product briefs. Save each one as a text file in 06-sidekicks/briefs/: bulk-callout.txt, handler-phone-app.txt, requisition-approval-chains.txt and routing-override-audit-log.txt. Copy the text exactly; don't summarise or reword anything. Tell me when the four files are saved.

### 6.

merge my code to the main branch

### 7.

Claude for this project I do not want to work in branches ever. Please always comit to Main going forward.

### 8.

Open 00-rook/data/callout-history.csv. Every row is one responder in one week: how many times we pinged them, and how many of those they took. Release 4.2 shipped on 12 August. Tell me what changed after that date. Show me the weekly numbers before and after, and show me the rows you used to get them.

### 9.

Pick one responder from the file who went quiet. Show me every week for them, how many times we pinged them, how many they took. Then tell me what happened to that person, week by week, in plain English

### 10.

Once somebody's gone quiet, what would have to happen for them to start getting pinged again? what is the overall synopsis of what is happening accross the board. How many responders are affected? Is the issue with the handler or the responders accepting the ping? What can we do to resolve the issue

### 11.

provide specific names for responders an handlers.
