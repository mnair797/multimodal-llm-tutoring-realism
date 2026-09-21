# Examples of Multimodal Event Signal

This document shows concrete examples of the multimodal (event-context) signal
used throughout the paper, so reviewers can see exactly what kind of
information is being added to the LLM's prompt in the `event` condition
versus withheld in the `no_event` condition.

The signal originates from a shared digital tutoring canvas and is logged
automatically by the tutoring platform's own instrumentation, time-synced
with the speech transcript. In the raw log, each event is a short text
description; these are what get inserted into the prompt (see
`llm_run_all_experiments.py`, `make_prompt()`).

There are 5,810 total event annotations across the dataset, comprising 440
unique description strings across four categories:

## 1. Tool use (49.2% of events)

Describes drawing and erasing actions on the canvas. Color and duration are
embedded directly in the description text.

```
draws with a red pen for about 5 seconds
draws with a firebrick pen for about 8 seconds
draws with a darkturquoise pen for about 5 seconds
used an eraser for about 5 seconds
highlights text in yellow
```

## 2. Canvas object manipulation (39.4% of events)

Describes changes to shared workspace elements — sticky notes, expression
boards, shapes.

```
added a sticky note to the canvas
edited expression of an expression board
deleted a shape from the canvas
writes a sticky note
```

## 3. Session-level events (8.9% of events)

Marks participants entering or leaving the session.

```
joined the session
left the session
```

## 4. Other (2.5% of events)

A small remainder of miscellaneous actions that don't fit the above three
categories.

```
performed a calculation
sent emoji message
```

---

## Why this matters for interpreting the paper's results

The paper's central mechanism finding (Discussion, RQ3) is that this event
signal helps in casual, conversational tutoring moments and does not help
in numerically precise ones. Looking at the examples above, this makes
intuitive sense: none of these descriptions carry exact numeric or
mathematical content (a number, a variable, an equation). They describe
*that* an action happened, not *what mathematical content* it involved.
This is why the signal is directly usable material in an open-ended,
affirming remark (e.g., referencing that a student was drawing) but
contributes nothing when a tutor's response needs to reproduce an exact
calculation.
