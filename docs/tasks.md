# HostCircle — Tasks

**Owner:** Matthew McCreary, Delivery Lead  
**Week 5 review:** November 19, 2026 · **Week 6 document cross-check:** November 25, 2026  
**Task list approval:** Pending review by Matthew (Delivery Lead), Kenneth (Architecture Lead), and Jacob (Product Owner).

## 1. Task List

**November 25, 2026 — first-build scope:** All three scenarios are approved as P1 (must-have). Start with T0 for setup, then build them in this order:

1. **S1 / R1 — Create a gathering** (T1–T6). A member creates a gathering for their group and can reopen it from the list.
2. **S2 / R2 — Assign a host** (T7–T8). A member of that gathering’s group is chosen as host.
3. **S3 / R3 — Record and review contributions** (T9–T11). The group can see each item alongside the member bringing it.

S2 comes before S3 in the build. The app should let a member add an item before a host is picked: both features need a saved gathering (T6).

The priority column shows what belongs in the first build. The status column shows whether each task is ready to build; T1, T7, and T9 keep their existing blocked statuses until the answers are recorded in the source documents. The Plan gives the team working answers for the Week 6 session.

| ID | Task | Priority | Traces to | Depends on | Done when | Status |
|---|---|---|---|---|---|---|
| T0 | Set up the starter app named in the final Plan. | P1 groundwork | ADR-00 (Plan v1.0) | None | The starter’s sample pages open in the browser. | Not started |
| T1 | Connect the group selector to the agreed source of groups and members. | P1 | R1 | T0 | The group selector shows the groups available to the test member using the setup agreed in Q6. | Blocked (Q6) |
| T2 | Show the gatherings for the selected group. | P1 | R1 | T1 | Selecting a group in the browser shows its gatherings without mixing in another group’s records. | Not started |
| T3 | Add the Create Gathering form. | P1 | R1 | T2 | From the selected group, a member can open the form and enter a title and date/time. | Not started |
| T4 | Save a completed gathering to the selected group. | P1 | R1 | T3 | After saving, the title and date/time appear together in that group’s list. | Not started |
| T5 | Catch missing gathering details. | P1 | R1 | T4 | On the Create Gathering form, a complete entry appears in the group list; leaving out the title or date/time identifies the missing field and adds no entry (R1 acceptance test). | Not started |
| T6 | Open a saved gathering from its group’s list. | P1 | R1, R2, R3 | T5 | Selecting a gathering opens its details with the same title and date/time as the list entry. | Not started |
| T7 | Save the member selected as host. | P1 | R2 | T6 | Using the permissions agreed in Q5, assigning a group member makes that person’s name appear as host on the gathering. | Blocked (Q5) |
| T8 | Reject a host who is outside the gathering’s group. | P1 | R2 | T7 | A group member can be saved and shown as host, but trying an outside member through the simulated API is rejected and leaves the displayed host unchanged (R2 acceptance test). | Not started |
| T9 | Record an item and the member bringing it. | P1 | R3 | T6 | With the contribution and permission rules agreed, saving an item returns its description and responsible member for that gathering in the simulated response. | Blocked (Q3, Q5) |
| T10 | Show the contributions for one gathering. | P1 | R3 | T9 | The gathering’s contribution list shows each saved item beside the name of the person bringing it. | Not started |
| T11 | Catch incomplete contributions. | P1 | R3 | T10 | A complete item and member appear together in the gathering’s list; leaving out either identifies the missing value and adds no entry (R3 acceptance test). | Not started |

## 2. Cross-Check (Week 5)

