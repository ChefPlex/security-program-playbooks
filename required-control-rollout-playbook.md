# Required Control Rollout Playbook

**How to land a security control that every existing customer must adopt or be blocked: who goes
first, what they are told and when, how exceptions work, and how to know it landed.**

The engineering in these programs is usually the small part. A verification check, a policy gate or
a new setting can be built by a very small team in a few weeks. The schedule is set by thousands of
customers who did not ask for the change, the support team that answers them, and the account teams
who will ask for exceptions. Plan the program around those three groups.

Written from driving a required domain verification control onto an existing customer base with a
very small engineering team, where the customer communication was the larger half of the work and
the highest-risk accounts went first. The worked example below is invented, and so are its numbers.

For standing the program up in the first place, start with the
[Security Program Intake and Kickoff Guide](security-program-intake-kickoff.md). This playbook picks
up once the control is approved and the question is how to put it in front of every customer.

---

## The Worked Example

**Fernhollow** is an invented software-as-a-service (SaaS) platform. Its customers send email to
their own contacts from inside the product, and the mail goes out under the customer's own domain.

The abuse: someone signs up for a free or low-cost account, adds a domain that belongs to somebody
else (a bank, a retailer, a hospital), and sends spam or phishing that appears to come from that
organization. Fernhollow's deliverability drops, and the impersonated organization's customers get
phished in its name.

The control: before a domain can be used to send, the account must prove it controls that domain,
usually by publishing a DNS (Domain Name System) record that Fernhollow generates. Email platforms
commonly do this, and building it is straightforward. The problem is that Fernhollow has about
40,000 active accounts sending today, and every one of them now has to do something.

| Segment (illustrative) | Accounts | Share of sending volume | Share of abuse reports |
|:---|:---|:---|:---|
| Free and trial | 28,000 | 9% | 71% |
| Low-cost paid | 9,500 | 23% | 24% |
| Standard and enterprise | 2,500 | 68% | 5% |

That table is the whole argument for the sequence in Step 2.

---

## Step 1: Separate the control from the adoption program

**Two different deliverables share one name, and they have different owners.**

1. **The control** is an engineering deliverable: the check, the setting that turns it on, the
   self-service path a customer uses to comply, and the switch that can turn it off again.
2. **The adoption program** is a customer deliverable: every in-scope account either complies,
   holds an approved exception, or is blocked, on a published date.

The control can be finished while adoption sits at 20 percent. Report them as separate lines, or
the status will read green for months while nothing changes for the abuse it was built to stop.

### Settle these in writing before the first customer hears about it

| Question | Owner | Why it cannot wait |
|:---|:---|:---|
| What threat does the control stop, in one sentence | Security | Every customer message and every exception decision is measured against it |
| What does "blocked" mean for an account that does not comply | Security + Product | Blocking sending is different from blocking login. Customers and support both need to know which |
| Who can approve an exception | Security | Left open, the decision goes to whoever escalates loudest |
| Who owns customer communication | One named person | Several teams will want to send something. One owner keeps the message and the dates consistent |
| What is the enforcement date for each wave | Program lead, approved by the executive sponsor | Customers, support and account teams plan to it, and it is harder to move once it is published |

---

## Step 2: Sequence by risk, starting where the abuse economics are

**Go first where the abuse is concentrated and the cost of disruption is lowest.** In most
platforms those are the same accounts.

Free and trial accounts are where the abuse economics work. An abuser can create them for nothing
and throw them away when they are blocked. They are also the accounts with the least revenue at risk,
the simplest configurations, and the fewest people to coordinate on the customer side. Starting
there removes most of the abuse first, at the lowest support cost per account, and it proves the
self-service path on accounts where a problem is cheap.

Then widen in order of falling risk and rising complexity. The largest accounts go last: they carry
most of the revenue, they have the most domains and the most people involved, and they are where
nearly every exception request will come from.

| Wave | Who | Why this position |
|:---|:---|:---|
| 0 | New sign-ups | Enforce at sign-up from the first day. It stops new abuse at no cost to any existing customer |
| 1 | Free and trial accounts | Most of the abuse, least revenue at risk, simplest to verify |
| 2 | Low-cost paid accounts | Second-highest abuse rate; mostly self-service, some support contact |
| 3 | Standard and enterprise | Most of the volume and revenue, least abuse; named contacts and account teams involved |

⚠️ **Wave 0 is the one most programs forget.** If new accounts can still sign up without the
control, every wave behind it is chasing a pool that keeps refilling.

---

## Step 3: Lay the work out as dependencies

**Each step produces something the next step needs.** Laid out that way, the plan shows what is
blocking what.

This is the Project Management Institute (PMI) process model applied to one program: every output
is the input to the next process, and the schedule comes from those links.

