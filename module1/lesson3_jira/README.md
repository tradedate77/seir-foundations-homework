# Module 1 / Lesson 3 — Jira Cloud + Agile: "A Day in My Shoes"

**Course:** SEIR Foundations
**Student:** Terelle Dorceus
**Space:** `SEIR-Saga_TerelleDorceus` (key: `SST`)

---

## 1. Overview

For this assignment I broke down one ordinary day of my life into an epic, stories, and
tasks in Jira Cloud, using a team-managed Scrum space. The epic is **`A day in my shoes`**
(SST-2), the five stories are the major blocks of my day, and the tasks are the individual
actions inside each block.

Every story and every task carries a **Story point estimate** and a **description of at
least two sentences**. The epic carries no points — it is the overarching goal, and the
work beneath it carries the estimate.

- **Estimation:** turned on space-wide under **Space settings → Features → Estimation → Story points**
- **Hierarchy:** the epic (SST-2) is the Parent of every story and every task
- **Task naming:** each task begins with its block in square brackets, e.g. `[Morning]`, `[Study]`, `[Laundry]`, `[Errands]`, `[Evening]`

---

## 2. Evidence table

Every work item I created, with its key, type, summary, and story point estimate.

| Key | Type | Summary | Story points |
|-----|------|---------|--------------|
| SST-2 | Epic | A day in my shoes | — |
| SST-3 | Story | Morning. Get started with the day | 5 |
| SST-4 | Story | Study SEIR training | 5 |
| SST-5 | Story | Errands including ATM run and store visit | 3 |
| SST-7 | Story | Evening including hygiene and bed prep | 1 |
| SST-20 | Story | Complete Laundry | 3 |
| SST-8 | Task | [Morning] Brush my teeth and clean my dental aligners | 1 |
| SST-9 | Task | [Morning] Shower and wash hair | 1 |
| SST-10 | Task | [Morning] Get Dressed | 1 |
| SST-11 | Task | [Morning] Make bed | 1 |
| SST-12 | Task | [Morning] Make Breakfast | 1 |
| SST-13 | Task | [Morning] Eat Breakfast | 1 |
| SST-14 | Task | [Morning] Wash dishes | 1 |
| SST-15 | Task | [Study] Finish reading Head First Agile | 2 |
| SST-16 | Task | [Study] Read and understand the homework assignment | 1 |
| SST-17 | Task | [Study] Create the Homework Assignment Jira | 3 |
| SST-18 | Task | [Study] Proofread Jira to make sure it meets requirements | 1 |
| SST-19 | Task | [Study] Work on the BAM extra credit exercises | 1 |
| SST-21 | Task | [Laundry] Sort clothes | 1 |
| SST-22 | Task | [Laundry] Wash and dry dark clothes | 1 |
| SST-23 | Task | [Laundry] wash and dry light clothes | 1 |
| SST-24 | Task | [Laundry] wash and dry white clothes | 1 |
| SST-25 | Task | [Laundry] Fold clothes | 2 |
| SST-26 | Task | [Laundry] Put away clothes | 1 |
| SST-27 | Task | [Evening] Eat Dinner | 1 |
| SST-28 | Task | [Evening] Put fresh sheets on bed | 1 |
| SST-29 | Task | [Evening] Brush teeth and dental aligners | 1 |
| SST-30 | Task | [Evening] Put on pajamas | 1 |
| SST-31 | Task | [Evening] Go to sleep | 1 |
| SST-32 | Task | [Errands] Check my engine Oil | 1 |
| SST-33 | Task | [Errands] Deposit money into bank account | 1 |
| SST-34 | Task | [Errands] Exchange water cooler bottles at supermarket | 1 |
| SST-35 | Task | [Errands] Purchase groceries for the week | 1 |
| SST-36 | Task | [Errands] Fill up car at the gas station | 1 |

**Item count:** 1 epic, 5 stories, 28 tasks

---

## 3. Summary figures

- **Space name and key:** `SEIR-Saga_TerelleDorceus` / `SST`
- **Total story points of all stories and tasks:** 49

---

## 4. My biggest task, and why I estimated it that way

My biggest task was **SST-4, Study SEIR training**, which I estimated at **5 story points**.
It covers the reading and homework work for the SEIR Foundations course, which takes multiple
sittings and is the largest block of focused effort in my day. I gave **SST-3, Morning**, the
same value because getting the entire morning routine done — shower, teeth, breakfast,
dishes, out the door — is comparably demanding. I anchored the middle of the scale at **3**
for work like the errand run and the full laundry cycle, and used **1** for single actions
that take only a few minutes, such as making the bed or brushing my teeth.

---

## 5. Screenshots

All screenshots are stored in `screenshots/` and every story and task screenshot shows the
**Story point estimate** field.

| Screenshot | What it shows |
|------------|---------------|
| `jira_estimation_on.jpg` | Space settings → Features, with **Estimation ON** and **Story points** selected |
| `jira_epic.jpg` | The epic `A day in my shoes` (SST-2) opened, with its child work items and their story points |
| `jira_epic2.jpg` | Continuation of the epic's child work items (additional rows below the fold) |
| `jira_story.jpg` | Story SST-4 opened: Parent (the epic), description, and Story point estimate |
| `jira_task.jpg` | Task SST-15 opened: Parent (the epic), description, and Story point estimate |
| `jira_backlog.jpg` | The backlog showing stories and tasks with their story point values |
| `jira_backlog2.jpg` | Continuation of the backlog (additional rows below the fold) |

The epic and backlog were each captured in two images so that the evidence stays large
enough to read; the second image in each pair continues the same view.

---

## 6. Repository structure

```
seir-foundations-homework/
└── module1/
    └── lesson3_jira/
        ├── README.md                 (this file — evidence table and summary figures)
        └── screenshots/
            ├── jira_estimation_on.jpg
            ├── jira_epic.jpg
            ├── jira_epic2.jpg
            ├── jira_story.jpg
            ├── jira_task.jpg
            ├── jira_backlog.jpg
            └── jira_backlog2.jpg
```

No API tokens, webhook URLs, or `.env` contents appear in this repository or in any screenshot.
