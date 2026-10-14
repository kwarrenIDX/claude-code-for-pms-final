# 04 · X-Ray Vision — prompts

**Context:** You joined Rook two weeks ago as PM on Dispatch.
Release 4.2 shipped on 12 August, just before you arrived, and
landed badly. Last session the numbers showed you what happened: a
ping used to wait ninety seconds and now it waits sixty, people
missed pings they used to catch, and missing one counts the same as
turning one down — so four responders stopped hearing from us
altogether.

At the end of the session, type wrap up, and Claude Code saves the
prompts you wrote yourself below — not the starter prompt — updates
CLAUDE.md, and saves your work to GitHub. By Module 6 this file is a
prompt library built from your own questions.

---

### 1.

you've called out two areas where the 4.2 changes have questions or comments in the code, are there any others that would affect the 4.2 release?

### 2.

is there anything else that is missing or was changed on August 12th?

### 3.

Can you tell me more about the reasoning behind why Supply is asking to not change anything in the Availability record?

### 4.

based on what we see for the 4.2 changes why would a responder that used to be consistent suddenly not be?

### 5.

for the 5 parts you listed can you turn that into a combined statement in the form of "Based on what I found, the reason some responders are getting no pings at all is ___,"

### 6.

The pings table on rook-database has the number trends. Read that alongside this code. Does it back up that responders who have missed pings are being passed over for new callout pings? And does that answer the engineering manager's question of 'Quick one, did the change to who gets asked first apply to responders who'd already been turning jobs down, or just new ones? Can't tell from the code and don't want to guess on this. Anyone know?'

### 7.

You said "The pings data supports “responders who miss pings get asked less”. It doesn’t support “responders who turn jobs down get asked less”." why does it not support this?

### 8.

who is the engineering manager so that we can respond to the question

### 9.

lets draft the message here

### 10.

can you give me the information on what Marcus was referencing on the release page to prompt him to ask that question?

### 11.

Find me the part of this code that takes points off somebody when they miss a ping or turn one down. Show it to me and explain it in plain English. Then find me every single thing in this code that puts points back on.

### 12.

Would a responder with a 0 score ever get a ping to be able to climb back up?

### 13.

Is there something that our code is missing that would help responders that have gone quiet?

### 14.

what does the glossary say specifically about declines and misses

### 15.

what would a responder with a 0 score need to do to get more work?

### 16.

If I'd only asked what the code does, not what it doesn't do, what would I have missed?
