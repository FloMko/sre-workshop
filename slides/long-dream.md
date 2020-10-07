### Site Reliability Engineering How Google Runs Production Systems
Hope is not a strategy

Note:
There is a phrase from aircraft indesutry, filled the whole book with one simple thing

---
### Sysadmins vs SRE
- inderect cost of ops/dev vs 'breaking the wall'
- direct cost of manual interaction vs automative  
same approach to hire SRE as SW

---
#### Pillars of SRE
- Ensuring a Durable Focus on Engineering
- Pursuing Maximum Change Velocity Without Violating a Service’s SLO
- Monitoring
- Emergency Response
- Change Management
- Demand Forecasting and Capacity Planning
- Provisioning
- Efficiency and Performance

---
### Ch2 The Production Environment at Google, from the Viewpoint of an SRE
- Borg (ancestor of k8s)
- Colossus (successor of Google File System)
- Bigtable (sql like)
- Spanner (sql with consistence around the world)
- Blobstore
- Global Software Load Balancer
- - geo dns load balancing
- - user sercvice level
- - RPC load balancing

---
#### part 2 of  List services
- Chubby (distributed lock system)
- Borgmon (same approach as prometheus)
- Protocols
- - Stubby (RPC)
- - protobufs (similar to Apache’s Thrift)
- Shakespear (case to study)

---
### Part II Principles
Note: the principles underlying how SRE teams typically work—the
patterns, behaviors, and areas of concern that influence the general domain of SRE
operations

---
### Ch3 Embracing Risk
Site Reliability Engineering seeks to
balance the risk of unavailability with the goals of rapid innovation and efficient service operations

#### Managing Risk
- The cost of redundant machine/compute resources
- The opportunity cost  

---
#### Measuring Service Risk
- Time-based availability = uptime/(uptime + downtime)
- Aggregate availability = successful requests/total requests  

---
#### Identifying the Risk Tolerance
- What level of availability is required?
- Do different types of failures have different effects on the service?
- How can we use the service cost to help locate a service on the risk continuum?
- What other service metrics are important to take into account?

Note: cost for risk tolerance could be counted, and they a different for Identifying the Risk Tolerance of Consumer Services
and Identifying the Risk Tolerance of Infrastructure Services

---
#### Motivation for Error Budgets
- Software fault tolerance
- Testing
- Push frequency
- Canary duration and size  

---
Our practice is then as follows:
- Product Management defines an SLO, which sets an expectation of how much uptime the service should have per quarter.
- The actual uptime is measured by a neutral third party: our monitoring system.
- The difference between these two numbers is the “budget” of how much “unreliability” is remaining for the quarter.
- As long as the uptime measured is above the SLO—in other words, as long as there is error budget remaining—new releases can be pushed.

---
### Ch4 Service Level Objectives
- An SLI is a service level indicator—a carefully defined quantitative measure of some
aspect of the level of service that is provided.
- An SLO is a service level objective: a target value or range of values for a service level
that is measured by an SLI
- Finally, SLAs are service level agreements: an explicit or implicit contract with your
users that includes consequences of meeting (or missing) the SLOs they contain.  

---
Indicators for diff sercices could be diff too:
- User-facing serving systems: availability, latency, and throughput
- Storage systems: latency, availability, and durability
- Big data systems: throughput and end-to-end latency.

Note:
Don’t pick a target based on current performance
Keep it simple
Avoid absolutes
Have as few SLOs as possible

---
### Ch5 Eliminating Toil
Toil is the kind of work tied to running a production service that tends to be manual, repetitive, automatable, tactical, devoid of enduring value, and that scales linearly as a service grows
- Manual
- Repetitive
- Automatable
- Tactical
- No enduring value
- O(n) with service growth  

---
Our SRE organization has an advertised goal of keeping operational work (i.e., toil)
below 50% of each SRE’s time. At least 50% of each SRE’s time should be spent on
engineering project work that will either reduce future toil or add service features.

---
### Ch6 Monitoring Distributed Systems
- Analyzing long-term trends
- Comparing over time or experiment groups
- Alerting
- Building dashboards
- Conducting ad hoc retrospective analysis (i.e., debugging)  

---
Some tips:
- Symptoms Versus Causes
- Black-Box Versus White-Box
- The Four Golden Signals
- - Latency
- - Traffic
- - Errors
- - Saturation
- Choosing an Appropriate Resolution for Measurements