| # | Work | Output | Needed before |
|:---|:---|:---|:---|
| 1 | Threat statement and control specification | One-page control definition, including what "blocked" means | 2, 3 |
| 2 | Segment the base and pull the account list per wave | Wave lists with contacts and current state | 5, 9 |
| 3 | Build the self-service path and the per-account status view | Working verification flow; a page where a customer can see what is left | 6 |
| 4 | Write the exception policy | Criteria, approver, maximum duration | 5 |
| 5 | Write every customer message for the wave, including the blocked notice | Approved message set with dates | 6 |
| 6 | Brief support and account teams; publish support runbook | Trained agents, macros, escalation path | 7 |
| 7 | Send the announcement | Wave notice delivered | 8 |
| 8 | Warning mode, then enforcement on the published date | Wave enforced | 9 |
| 9 | Measure the wave against the go/no-go criteria for the next one | Go or hold decision with a name on it | Next wave |

Items 3, 4 and 5 can run in parallel. Item 6 cannot start until all three are done, because support
cannot be trained on a flow, a policy or a message that does not exist yet. That join is usually the
real critical path, and it is where a small engineering team becomes the constraint.

---

## Step 4: Run the waves on a fixed cadence with a fixed lead time

**Every wave gets the same notice window, and every window ends in an enforcement date.** Customers
in each wave get the same amount of time, the program can stagger waves so support load is spread
out, and slippage in one wave is visible against the others.

The structure is borrowed from a site-by-site infrastructure rollout: each site had the same
fixed block of pre-work ending in a zero-length cutover milestone, and the sites started a week
apart. The same shape works here, with customer segments in place of sites.

### Illustrative Fernhollow timeline

| Wave | Announce | Reminder | Warning mode starts | Final notice | Enforcement |
|:---|:---|:---|:---|:---|:---|
| 0 | Day 0 (release note) | - | - | - | Day 0 |
| 1 | Day 7 | Day 21 | Day 28 | Day 35 | Day 42 |
| 2 | Day 35 | Day 63 | Day 77 | Day 91 | Day 98 |
| 3 | Day 63 | Day 105 | Day 126 | Day 140 | Day 154 |

Wave 3 gets three months of notice where wave 1 gets five weeks. The lead time should match how much
the customer has to do and who has to do it. On larger accounts the person who uses the product is
often not the person who can publish a DNS record, so the request has to travel inside the
customer's own organization before anything happens.

⚠️ **Do not let waves overlap at their enforcement dates.** Enforcement week is when support load
peaks. Two waves enforcing in the same week doubles the peak instead of spreading it.

---

## Step 5: Own the customer communication as a workstream

**The communication is a deliverable with an owner and a schedule.** On a control like this it is
most of the program, and it is the part a small engineering team cannot absorb.

### Every message answers the same five things

1. What is changing, in one sentence.
2. Why, stated as the risk to the customer and their recipients, not as an internal policy.
3. The date, written out, and what happens on it.
4. The one action the customer needs to take, with a link straight to it.
5. How to get help.

### The message sequence per wave

| Message | Channel | Audience |
|:---|:---|:---|
| Announcement | Email to account administrators; in-product banner; documentation page | Every account in the wave |
| Reminder | Email, banner | Accounts not yet compliant |
| Warning mode notice | Email, in-product warning at the point of use | Accounts not yet compliant |
| Final notice | Email, banner | Accounts not yet compliant |
| Enforcement notice | Email, in-product message on the blocked action | Accounts now blocked |
| Restore instructions | Linked from the blocked message | Accounts now blocked |

**Write the enforcement notice and the restore instructions before the announcement goes out.** The
blocked customer is the one most likely to contact support, and the one with the least patience for
a message that explains the policy but not the fix.

### Internal audiences hear first

Support, sales and account teams get every message, the dates and the exception process at least a
week before customers do. An account manager who learns about the change from a customer will
promise something the program cannot honor, usually an exception.

For wave 3, the account team sends or co-signs the message. A large customer expects to hear about a
change like this from the person who manages the relationship. An account manager who signed the
message is also less likely to argue for a delay.

---

## Step 6: Warn before you block, and keep the switch

**Enforcement comes in two stages: soft, then hard.**

- **Warning mode:** the action still works, but the user sees a clear warning at the point of use
  with the date it will stop working and a link to fix it. This is the message most customers act
  on, because it shows up while they are doing the thing that will break.
- **Enforcement:** the action is blocked for unverified domains, with the restore instructions in
  the block message.

It is the same pattern as a soft launch followed by a hard launch on a system cutover: users get a
period where the new rule is live and visible but not yet final.

Keep a per-wave switch that can return a wave to warning mode. If the self-service path breaks on
enforcement day, the program needs a way back that does not take an engineering release. Decide in
advance who can use the switch, and record every use.

---

## Step 7: Exceptions are a list with an end date

**Every program like this gets exception requests, and nearly all of them come from the accounts
with the most revenue.** The incentive is plain: an account manager whose largest customer is
unhappy will ask for an exception, because the exception is the fastest way to make the call end.

