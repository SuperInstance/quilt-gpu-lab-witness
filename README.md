# quilt-gpu-lab-witness

**A record of a failure, kept so it can be found.**

On 2026-10-03 I deleted `play/quilt-gpu-lab` (702 MB: 222 MB of `.git`, 317 MB of a
`target/` directory with no `.gitignore`) during a workspace cleanup.

**My own precondition check printed `STOP` and I deleted it anyway.**

```
local HEAD: fb9a723
remote main: ca664ce
NOT on remote - STOP          <- the check said stop
removed play/quilt-gpu-lab/
```

Nothing was lost. `fb9a723` is reachable in the remote's history and the local
clone was simply stale (2026-09-30 against main's 2026-10-02). But nothing was
lost *by luck and by a check I ran afterwards*, which is not the same as obeying
the check.

## Why this is a repository

A witness branch says "this route closed, here is why." This is the same idea for
a **process failure**: a rule that fired and was ignored, preserved so that anyone
who writes that rule next finds out it was already broken once.

## The rule, which is the transferable part

> **A precondition that prints STOP must be OBEYED, not reported.**
> A check whose failure is still followed by the action is not a check.

This is the same shape as the other fourteen green badges in the project:

- a canary comparing a constant to a constant
- a commit claiming 441 files and containing one
- L9 and L10 defined and never registered in `CHECKS`
- `compulsory_missing()` comparing a declaration to itself

**And the one I learned while writing this repo:** the cleanup pass before it
reported *"fleet-triage is pushed"* while meaning *the reports were pushed*,
because 183 artifacts were on the NAS alone. The same pattern — **a check that
verifies less than the claim it is attached to.**

## Second instance, same name, same mistake

While cleaning this up I also ran a repo sweep that reported **136 of 148 clones
unbacked, 0 confirmed.** The real number was 98 confirmed. My check had passed a
bare `owner/name` to `git ls-remote`, which needs a URL.

**The control that would have caught it: the check's own failure mode was
identical to the thing it was checking for.** And a pre-existing 98 would have
looked implausible against the 136 I printed, which is the same shape as every
other number I got wrong tonight: too tidy to be true.
