# HostCircle — Tasks

**Owner:** Matthew McCreary, Delivery Lead  
**Week 5 review:** November 19, 2026  
**Approval:** Pending review by Matthew (Delivery Lead), Kenneth (Architecture Lead), and Jacob (Product Owner).

## 1. Task List

Jacob said to build **creating a gathering (R1)** first. A gathering gives the group a place to name a host and list what people are bringing. The [Specification, Section 4](specification.md#4-requirements--acceptance-tests) still calls all three features *proposed P1*. Picking a host (R2) and tracking contributions (R3) are listed so we can review them. Their order and inclusion in the first build are still open. Either can follow R1; recording an item does not require a host to be assigned first.

These are planned build tasks. The checks describe what we will look for during the build.

| ID | Task | Priority | Traces to | Depends on | Done when | Status |
|---|---|---|---|---|---|---|
| T0 | Set up the starter app named in the final Plan. | P1 groundwork | ADR-00 (draft) | None | The starter’s sample pages open in the browser. | Not started |
| T1 | Connect the group selector to the agreed source of groups and members. | Proposed P1 | R1 | T0 | The group selector shows the groups available to the test member using the setup agreed in Q6. | Blocked (Q6) |
| T2 | Show the gatherings for the selected group. | Proposed P1 | R1 | T1 | Selecting a group in the browser shows its gatherings without mixing in another group’s records. | Not started |
| T3 | Add the Create Gathering form. | Proposed P1 | R1 | T2 | From the selected group, a member can open the form and enter a title and date/time. | Not started |
| T4 | Save a completed gathering to the selected group. | Proposed P1 | R1 | T3 | After saving, the title and date/time appear together in that group’s list. | Not started |
| T5 | Catch missing gathering details. | Proposed P1 | R1 | T4 | On the Create Gathering form, a complete entry appears in the group list; leaving out the title or date/time identifies the missing field and adds no entry (R1 acceptance test). | Not started |
| T6 | Open a saved gathering from its group’s list. | Proposed P1 | R1, R2, R3 | T5 | Selecting a gathering opens its details with the same title and date/time as the list entry. | Not started |
| T7 | Save the member selected as host. | Proposed P1 | R2 | T6 | Using the permissions agreed in Q5, assigning a group member makes that person’s name appear as host on the gathering. | Blocked (Q5) |
| T8 | Reject a host who is outside the gathering’s group. | Proposed P1 | R2 | T7 | A group member can be saved and shown as host, but trying an outside member through the simulated API is rejected and leaves the displayed host unchanged (R2 acceptance test). | Not started |
| T9 | Record an item and the member bringing it. | Proposed P1 | R3 | T6 | With the contribution and permission rules agreed, saving an item returns its description and responsible member for that gathering in the simulated response. | Blocked (Q3, Q5) |
| T10 | Show the contributions for one gathering. | Proposed P1 | R3 | T9 | The gathering’s contribution list shows each saved item beside the name of the person bringing it. | Not started |
| T11 | Catch incomplete contributions. | Proposed P1 | R3 | T10 | A complete item and member appear together in the gathering’s list; leaving out either identifies the missing value and adds no entry (R3 acceptance test). | Not started |

## 2. Cross-Check (Week 5)

I compared the [Specification acceptance tests](specification.md#4-requirements--acceptance-tests) with the [Data & API Design](data-api-design.md#4-api-endpoints), the [Plan’s screen and build order](plan.md#3-screens--build-order), and the tasks above. The API paths below describe the simulated course build.

| Req ID | Acceptance test: what to check | Data & API: entity / endpoint | Plan: ADR or screen | Tasks |
|---|---|---|---|---|
| R1 — Create a gathering | Enter a title and date/time: both appear in the selected group’s list. Leave out either: no gathering is added, and the form identifies what is missing. | Group, GroupMember, Gathering. `POST` and `GET /v1/groups/{groupId}/gatherings` save and list gatherings; `GET /v1/gatherings/{gatheringId}` opens one. | Pending: Section 3 has no entry for selecting a group, creating a gathering, or opening one. | T1–T6 |
| R2 — Assign a host | Assign someone in the group: their name appears as host. Try someone outside the group: the assignment is rejected. | Member, GroupMember, Gathering (`host_member_id`). `PATCH /v1/gatherings/{gatheringId}` saves the host; the gathering read returns the host reference. | Pending: Section 3 has no entry for assigning a host on a gathering. | T6–T8 |
| R3 — Track who is bringing what | Enter an item and member: both appear together. Leave out either: nothing is added, and the form identifies what is missing. | Gathering, Member, GroupMember, Contribution. `POST` and `GET /v1/gatherings/{gatheringId}/contributions` save and list contributions. | Pending: Section 3 has no entry for recording or viewing contributions. | T6, T9–T11 |

**Result:** All three proposed requirements have an acceptance test, a data/API path, and tasks. T0 is shared setup and points to ADR-00. The cross-check is **not complete**: ADR-00 still has a blank database choice, and the Plan does not yet have screen entries to trace back to the requirements.

Before sign-off:

- **Jacob:** Record whether R2 and R3 must be in the first build alongside R1. Confirm the contribution details (Q3) and who can assign hosts or record items for someone else (Q5).
- **Kenneth:** Finish the Plan on `Review`, including ADR-00, the screen order, and how group/member choices will be available (Q6). The API draft does not define group or member setup endpoints; fixed sample data and managing real groups would lead to different tasks.
- **Matthew:** Replace the pending Plan references with its actual screen numbers or ADRs, check that every P1 screen has a task, and update the priorities and order to match. Check the Style Guide too: it mentions Dashboard and Profile, but neither has a requirement in the current Specification. Bring that difference to the team before adding either screen.
- **Team:** Review the revised list and record the approval date once all three leads agree.

## 3. Gap Log (Weeks 6–7)

The validation build has not been recorded here yet. During the build, record what the builder had to guess, which document caused it, what changed, and whether the next run confirmed the fix.

## 4. Build Notes (Week 8)

Record the live app address, which requirements were simulated, and any lower-priority tasks left out after testing.

## SVC Self-Check

- [x] **Simple:** Each row names one action and says where to check the result.
- [x] **Valuable:** Every task points to a requirement or to the starter decision in ADR-00.
- [ ] **Complete:** The final P1 list, Plan references, build order, and team approval are still pending. The gap log and build notes belong to later weeks.
