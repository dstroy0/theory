# Peer sessions

**Purpose:** How other sessions that build on the engine take it. No edit here waits on them, and no build of theirs reads a tree in motion.
**Scope:** the RSNA knee session and the CASMI chemistry session, as of 23 September.

Ruled by 23 September: the chemistry engine does not take precedence over the engine's own work, and a peer uses a git dependency.

- Peer sessions (the RSNA fork at D:\kaggle\rsna_knee_abnormality\engine, the CASMI chemistry session at D:\kaggle\casmi_2026) build the engine from a git dependency pinned at a commit, never from this working tree in place.
- Engine source edits are never paused or held because a peer is building. A peer that asks for a hold is pointed at a pinned checkout instead.
- Peers are told before a record-API shape change lands in a commit, and GPU runs are coordinated by message until tessera (the device-job scheduler) exists.
- Commits happen only when Doug asks; peers move their pins then.
