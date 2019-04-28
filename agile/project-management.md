#Project Management 

## What is agile project management?
Agile project management is an  iterative approach to managing software development projects that focus on continous releases and incorporating customer feedback with every iteration.

### History
Stemming from Toyota's lean manufacturing concept of the 1940s, software development teams have embraced agile methodologies to reduce waste and increase transparency while quickly addressing their customers' ever-changing needs. A stark change from waterfall project management that focus on "big bang" launches, agile help software teams collaborator better and innovate faster than ever before.

Traditional agile project management can be categoried into two frameworks: scrum and kanban. While scrum is focused on fixed-length project iterations, kanban is focused on continous releases. Upon completion, the team immediately moves on to the next.

### How scrum works
Scrum is a framework for agile project management that uses fixed-length iteration of work, called sprints. There are four ceremonies that bring structure to each point.

It all start with the backlog, or body of work that need to be done. In scrum, there are two backlogs: one is the product backlog(owned by the product owner) which is the prioritized list of features, and the other is the spring backlog which is filled by taking issues from the top of the product backlog until the capacity for the next spring is reached. Scrum teams have unique role specific to their stake in the process. Typically there's a scrum master, or champion of the scrum method for the team; the product owner who's the voice of the product; and the scrum team, who are often cross-functional team members in change of getting s@#$ done.

#### The four ceremonies of scrum
|Sprint Planning|Sprint Demo|Daily Standup|Retrospective|
|---------------|-----------|-------------|-------------|
|A team planning meeting that determines that what to complete in the coming sprint|A sharing meeting where the team shows what they've shipped in that spring|Also known as a stand-up, a 15 minute mini-meeting for the software team to sync|A review of whant did and didn't go well with actions to make the next spring better.|

#### The scrum board
A scrum board is used to visiualize all the work in a given sprint. During the spring planning meeting, the team moves items from the product backlog into the sprint backlog. Scrum boards can have multiple steps visible in the workflow, like To Do, In Progress, and Done. Scrum boards are the key component for increasing transparency in agile project managment.

### How Kanban works
Kanban is a framework for agile project management that matched the work to the team capacity. It's focused on getting things done as fast as possible, giving teams the ability to react to change even faster than scrum.

Unlike scrum, kanban has not backlogs(usually). Instead, work sit in the To Do Column. This enables kanban teams to focus on continous releases, which can be done at any time. All work is visible, scoped, and ready to execute on so that when something is completed, the team is immediately moves on to the next. The amount of work is matched to the team capacity through WIP limts, which is a predefined limit of work that can be in a single column at one time(except the To Do Column). The kanban framework includes the following four components:
#### The four component of kanban
|List of work(or stories)|Columns or lanes|Work in progress Limit(WIP)|Continous Releases|
|------------------------|----------------|---------------------------|------------------|
|List of work, or stories are defined as issues or task that need to get done|Used on kanban board to distinguish tasks from different workstreams, users, projects, etc.|A rule to limit the amount of work to be done based on the team capacity|The team works on the amount of stories withing the WIP limit and can release at anytime.|

#### The kanban board
A kanban board is used to visiualize all the work that being done. It's also used for planning resource allowing project managers to see the work and develop timelines accordingly. A kanban board is structured into columns and lanes that stories pass through on their way to completion. Stories sit in the To Do column until the WIP limit allow for the next task to be worked on. The list of work should be split into relatively small issues and organized by priority.

### Estimate, report and plan
Whatever agile framework you choose to support your software development, you'll need a way to see your team's progress so you can plan future work on sprints. Agile project estimating helps both scrum and kanban teams understand their capacity. Agile reports show the team's progress over time. And backlog grooming helps project managers keep the list of work current and redy for the team to tackle.

#### Agile project estimating
Project estimating is an extremenly important aspect of both kanban and scrum project management. For kanban, many teams set their WIP limit for each state based on their previous experience and team size. Scrum teams use project estimating to identify how much work can be done in particular sprint.  Many agile teams adopt unique estimating techniques like planning poker, ideal hours, or story points to determine a numeric value for the task at hand. This gives agile team a point of reference to refer back to during spring retrospectives to see how their team performed.

#### Agile Reporting
Project estimations come into play at the beginning and end of each sprint.  They help teams determine what they can get done at beginning of the sprint, but also show how accurate those initial estimates where at the end. Agile reports, such as Burndown charts, show how many story points are completed during the spring. 

