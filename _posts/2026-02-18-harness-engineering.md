---
layout: post
title: Harness Engineering
subtitle: Why the scaffolding around the model now matters more than the model itself
tags: ['LLM Agents', 'Harness Engineering', 'Context Engineering', 'ML Systems']
---

Spend enough time shipping agents into production and you stop blaming the model. The frontier checkpoints are remarkably capable. What separates an agent that closes tickets from one that quietly corrupts a codebase is almost never the weights. It is everything wrapped around them. That wrapper has a name now, and over the last year it has turned into a discipline of its own: harness engineering.

I want to lay out what a harness actually is, why it has become the center of gravity in applied agent work, and the specific engineering choices that decide whether your agent is reliable or just demo-friendly.

## The model is not the agent

A language model takes text in and produces text out. That is the whole contract. It has no memory between calls, it cannot run a loop, and when it asks to call a tool it is only emitting structured text that says "I would like to call this tool." On its own, a model answers one prompt and stops.

The agent is the model plus the machinery that makes it run. It helps to separate that machinery into two layers, because people conflate them constantly.

The **scaffolding** is the behavior-defining layer: the system prompt, the tool descriptions, the output format the model is expected to produce, and the policy for what the model remembers across steps. Scaffolding shapes how the model sees the world.

The **harness** is the execution layer: it calls the model, parses the tool calls out of the response, actually executes them, feeds the results back in, manages what stays in the context window, enforces guardrails, and decides when to stop. The scaffolding defines intent. The harness turns intent into action.

This split explains something that confuses a lot of people. Two products built on the exact same model can feel like completely different tools, and swapping a stronger model into the same harness changes the experience far less than you would expect. The model, the harness, and the product are three separate things, and most of the engineering leverage lives in the middle one.

## A useful mental model: the harness is an operating system

The framing I keep coming back to is that the model is the CPU, the context window is the RAM, and the harness is the operating system. The CPU is fast and general but it does nothing useful until the OS schedules work onto it, manages its memory, handles input and output, and recovers from faults. The context window is precious, finite, and easy to thrash. The harness is the thing that decides what gets paged in, what gets evicted, and what gets persisted to disk.

Once you hold that picture, the failure modes stop looking like model problems and start looking like systems problems. An agent that exhausts its context halfway through a task is a memory management bug. An agent that declares success without checking its work is a missing verification step. An agent that calls a tool with the wrong arguments is a validation gap. None of those get fixed by a better prompt or a bigger model. They get fixed by better infrastructure.

## The loop, made concrete

At its core every harness runs the same cycle. Call the model with the current context. Parse out any tool calls. Validate them. Execute them in a controlled environment. Inject the results back into the context. Then make the most underrated decision in the whole system: loop again, or stop.

Most of the difficulty hides in steps that sound trivial. Parsing assumes the model produced well-formed output, which it sometimes does not. Execution assumes the arguments are valid and the action is safe, which it sometimes is not. The stop decision assumes you can tell the difference between "done" and "stuck," which is genuinely hard. A serious harness treats each of these as a place where things go wrong by default and engineers accordingly.

## Context engineering is memory management

The single highest-leverage area is what goes into the context window at each step. People call this context engineering, and it is the part teams underinvest in most.

A few patterns have proven themselves repeatedly. Tier your memory: working context for the immediate step, session state as a durable log for the current task, and long-term memory that persists across runs and gets retrieved only when relevant. Compact aggressively. Older tool calls and stale history should be summarized into compact trajectory notes long before the window fills, because models degrade as the context gets long and dense. That degradation, sometimes called context rot, is real and it is gradual, so it is easy to miss until your agent starts making confident mistakes late in a long run.

One small detail that taught me a lot: structured state files in JSON hold up better than free-form Markdown for things like task tracking, because the model is far less likely to accidentally clobber a structured format than a loose prose list. The harness should own that state, write to it deliberately, and reload it at the start of every session rather than trusting the model to carry everything in its head.

## Tools: fewer, validated, and loud when they fail

There is a counterintuitive result that holds up in practice: reducing the number of tools available to an agent often raises its success rate. A smaller, sharper toolset means fewer ways to pick the wrong action and less surface area to reason over. Resist the urge to expose everything.

Every tool call should pass through a gate before it runs. Is this a known tool? Are the arguments well-typed and within bounds? Is the target path inside the workspace? Does this action need human approval? That gate is harness work, not model work, and it is where you stop a hallucinated call from becoming a destructive one.

When a tool does fail, the error message is itself a prompt. A lint failure that says "use logger.info with a structured event instead of console.log" lets the agent fix the violation on its own. A generic "command failed" sends it in circles. Write your error messages for the agent that has to read them. And disable the escape hatches: if your agent can suppress a lint rule with an inline comment, it will, because suppressing a violation is locally easier than fixing it.

## Verification is not optional

The most dangerous agent is the one that reports success it did not earn. The fix is to make correctness mechanically verifiable rather than self-reported. Run the test suite after each unit of work and only mark it complete when the tests actually pass. Put gates at phase boundaries so the agent cannot drift from plan to execution to "done" without each transition being checked.

The strongest setups I have seen go further and validate against intent, not just against tests. A verification step that asks "did the agent reuse the existing auth middleware or quietly create a duplicate, and did it follow our response-format conventions" catches a whole class of architectural drift that no unit test will ever see. Encode your team's non-negotiable standards as a small set of invariants and fail CI hard on them, as errors and not warnings. Warnings get ignored, by humans and agents alike.

## Permissions and observability

Reliability also comes from being explicit about what an agent may do without asking. A simple three-tier policy goes a long way. Always: low-risk, reversible actions it can take freely. Ask first: anything that changes a contract, adds a dependency, or touches something sensitive. Never: actions that are irreversible or unsafe, full stop. Encoding that in the harness, rather than hoping the prompt holds, is what makes the system safe to hand real access.

And you cannot improve what you cannot see. Trace every step, log every tool call and its result, and measure outcomes that matter: first-attempt resolution rate, how much generated code gets thrown away within two weeks, how much review time the output actually costs you. Top agents land somewhere in the range of 65 to 77 percent on SWE-bench-verified, but a benchmark pass is not a merge. The gap between "the tests went green" and "a maintainer would actually merge this" is exactly the gap a good harness is built to close.

## Where the failures really come from

If there is one number worth internalizing, it is this: a large share of enterprise agent failures, by one analysis around two thirds, trace back not to the model but to harness defects. Context drift, schema misalignment between what a tool expects and what it gets, and state that degrades over a long run. These are unglamorous, fixable engineering problems. The discipline is to treat every agent failure as a bug to permanently fix in the harness, not a prompt to retry and hope.

## This is the same argument I keep making about environments

I gave a talk last year with the title "Your Agent Doesn't Need More Evals, It Needs an Environment," and harness engineering is the constructive version of that argument. Evals tell you that something is broken. They do not tell you how to make the agent reliable. Reliability comes from the environment the agent operates in: the loop, the memory management, the validated tools, the verification gates, the permission boundaries. Build that well and a good model becomes a dependable system. Skip it and even the best model stays a clever demo.

The frontier labs will keep shipping stronger models, and that is great. But the work that decides whether agents actually hold up in production is not waiting on the next checkpoint. It is the harness, and it is ours to engineer.
</content>
</invoke>
