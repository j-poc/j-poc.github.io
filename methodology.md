---
layout: default
title: Methodology
description: How Holdout counts how often AI names you, why every number has a range, and how the holdout test decides whether the work did anything.
---
<p class="lead">AI gives a different answer each time you ask. So we ask many times, show the count behind every number, and test our own work against questions we left alone.</p>

> Holdout has not launched yet. This is the method the service is built to follow.

## 1. What we count

We write down the questions your buyers ask, such as "what is the best tool for a small team that needs X?" We ask an AI each question several times. For each answer we check two things: was your company named, and was your website one of the pages it linked to. A question that already has your name in it does not count as being named.

## 2. Why we ask more than once

In a test of 2,961 runs, an AI gave the same list of brands twice less than 1 time in 100. The order matched about 1 time in 1,000 ([SparkToro, January 2026](https://sparktoro.com/blog/new-research-ais-are-highly-inconsistent-when-recommending-brands-or-products-marketers-should-take-care-when-tracking-ai-visibility/)). That is why we never show a rank from one answer. We show how often you were named, out of how many answers.

Every rate also has a likely range. "3 of 16, likely range 7% to 43%" means: if we asked many more times, the true rate would very likely land between 7% and 43%. Few answers give a wide range. More answers narrow it. The range is a 95% Wilson interval.

## 3. The half we leave alone

When you start, we split your questions in two at random, keeping the same mix of question types on each side. We work on one half. We leave the other half alone. The split is fixed on day one and never changes, so nobody can pick the winners afterwards.

AI answers drift on their own from week to week. The half we left alone shows that drift. Whatever the worked-on half gains beyond it is the lift.

## 4. The three results

Lift is the change in the worked-on half minus the change in the half we left alone, from before we first found a fix on your page to after. We work out its likely range by re-drawing the questions and answers 6,000 times.

We read the verdict every week, and every extra look is another chance to mistake noise for a lift. So each weekly look uses a stricter range (99.6%, not 95%). Across the first 12 weekly looks together, the chance of a false "lift detected" stays near 5%. The price is that small lifts take longer to show.

<dl class="results">
  <dt>Lift detected</dt>
  <dd>The questions we worked on rose more than the ones we left alone, and the whole likely range of that gap is above zero.</dd>
  <dt>No lift detected</dt>
  <dd>We had enough answers to see a 15-point gap if there was one, and the likely range still includes zero.</dd>
  <dt>Not enough data yet</dt>
  <dd>Fewer than two measurements since we first found a fix on your page, or too few answers to tell. The report says how many more weekly runs we need.</dd>
</dl>

"No lift detected" is a real result. We report it the same way as good news. We tested this method on made-up data with no effect in it. Looking every week for 12 weeks, it said "lift detected" by mistake in no more than 1 workspace in 20.

## 5. Who does the work

Holdout does the work itself. You connect your site once. Holdout writes the first fixes and sends them to your site, unless you chose Review first, and does the same each week. A fix counts as live only once we open the public page and find its text.

- **Autopilot (the default).** Fixes that pass our fact check are sent on their own once your site is connected, up to a weekly limit you set. Each one is logged with what changed, and you can undo it in one click or pause everything.
- **Review first (your choice).** Nothing is sent to your site until you approve it.

In both modes, a draft with a number we cannot trace to your site or to a fact you gave us is not sent. It is held back for you. We never send messages to other people or sites in your name.

## 6. Which AI we ask, and how

We reach each AI through a door made for software: its official API, its command-line tool, or a data provider. Web search is on where the engine has it. That is not always the same as the app on your phone, and answers can differ. Every report names the engine, the model and the way we asked it.

<div class="table-wrap" role="region" aria-label="Engines and how we reach them" tabindex="0">
<table>
<thead><tr><th scope="col">Engine</th><th scope="col">How we reach it</th><th scope="col">Status</th></tr></thead>
<tbody>
<tr><td>Claude</td><td>Official API with web search</td><td>Built, not yet tested live</td></tr>
<tr><td>Claude (Claude Code)</td><td>The Claude Code command-line tool, with web search. Used for audits we run ourselves</td><td>Tested live</td></tr>
<tr><td>ChatGPT</td><td>Official API with web search</td><td>Built, not yet tested live</td></tr>
<tr><td>Perplexity</td><td>Official API</td><td>Built, not yet tested live</td></tr>
<tr><td>Gemini</td><td>Official API, grounded in Google Search</td><td>Built, not yet tested live</td></tr>
<tr><td>ChatGPT app answers</td><td>Collected by a data provider (DataForSEO)</td><td>Built, not yet tested live</td></tr>
</tbody>
</table>
</div>

## 7. What we do, and what we will not sell

We only do work that has evidence behind it. Here is the evidence, with its limits.

<div class="table-wrap" role="region" aria-label="Evidence for each kind of work" tabindex="0">
<table>
<thead><tr><th scope="col">Work</th><th scope="col">What the study found</th><th scope="col">Limit</th><th scope="col">Our call</th></tr></thead>
<tbody>
<tr><td>Add real numbers, quotes and named sources to a page</td><td>In a 10,000-question test, adding quotes, statistics and cited sources raised how visible a page was in AI answers by up to 40%. Stuffing keywords made it worse. <a href="https://arxiv.org/abs/2311.09735">Princeton and Georgia Tech, GEO paper (KDD 2024)</a></td><td>Run on a test engine in 2023, not on today's apps.</td><td>We do this</td></tr>
<tr><td>Be on the pages the AI finds when it searches</td><td>Across 50,553 ChatGPT answers, the page found first was cited 58.4% of the time and the page found tenth 14.2%. Headings that match the question were cited more often (41.0% against 30.2%). <a href="https://www.airops.com/report/the-fan-out-effect-what-happens-between-a-query-and-a-citation">AirOps, The Fan-Out Effect (April 2026)</a></td><td>A large sample, but a study of what happened, not an experiment.</td><td>We do this</td></tr>
<tr><td>Be mentioned on other sites and on YouTube</td><td>Across 75,000 brands, mentions on YouTube and around the web were the things most closely tied to being named by AI. Links were only weakly tied. <a href="https://ahrefs.com/blog/ai-brand-visibility-correlations/">Ahrefs, AI brand visibility study (December 2025)</a></td><td>Bigger brands get more of both, so this shows a tie, not a cause.</td><td>We only draft the pitch. You decide whether to send it</td></tr>
<tr><td>Schema markup</td><td>1,885 pages that added it were compared with 4,000 that did not. The change in AI citations could not be told apart from zero. <a href="https://ahrefs.com/blog/schema-ai-citations/">Ahrefs, schema and AI citations (May 2026)</a></td><td>Only tested on pages that were already cited.</td><td>We do not sell this as a fix</td></tr>
<tr><td>An llms.txt file</td><td>Across about 300,000 websites, having the file had no tie to being cited by AI. <a href="https://seranking.com/blog/llms-txt/">SE Ranking, llms.txt study (October 2026)</a></td><td>We publish one ourselves because it costs nothing. We do not charge anyone for it.</td><td>We do not sell this as a fix</td></tr>
</tbody>
</table>
</div>

## 8. What this cannot tell you

- It cannot promise that AI will name you. Nobody controls that.
- It measures the questions on your list. Buyers ask others too.
- A fix can spill over and help the half we left alone. That makes the lift look smaller than it is, never bigger.
- It takes weeks. With the roughly 24 questions a workspace starts with, each asked 3 times a week, expect a few weekly runs after the first fix before a result is clear.