Note:
as far as i remember there is a two approach:
- USE is an acronym for Utilization, Saturation, and Errors
- RED: Rate, Errors, and Duration (distribution)
resolution - SLO meets and cpu cost

---
### Ch7 The Evolution of Automation at Google

“If we are engineering processes and solutions that are not automatable, we continue having to staff humans to maintain the system. If we have to staff humans to do the work, we are feeding the
machines with the blood, sweat, and tears of human beings. Think
The Matrix with less special effects and more pissed off System
Administrators.” - Joseph Bironas
  
---
- Consistency
- A Platform
- Faster Repairs
- Faster Action
- Time-saving
- Reliability Is the Fundamental Feature

Note:
Soothing the Pain: Applying Automation to Cluster
Turnups

---
### Ch8 Release Engineering
Philosophy:
- Self-Service Model
- High Velocity
- Hermetic Builds
- Enforcement of Policies and Procedures  
Continuous Build and Deployment:

---
### Ch9 Simplicity
- System Stability Versus Agility
- The Virtue of Boring
- - Push back when accidental complexity is introduced
- - Constantly strive to eliminate complexity in systems 
- I Won’t Give Up My Code!
- The “Negative Lines of Code” Metric
- Minimal APIs
- Modularity
- Release Simplicity

---
### Part III Practices
Note: the biggest part of book

---
### Ch10 Practical Alerting from Time-Series Data
There is a deep reference of 'prometheus-like' borgmon system

---
### Ch11 Being On-Call
- primary and a secondary on-call rotation
- Balance in Quantity: 25% time on call
- Balance in Quality: sufficient time to deal with any incidents and follow-up activities (postmortems) - 2 issue per 12h  

---
Approach to deal under stress:
- Intuitive, automatic, and rapid action
- Rational, focused, and deliberate cognitive functions  
on-call resources are:
- Clear escalation paths
- Well-defined incident-management procedures
- A blameless postmortem culture

---
Avoiding Inappropriate Operational Load:
- Operational Overload
- - measure load
- - fix waste alerts
- - give back the pager
- A Treacherous Enemy: Operational Underload
- - ???
- - DiRT (Disaster Recovery Training)
- - “Wheel of Misfortune” exercises

---
### Ch12 Effective Troubleshooting
- Problem Report (expected behavior, the actual behavior, how to reproduce the behavior)
- Triage (make the system work as well as it can under the circumstances (stop bleeding))
- Examine
- Diagnose
- - Simplify and reduce
- - Ask “what,” “where,” and “why”
- - What touched it last
- - Specific diagnoses
- Test and Treat  

---
Negative Results Are Magic
- Negative results should not be ignored or discounted
- Experiments with negative results are conclusive
- Tools and methods can outlive the experiment and inform future work
- Publishing negative results improves our industry’s data-driven culture
- Publish your results

---
### Ch13 Emergency Response
This chapter dedicated to some real exercise for detailed explanation how to deal in case of problem
- Test-Induced Emergency
- Change-Induced Emergency
- Process-Induced Emergency  

and general plan:
- Details
- Response
- Findings:
-  What went well / What we learned

---
Learn from the Past. Don’t Repeat It:
- Keep a History of Outages
- Ask the Big, Even Improbable, Questions: What If...?
- Encourage Proactive Testing

---
### Ch14 Managing Incidents
Google loves komitets!
Elements of Incident Management Process:
- Recursive Separation of Responsibilities
- - Incident Command
- - Operational Work
- - Communication
- - Planning
- A Recognized Command Post
- Live Incident State Document
- Clear, Live Handoff

---
Best Practices for Incident Management:
- Prioritize
- Prepare
- Trust
- Introspect
- Consider alternatives
- Practice
- Change it around

Note:
-  Stop the bleeding, restore service, and preserve the evidence for root causing.
- Develop and document your incident management procedures in advance,
in consultation with incident participants.
- Give full autonomy within the assigned role to all incident participants.
- Pay attention to your emotional state while responding to an incident. If
you start to feel panicky or overwhelmed, solicit more support.
- Periodically consider your options and re-evaluate whether it
still makes sense to continue what you’re doing or whether you should be taking
another tack in incident response
- Use the process routinely so it becomes second nature
- Were you incident commander last time? Take on a different role
this time. Encourage every team member to acquire familiarity with each role.

