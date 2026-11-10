# BrowserStack Test Management: Demo Notes (TAB / Tabcorp)

| | |
|---|---|
| **Meeting** | Sidhant's Zoom Meeting (BrowserStack demo for Tabcorp QA) |
| **Date** | 11 March 2026 (from recording start time) |
| **Length** | ~51.5 minutes. Screen share from 00:52–08:42 and 09:21–48:43 |
| **Source files** | `Sidhant's Zoom Meeting.mp4`, `Sidhant's Zoom Meeting.transcript.vtt` / `.txt`, `Sidhant's Zoom Meeting.chapter.vtt` |

> These notes come from the Zoom transcript and chapter markers. I did not review the video frames (ffmpeg isn't installed), so screen-only details like exact UI labels and menu paths may be missing. Names are spelled as Zoom's auto-transcript wrote them and may be wrong: "Namrish/Amish/Amrish" is the speaker shown as `saraiyan`, and "TAPCOP/Tapcode" means Tabcorp.

## Attendees

| Name | Side | Role in call |
|---|---|---|
| Sidhant | BrowserStack | Account / sales lead, ran the wrap-up |
| Shrikrishna Umbare ("Sri") | BrowserStack | Solutions engineer, gave the demo |
| saraiyan ("Namrish") | Tabcorp | QA lead / decision maker, asked most questions |
| Neha Arora | Tabcorp | QA, asked about tags |
| Subodh Kant Dinanath Verma | Tabcorp | QA, asked about grouping test runs |

## Context

- This was a follow-up to an earlier call. That call covered pain points but had no end-to-end demo. Tabcorp asked to see the AI features on their own use cases.
- Sri prepared a **sample PRD** for a TAB feature (sharing a pending bet via a unique shortened URL / deep link). He also created sample Jira items: **Tabcorp Requirement 1**, **Requirement 2** and **Defect 1** (one Jira key appears as "CAN-7").
- Tabcorp asked him to start from basics because the team had not used the tool before.

## Product overview: BrowserStack Test Management (TM)

Main sections: **Test Cases → Test Runs (manual + automation) → Test Plans → Reports**, plus **Project Insights** (dashboards).

### 1. Projects and import
- The dashboard lists all projects. You can create a new project.
- **Quick import** from TestRail, Xray, Zephyr Scale, qTest and others. CSV import is also available.

### 2. Test cases
- Folder hierarchy with **nested folders** (folders inside folders, like a file manager). The demo used *Security* and *Authentication* folders.
- Ways to create test cases:
  - **Manual**: title, description, preconditions, steps and expected results. Steps can be reordered by drag and drop, and URLs or files can be attached.
  - **AI-assisted manual**: type a title, then click AI to fill in the description, preconditions and steps. Context comes only from the title and description.
  - **Generate with AI from requirements**. Sources include Figma, Azure DevOps, Jira, Linear, Confluence, SharePoint, OneDrive and file upload. **You can combine several sources**, e.g. Jira plus Figma.
- AI generation settings:
  - Filter by category
  - Output format: plain text / steps / **BDD**
  - Priority and language
  - Optional **generated datasets**
  - Free-text prompt to steer the output
  - **Refine** prompt after generation, e.g. merge small repeated steps
- AI flow: it builds context from the inputs, groups it into categories, then generates cases. The demo PRD produced roughly 38–50 cases, grouped into categories such as "Visibility of share button" and "Generation of unique shortened URL for bet sharing".
- **Human in the loop**: generated cases must be reviewed before they are added. Add one at a time or all at once. Added cases get an **AI-generated tag**.
- **Fields**: owner, state, priority, type and tags. **Custom fields** (single or multi-select with custom options) work on test cases, test results and test plans. Fields can be mapped during import.
- **Datasets** (data-driven tests): a table of variables and values with rows and columns, e.g. several username/password pairs. Datasets can be **step-level**. A manual run then expands per data row.
- Grid: show or hide columns. Bulk actions include edit, create, export and share.

### 3. Jira integration (two-way)
- **TM → Jira**: link a requirement to test cases in TM. A **TM plugin in Jira** then shows the linked test cases and runs on the Jira issue.
- **Jira → TM**: in the Jira plugin, use *Generate test cases using AI* or *Generate test runs using AI*. Pick the TM project and folder, and it builds context from the issue description. Results are still reviewed before they are added.
- **Defects**: when a manual step fails you can link an existing Jira defect or create a new one. The defect issue shows the test result and which step failed.

### 4. Test runs
- **Manual runs**: select cases using filters such as priority (P0/critical), add **configurations** (OS/device/browser, e.g. Windows 10 Pro, Android 13 Pixel), assign a user, and link requirements and a test plan. Run size = cases × configurations (2 × 2 = 4).
- Configurations are only labels from a default list and can be extended. They don't open devices. Testers can launch the matching device in **BrowserStack Live** (real-device manual testing) if that combination exists.
- Execution: mark each step pass/fail and add notes, files and defects. Overall status can be Passed, Failed, Retest or Blocked. Progress and a summary of passed/failed/blocked are shown.
- **Clone run**, e.g. for the next version, with options to copy all cases and tags.
- **Run groups**: select runs and choose *Add to run group*. Example use: older-OS sanity runs vs latest-OS integration runs.
- **Tags** on runs, including custom tags such as squad or environment, to categorise runs.
- **Automation runs**: results from BrowserStack Automate or other suites show up in TM. **Test case ID tagging** maps automated tests to manual cases, so each case shows both its manual and automated history. Runs are created automatically when Automate is integrated.

### 5. Test plans
- Similar to a sprint: start and end dates, owner, description, files and linked requirements.
- Link several test runs to see per-run and overall progress, pass/fail counts and results over time. Overdue plans are flagged and can be marked complete.

### 6. Reports
- Categories: **Requirement traceability** (requirement → test cases → runs → defects → configuration → plan → executor and time), **Test run summary**, **Test plan summary**, **Test run details** and **Test case activity**.
- Filter by requirement, specific cases and so on. **Scheduled reports** are supported.

### 7. Project Insights (dashboards)
- Default widgets: active runs, pass/fail, test case types, trends and defect log.
- Custom dashboards and widgets, e.g. test case breakdown or active runs.

### AI and data privacy
- Uses several LLMs, including **Anthropic and OpenAI**.
- BrowserStack says customer data, including uploaded PRDs, is **not used for training**. Models are trained on BrowserStack's own dummy data. *Worth confirming in writing during security review.*

## Tabcorp requirements and concerns raised

| # | Raised by | Need | Response | Status |
|---|---|---|---|---|
| 1 | saraiyan | **Limit how many AI test cases are generated** (e.g. default 3 end-to-end cases). Reviewing 50 by hand isn't practical, and an agentic workflow needs a configured default. | No configurable limit today. You might steer it in the prompt. Will check with product. | **Open: BrowserStack to follow up** |
| 2 | saraiyan | **Release → cycle → squad structure**: Release 1.0 has Integration (new features) and Regression (impacted areas) cycles, each across 2–4 OSes, with ~20 squads contributing. | Today's model is runs plus clone per version. Sri wasn't sure how to model this. saraiyan said the current approach works only for 1–2 teams and "is not going to be useful at all" as shown. | **Open: blocker. BrowserStack to follow up** |
| 3 | saraiyan | Test data per test case and **per step**, visible during execution | Datasets, including at step level. Manual runs expand per data row. | Answered, to verify in POC |
| 4 | saraiyan | How data sets and case selection work in the integration phase | Covered in test runs (filters, configurations) | Partly answered |
| 5 | saraiyan | Where configuration options come from | Default list that can be extended. Labels only, use Live for real devices. | Answered |
| 6 | Neha Arora | Tag runs by squad or environment | Yes, custom tags and fields in settings | Answered |
| 7 | Subodh Verma | Group runs, e.g. sanity on older OSes vs integration on latest OSes | Run groups | Answered |
| 8 | saraiyan | Is uploaded PRD data used for training? | No | Answered (verbally) |

Other context: saraiyan mentioned a separate conversation about a **web rebuild** and wants to avoid doing work twice.

## Outcome and next steps

- When asked whether the features match current workflows, saraiyan said **"No"** for now.
- Next step: **POC**. saraiyan will contact Sidhant separately with a **list of workflows to try** and tentative dates. He wants to start **the week after the call**, without clashing with releases.
- BrowserStack will check with product on **item 1** (AI case limit) and **item 2** (multi-squad release structure).
- BrowserStack will hold **weekly or twice-weekly check-ins** during the POC.
- Sidhant shared a **getting-started documentation link** in the Zoom chat (not in the transcript) and will share Sri's email.

## Suggested POC workflows (my suggestions, not from the meeting)

1. Model one real release (e.g. a Flutter app release) with Integration and Regression cycles, several squads and iOS/Android configurations. Check that dashboards and reports roll up per release, cycle and squad (item 2).
2. Generate AI test cases from a real Jira story plus a Figma design. Check quality, duplication and whether the prompt can cap the count (item 1).
3. Import existing test cases (CSV or current tool) with custom-field mapping.
4. Data-driven login case with a step-level dataset, run manually on 2 configurations.
5. Connect automation (e.g. Flutter integration tests or web automation on BrowserStack Automate) using test case ID tagging. Check that manual and automated history appear together.
6. Jira two-way flow: requirement link, failed step → defect, then a requirement traceability report.
7. Security and privacy: get written confirmation on LLM data handling and retention.
