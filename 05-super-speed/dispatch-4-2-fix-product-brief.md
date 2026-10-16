# Rook Dispatch: fixing the quiet-responder problem from 4.2

**For:** Helen Achebe · **From:** PM, Rook Dispatch · **Date:** 2026-10-16 · **Status:** Draft for discussion, nothing built yet

## Executive summary

Since release 4.2 (12 Aug), four responders have all but stopped getting pings: Vesper, Farlight, The Undertow and Meteor Mite. Together they went from about 49 pings a week to about 3, while the other 12 responders got more. Handlers are also telling us pings vanish before they can answer.

**The cause.** Dispatch ranks responders by *routing priority*, and a **Missed** ping lowers that rank as much as a **Turned down** one, even though a miss often just means the responder didn't get to the phone in time. 4.2 made misses far more likely (ping wait cut from 90 to 60 seconds), and a responder's rank never recovers on its own. Once someone slips down the order they get fewer pings, so they have fewer chances to climb back.

**The fix.** Make a miss count for less than a turn-down, let rank recover with time, and show handlers when a responder has gone quiet. This is not a revert of 4.2, and it is more than changing a number.

## Business problems

| Problem | Additional information | Priority |
| --- | --- | --- |
| Four responders have stopped getting pings | About 49 pings a week down to about 3. None of the 45 related tickets is resolved. | High |
| A missed ping is punished like a refusal | Responders who were slow once keep sliding down the order. | High |
| Rank never recovers | Nothing lets a quiet responder climb back. Wen Li flagged this years ago. | High |
| Handlers say pings are "gone before I could answer" | About a third of tickets. Missed pings rose from 2% to 18% of all pings on release day. | High (not addressed by this fix: ping wait stays at 60 seconds) |
| Handlers can't tell why a responder is quiet | Kip sees Meteor Mite silent and The Gale nonstop, with nothing on screen to explain it. | Medium |

## Who it's for

- **Quiet responders**, first. They get back into the rotation.
- **Their handlers**, who currently have nothing to tell them.
- **Responders who are busy now** get a little fewer pings. That is the trade-off.

## What changes once it ships

1. **A Missed ping costs less than a Turned down one.** Not answering in time is no longer treated as saying no.
2. **Routing priority recovers with time.** A responder who has slipped down the order drifts back toward the middle, without needing pings to do it.
3. **Handlers can see it.** The coverage card shows, for each responder the handler looks after, how their pings went over the last 7 days: how many were sent, and how many were **Taken**, **Turned down** and **Missed**, next to the week before. It also shows a plain label called "Place in Responder Ping Order": "Lower than usual", "Recovering" or "Normal". It shows direction, not a score or a position. When a responder has gone quiet, the card flags "Quieter than usual", says why, and says it recovers.

## What changes for responders

Two small changes on the responder's phone. The 60-second ping wait stays.

1. **A countdown on the ping.** The responder sees how many seconds are left. It doesn't give them more time, but it ends the surprise of a ping vanishing, which is what handlers describe.
2. **A note after a missed ping.** The responder is told the ping moved to the next responder, and that a miss counts for less than turning it down. Today a missed ping just disappears.

Not yet checked with the phone app's designer (Sofia Marino) or engineering, and not sized.

## What it does for Vesper

Before 4.2, Vesper took 70 of 72 Old Town callouts. After, Vesper took 5 of 38, and 9 of Vesper's 17 pings were Missed. Aunt Dot described it: the phone buzzes, Vesper runs down the stairs, "and by the time he's got a thumb on the screen, it's gone." Then weeks of silence. These are one story: miss, then fewer pings.

After the fix, the same missed pings cost Vesper far less, and with each quiet week Vesper's rank climbs back. Vesper starts getting Old Town callouts again within days, not months. Aunt Dot's card shows Vesper's week of pings split into Taken, Turned down and Missed, so she can see for herself that the problem is missed pings and not refusals, plus "Quieter than usual" and when to expect it to change. Vesper won't automatically take first place back, because the responders who took over also drift toward the middle.

## What it does not do

- **It does not revert 4.2.** Wide-area responders keep the proximity weighting they asked for.
- **It does not change the 60-second ping wait.** That stays. Because misses will keep happening, the fix makes sure they no longer push a responder out of the rotation. Handlers will still see some pings go before they can answer.
- It does not change who is eligible for a callout, or add handler settings.
- It does not guarantee equal pings. Distance still matters most.

## How we know it will work

- **The evidence points one way.** Interviews, support tickets and ping data agree on what happened and in what order: pings missed, then the responder goes quiet. Misses jumped on release day; quiet followed about five days later.
- **We test before we ship.** We replay the last ten weeks of pings against the new rules and check that the four quiet responders come back and the other 12 aren't hurt. The cause is strongly supported but not yet proven, because the data has no rank scores. This test is how we close that gap.
- **After release, we measure.** Within four weeks:
  - the four responders' pings a week climbing back toward 49
  - overall **acceptance rate**, **time-to-accept** and **coverage gap** no worse
  - new "quiet" tickets dropping

## Decisions needed

1. **Wen Li to review the numbers** (how much a miss costs, how fast rank recovers) before anyone changes the code. She owns it.
2. **Tell Supply first.** More pings to quiet responders changes what Supply sees in the Responder Availability Record.
