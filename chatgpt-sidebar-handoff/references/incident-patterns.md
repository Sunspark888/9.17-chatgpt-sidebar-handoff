# Incident Patterns

## 2026-09-23 — Generated reply attachment visible, local download unverified

- Stage: receive files from an existing ChatGPT reply, then return a prepared local ZIP to that same conversation.
- Observed: the exact conversation and reply were readable. `read_thread` supplied reply text and user-uploaded attachment paths, but no local path for the assistant-generated review ZIP. The reply contained a prose download button and separate file cards with `下载文件` controls; the user identified the latter as the intended UI path. Browser control calls sometimes took 20–45 seconds or timed out. Clicking the prose button and a file-card download control did not produce the named file in the checked local download and temporary directories. A file preview was readable, which established content access but not file transfer.
- User correction: the destination composer displayed `极高`, and the user explicitly said to keep that setting. An attempted model-menu change had not succeeded; the pending attachment and setting remained intact.
- Confirmed: the local handoff ZIP had already passed its own source and CRC checks. It was later attached to the exact target conversation, and the sent message group visibly contained both filename and handoff text. The recipient's explicit readable acknowledgement was still pending at the time of this record.
- Unknown: whether the browser saved the generated files to an unobserved destination, blocked the downloads, or dropped the click internally. No account, permission, model-capability, or website cause was proven.
- Recovery: inspect the file cards separately from prose links, preserve the distinction between preview and local file, and continue the independently authorized outbound handoff from `LOCAL_READY` through visible `SENT`.
- Promoted rule: use the specific file card's download control and verify a real local file; do not stop an authorized handoff at local packaging or infer downloaded/sent status from a click or tool completion.

## 2026-09-12 — Scoped inventory and divergent same-conversation tabs

- Source: completed handoff evidence reported by the main task; this maintenance pass did not retest the browser or send material.
- Observed: global `cua.getState()` repeatedly failed or returned no usable inventory, but the runtime-supported `cua.listTabs({browser: 'iab'})` subsequently succeeded. Multiple existing tabs matched the exact conversation URL: tabs 1 and 6 lacked the latest reply, while tab 5 displayed it. Native `read_thread` also lagged.
- Confirmed by the reporting task: tab 5 DOM showed the corrected attachment sent and the Planner's actual unpacking acceptance with PASS/STOP; the paper handoff was complete.
- Unknown: why inventory routes and same-conversation views differed; this does not establish a network, account, or conversation-branch cause.
- Promoted rule: allow one supported browser-scoped inventory check for a known target browser; inspect other existing exact-URL tabs read-only when content is missing. Stop at verified acceptance without refreshing drafts, opening duplicates, or resending.

## 2026-09-12 — Targeted extension bridge reset, reconnection pending

- Evidence: user screenshot showed the ChatGPT Edge extension enabled, site access enabled and file URL access enabled. The registered native-host manifest and its executable existed. A running extension-host process matched that exact executable.
- Authorized action: verified process identity, then stopped only that extension-host process (PID 19488). Browser and desktop application processes were left running; no profile, credentials, permissions or registration files were changed.
- Observation: subsequent browser inventory returned only the in-app browser; the previous Edge extension entry disappeared. This proves the old bridge was disconnected, not that reconnection or uploads were repaired.
- Follow-up: bridge automatically restarted as PID 27464 and Edge reappeared with a new browser handle. Both before and after the user confirmed the toolbar panel was open, listing tabs still failed after about 21 seconds. The targeted bridge reset did not repair control. Full browser/app restart remains untested. Do not repeatedly kill the bridge or report repair from discovery alone.

## 2026-09-12 — Direct text delivery to an existing ChatGPT conversation

- Authorized test: two short labelled connectivity messages to the exact existing ChatGPT conversation, followed by short acknowledgements. No files or project decisions were sent.
- Tool completion times: 10.142 and 6.821 seconds. Both unique messages and their respective GPT acknowledgements were later verified by read_thread under the same conversation ID as the website URL.
- Approximate local-call-start to server user-message timestamps: 9.778 and 6.534 seconds; to server reply timestamps: 15.264 and 12.032 seconds. Cross-clock figures are approximate, not a controlled latency benchmark.
- Early reads returned older conversation state; the first receipt was only observed about 124 seconds after call start. The open sidebar DOM did not show the test messages during those observations. Observation delay must not be reported as generation latency or used to trigger duplicate sends.
- Edge extension was discoverable, but both cua.listTabs and the documented browser.tabs.list returned fetch failures after roughly 21 seconds. Its internal cause and upload performance remain unverified; no extension repair success is claimed.
- Reusable rule: direct text transport can continue an existing ChatGPT conversation; distinguish accepted calls, persisted messages, replies and stale UI observations. File delivery still needs a separately verified attachment-capable route.

