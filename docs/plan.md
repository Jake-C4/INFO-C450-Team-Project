# HostCircle — Plan

**Plan owner and maintainer:** Matthew McCreary (Delivery Lead)  
**Version:** 1.0 · November 25, 2026  
**Team approval:** Pending. Reviewers: Jacob (Product Owner), Kenneth Elmore (Architecture Lead), and Matthew McCreary (Delivery Lead). No approval date has been recorded.

## 1. Approach

HostCircle gives a group one place to see when it’s getting together, who agreed to host, and who’s bringing each item. All three scenarios are approved as must-haves (P1). The build order is **S1 / R1 — create a gathering**, **S2 / R2 — assign its host**, then **S3 / R3 — record what people are bringing**. The class version uses fictional groups and saves changes in one browser so we can try the whole flow without setting up accounts.

## 2. Key Decisions (ADRs)

An ADR records a choice and why we’re using it. These are the decisions for this version of the Plan.

| ADR | Decision | Req IDs | Alternative considered | Why this one |
|---|---|---|---|---|
| ADR-00 | Start with the course’s `collection-template`: Vue 3 with no build step, Vue Router 4 with hash routes, and Bootstrap 5. Simulate the backend for class. Design for **PostgreSQL via Supabase** later. The final app will run on GitHub Pages from the repository root. | R1, R2, R3 | Connect the class app to a live database now. | We can try each scenario without accounts or database keys. The trade-off is that changes aren’t shared across browsers. |
| ADR-01 | Use one fictional current member and a fixed set of groups and memberships. Skip the login picker and group setup for this build. | R1, R2, R3 | Add sign-in and let people create groups. | The scenarios start with someone who already belongs to a group. Fixed memberships let us check who can see a gathering and who can host it. |
| ADR-02 | Use one seed JSON file per entity in `data/`. Put reads and writes in `data.js`. Load the seeds on first use and keep edits in `localStorage` after that. | R1, R2, R3 | Have each page read and change its own copy. | A gathering saved on one page must still be there when someone opens its details or refreshes. |
| ADR-03 | Show the host and contributions on the same gathering detail page. | R2, R3 | Give each feature its own page. | A member can see “Jo is hosting” and “Alex is bringing ice” without finding the gathering twice. |
| ADR-04 | Check group membership in `data.js` before reading or changing a gathering. Also check that a selected host or contributor belongs to that group. | R1, R2, R3 | Rely only on the names shown in each picker. | A name sent directly to the simulated save function must pass the same check as a name picked on screen. |
| ADR-05 | Accept the date/time in the viewer’s local time, store and return it in UTC, and show local time with the zone on both the list and detail page. | R1 | Display UTC everywhere. | A person who enters 7 p.m. should see 7 p.m. when reopening the gathering in the same time zone. |

