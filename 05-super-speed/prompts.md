# 05 · Super Speed — prompts

**Context:** You joined Rook two weeks ago as PM on Dispatch.
Release 4.2 shipped on 12 August, just before you arrived, and
landed badly. You've spent three sessions finding out what went
wrong: the two piles of feedback that didn't agree, the numbers that
hid four people inside an average, and the code that settled it —
the shorter timeout is what did it.

At the end of the session, type wrap up, and Claude Code saves the
prompts you wrote yourself below — not the starter prompt — updates
CLAUDE.md, and saves your work to GitHub. By Module 6 this file is a
prompt library built from your own questions.

---

### 1.
Helen has asked for a product brief on what we would do to fix the issues with release 4.2. Her request is here - 05-super-speed/director-request.txt. There is additional context in the rook-wiki that you should use when defining the brief, use exact terms and names. We need to address at least three things - who the fix is for, what changes once it ships, and what it does not do. We talked about Vesper's experience last time so we can use them as an example of how things will change. The brief should include an executive summary a table of business problems listing the general problem, any additional information, and priority of high med or low, then go into the details of the fix, why it works and what it will do for the quiet responders

### 2.
we have to cut this down A LOT there is too much technical jargon for Helen, she is an executive and doesn't care where the numbers came from, she cares about what the issue was, what the solution is and how we know it's going to fix the issue

### 3.
lets remove the last two entries from the problems table

### 4.
remove the names of the handlers in the who it's for section

### 5.
we are not going to change the ping back from 60 to 90

### 6.
remove the bullet about callouts that end with no one taking them

### 7.
remove the bullet about identity

### 8.
change the missed chare target

### 9.
what is still missing from this brief?

### 10.
Save the brief exactly as it stands now as 05-super-speed/brief.md. Show me the file when it's done.

### 11.
I think the handlers need more information on the coverage card than just "quieter than usual' they should see a break down of the actual responses per responder they handle

### 12.
Take the brief you just wrote and build me a working prototype, an actual screen I can click through, not a description of one. Save it as 05-super-speed/prototype.html, a single file I can just open in my browser. Show me where this would actually happen, and make at least one thing on it respond when I click it.

### 13.
this looks pretty good for a handler, and we may make some changes later, but I'm interested to know what a responder would see and if we have any changes to their side

### 14.
I dont see that handlers can see the rank of their responders in the new card - can you show me where that is?

### 15.
for the changes for a responder lets add 1 and 2

### 16.
no need to change it in the brief, but I do think we need to change some of the cards at the top of the handler screen. can we color the missed pings card a translucent red

### 17.
if the missed pings is 0% can it be translucent green, otherwise keep it the red color if it is anything over 0%

### 18.
for the responder phone message when the miss a ping, can we change the last line to say that A missed callout counts for less than turning one down but does still affect your priority in receiving future callouts. Your priority order does recover over time.

### 19.
change it one more time to “A little. A missed callout counts for less than turning one down but does still affect your priority in receiving future callouts. Your priority order does recover over time.”

### 20.
for the pills in the place in responder order I think we should only show the pill they are currently in. and for normal the pill should be the same gree as we chose for the 0% misses

### 21.
lets change the cards at the top one more time, can we have a card for Accepted, last 7 days that is the same red when it is 0

### 22.
and lets remove the Pings sent last week card, but allow the handler to click back a week, like we are showing the illustration of a potential next week, lets let them click back to the actual last week