#### Backlog Management and grooming
A product backlog is a pritorized list of worl for the development to do that comes from product roadmap and its requirements. The development team pulls work from the product backlog for each sprint.
Gromming and maintaining your backlog help teams achieve their long-term goals by continually adding and removing items based on the team's long term capacity and changing business objectives. 

## Runing agile program (without losing your mind)
>Some may think transitioning to agile means losing sight of the bigger picture. We couldn't disagree more.

Early adapter of agile development were small, self contained teams working on small, self-contained projects. They proved the agile model can work, to the joy and betterment of software makes around the world. More recently, larger organizations are scaling agile beyond single teams or projects, and seeking ways to apply it to whole programs.
This is not without its challenges. But that doesn't mean it can be done!

### Waterfall versus agile
Traditional project management styles, like waterfall, build in phases. Below is an illustration of a standard waterfall project. This style of product development folds everything into a single, "bing bang", high risk release. Once a project passes one phase, it's painful to revisit it because teams are always pressing forward to the next stage.
![waterfall versus agile](assets/waterfall_release_process.svg)
Traditional project management styles often create "critical paths", where the project can't move forward until a blocking issue is resolved. TO add insult to injury, the end customer can't interact with the product until it's fully complete. Thus, important issues in the product design, and code, go unddiscovered until release.

Let's constrast that with an agile project management style, which takes an iterative approach to development with regular feedback intervals. These iterations allow for the team to be diverted to (and productive in) another area of the project while a blocking issuse is resolved.
![Agile release train](assets/agile_release_train.svg)
##### Besides removing critical paths, iterations let you inyeract with the product during development
This, in turn, gives the team constant opportunities to build, deliver, learn, and adjust. Market changes don't catch you flat-footed and teams are prepared to adapt quickly to new requirements.
An even greater benefit is shared skill sets among the software team. The team's overlapping skill sets add flexibility to the work in all parts of the team's code base. This way, work and time isn't wasted if the project direction changes.

### How to build a great agile program
When a program transistions from traditional project management to agile, the team and the stakeholders must embrace two important concepts:
1. The product owner's focus is to optimize the value of the development team output. The development team relies on the product owner prioritizing the most important work first.
2. The development team can only accept works as it has capacity for it. The product owner doesn't push work to the team or commit them to arbitrary deadlines. The development team pulls work from the program backlog as it can accept new work.

### Roadmaps
A roadmap outlines how a product or solution develop over time. Roadmaps are composed of initiatives, which are large areas of functionality, and include timelines that communicate when a feature will be avialable. As the program develops, it's accepted that roadmap will change - sometimes subtly, sometimes broadly. The goal is to keep the roadmap focused on current market conditions and long term goals.

### Requirements
Each initiative in the roadmap break down into a set of requirements. Agile requirements are lightweight description of required functionality, rather than the 100-page documents associated with traditional projects. They evolve over time and captialize on the team's shared understanding of the customer and the desire product. Agile requirements remain lean while everyone on the team develops a shared understanding via ongoing coverstion and collaboration. Only when implementation is about to begin are they are fleshed out with full details.

### Backlog
THe backlog sets the priorities for the agile program. The team includes all work items in the backlog: new features, bugs, enhancements, technical or architectural tasks, etc. The product owner prioritizes the work on the backlog for the engineering team. The development team then uses the prioritized backlogs as its single source of truth for what work need to be done.

### Agile delivery vehicles
Agile can be implemented using various frameworks(like scrum and kanban) to deliver software. Scrum teams use sprints to guide development, and kanban team often work without fixed work intervals. Both frameworks, however, use largely delivery vechiles like epics and version to structure development for a synchronized release cadennce out to production.

### Agile metrics
Agile team thrive on metrics.Work in progress(WIP) limit keep the team, and the business, focused on delivering the highest priority work. Graph like burndown and control charts help the team predict their delivery cadence, and continous flow diagrams help identify bottlenecks. These metrics and artificts keep everyone focused on the big goals and boost confidence in the team's ability to deliver future work.

### Agile run on trust
An agile program cannot function without a high level of trust amongst team members. It requires candor to have diffcult conversations regarding what's right for the program and the product. Because conversations happen at regular intervals, ideas and concerns are regularly expressed. That means team member also have to be confident in each other's ability(and willingness) to execute on the decisions made during those conversations.

## Wordflow
>Everyone hate "process", but let's face it: without an established workflow, you're going nowhere fast.
