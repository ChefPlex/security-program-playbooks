# Sample Security Incident Escalation

A worked example showing what a Sev1 escalation looks like using the incident response framework. Two versions: the weak version that shows up in real life, and the complete version that actually helps people respond.

---

## The Scenario

3:47 AM. Your intrusion detection system fires. A system administrator pages the on-call engineer. The engineer is trying to figure out what to do next.

---

## Weak Escalation

**Subject:** Possible security issue

We may have had a breach. Looking into it now. Will update when we know more.

### Why This Fails

No severity level. No systems named. No data scope. No containment status. No owner. No next step. No timeline. The people who receive this can't help, can't make decisions, and can't start preparing communications. Everyone waits for the next update, which may not come for hours.

This is the escalation that gets sent when the responder is trying to solve the problem before communicating it. That instinct is understandable. It is also wrong.

---

## Complete Escalation

**Subject:** [Sev1] Active intrusion detected - [System Name] - IRT convening now

**To:** Incident Response Team distribution list  
**Time:** 03:52 AM

Sev1 incident in progress. Calling this at Sev1 because the affected system has adjacency to [data type] and the intrusion appears active. Will revise down if investigation confirms narrower scope.

**What we know:**
Intrusion detection flagged anomalous outbound traffic from [System Name] at 03:41 AM. On-call confirmed unauthorized access is active. System has not been isolated yet - doing that now.

**Data at risk:**
[System Name] stores [data classification - e.g., customer PII / internal credentials / financial data]. Scope of access is not yet confirmed.

**Current status:**
- Intrusion detection: fired at 03:41 AM
- On-call response: engaged at 03:47 AM
- System isolation: in progress
- Log preservation: not yet started - next action
- Severity: Sev1, under review

**Immediate actions underway:**
1. Isolating [System Name] from network now
2. Preserving system image and logs before any modification
3. Blocking identified access paths

**What I need from the team:**
- IRT lead: confirm you're online and taking the investigation lead
- Legal: stand by - PII may be in scope, GDPR clock may apply
- Communications: no external messaging without Legal and Communications sign-off - hold

**Next update:** 05:00 AM or sooner if scope changes significantly

[Name], On-call Engineer

---

### Why This Works

Severity is explicit and the reasoning is stated. The responder isn't claiming certainty - they're calling Sev1 based on what they know and flagging that it will be revised if the facts support it. That is honest and appropriate.

Data scope is named even before it is confirmed. "May be in scope" is more useful than silence.

Containment status is current. The people reading this know the system isn't yet isolated - that tells them the clock is running.

The asks are specific and named by function, not by individual. Legal doesn't need to know what to do - they know their role. The ask is to confirm they are engaged.

The GDPR flag is there immediately. If EU residents are in scope, the 72-hour notification clock starts at confirmed breach. Getting Legal aware at 3:52 AM instead of 9:00 AM is the difference between meeting the deadline and missing it.

The hold on external messaging is explicit. During an active incident, the most common mistake after insufficient containment is premature external communication. The hold protects against that without requiring everyone to remember the policy under pressure.

---

## The Pattern

Every incident escalation should answer five questions immediately:

**1. What is the severity and why?**
Name the level. State the reasoning. Signal that you will revise it as facts emerge.

**2. What happened and when?**
Specific, factual, timestamped. Not "we may have had an issue" - "intrusion detection fired at 03:41 AM."

**3. What data is at risk?**
Even if unconfirmed, name the classification. This determines who needs to be in the room and how fast.

**4. What is the current containment status?**
Is the system isolated? Are logs preserved? Are access paths blocked? The responders who receive this need to know what has and hasn't been done.

**5. What do you need from them, specifically?**
Name the function, name the ask, name the timeline. "Legal: stand by" is an ask. "Team: please advise" is not.

---

## A Note on Timing

The escalation in this example goes out 11 minutes after the intrusion detection fires. That is the right instinct. The temptation is to spend those 11 minutes understanding the incident better before communicating. Resist it.

The response team cannot help until they know. Every minute they spend uninformed is a minute they could have spent preparing legal guidance, drafting communication holds, pulling system documentation, or simply being ready to act. An incomplete escalation sent at 03:52 AM is worth more than a complete one sent at 05:00 AM.

You will have more information at 05:00 AM. Send the update then too.

---

*Part of the [security-program-playbooks](https://github.com/ChefPlex/security-program-playbooks) repo. See the [Security Incident Response Template](../security-incident-response-template.md) for the full framework.*
