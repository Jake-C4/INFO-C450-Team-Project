# HostCircle — Specification

**Version:** 0.2 (draft)

**Review status:** The Constitution is approved. This is the Week 2 draft for team review; prototype testing and final approval remain Week 3 work.

## 0. Constitution

Non-negotiable principles this product must never violate, regardless of feature.

| # | Principle | Why it exists |
|---|---|---|
| **1** | **HostCircle must support fair hosting rotation and must not give one regular group member permanent control over another member's hosting assignments or contributions.** | The purpose of HostCircle is to help groups share hosting responsibilities fairly. Giving one regular member permanent control over another member's participation could work against that purpose. |
| **2** | **HostCircle must not publicly rank, score, or shame members based on how often they host or contribute.** | Tracking is meant to help the group stay organized and fair, not turn gatherings into a competition or create pressure between friends. |
| **3** | **HostCircle must only collect information needed to organize the group, its gatherings, hosting rotation, and contributions.** | The product should stay focused on organizing gatherings and avoid collecting unnecessary personal information. |

---

## 1. Problem & Evidence

**Problem and success:** Early interviews suggest that planning can be spread across texts, social media events, and memory, making it hard to keep track of guests and what people are bringing. One host described duplicate snacks and missing ice, another saud some friends miss event details and that the same person usually hosts. These are early findings from three people, not proof that all groups have the same problem. Proposed success measure during a four-week pilot, at least 80% of gatherings created have a host and at least one contribution recorded, with participant feedback about whether responsibilities feel fairly shared. The team must confirm the measure and whether a pilot is feasible.

**Evidence:**

| ID | What we heard or saw | From whom |
|----|----------------------|-----------|
| E1 | Planning begins about 2–3 weeks before an event; the host selects a date and uses a group chat to estimate attendance. | Two friends who host gatherings; combined interview notes. The notes do not separate which person made each point. |
| E2 | Food and drinks are difficult to coordinate, the notes describe duplicate snack items while important things such as ice may be missed. | Same combined interview notes as E1. |
| E3 | Hosting is chosen randomly, while the host keeps a list of people and the items they plan to bring to avoid duplicates or anything missing. | Same combined interview notes as E1. |
| E4 | The host keeps backup supplies and may ask someone else to cover an item when a person cancels. | Same combined interview notes as E1. |
| E5 | She plans through texts and Facebook event posts, but worries that friends who do not check Facebook may miss the invite. | Matthew's wife, interviewed by Matthew; interview date not provided. |
| E6 | She finds it hard to track what people are bringing and currently relies on memory and scattered text messages. | Matthew's wife, interviewed by Matthew; interview date not provided. |
| E7 | For her own event, she hosted because she started it; for other gatherings, whoever wants to host does. She said it is usually the same person hosting. | Matthew's wife, interviewed by Matthew; interview date not provided. |
| E8 | When plans or contributions change, she may change the food choices. | Matthew's wife, interviewed by Matthew; interview date not provided. |

---

## 2. Scope

**In scope (proposed):** Creating gatherings, assigning a host to a gathering, and recording contributions with the item and responsible member.

**Out of scope (proposed):** Features beyond those listed above, unless we decide to change the scope after reviewing the questions. Event invitations, RSVP/headcount tracking, and automatic hosting rotation are not defined yet.

---

## 3. User Scenarios

**S1: Create a gathering** — Proposed priority: P1
- Actor: Group member
- Trigger: The group decides to hold a gathering.
- Steps: Select a group > enter a title and date/time > create the gathering.
- Success outcome: The gathering appears in the group's gathering list.
- Failure outcome: A required title or date/time is missing > the gathering is not created and the missing information is identified.

**S2: Assign a host** — Proposed priority: P1
- Actor: Group member
- Trigger: The group has decided who will host a gathering.
- Steps: Open the gathering > select a member of the group as host > save the assignment.
- Success outcome: The selected member is shown as the gathering's host.
- Failure outcome: The selected person is not a member of the group > the assignment is not saved and the issue is identified.

