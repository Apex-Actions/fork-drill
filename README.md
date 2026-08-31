# fork-drill

A deliberately trivial repository whose only purpose is to exercise **Apex Actions'** untrusted-run
path against a real fork pull request — the live exercise owed by Phase 26, slice 1.

`trust` is decided by comparing the head repository's numeric id against the base's
(`control-plane` `src/domain/trust.ts`), so a fork owned by anyone — including a maintainer — is
untrusted. A pull request opened here from a fork must:

1. produce a run that starts **no jobs** until somebody with write access approves it;
2. once approved, run with **no repository secrets** in the job's environment;
3. hold a **read-only** token;
4. be denied **cache writes**;
5. be leased only to a runner that **belongs to a pool** — never the box.

`DRILL_SECRET` exists on this repository so that its *absence* from a fork run means something. A
`push` run on a branch of this repository is the control: it must see the secret as present.

Nothing here prints a secret's value. Presence is reported as a word, never as bytes.

The control: a push run on this repository is trusted and must see the secret.

## The contribution

This line was written from a fork the base repository does not control. If the untrusted path
works, the run it triggers starts no jobs until somebody approves it, and then sees no secrets.
