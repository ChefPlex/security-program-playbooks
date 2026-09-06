# Encryption Program Playbook

**How to run an encryption program across a large distributed estate: what to do first, what will
actually block you, and how to end it honestly.**

Written from running encryption modernization at scale - coverage from roughly 10 percent to 80
percent plus across 300+ distributed services, 100+ engineering teams and 10+ cloud environments,
plus a TLS upgrade program that ran twice and key lifecycle work on hardware security modules.

Companion piece: [Lessons from Leading Large-Scale Encryption Programs](https://github.com/ChefPlex/learning-notes/blob/main/lessons-from-large-scale-encryption-programs.md)
in learning-notes. **This is how to run one. That is what nobody tells you.**

These are patterns that generalize. The specifics of any one estate will not.

---

## Step 1: Do the analysis first, and find out how big the problem actually is

**When you're handed an encryption mandate, the first job isn't encrypting anything. It is
establishing the size of what you're up against.** Programs that skip this start remediating the
services they already know about, which are never the ones that matter, and discover the real scope
in month four when the number stops moving.

### The two unknowns, and they are different problems

This is the part that surprises people, and getting it wrong costs a quarter:

1. **Nobody knows what is already encrypted.** Teams believe they're covered, or believe they're
   not, and both are frequently wrong.
2. **Nobody knows what *needs* to be encrypted.** This is a separate question with a separate
   answer, and it's the one people skip because it sounds like it should be obvious.

**Treating these as one question is the first mistake available.** The first is a state problem you
resolve by looking. The second is a requirements problem you resolve by deciding, and it needs
someone with the authority to decide.

### It is a service-by-service walk, and there is no shortcut

You go through the services one at a time and establish what each one actually does, what data it
touches, and whether it is in or out of scope. **There is no tool that produces this for you**,
because the in-or-out determination depends on what the service is *for*, which isn't a property
you can scan.

⚠️ **Anti-pattern: starting remediation before the denominator is stable.** Coverage that climbs
because the denominator shrank is the most common way one of these programs lies to itself, and it
is very hard to unwind once it has been reported upward.

### Expect several programs, not one

At scale this is not a single effort. Encryption in transit and encryption at rest are related but
separately scoped, with different owners, different failure modes and different definitions of
done. **Running them as one program produces a status report nobody can act on.**

## Step 2: Finding the services is easy. Finding who owns them is the work.

**Finding all the services is tedious. Finding all the owners is the thing that stalls the
program.**

The specific failure, and it's structural rather than anyone's fault: **many legacy services are
part of the base system and no longer have a development team around them.** They run, they're
load-bearing, and the group that built them has been reorganized, moved on, or dissolved. Nothing
about the service says this. It looks exactly like the ones with active owners.

So when you need work done on one, **it takes real finagling to work out who can actually do it** -
who has the access, who has the context, and who can be persuaded to take it on when it's not
their job.

### What this means for planning

- **Ownership discovery is a workstream, not a lookup.** Budget for it. It is not a week.
- **Some services will need an owner assigned before any encryption work can start.** That
  assignment is a management action, not a technical one, and it's on the critical path.
- **The orphaned services are disproportionately the old, central, high-blast-radius ones.** The
  hardest to find an owner for and the least safe to touch, which is exactly the wrong correlation.

⚠️ **This is where a plan built on service counts goes wrong.** Two hundred services with active
teams and twenty orphans is not a 220-service program. The twenty will take longer than the two
hundred.

## Step 3: "Encrypted" covers four different problems, and they need four plans

**A service being "encrypted" is at least three separate questions**, and conflating them produces a
coverage number that can't be defended to an auditor and a plan that can't be executed.

| Axis | The question | Typical implementation |
|---|---|---|
| **In transit** | Is traffic between services protected | Often a **sidecar**, so the service itself does not implement TLS |
| **At rest** | Is stored data protected | Storage-layer or application-layer, depending on the datastore |
| **Standard and version** | Is it using a version that is still acceptable | TLS 1.1 vs 1.2 vs 1.3 |

**Run these as separate programs.** They have different owners, different implementation paths,
different failure modes and different definitions of done. A single "encryption program" that
bundles all three produces a status report nobody can act on, because the blockers in one have
nothing to do with the blockers in another.

### 🔑 Coverage work and currency work are different jobs

This is the distinction that most reorganizes the plan, and it's easy to miss because both are
called "encryption."

**Coverage work: get encryption where there's none.** In transit and at rest, implemented on
services that do not have it. This is what moves a coverage percentage. It is finite in principle -
there is a denominator and you can finish it.

**Currency work: upgrade what is already encrypted to a standard that's still accepted.** Older
protocol versions get retired, and you've to move off them to stay inside current regulation and
a defensible security posture. **A service can be fully encrypted and still be a finding**, because
it is encrypted with something that is on its way out.

Consequences worth planning around:

- **Currency work never ends, and this is observed rather than predicted.** The TLS upgrade program
  described here **ran twice.** Today's acceptable version is a future deprecation, so the effort
  that moved an estate to one version runs again for the next one. **Coverage is a project;
  currency is an operating obligation**, and treating the second as a project is why it keeps
  surprising people. ⚠️ **If you're scoping the first one, scope it as the first of several** -
  the tooling, the inventory and the team relationships you build are the durable asset, not the
  version number you landed on.
- **The drivers are different.** Coverage is driven by risk and baseline. Currency is driven by
  external deprecation timelines and regulation, which means **the deadline is set outside your
  organization and isn't negotiable** - unlike almost everything in Step 5.
- **They compete for the same engineers.** The teams who would do coverage work are the teams who
  have to do currency work, and an externally-fixed deadline wins. Coverage slips when a deprecation
  lands, and it slips invisibly unless both are on the same plan.

⚠️ **Still to settle in writing before anyone argues:** who holds the key (a key the provider holds
is a different control from a key you hold, and reporting them as one number is how a program passes
an audit it should have failed), what the version floor is and what happens to things below it, and
what legitimately can't be encrypted and how that's recorded rather than hidden.

### 🔑 Pick the implementation a service team never has to touch

**The sidecar was how a large share of services got done, for the straightforward reason that it
was easier.** In-transit encryption terminated in a sidecar doesn't require changing the service
itself.

That's worth stating as a design principle rather than an implementation detail, because of what
it does to the plan:

- **It decouples coverage from per-service engineering work.** You're no longer waiting on 100+
  teams to each schedule a change.
- **It's therefore the lever against the ownership problem in Step 2.** A service with no
  development team cannot be modified, but it can be wrapped. **The approach that needs no code
  change is the only approach available for the services nobody owns.**
- **It concentrates the work into a capability rather than a campaign.** Building the sidecar path
  well once beats persuading a hundred teams individually, and it's the difference between a
  program that scales and one that grinds.

⚠️ **This asymmetry has a consequence to plan for: it doesn't apply evenly across the axes.**
In-transit has an implementation that routes around missing owners. At-rest generally does not -
it tends to require a change where the data lives. **Expect in-transit coverage to move faster than
at-rest, and expect the ownerless services to be a much harder problem for at-rest**, because there
the missing team cannot be engineered around.

## Step 4: Estimate the work, then find the dependency that actually sets the schedule

Once scope is settled you've to answer two things: **how long will this take, and what depends on
what.** The second one is where the real answer lives.

### The estimate has enormous variance, and that is a finding rather than a failure

Individual pieces of this work range from **a few days to several quarters.** That spread isn't
imprecision in the estimate. It's a real property of the estate, and a plan that reports a single
average has hidden the only number that matters.

**Estimate the tail separately from the body.** A program where most services take a week and a
handful take three quarters isn't well described by its mean, and leadership will make the wrong
staffing decision from an average.

### The dependency that actually sets your timeline is human

Encryption dependencies look technical on a diagram - this service must go before that one, the
certificate authority must exist before anything can use it. **In practice the binding constraint
is people: who is going to do the work, and how much of it can they take on.**

### 🔑 The concentration trap, and it is the thing to design around

Here is how it goes wrong, and it follows directly from the ownership problem in Step 2:

**When nobody owns the legacy services, all of that work lands on one team.** The team that still
has the context, or the access, or simply the willingness. And because the ownerless services are
disproportionately the old central ones, that single team ends up carrying a large share of the
hardest work in the program.

**At that point the schedule is not set by priority. It is set by the throughput of one team**, and
no amount of re-sequencing the backlog changes the completion date. This is the single most
important thing to discover early, because:

- **It's invisible on a service-count plan.** The plan says 300 services across 100 teams, which
  sounds parallel. It is not parallel. A large fraction of it is serialized through one group.
- **Re-prioritizing doesn't help.** Reordering a queue that has one server changes what finishes
  first, never when everything finishes.
- **It compounds with risk.** The concentrated work is the legacy, central, high-blast-radius work,
  so the one team carrying it's also the one that can't afford to move fast.

⚠️ **Model the bottleneck team explicitly in the plan, by name, with its capacity.** A dependency
map that shows service-to-service ordering and hides the fact that forty of them queue behind the
same five engineers is worse than no map, because it looks rigorous.


## Step 5: Negotiate the triple constraint, in both directions

**The ask always arrives the same way: do it faster, for less money, with fewer people. That is
never the case, and it's never going to be the case.** Better, faster, cheaper - pick two. The
iron triangle isn't a project-management cliché here, it's the actual shape of the conversation
you are about to have repeatedly.

So the job is negotiation, and it runs in **two directions that are different conversations.**

### Up the executive chain

What you're negotiating: **time, money, and resources against scope.** What leadership needs from
you isn't reassurance, it's the tradeoff stated plainly - what gets done by when at this funding
level, and what specifically does not.

**This is where the estimate variance from Step 4 earns its keep.** "Most of it takes a week, some
of it takes three quarters, and here is which is which" is a fundable statement. An average is not.

### Down to the engineering teams

What you're negotiating: **who actually does what, and how long it actually takes.** These aren't
the same numbers you were given from above, and reconciling them is the work.

**Teams will tell you the truth about duration if you aren't using it against them.** The estimate
that comes back from the people who have to do it's the only one worth carrying upward.

### The four things that have to come out matched

The negotiation is finished when these agree with each other, and not before:

| Question | Why it binds |
|---|---|
| **Who is going to do what** | Determines whether the plan is parallel or serialized (Step 4) |
| **How long it will take** | The tail, not the average |
| **How much it will cost** | The number that gets traded against scope |
| **How many resources you can actually get hold of** | The one that is usually decided elsewhere and handed to you |

⚠️ **These are frequently negotiated separately and by different people, which is how a program
ends up with a committed date, a funded headcount, and a scope that can't all three be true.**
Getting them into one conversation is most of the value a program manager adds here.

### The negotiation that actually matters is about the bottleneck

Given the concentration trap in Step 4, the highest-leverage thing to negotiate isn't the deadline.
**It's relief for the team that's carrying the ownerless work** - funding, borrowed engineers,
formally reassigned ownership, or an explicit decision to accept their throughput as the program's
rate limit. Everything else is rearranging a queue with one server.

### 🔑 The ownership model that works: central builds the path, teams adopt it

The usual framing offers two options, and **both of them are wrong at this scale:**

- **Central team does the work for everyone.** Does not scale. The central team becomes the
  bottleneck for 300+ services and you've built the concentration trap on purpose.
- **Every team does its own.** Does not converge. A hundred teams each solving the same problem
  produces a hundred slightly different answers, arriving over an unbounded period, and no
  consistent control to report on.

**What actually worked was a third thing: the central team built the sidecar path, and the teams
adopted it.**

That distinction carries most of the program:

- **The central team's deliverable is a capability, not encrypted services.** Build the path once,
  properly, with the hard problems solved in it. That is a bounded piece of work for a small team.
- **Adoption is the teams' work, and it's small work.** You aren't asking a hundred teams to
  design an encryption approach. You're asking them to adopt one that exists and is known to work.
  **That's a completely different request**, and it's the difference between a quarter of
  negotiation and a sprint of adoption.
- **It converts the problem from persuasion to enablement.** Step 5's priority fight gets much
  easier when what you're asking for is small, proven, and someone else has already absorbed the
  risk.
- **And it's the only model that covers the orphans.** Because the sidecar path doesn't require
  changing the service, **the central team can apply it to services with no owner** rather than
  waiting for a team that does not exist. One mechanism, two delivery modes: adopted by teams that
  have one, applied centrally where nobody does.

⚠️ **The failure mode of this model is shipping a path that's not actually easy to adopt.** If
adoption takes a team two weeks of integration work, you haven't built a paved road, you've
built a standard with a document. **Measure adoption cost, not just path completion**, and treat a
slow adoption as a defect in the path rather than a lack of commitment from the team.

## Step 6: Publish the schedule, then run it as an unblocking operation

Now you have a program. Now you have to finish it.

### Publish with the information you have

**Publish a schedule built on the best information currently available.** Not the best information
obtainable - the best you currently have. A program that waits for a complete picture before
committing to a date never commits to a date, and the picture is never complete anyway, because
Step 2 guarantees you are still discovering owners.

### Build in 10 to 20 percent, and say that you did

**Add at least ten to twenty percent on top**, because you're going to hit blocks you can't
currently see. This is not padding and it should not be hidden. **You know with certainty that
unknown blockers exist** - you just spent Step 1 establishing that nobody knows what is encrypted
and nobody knows who owns half of it. A schedule with no allowance for that is not optimistic, it
is unfinished.

⚠️ **Buffer that's concealed gets spent by someone else.** Stated buffer is a negotiating position
you can defend. Hidden buffer is a discovery that costs you credibility the first time someone finds
it.

### 🔑 The standing escalation deal: get blocked, punt it to me

**This is the single most useful mechanism in the execution phase, and it has to be negotiated up
front with every team.**

The agreement: **teams work their piece. If they get blocked for more than a few days, they stop
working it and hand it to the program manager**, whose job is then to go remove the roadblock.

Why it matters more than it sounds:

- **The default failure is silent stalling.** A blocked team doesn't usually escalate. They work
  around it, work on something else, or wait - and the block surfaces weeks later in a status
  meeting, having cost the whole time it sat there.
- **It puts a clock on it.** "Several days" is a threshold anyone can apply without judgment. Making
  the trigger a duration rather than a severity is what makes it actually fire, because nobody has
  to decide whether their problem is important enough to bother you with.
- **It routes the work to the person who can do it.** Most encryption blockers aren't technical.
  They're access, ownership, a dependency on a team that hasn't answered, a decision nobody will
  make. **Engineering teams are badly placed to clear those and a program manager is well placed to.**
- **It converts an invisible risk into a visible queue.** The blocker list becomes the real status
  of the program, and it's a far better instrument than percent complete.

**This is also the mechanism that surfaces the Step 4 concentration trap in practice.** When the
same team's blockers keep arriving, the bottleneck stops being an inference from the plan and starts
being a weekly fact.

⚠️ **The deal only works if the escalation is actually cheap for them.** If punting a blocker gets
a team questioned about why they were blocked, they will stop punting, and the mechanism quietly
dies while appearing to be in place.

## Step 7: Key management is the program that outlives the program

Once hardware security modules are in the picture you've inherited a second program, and it does
not end when coverage does. **Coverage is a project. Key management is an operating obligation**,
the same distinction as currency work in Step 3.

What it actually consists of:

- **Key rotation**, on a schedule, forever
- **Master keys**, and everything that depends on them
- **Key ceremonies** - the formal, witnessed procedures for generating and rotating master material.
  **The job is making sure they actually happen**, which is a scheduling and people problem far more
  than a technical one

### 🔑 The certificate chain goes out of sync, and this is the failure to design for

**A certificate hierarchy has independent expiration dates and independent rotation periods at every
level, and they are almost never designed together.**

The root or master certificate expires on one date. The intermediate certificates below it have a
different expiration and a different rotation period. The leaf or end certificates have another
again. **These get set at different times, often by different people, frequently by whatever the
default was.** Nothing forces them into a coherent schedule.

So they drift out of sync. And when a certificate higher in the chain expires or rotates out from
under the ones below it, **everything underneath breaks at once.** That's the property that makes
this different from an ordinary expiry: the blast radius isn't one service, it's every service
under that point in the hierarchy.

### What "ready" means, concretely

You cannot fully prevent this. Hierarchies are long-lived, people change, and a rotation period set
three years ago by someone who has left is still running. **So the control is readiness, not
prevention:**

> **You have to be able to get in very quickly and recreate certificates.**

That means, before you need it:

- **The access exists and is known to work.** Verified recently, not assumed from documentation.
- **The procedure is rehearsed**, not written down and never run. A runbook nobody has executed is a
  hypothesis.
- **The people who can authorize it are identified and reachable**, including out of hours.
- **The dependency map is current** - you need to know what sits under the certificate you're about
  to replace before you replace it.

⚠️ **The tension worth naming, because it's structural rather than an oversight: the ceremony
formality that makes master key handling secure is exactly what makes emergency re-issuance slow.**
If your key ceremony needs four named people and two weeks of notice, you don't have a rapid path,
and you will discover that on the day you need one. **Design the emergency path deliberately, and
accept that it will have different controls from the routine one** - compensating controls, after
the fact review, more witnesses rather than fewer. What you cannot do is pretend the routine
ceremony is also the emergency procedure.


## Step 8: Rollout, and what actually breaks

**The thing that always breaks is the thing you didn't know you were going to run into.** That's
not a useful prediction on its own, so here is what it looks like in practice, in the three shapes
it reliably takes.

### External events reprioritize your teams away from you

An incident lands somewhere else and the team working your service is gone. **This is not a
scheduling failure and it can't be prevented**, because the incident is more important than your
program and everyone including you would make the same call.

**Plan for it as a certainty rather than a risk.** A schedule with no allowance for teams being
pulled away is a schedule that will be wrong, and the buffer from Step 6 is partly for this.

### A service simply cannot be encrypted, so the work moves

Sometimes the answer is that this service can't be made to work, and the resolution isn't to keep
pushing. **You move the work to a different service and solve it a different way.**

⚠️ **This is a normal outcome, not a defeat, and treating it as a defeat is how programs stall.**
The loop from Step 2 - back to the architects, back to the engineering teams, find another path -
exists precisely for this. What matters is recognizing early that a given path isn't tenable, so
the re-solve happens in week two rather than month four.

### 🔑 People get tired of the program

**This is the failure mode nobody schedules for, and on a multi-quarter program it's the one most
likely to quietly kill your velocity.**

The work isn't glamorous, it's not on anyone's roadmap, and it goes on for a long time. Teams
that were engaged in month one are worn down by month nine. Nothing in a burndown chart shows it.

**Keeping everyone engaged, and keeping the reporting flowing up the chain, is genuinely half the
battle.** Not a soft skill layered on top of the real work. The actual work.

Which comes back to communication, and to one thing in particular that costs nothing:

> **Celebrate the successes of the teams doing the work.**

It keeps a team invigorated because it tells them you're behind them, **and because it makes them
look good to their own management.** That's the part that matters. You are asking teams to spend
their capacity on something that was not their priority. Making sure their leadership sees them
doing it well is the closest thing to a currency you've, and it's renewable.

## Step 9: Measure it with a script, not with a survey

**Once you know how many nodes are in scope, write something that goes out and counts how many
still need to be done.**

This is the highest-return small investment in the program:

- **It replaces self-reported status with observed state.** A hundred teams reporting their own
  progress produces a number that's wrong in a consistent direction, and never the pessimistic one.
- **It makes the number cheap to refresh.** Manual status collection is expensive enough that it
  happens monthly. A script can run continuously, so a regression surfaces in days instead of at the
  next reporting cycle.
- **It's what makes the dashboard honest.** A dashboard built on self-reporting renders what people
  believe. A dashboard built on a count renders what is true.
- **It feeds the executive view from Step 5.** The bird's-eye picture that lets leadership celebrate
  and unblock is only as good as the data under it, and hand-collected data isn't good enough to
  act on.

⚠️ **Guard the denominator.** Coverage that improves because scope shrank is the classic way this
metric lies, and it is not always deliberate. Instrument the denominator and the numerator, and
report movement in both.

## Step 10: Close it at the point of diminishing returns, and say so

**It ends at diminishing returns, not at a wall.** This is the part vendors never write about, and
every large program hits it.

### What actually happens

The program runs up through roughly 80 percent and gets almost everything done. What remains is a
handful of legacy services where **there's no realistic path to completion**, and the honest move
is to **deliberately scope them out** rather than carry them indefinitely.

Then the program ends in the way programs actually end: **the program manager comes off.** Most of
the work is done, the remainder is a slow dribble over the following months, and it doesn't need
dedicated reporting anymore. Someone checks in periodically, helps with the occasional item, and
those items are genuinely past the end of the program.

### This generalizes well beyond encryption

**Get a large program to 80 or 90 percent and the final 10 percent may simply never get done.** The
mature response isn't to grind, it's to **look at the residual again and ask whether it should
still be in scope.** Frequently the answer is no.

**So you reduce scope, and against the revised scope you've completed 90, 95, or 99 percent** -
and it meets the bar for what actually needed to get done to get the program out the door.

⚠️ **This isn't moving the goalposts, provided you do it in the open.** The difference between
legitimate scope reduction and quietly redefining success is entirely whether the decision is made
explicitly, recorded, and agreed by the people who set the original target. **Make it a decision
with a name on it.**

### The signal that a program is over is organizational, not numerical

**Keeping a program manager on a tail that doesn't need one is its own kind of failure.** When the
remaining work no longer justifies coordination, the coordination should stop. Reading that moment
correctly, and saying so rather than defending headcount, is part of running the program well.
