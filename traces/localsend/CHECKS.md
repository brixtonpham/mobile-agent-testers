# Checks of the LocalSend traces

The checks (checkpoints) in the 10 traces of commit 983557a were written by an AI assistant,
not by a person, from the screens captured during exploration. They follow the rule fixed in
amendment 4 of `paper/protocol.md` in explore-compile-replay (commit d9e3931), before any output
was opened:

- only the 10 traces whose first path replay passed (runs 45701272440 and 45706788156 of
  mobile-agent-testers, tag `eval-localsend-1`);
- steps are never changed: only checkpoints are added;
- a check names an element by its text, never by index or position;
- on each new screen, at most 2 checks: one `visible` check on a text that names the screen
  (title, heading or selected tab), plus one on the intended change;
- no check on text that varies: device name, addresses, times, devices found nearby;
- a finding becomes a failed check only if the author confirms it.

How the rule was applied:

- a check sits on the step after which the screen was captured, and its text is a whole
  element text on that screen (checked by script);
- the Receive tab has no title: after `launchApp`, the check is the intended change, the
  "Receive via link" button of the first screen;
- the Send tab is named by its "Nearby devices" heading; "Selection" changes once a file or a
  text is selected;
- after a text or a file is selected, the intended change is the "Edit" button of the selection;
- scrolls, a section that opens in place and the screens of the system file picker get no check;
- the size of a selection is never checked: it depends on the typed text, which stays private.

Every check id starts with `ai-`. 78 checks in the 10 traces, none recorded as failed. The texts
the agent typed are not in this folder: they come from the `REPLAY_PARAMS` secret at replay.

## Kept after the first replay

Each trace was then replayed once with its checks, on the unchanged app (the APK kept on the
pre-release `eval-localsend-1`, SHA-256 `23301784…`), in run 45715086835. As `paper/protocol.md`
says, a trace is kept only if this replay gives what it recorded. 7 of the 10 are kept, with 62
checks. [SHA256SUMS](SHA256SUMS) lists them: they are frozen from this commit on.

The 3 dropped traces, with the stop shown on the run page and its likely cause. The cause is
inferred: what the phone showed at the stop is not public.

- `grace-20261125T215820`: target not found at step s4, a tap in the system file picker on
  `ui.xml`. That file is the screen dump that the agent's tools write to the phone
  (`scripts/android/screen.py`). A phone that only replays does not have it.
- `layla-20261125T212314`: target not found at step s7, the tap on the confirm button just
  after typing, with the app in front. The same step passed in the replay right after the
  exploration, on the phone of the exploration. Cause not known yet.
- `olga-20261125T215851`: check `ai-s16-about-page` failed after step s16, a tap on the first
  "Open" button after three swipes, with another window of the system in front. A swipe is
  replayed at Maestro's speed, so the list likely stopped on another row, and the first "Open"
  was another button.