**S3: Record and review contributions** — Proposed priority: P1
- Actor: Group member
- Trigger: A member agrees to bring an item to a gathering.
- Steps: Open the gathering > enter the item and responsible member > save > review the contribution list.
- Success outcome: The item appears with the responsible member in the gatherings contribution list.
- Failure outcome: The item or responsible member is missing > the contribution is not saved and the missing information is identified.

---

## 4. Requirements & Acceptance Tests

The requirements and priorities below are draft proposals. The Product Owner and team must confirm them; the interview does not establish that each is a P1 need.

| ID | Requirement | Priority | Acceptance test (do → expect) | Evidence |
|----|-------------|----------|-------------------------------|----------|
| R1 | When a group member creates a gathering with a title and date/time, the system shall save it to the selected group and make it available in that group's gathering list. | Proposed P1 | Create a gathering with a title and date/time > it shows in the selected groups list with those details. Submit without a title or date/time > it is not created and the missing information is identified. | E1, E5 |
| R2 | When a group member assigns a host to a gathering, the system shall associate the selected group member with the gathering as its host. | Proposed P1 | Assign a member of the gatherings group as host > that member is shown as host. Attempt to assign a person outside the group  > the assignment is rejected. | E3, E7 |
| R3 | When a group member records a contribution for a gathering, the system should save the item description and responsible member and make them available in the given gatherings contribution list. | Proposed P1 | Record an item and member > both appear together in the gatherings contribution list. Omit either value > the contribution is not saved and the missing information is identified. | E2, E3, E4, E6 |

---

## 5. Open Questions

| ID | Question | Raised by | Owner | Status |
|----|----------|-----------|-------|--------|
| Q1 | Does HostCircle need to calculate or recommend whose turn is next, or are recording past hosts and assigning each gatherings host sufficient? The Constitution requires fair rotation, while interviewees described informal hosting and one said the same person usually hosts. | Product Owner, after interviews | Product Owner and team | Open |
| Q2 | Should estimated headcount or RSVP tracking be included in the first version? Interviewees described estimating attendance in group chats, and one said some friends miss Facebook event invitations. | Product Owner, after interviews | Product Owner and team | Open |
| Q3 | What qualifies as a contribution, and is quantity needed? | Product Owner, after interview | Product Owner and team | Open |
| Q4 | Should duplicate items be prevented or epressed, or left to the group to rfigure out? What happens when a contributor cancels? | Product Owner, after interview | Product Owner and team | Open |
| Q5 | Who may assign a host or record and edit a contribution for another member? | Product Owner / Architecture Lead | Product Owner and team | Open |
| Q6 | Will the prototype use fixed sample groups and members, or must users create groups and manage membership? | Architecture Lead | Architecture Lead | Open |
| Q7 | Is a four-week pilot doable? How will feedback about fair sharing be collected? | Product Owner | Product Owner and team | Open |
| Q8 | Is the proposed 80% pilot measure appropriate, and what counts as a gathering with a recorded contribution? | Product Owner | Product Owner and team | Open |
| Q9 | Should HostCircle send or display gathering details so members who do not check a social media event or group chat still see them? | Product Owner, after Matthew's interview | Product Owner and team | Open |

---

## 6. Constraints & Assumptions

- The four-week pilot and 80% measure are proposals.
- E1–E4 combine notes from two friends and do not identify which person made each point. E5–E8 summarize a separate interview Matthew conducted with his wife; her interview date was not provided.
- Hosting rotation, headcount tracking, contribution rules, and permissions remain unresolved until the open questions are answered.

---

## 7. Review & Approval

**Week 2 draft review:** Jake (Product Owner) — reviewed the draft requirements and priorities on 2026-11-22. This review does not approve the final Specification; Week 3 prototype evidence and team approval are still required.