**Updated November 25 for Week 6.** I matched Jacob’s [Specification, §4](specification.md#4-requirements--acceptance-tests) and Kenneth’s [Data & API Design, §4](data-api-design.md#4-api-endpoints) to my [Plan v1.0, §3](plan.md#3-screens--build-order). All three scenarios are P1, in the order S1 → S2 → S3. The [Review branch used for this check](https://github.com/Jake-C4/INFO-C450-Team-Project/tree/72f97adc0dd78df485f36162c7a2b9944b48192a/docs) still labels them “proposed” and still has the template Plan. This table uses the approved scenario order and the completed local Plan.

| Scenario | Acceptance test | Data & API: what it reads or saves | Plan v1.0 | Tasks |
|---|---|---|---|---|
| S1 / R1 — Create a gathering | A title and date/time appear in the selected group. Leave out either field: identify it and add nothing. | Group, GroupMember, Gathering. `POST` and `GET /v1/groups/{groupId}/gatherings`; `GET /v1/gatherings/{gatheringId}` for detail. | §3 rows 2–4; ADR-01, ADR-02, ADR-04, ADR-05 | T1–T6 |
| S2 / R2 — Assign a host | Show the selected group member as host. Reject someone outside the group and keep the saved host. | Gathering.host_member_id, Member, GroupMember. `PATCH` and `GET /v1/gatherings/{gatheringId}`. | §3 rows 4 and 5; ADR-01 through ADR-04 | T6–T8 |
| S3 / R3 — Record and review contributions | Show the item next to the person bringing it. Leave out either value: identify it and add nothing. | Contribution, Gathering, Member, GroupMember. `POST` and `GET /v1/gatherings/{gatheringId}/contributions`. | §3 rows 4 and 6; ADR-01 through ADR-04 | T6, T9–T11 |

**Result:** All three requirements trace through to a task. Going back the other way, every numbered Plan row and every task is accounted for. T0 covers shared setup (row 1, ADR-00). T6 opens the gathering that S2 and S3 both use. The cross-check now has all its references. Questions Q3, Q5, and Q6 in the Specification and the team’s document sign-offs are still open.

## 3. Gap Log (Weeks 6–7)

**November 25, 2026 — Week 6 team validation.** Jacob (Product Owner), Kenneth Elmore (Architecture Lead), and Matthew McCreary (Delivery Lead) worked through T0–T11. For this session we used the documents from `main` plus Matthew’s local Plan v1.0. We built in `validation/`, one task at a time. Section 1 keeps the task statuses for the final build.

**What we saw**

| Tasks | Result |
|---|---|
| T0–T6 · Create a gathering (S1/R1) | The starter’s detail page opened. Alex could choose Game Club or Sunday Supper. We saved Game Night in Game Club and it stayed after a refresh. An empty title or date/time produced an error beside that field and added no gathering. For Sunday Potluck, the list and detail showed the same time, and **Back to group** returned to Sunday Supper. |
| T7–T8 · Pick a host (S2/R2) | Jo stayed listed as host after a refresh. We didn’t see Sam in Game Club’s picker. Copilot also tried saving Sam on a test copy of Game Night. The save was rejected and Jo stayed host. T7 already handled this rule, so T8 needed a check but no new code. |
| T9–T11 · Record who’s bringing what (S3/R3) | In a separate test, Copilot saved “Two bags of ice” for Jo before a host was picked. On Sunday Potluck, “Paper Plates — Alex” stayed visible after a refresh. The final missing-item and missing-member checks showed “Enter an item to bring.” and “Choose a responsible member.” The saved count stayed at one. |

**What we need to clarify**

| Task | Question or assumption | Document and section | What we used in Week 6 | Who will follow up? | Week 7 fix and re-run result |
|---|---|---|---|---|---|
| T2 | The Plan names Sunday Potluck but doesn’t give that sample gathering its own date and time. | Plan §2, Q6 and ADR-05; §4 time-zone example | Copilot reused the Indianapolis example: December 5, 2026, at 7:00 p.m. EST. | Matthew and Kenneth: agree on a sample time and use it in the Plan and test data. | Pending. |
| T9, T11 | When there is only one eligible member, does the form start blank or select that person? The rule requires a member, but doesn’t say how the picker starts. | Specification §4, R3; Plan §3, row 6; Tasks §1, T11 | Sunday Supper had only Alex. Some automated attempts reused a previous value and saved a temporary item. Copilot removed those items; it checked an explicitly blank member and repeated the tests on a fresh form. The errors worked. The intended default is still open. | Jacob: confirm the intended first choice. Kenneth: check the save rule. Matthew: write the steps for repeating the check by hand. | Pending. |

For this run we used fixed sample groups (Q6), one item and one responsible member (Q3), and permission for a group member to record an agreed host or item for another member of the same group (Q5). Those are the Plan’s working answers. The Specification still needs the team’s decisions recorded in §5.

## 4. Build Notes (Week 8)

Reserved for the final build: live URL, simulated requirements, and anything left out.

## SVC Self-Check

- [x] **Simple:** Each task describes one step and an observable result.
- [x] **Valuable:** Every task points to a requirement or to the shared starter decision.
- [ ] **Complete:** The P1 cross-check and Week 6 live session are recorded. Team approval, the Week 7 fixes and re-run, and the Week 8 build notes are still pending.
