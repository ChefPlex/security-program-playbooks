# Incident Tabletop Exercise Kit

This kit is for running a tabletop exercise that finds the gaps in an incident response plan and
gets them fixed, with evidence an auditor will accept. Many tabletops end with an attendance list and
a slide, and the plan is the same version it was before.

It sits beside the [Cyber Incident Management Playbook](cyber-incident-playbook.md).
The playbook says what the incident commander and the response team do. The exercise tests whether they can do it, with the
people actually on staff, inside the time the plan assumes.

Written from planning and running a combined incident response and disaster recovery tabletop at a
small company that holds regulated data, with an outside facilitator and the chief executive in the
room. The scenario below follows the same shape. The company, the people and the times are invented.

---

## Why Run One, and What It Does Not Prove

1. **To find the gaps in the plan before an incident does.** The plan names a role that nobody holds.
   The backup that is "tested quarterly" was last restored a year ago. The notification decision
   has no owner. Gaps like these usually show up in the first hour of a tabletop.
2. **To meet a requirement to test the plan.** Several frameworks expect periodic testing of
   incident and contingency plans. The HIPAA (Health Insurance Portability and Accountability Act)
   Security Rule, for example, lists testing and revision of contingency plans at
   [45 CFR 164.308(a)(7)(ii)(D)](https://www.law.cornell.edu/cfr/text/45/164.308). An auditor will
   ask for the evidence, so plan the evidence before the session (Section 5).

**A tabletop does not test whether the restore works.** If the plan depends on restoring from backup inside four hours, prove that with a technical restore
test. The tabletop tests whether anyone knows who orders the restore and what they do while it runs.

[NIST SP 800-84](https://csrc.nist.gov/pubs/sp/800/84/final) (September 2006) covers the design,
conduct and evaluation of exercise programs in more depth. This kit is the short version for a team
running its first or second one.

---

## Section 1: Scope and Objectives

### Set the scope in writing

| Decision | Example |
|:---|:---|
| Which plans are under test, with version | Incident response plan v2.1; disaster recovery plan v1.4 |
| Which systems are in the scenario | Employee endpoints, the production database, backups, the customer support tool |
| Which phases of the response | Identification through recovery and notification decision. Not the forensic investigation itself |
| Duration | Two and a half hours, including the hot wash |
| What is out of scope | Live systems. Nothing is touched during a tabletop |

A combined incident response and disaster recovery exercise is worth the extra hour at a small
company. The same five people run both plans, and the handoff between them (when does containment
end and recovery begin, and who decides) is where small teams lose the most time.

### Write three to five objectives, each one testable

An objective is useful if the after-action report can mark it met, partly met or not met without
argument.

| Objective | Plan section it tests | Met if |
|:---|:---|:---|
| Declare an incident and set severity within the time the plan allows | IR plan, identification | Severity set and recorded inside 30 minutes of exercise time |
| Contain a compromised endpoint without destroying evidence | IR plan, containment | The team isolates the device rather than wiping it, and names who does it |
| Decide whether to restore, and from which backup | DR plan, recovery | Restore point chosen with reason; restore owner named |
| Make a documented notification decision | IR plan, notification | Decision, reasoning and decision-maker recorded, with the clock start time |
| Communicate with a customer who asks before the facts are known | Communications | Holding statement exists or is drafted, and one person owns customer contact |

⚠️ **"Raise awareness" is not an objective.** It cannot fail, so it cannot find anything.

---

## Section 2: Roles

| Role | Who | What they do |
|:---|:---|:---|
| **Facilitator** | Outside the team if possible | Runs the clock, reads the injects, asks the follow-up questions, keeps the room honest |
| **Scribe and evidence keeper** | Someone not playing | Records decisions with exercise timestamps, keeps the evidence package |
| **Players** | The people the plan names | Make the decisions as they would in a real incident |
| **Observers** | Optional | Watch and take notes. They do not speak until the hot wash |

**Players, by plan role:**

- Executive decision-maker (often the chief executive at a small company)
- Incident lead
- Technical lead for systems and recovery
- Privacy or compliance lead, who owns the notification decision
- Communications owner for customers and staff
- Legal counsel, on call if not in the room

### Why the facilitator should come from outside

An internal facilitator runs the exercise for a room that includes their own management. Under that
pressure, the hard injects get softened and the awkward questions do not get asked. An outside
facilitator has no reason to protect anyone's plan.

If an outside facilitator is not available, use someone senior from a different function, and agree
before the session that they can ask the executive the same follow-up questions as anyone else.

### At a small company, one person holds several roles

That is normal, and it is one of the things to test. If the incident lead is also the person who
restores the database and the person who decides on notification, the scenario should put all three
demands on them in the same 15 minutes, and check whether the plan names a backup for each.

---

## Section 3: The Scenario

**Ransomware on an employee laptop** is a good first scenario. It is common and starts small, and the
injects can push it into every part of the plan.

**Company (invented):** Ashgrove, about 60 staff, a software-as-a-service product, customer records
that include regulated personal data, production in a public cloud, staff on company laptops.

### Timeline and injects

Exercise time is the time in the room. Scenario time is the time in the story. The facilitator
announces both with each inject, because some decisions only make sense once the room knows that
hours have passed.

| # | Exercise time | Scenario time | Inject (what the facilitator reads) | Question to the room | Plan section |
|:---|:---|:---|:---|:---|:---|
| 1 | 0:00 | Tuesday 08:40 | A support engineer reports a ransom note on her laptop screen. Files on the laptop show a new extension. | Who is told first, and who decides this is an incident? | Identification |
| 2 | 0:15 | 09:10 | The laptop holds a synced folder with last week's export of customer support tickets, which include customer names and account details. | Does this change the severity? Who decides whether data was exposed? | Severity, data classification |
| 3 | 0:30 | 09:45 | A second laptop, belonging to an engineer, shows the same note. That engineer has administrator access to the backup console. | What do you do about the backups right now? Who has the authority to do it? | Containment |
| 4 | 0:50 | 11:30 | The executive receives an email from the attacker: pay, or a sample of customer data will be published in 72 hours. | Who answers this, if anyone? Who is told? Does law enforcement get called? | Escalation, legal |
| 5 | 1:10 | 14:00 | A customer emails support asking whether a rumor on a forum about an Ashgrove breach is true. | What does support say today? Who approves it? | Communications |
| 6 | 1:25 | Wednesday 09:00 | The technical lead reports the last clean backup of the production database is 26 hours old. The plan's recovery point objective is 4 hours. | Restore from 26 hours ago, or keep investigating? Who decides, and what is lost? | Disaster recovery |
| 7 | 1:40 | Thursday 16:00 | Forensics confirms the support export was copied off the first laptop before encryption. | Is this a notifiable breach? When did the clock start? Who signs the decision? | Notification |
| 8 | 1:55 | - | The facilitator stops the scenario. | - | Hot wash |

The injects escalate on purpose. Inject 1 can be handled from memory. Inject 6 needs a decision the
plan probably does not cover, and inject 7 needs a decision the plan must cover.

⚠️ **Write the injects so the room cannot resolve them in one move.** If inject 3 can be answered
with "we have immutable backups, next", either the plan is in good shape or the inject is too
easy. Ask the follow-up: when were they last restored, and who has seen it work?

---

## Section 4: Facilitator Script

### Before the session (one to two weeks)

- Agree the scope, objectives and injects with the executive sponsor. Do not share the injects with
  players.
- Send players the plan versions under test and ask them to read their own sections.
- Confirm the scribe and the evidence package template (Section 5).
- Book two and a half hours. A tabletop squeezed into one hour stops at inject 4, which is before
  the hard decisions.

### Opening (10 minutes)

Read the ground rules out loud:

1. Treat it as real. Answer as you would on the day, not as the plan says you should.
2. Decisions are made out loud, by the person who would make them, and the scribe records them.
3. Saying "I don't know" or "the plan doesn't say" is a useful answer. It is what the exercise is
   for.
4. Nobody is being graded. The plan is what is under test.
5. Side issues go on a parking list and get picked up in the hot wash.

### Per inject (10 to 20 minutes each)

1. Read the inject and the scenario time.
2. Ask the question in the table.
3. Let the room work. Do not answer for them.
4. Ask at least one follow-up:
   - "Who specifically does that?"
   - "Where is that written down?"
   - "What if that person is on a plane?"
   - "How long does that take?"
5. Confirm the decision out loud so the scribe records it accurately, then move on.

If the room stalls for more than five minutes, record "no decision" as a finding, with the reason,
and read the next inject.

### Hot wash (20 to 30 minutes)

Go around the room with three questions, and record the answers word for word:

1. What worked?
2. What did not, or what did we not know?
3. What one change to the plan would have helped most?

Close by confirming who writes the after-action report and when it is due (within five business
days).

---

## Section 5: Evidence Capture for Auditors

**Decide what the evidence package contains before the session.** Reconstructing it afterward from
memory and calendar invitations leaves gaps.

| What an auditor will ask | Artifact | Owner |
|:---|:---|:---|
| Did the exercise happen, and when? | Calendar invitation and the attendance list with each person's role | Scribe |
| What was tested? | Scope and objectives document; plan versions under test, by version number | Facilitator |
| What scenario was used? | The injects as delivered | Facilitator |
| What decisions were made? | Decision log with exercise timestamps and the decision-maker for each | Scribe |
| What did you learn? | After-action report, approved by the executive sponsor | Incident lead |
| What did you change? | Remediation items with owners and dates, and evidence of each one closing | Incident lead |
| Was the plan updated? | Plan revision history showing the new version and what changed | Plan owner |

Keep the package in one place with a date in the folder name. The last two rows are the ones that
usually go missing. An auditor reading an after-action report with six findings will ask what
happened to each of them.

---

## Section 6: After-Action Report Format

Three to five pages is plenty for a two-hour exercise.

```
After-Action Report: [Exercise name]
Date held:                Facilitator:
Plans tested (versions):  Approved by:            Date approved:

1. Summary
   Two or three sentences: what was tested, the overall result, the most important finding.

2. Objectives
   | Objective | Met / Partly met / Not met | Evidence (decision log reference) |

3. What worked
   Specific behaviors or plan sections that held up, each tied to an inject.

4. Findings
   | # | Finding | Inject | Plan section | Severity | Owner | Due date |

5. Plan changes
   What changes in the plan, in which section, and the new version number.

6. Next exercise
   Date, scenario direction, and which findings it will re-test.
```

### Writing findings that get fixed

- **One finding per gap.** "Communications were weak" is three findings and none of them can be
  assigned.
- **Name the plan section.** If the gap is that the plan has no section, say so. That is the finding.
- **Every finding gets an owner and a date.** Without both, it will not get done.
- **Re-test the important ones.** The next exercise should put the room back in front of the top
  findings to show they were fixed.

---

## Where This Breaks

A tabletop shows whether the team knows what to do. It cannot show that the systems will do what
the team expects. The 26-hour backup in inject 6 is only a scenario detail until someone checks the
real backup age and runs a real restore. Pair each tabletop with at least one technical test, and
treat a tabletop that finds nothing as a sign the injects were too easy.

---

*Version 1.0. Last reviewed October 2026. Propose changes via pull request.*

*Part of the [ChefPlex TPM portfolio](https://github.com/ChefPlex/tpm-templates/blob/main/PORTFOLIO-GUIDE.md). Maintained by [Eric White](https://www.linkedin.com/in/edwhite). Licensed under [CC BY 4.0](LICENSE).*