**Rules to use for this build:** The [Specification, §5](specification.md#5-open-questions) still lists Q3, Q5, and Q6 as open. The rules below give each affected task a clear starting point for validation; they don’t record a team decision in the Specification.

| Question | Rule for this version |
|---|---|
| Q6 — Where do groups and people come from? | Start as **Alex**. Alex belongs to **Game Club** and **Sunday Supper**. **Jo** belongs to Game Club; **Sam** belongs only to **Book Swap**. Read the choices from `GroupMember` in `data.js`; no new group-management endpoint is needed. Game Club starts empty; Sunday Supper has a “Sunday Potluck” gathering. |
| Q3 — What counts as a contribution? | Save one item description and one responsible group member. “Two bags of ice” can be the whole description; there’s no separate quantity or category field. |
| Q5 — Who can enter a host or item? | A member of the gathering’s group may record the host the group agreed on or an item another group member agreed to bring. No member gets permanent control. Editing or deleting contributions, ranking members, and automatically choosing the next host are outside this build. |

**Data contract:** Follow the [Data & API Design](data-api-design.md), §§1 and 4. The five entities are `Group`, `Member`, `GroupMember`, `Gathering`, and `Contribution`. Each documented endpoint gets one matching function in `data.js`; the app calls these functions locally. Keep the stored field names and the response names as documented: for example, `host_member_id` is returned as `hostMemberId`. A contribution’s `memberDisplayName` comes from the Member record.

## 3. Screens & Build Order

The row numbers below are the references for [Tasks, §2](tasks.md#2-cross-check-week-5). Rows that share a route are parts of the same page.

| # | Screen or part | Route | What the member sees or does | Req IDs | Priority | Tasks |
|---|---|---|---|---|---|---|
| 1 | Starter and shared data setup | No separate screen | At T0, open the starter’s sample list and detail page. Add HostCircle data functions as each scenario needs them. | R1, R2, R3 | P1 setup | T0 |
| 2 | Select a group and see its gatherings | `#/groups`; `#/` opens here | Start in Game Club. Show only Alex’s groups. Switching groups shows that group’s gathering titles and times. If the list is empty, show “No gatherings yet.” | R1 | P1 | T1–T2 |
| 3 | Create Gathering form | `#/groups`, in the selected group | Select **Create Gathering**, enter a title and date/time, then save. Show confirmation and the new entry in this group’s list. If either field is blank, identify it beside the field and save nothing. | R1 | P1 | T3–T5 |
| 4 | Gathering detail | `#/gatherings/:id` | Select a title in the list. Show the same title and local date/time. **Back to group** returns to that gathering’s group. Rows 5 and 6 use the gathering opened here. | R1, R2, R3 | P1 | T6 |
| 5 | Host | On gathering detail | Show “No host assigned” until someone is picked. Choose a group member, save, and show their name. If an outside member is sent to the save function, explain why it was rejected and keep the previous host. | R2 | P1 | T7–T8 |
| 6 | Contributions | On gathering detail | Enter an item, choose its responsible member, and select **Add Contribution**. Show each saved item beside its member. Identify any missing field and add nothing until both are filled. | R3 | P1 | T9–T11 |

**Why this order:** Saving one gathering and opening it again gives us a real record in the prototype for checking the other two scenarios. We build the host feature before contributions, but a member can offer to bring something before a host is picked. Both features depend on the gathering, not on each other.

**Checks to use with the task list:** These are expected results, not reports of completed tests. T0 and ADR-00 apply to all three.

| Scenario | Plan and task references | Data & API operations | How to check it |
|---|---|---|---|
| S1 / R1 | Rows 2–4; ADR-01, ADR-02, ADR-04, ADR-05; T1–T6 | `POST` and `GET /v1/groups/{groupId}/gatherings`; `GET /v1/gatherings/{gatheringId}` | Create “Game Night” in Game Club. Its title and time appear in Game Club, survive a refresh, and match the detail page. Sunday Supper stays unchanged. Try once without a title and once without a date/time: each attempt identifies the missing field and adds no gathering. |
| S2 / R2 | Rows 4 and 5; ADR-01 through ADR-04; T6–T8 | `PATCH /v1/gatherings/{gatheringId}`; read back with `GET` on the same path | Assign Jo to Game Night. Jo’s name appears as host. Try Sam through the simulated save function: it rejects the assignment and Jo stays listed. |
| S3 / R3 | Rows 4 and 6; ADR-01 through ADR-04; T6, T9–T11 | `POST` and `GET /v1/gatherings/{gatheringId}/contributions` | Open a gathering with no host. Save “Ice” for Alex: the item and name appear together there. Try again without the item, then without the member. Each attempt names the missing field and leaves the list unchanged. |

**Week 6 setup:** From the local team repository, use `docs/` for the planning documents and `validation/` for the temporary app. Start the live team session with T0 only. Once it creates the starter, serve that folder locally; for example, run `python -m http.server 8000 --bind 127.0.0.1 --directory validation` and open `http://127.0.0.1:8000/`. Continue with one P1 task at a time. Record guesses or disagreements in [Tasks](tasks.md), §3, including which document and section caused them. Leave task statuses unchanged and keep the temporary app out of GitHub.

## 4. Risks & Assumptions

| Risk or assumption | Likelihood | Impact | What I’ll do about it |
|---|---|---|---|
| A group may need item quantities, different rules about entering items for other people, or a way to add members. | Medium | High | Use the Q3/Q5/Q6 rules above in validation. Check them with Product and Architecture. If an answer changes, update the Specification, data design, Plan, and affected tasks before changing the app. |
| A time near midnight UTC could show the wrong day locally. | Medium | High | In an Indianapolis time zone, enter December 5, 2026 at 7 p.m. The stored value should be `2026-12-06T00:00:00Z`, but the list and detail should both show December 5 at 7 p.m. |
| Someone may expect a second browser to show the same changes or mistake a demo member for a real account. | High | Medium | Use fictional names, identify Alex as the demo member, and use one browser for the walkthrough. Shared data or real access control would need a separate backend. |
| The Style Guide names Dashboard and Profile, but the three scenarios don’t use either page. | Medium | Medium | Use the [Style Guide](style-guide.md) for labels, buttons, confirmations, errors, and keyboard focus. For this build, use Groups and gathering detail for navigation. I’ll bring the extra navigation labels into line when I update the guide. |
| Displaying the agreed host may not be enough to help the group share hosting fairly. | Medium | Medium | Ask whether members need a next-host suggestion during evaluation (Specification Q1). Keep recording the agreed host in this version; don’t introduce a score or ranking. |

## SVC Self-Check

- [x] **Simple:** The approach describes one gathering from start to finish. Each ADR states a choice and a reason.
- [x] **Valuable:** Every decision and screen points to a requirement. Each P1 scenario has a task and a result someone can check.
- [ ] **Complete:** The stack, database target, screen order, and risks are filled in. Formal team sign-off is still pending.

I’ll keep this file at `docs/plan.md`. When a decision changes, I’ll add a new ADR and mark the old one “Replaced by” the new ID so the task references still make sense. After a change to an approved version, I’ll bump the version, check the affected tasks, and record the review date once the team agrees.
