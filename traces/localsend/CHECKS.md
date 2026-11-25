# Checks of the LocalSend traces

The checks (checkpoints) in these 10 traces were written by an AI assistant, not by a person,
from the screens captured during exploration. They follow the rule fixed in amendment 4 of
`paper/protocol.md` in explore-compile-replay (commit d9e3931), before any output was opened:

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

Every check id starts with `ai-`. 78 checks in all, none recorded as failed. The texts the agent
typed are not in this folder: they come from the `REPLAY_PARAMS` secret at replay.