---
### Ch15 Postmortem Culture: Learning from Failure
- User-visible downtime or degradation beyond a certain threshold
- Data loss of any kind
- On-call engineer intervention (release rollback, rerouting of traffic, etc.)
- A resolution time above some threshold
-  A monitoring failure (which usually implies manual incident discovery)  

---
Collaborate and Share Knowledge
- Real-time collaboration
- An open commenting/annotation system
- Email notifications  

---
Introducing a Postmortem Culture
- Postmortem of the month
- Google+ postmortem group
- Postmortem reading clubs
- Wheel of Misfortune

---
### Ch16 Tracking Outages
- Escalator/Outalator
- Aggregation
- Tagging
- Analysis

---
### Ch17 Testing for Reliability
- Traditional Tests
- - Unit tests
- - Integration tests
- - System tests
- - - Smoke tests
- - - Performance tests
- - - Regression tests
- Production Tests
- - Configuration test
- - Stress test
- - Canary test

---
### Ch18 Software Engineering in SRE
- design and create software with the appropriate considerations
- embedded in the subject matter easily understand the needs and requirements of the tool being developed
- A direct relationship with the intended user—fellow SREs—results in frank and high-signal user feedback

Note: 
case of capasity planner

---
### Ch19 Load Balancing at the Frontend
Load Balancing Using DNS
- Recursive resolution of IP addresses
- Nondeterministic reply paths
- Additional caching complications  

Load Balancing at the Virtual IP Address
IPv4+GRE

---
### Ch20 Load Balancing in the Datacenter
- A Simple Approach to Unhealthy Tasks: Flow Control
- A Robust Approach to Unhealthy Tasks: Lame Duck State
- Limiting the Connections Pool with Subsetting
- - Picking the Right Subset (A Subset Selection Algorithm: Deterministic Subsetting)
- Load Balancing Policies
- - Simple Round Robin
- - Least-Loaded Round Robin
- - Weighted Round Robin

Note:
srr a spread of up to 2x in CPU consumption from the least to the most loaded
• Small subsetting
• Varying query costs
• Machine diversity
• Unpredictable performance factors

---
### Ch21 Handling Overload
- The Pitfalls of “Queries per Second”
- Per-Customer Limits
- Client-Side Throttling
- Criticality
- Utilization Signals
- Handling Overload Errors 
- - per-request retry budget
- - per-client retry budget
Note:
Avoiding overload is a goal of load balancing policies. But no matter how efficient
your load balancing policy, eventually some part of your system will become overloa‐
ded.

---
### Ch22 Addressing Cascading Failures
Preventing Server Overload
- Load test the server’s capacity limits, and test the failure mode for overload
- Serve degraded results
- Instrument the server to reject requests when overloaded
- Instrument higher-level systems to reject requests, rather than overloading servers  


Note: 
there so much reason to start cascading failures
Queue Management, Load Shedding and Graceful Degradation, Retries, Latency and Deadlines

---
- Increase Resources
- Stop Health Check Failures/Deaths
- Restart Servers
- Drop Traffic
- Enter Degraded Modes
- Eliminate Batch Load
- Eliminate Bad Traffic

---
### Ch23 Managing Critical State: Distributed Consensus for Reliability
Replica state machine and Paxos  
ACID (Atomicity, Consistency, Isolation, and Durability) vs BASE (Basically Available, Soft state, Eventual consistency)

Note:
there so much to talk about consensus, leader election, cap-theorem so read by yourself

---
### Ch24 Distributed Periodic Scheduling with Cron
Storing the State

---
### Ch25 Data Processing Pipelines
Data pipelines:
- coroutines
- DTSS communication files
- UNIX pipe 
- ETL pipelines
- “Big Data,” or “datasets that are so large and so complex that traditional data
processing applications are inadequate.”

---
Drawbacks of Periodic Pipelines in Distributed Environments:
- Monitoring Problems in Periodic Pipelines
- “Thundering Herd” Problems
- Moiré Load Pattern  

Note: Workflow as Model-View-Controller Pattern

---
### Ch26 Data Integrity: What You Read Is What You Wrote
Trade-offs between:
- uptime
- latency
- scale
- velocity
- privacy  

Note:
Backups Versus Archives  
Data Integrity Is the Means; Data Availability Is the Goal  
Delivering a Recovery System, Rather Than a Backup System