## 2026-09-11 — Cross-task connection failures and a slow page read

- Stage: investigate failed self-media and litchi handoffs.
- Observed: self-media reported control connection failure and no confirmed recipient. Historical litchi upload failures were followed by the already recorded readable receipt; do not classify that old package as still unsent. In this diagnostic task, two getState calls separated by a reset returned no browser inventory and fetch errors.
- Recovery observed: open_in_codex for the known litchi URL returned queued; after navigating to the owning Codex task, listTabs returned the exact destination. Automatic AX reading through cua.getTab then timed out at 30 seconds. The documented browser/tab handles plus playwright.domSnapshot with a 45-second outer timeout returned page content and composer in about 33 seconds.
- Confirmed: browser inventory and DOM access recovered in this task; the ChatGPT conversation was accessible. No upload or send was performed by this diagnostic task. Self-media recipient remains unconfirmed.
- Unknown: the internal cause of the initial inventory transport failure, whether sidebar activation or the scoped inventory call caused recovery, and whether other tasks recover without the same steps.
- Promoted rule: distinguish inventory availability from page-read latency; use current documented entry points and one bounded read-only fallback before declaring all browser control unavailable.

Use this file for evidence-backed failure patterns that are too detailed for `SKILL.md`. Promote only the reusable decision rule to the entrypoint.

For each new incident, record:

- date and operation stage;
- observed symptom;
- confirmed page/delivery state;
- what remained unknown;
- recovery that actually worked;
- reusable rule promoted to `SKILL.md`.

Do not include credentials, cookies, tokens, private prompt contents, or unrelated project data.

## 2026-09-11 — Control timeout during ChatGPT attachment handoff

- Stage: locate tab, attach ZIP, send, and wait for acknowledgement.
- Observed symptoms: `nodeRepl.fetch request failed`, several approximately 30-second Playwright/CUA timeouts, and JavaScript-session resets.
- Confirmed state: the target ChatGPT conversation remained logged in with a visible composer. After an upload call, the exact ZIP filename appeared in the composer. After a send timeout, the draft and attachment were still present rather than sent. A direct click on `发送提示词` created the sent-message group, and GPT later replied that the file was received and readable.
- Unknown: the internal cause of the CUA/telemetry delay was not observable, so it must not be attributed to the account, website, or network as a single proven cause.
- Recovery: reinitialize with one lightweight call, list existing tabs, reacquire the exact tab, inspect fresh DOM, and perform only the missing transition.
- Promoted rule: every timeout makes the action outcome unknown; verify state before retrying and track `LOCAL_READY → ATTACHED → SENT → RECEIVED_READABLE`.

## 2026-09-11 — Browser inventory unavailable after a clean reset

- Stage: reconnect to the in-app browser before a read-only server-environment check.
- Observed symptom: `cua.getState()` returned no browser inventory and `nodeRepl.fetch request failed`; after resetting the JavaScript session, the first clean `cua.getState()` call returned the same transport error and again no browser inventory.
- Confirmed state: only the control-service transport failure was observed. No page state, login state, tab state, or server state was visible through CUA during these calls.
- Unknown: whether the browser pages themselves were healthy and whether the failure was transient or host-level.
- Recovery: no page-level recovery was attempted because the inventory layer remained unavailable. The task continued with local evidence work and purpose-built thread tools remained the fallback for text-only coordination.
- Promoted rule: after one reset plus one lightweight inventory retry returns the same transport error, stop page-level retries for the turn; do not reload, re-login, re-upload, or resend without fresh state evidence.

## 2026-09-11 — Skill validator location and Windows decoding

- Stage: maintenance verification after updating this skill.
- Observed symptom: the entrypoint named only `quick_validate.py`, so its authoritative location and invocation were ambiguous. Running the bundled validator with the default Python text encoding raised `UnicodeDecodeError` for GBK while reading the UTF-8 skill file; `--help` was interpreted as a skill path rather than a supported help option.
- Confirmed state: the authoritative validator is bundled with `skill-creator` at `C:\Users\HP\.codex\skills\.system\skill-creator\scripts\quick_validate.py`. Running it with `python -X utf8` against this skill returned `Skill is valid!`.
- Unknown: whether a future validator version will set UTF-8 internally or add a help interface.
- Recovery: use the fixed UTF-8 command recorded in `SKILL.md`; do not copy or reimplement the validator inside this skill.
- Promoted rule: validation instructions must name the bundled validator and enable UTF-8 explicitly on this host.
