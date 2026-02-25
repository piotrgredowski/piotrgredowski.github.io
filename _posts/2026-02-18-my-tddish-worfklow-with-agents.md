---
titel: My TDD-ish workflow with agents
date: 2026-02-18 18:00:00 +0100
categories: [Blogging]
tags: [TDD, test-driven development, agents, OpenCode, Github Copilot, gsd, code review, software development]
---

This post was a response to a post from [Michał Warda on Linkedin](https://www.linkedin.com/feed/update/urn:li:activity:7429185377539682304):.

I’ve moved toward a TDD-ish workflow with agents:
- plan the implementation,
- review that plan,
- implement all tests I see for the planned interface,
- let the agent cook (sometimes in a Ralph Wiggum loop) until the suite stays green.
Tests are usually as much "end to end" as they can be.
If agent needs to change the tests, I get a notification and review those first.

I’m usually toggling between build/plan modes in [Opencode](https://opencode.ai//[`pi`](https://shittycodingagent.ai/) for my private stuff and Github Copilot for work, occasionally using [gsd](https://github.com/gsd-build/get-shit-done) to just get it done.

I’m totally comfortable with that this black-box approach for small personal tools or CLI scripts, but I feel like the "no-read" policy hits a wall at scale. And for teams of people ;).

For work-related projects shared with a team - or anything complex enough to last more than a single session - I still find manual review essential.

I mean - manual review of most of the code. OpenCode with web interface is great for that - you can add comments for written code, and agent will fix/implement them. That's quite awesome! I will record a video soon showing my workflow using OpenCode.

Tests (even manual ones, but yeah... let us be serious) prove the code works today, but a code review ensures I won't hate myself when I have to refactor it one week/six months from now. Maintainability is usually where the "unread implementation" debt starts to show its face. Making my colleagues to look at not reviewed by me code - I couldn't stand it. At the end of the day, I can't really explain away 'suboptimal' code by blaming the agent. I still believe we’re responsible for the code we sign our names to in a commit. If my name is on the blame, I need to know what’s under the hood. ;)

If that makes me old-school, so be it!