If exceptions are cheap to get and never expire, every large account will hold one, and the control
will have a gap exactly as big as the part of the customer base that sends the most mail.

### Exception rules that hold up

| Rule | Why |
|:---|:---|
| Security approves; sales and account teams can request but not approve | Approval sits with the team that owns the risk |
| Every exception has a written reason that names a real blocker | "The customer is busy" does not qualify |
| Maximum duration is fixed in the policy (for example, 30 days) and expiry is automatic | An exception that needs someone to remember to remove it will not be removed |
| One renewal at most, approved one level higher | Repeated renewals are a permanent exemption that nobody decided |
| The list is reported every week with counts, ages and owners | Growth in the list is seen the week it happens |
| Compensating control where one exists (for example, a lower sending limit while the exception is open) | Reduces the risk the exception leaves open |

Legitimate reasons do exist: a customer mid-migration to a new DNS provider, a domain held by a
parent company that needs its own approval process, a regulated customer with a change freeze.
These are the cases the process is for. Write them down as examples in the policy, so the approver
has something to compare a request against.

---

## Step 8: Publish the enforcement dates and hold them

**Moving an enforcement date once teaches every customer that the dates move.** After that, the
customers who would have acted before the deadline wait to see whether it holds, and the next wave
starts with lower compliance than the last.

### Decide the go/no-go criteria before the first wave

Write down in advance what would justify a delay. Good criteria measure whether the program is ready
to support enforcement:

| Criterion | Example threshold (illustrative) |
|:---|:---|
| Self-service verification succeeds on the first attempt | 95% or higher over the prior 7 days |
| Support backlog for this control | Under 2 days to first response |
| Restore path tested end to end on a blocked test account | Passed in the last 7 days |
| Exception queue | No requests waiting more than 3 business days for a decision |

**Low adoption is not on this list, and it should not be.** Low adoption a week before enforcement
is normal. Most customers act on the warning or the block, not the announcement. A program that
delays because adoption is low will delay every wave.

The executive sponsor approves any change to a published date, with the reason recorded.

---

## Step 9: Plan the support load around the dates

**Support volume follows the calendar, not the account count.** Contacts spike after the
announcement, drop, climb again in warning mode, and peak in the days on either side of
enforcement.

### What to have in place before wave 1

- **A support runbook** with the five most likely questions and their answers, written from the
  customer messages so the two never disagree.
- **Saved replies** for each of those questions.
- **A status lookup** so an agent can see an account's verification state without escalating to
  engineering. Without it, every ticket becomes an engineering ticket, and a very small team cannot
  absorb that.
- **An escalation path** to the engineering team for real defects, with a named owner on
  enforcement days.
- **A forecast** per wave. Use the first wave to calibrate the rest. Fernhollow's illustrative
  assumption: 3 percent of notified accounts contact support in waves 1 and 2, 15 percent in wave 3,
  with half of all contacts in the week either side of enforcement.

Self-service is what keeps this affordable. Every improvement to the verification flow between
waves shows up as fewer tickets in the next one, so track the top ticket reasons after each wave and
feed them back to engineering.

---

## Step 10: Measure adoption from system state

**Count compliance from the platform, not from customer replies or account team updates.** The
system knows which accounts are verified.

### The measures that matter

| Measure | What it tells you |
|:---|:---|
| Percent of in-scope accounts compliant, per wave | Adoption progress |
| **Percent of sending volume from verified domains** | Whether the risk is actually reduced. This is the number to report upward |
| Abuse reports per week, before and after each wave | Whether the control is doing its job |
| Blocked attempts after enforcement | How much behavior the control is stopping |
| Open exceptions: count, age, volume they cover | The size of the gap you have chosen to leave |
| Support contacts per wave, top reasons | Where the self-service path needs work |

⚠️ **Guard the denominator.** Dormant accounts that never send will sit at "not compliant" forever
and drag the percentage down. Decide how dormant accounts are handled (enforce at next use, for
example) and report them on their own line. Removing them from the denominator without saying so
makes adoption look better than it is.

---

## Closing the Program

**The program ends when the last wave is enforced and what remains is operational work.** At that
point new sign-ups are covered by wave 0, exceptions run through a standing process, and the
control is part of the product.

At close-out, hand over three things with named owners: the exception process and its weekly
report, the support runbook, and the adoption and abuse measures. It is normal for the program lead to
hand off after the main delivery. A second phase, such as extending the same control to a
new feature or tightening it, is a new program with its own sequence.

### Where this breaks

The sequence works when the riskiest accounts are also the simplest. When the abuse sits in the
largest accounts instead, the order in Step 2 does not hold, and the program has to start with the
hardest wave and the longest lead time. The rest of the playbook still applies, but expect the
schedule to be set by wave 3 from the first day.

---

*Version 1.0. Last reviewed October 2026. Propose changes via pull request.*
