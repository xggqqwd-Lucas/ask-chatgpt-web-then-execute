---
name: ask-chatgpt-web-then-execute
description: Use the signed-in ChatGPT website in Codex's in-app Browser as an independent reasoning partner, retrieve its visible answer, and then let Codex carry out the requested local work. Use when the user asks to consult, ask, or have ChatGPT Web think first and then wants Codex to implement, edit, test, or otherwise execute. Do not use for ordinary questions that Codex can answer directly or when the user requests the OpenAI API instead of the website.
---

# Ask ChatGPT Web, Then Execute

Use ChatGPT Web for a bounded consultation, then return control to Codex for execution. Keep the two roles distinct: ChatGPT supplies analysis or critique; Codex evaluates that output against the user's request and performs the authorized work.

## Browser Requirement

Use only the Codex in-app browser through the currently exposed `mcp__cua_repl.js` tool and its `cua` API. Do not require the retired `browser:control-in-app-browser` skill, import a cached `browser-client.mjs`, or assume an `agent.browsers` runtime. Read and follow the live documentation returned by the current tool.

On the first `cua_repl` invocation, execute exactly one supported entry-point call, such as `await cua.getState();`, with no additional API calls. Inspect the returned browser inventory and documentation before continuing. Reuse an existing ChatGPT tab in the in-app browser when available; otherwise open one using `await cua.createBrowserTab("iab", "https://chatgpt.com/", { visible: true });`. Browser inventory IDs can differ from the `"iab"` creation alias; use the IDs and tab references returned by the current session for discovery and binding.

Use only interaction methods documented by the selected browser. For DOM-only tabs, use its documented Playwright locators; native input wrappers may be unavailable. Observe the updated page after actions. After a runtime reset or conversation compaction, follow the tool's initialization or documentation-refresh requirements before continuing.

Treat the user's browser choice as a hard constraint; do not substitute Edge, Chrome, an external browser, an API, or a newly created browser profile. If the current session does not expose `cua_repl` or the in-app browser is unavailable, report the specific missing capability. Do not attempt legacy imports or claim that consultation occurred.

Reuse an existing signed-in ChatGPT tab when available. Otherwise open `https://chatgpt.com/` in the in-app Browser. Never inspect, extract, copy, or persist cookies, session data, local storage, passwords, tokens, or account credentials.

If ChatGPT shows a sign-in screen, make the tab visible and hand control to the user. Ask the user to complete passwords, passkeys, OTPs, CAPTCHAs, and other authentication steps personally, then tell Codex when the account is ready. Do not type or read those secrets.

## Consultation Workflow

1. Clarify the deliverable only when the missing detail would materially change the result. Otherwise infer a narrow, reversible experiment or task from the request.
2. Build a compact prompt containing the objective, only the necessary context, constraints, and the desired answer format. Tell ChatGPT to analyze or advise, not to claim that it changed local files or ran Codex tools.
3. Before sending, remove secrets and unrelated local context. Treat submitting text or files to ChatGPT as external transmission. Follow the Browser confirmation rules, and obtain action-time confirmation whenever required, especially for sensitive data, personal files, or representational communication.
4. Submit the prompt in the signed-in ChatGPT tab. Prefer visible labels, roles, and current page state over hard-coded selectors or model names.
5. Wait for a stable completed response. For a quick consultation, poll visible page state in the current turn at reasonable intervals. Do not confuse partial streaming text, a stopped generation, an error banner, or a clarification request with completion.
6. If ChatGPT asks a clarification that requires user judgment, return the question to the user instead of answering on the user's behalf. If the page errors, make one evidence-based retry at most; then report the blocker.
7. Extract the final visible answer and preserve meaningful qualifications, uncertainty, and requested structure. Do not rely on hidden page state.
8. Evaluate the advice independently. Resolve conflicts in favor of the user's instructions, workspace evidence, applicable skills, and verified facts. Do not execute unsafe, unauthorized, irrelevant, or obviously incorrect suggestions.
9. Perform the requested Codex work using normal local tools. Validate the result in proportion to its risk, then report what came from ChatGPT, what Codex decided, and what Codex actually changed or verified.

## Minimal Experiment

For a smoke test, ask ChatGPT a harmless question with a deterministic, easily checked answer, such as:

> Give exactly three concise checks Codex should perform before editing an unfamiliar repository. Use a numbered list and no introduction.

Retrieve the three checks, briefly assess whether they are reasonable, and perform only a harmless read-only local action that demonstrates Codex resumed control, such as listing the current workspace's top-level files. Do not modify project files during the smoke test.

## Boundaries

- Keep ChatGPT in an advisory role. Never represent its answer as verified merely because it came from ChatGPT.
- Do not upload local files unless the user specifically authorizes the exact files and purpose at action time when required.
- Do not perform account management, purchases, sharing, publishing, deletion, or permission changes as part of this skill unless separately and explicitly requested.
- For long-running Deep Research, explain the expected handoff and use an available Codex heartbeat or automation only when the user asks for asynchronous follow-up. Do not busy-wait indefinitely.
- Keep browser tabs temporary unless the user needs the ChatGPT tab for login or continued work; mark that tab for handoff when appropriate.

## Clean-Room Provenance

This skill is an independent implementation of the high-level pattern popularized by `Wangnov/chatgpt-skill`: consult ChatGPT Web, retrieve its answer, then continue in Codex. Do not import or execute that repository, and do not copy its prose, prompts, selectors, scripts, or implementation details into this skill.