---
How Google SRE Faces the Challenges of Data Integrity
The 24 Combinations of Data Integrity Failure Modes:
- First Layer: Soft Deletion
- Second Layer: Backups and Their Related Recovery Methods
- Overarching Layer: Replication
- 1T Versus 1E: Not “Just” a Bigger Backup
- Third Layer: Early Detection
- Knowing That Data Recovery Will Work 

Note:
The 24 Game, also called Arithmetic 24 or Math 24, consists
of the challenge of taking four numbers and combining them
with the operations + - x / (add, subtract, multiply, divide)
to reach a result of 24. 

---
### General Principles of SRE as Applied to Data Integrity
- General Principles of SRE as Applied to Data Integrity
- Trust but Verify
- Hope Is Not a Strategy
- Defense in Depth

---
### Ch28 Reliable Product Launches at Scale
committees! google loves committees  
Case of study: NORAD (the North American Aerospace Defense Command) to host a Christmas-themed website that tracked Santa’s progress around the world

---
### Launch Coordination Engineering
- Breadth of experience
- Cross-functional perspective
- Objectivity

---
#### Setting Up a Launch Process
- Lightweight
- Robust
- Thorough
- Scalable
- Adaptable

---
Tactics
- Simplicity
- A high touch approach
- Fast common paths
- The Launch Checklist

---
### Part IV Management

---
### Accelerating SREs to On-Call and Beyond  
|Recommended patterns|Anti-patterns|
|-|-|
|Designing concrete, sequential learning experiences for
students to follow|Deluging students with menial work (e.g., alert/ticket triage) to
train them; “trial by fire”|
|Encouraging reverse engineering, statistical thinking, and
working from fundamental principles|Training strictly through operator procedures, checklists, and
playbooks|
|Celebrating the analysis of failure by suggesting
postmortems for students to read|Treating outages as secrets to be buried in order to avoid blame|
|Creating contained but realistic breakages for students to
fix using real monitoring and tooling|Having the first chance to fix something only occur after a
student is already on-call|
|Role-playing theoretical disasters as a group, to
intermingle a team’s problem-solving approaches|Creating experts on the team whose techniques and knowledge
are compartmentalized|
|Enabling students to shadow their on-call rotation early,
comparing notes with the on-caller|Pushing students into being primary on-call before they achieve
a holistic understanding of their service|
|Pairing students with expert SREs to revise targeted
sections of the on-call training plan|Treating on-call training plans as static and untouchable except
by subject matter experts|
|Carving out nontrivial project work for students to
undertake, allowing them to gain partial ownership in the
stack|Awarding all new project work to the most senior SREs, leaving
junior SREs to pick up the scraps| 
Creating Stellar Reverse Engineers and Improvisational Thinkers: 
- Reverse Engineers: Figuring Out How Things Work
- Statistical and Comparative Thinkers: Stewards of the Scientific
Method Under Pressure
- Improv Artists: When the Unexpected Happens
- Tying This Together: Reverse Engineering a Production Service  
Five Practices for Aspiring On-Callers:
- A Hunger for Failure: Reading and Sharing Postmortems
- Disaster Role Playing
- Break Real Things, Fix Real Things
- Documentation as Apprenticeship
- Shadow On-Call Early and Often

---
### Dealing with Interrupts
did you read 'Time management for system admins'?

Note:
people - imperfect machine

---
### Embedding an SRE to Recover from Operational Overload
Phase 1: Learn the Service and Get Context
- Identify the Largest Sources of Stress
- Identify Kindling  

Phase 2: Sharing Context
- Write a Good Postmortem for the Team
- Sort Fires According to Type  

---

Phase 3: Driving Change
- Start with the Basics
- Get Help Clearing Kindling
- Explain Your Reasoning
- Ask Leading Questions

---
### Communication and Collaboration in SRE
So important, i'll leave this blank

---
### The Evolving SRE Engagement Model
SRE Engagement: What, How, and Why
The SRE Engagement Model:
- System architecture and interservice dependencies
- Instrumentation, metrics, and monitoring
- Emergency response
- Capacity planning
- Change management
- Performance: availability, latency, and efficiency  

The PRR Model

Note:
Product readiness reviuew

---
### Lessons Learned from Other Industries
Preparedness and Disaster Testing
- Relentless Organizational Focus on Safety
- Attention to Detail
- Swing Capacity
- Simulations and Live Drills
- Training and Certification
- Focus on Detailed Requirements Gathering and Design
- Defense in Depth and Breadth  

---
### ME
Maxim Ermolenko
tg - @flomko
presentation https://github.com/FloMko/sre-workshop