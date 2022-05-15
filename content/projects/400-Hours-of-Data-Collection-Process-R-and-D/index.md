---
title: "Reflecting on 400 Hours of Data Collection R&D"
date: 2022-05-11T21:37:12-04:00
tags: ["Olin Social Technology Enterprise with Purpose (STEP)"]
categories: ["Software Development"]
featured: true
draft: false
readmore: true
---

STEP (Social Technology Enterprise with Purpose) was an 400+ hour experimental course that aimed to give students real-world engineering experience within the freedoms afforded by an education-first structure. I worked on the gesture ring, designing a gesture set and building the data collection process for training a machine-learning gesture classification model.

**30-second demo video**:
{{< youtube id=gtr_1zKshnQ title="STEP Gesture Ring Demo" >}}

This reflection includes criticisms and creative proposals that may be interesting for faculty developing iterations of STEP-like courses,  engineering students grappling with the process of designing a process, and my future self before I set on my next AR/VR, accessibility, machine-learning, or large integrated software development project.

<!--more-->

{{< table_of_contents >}}

## Preface

At the point of deciding to take STEP, I had experienced five software internships, four years of coding practice, two full years of project-based Olin education, and one year of in-person engineering experience. Though I came in with a significant level of technical expertise, I was very aware of the gaps in my knowledge, especially ones caused by the intersection of my major (Computing) and the virtual nature of the pandemic. For instance, Olin had emphasized the importance of acquiring user experience design skills, but as I had taken the user design course during the pandemic, I only had experience in conducting virtual user interviews and primarily developed software ideas. Furthermore, in college, nearly all my technical coursework, internship, and project experience was virtual, so my skills for in-person collaborative work were admittedly undeveloped. Thus, in taking STEP, I hoped to fill the gaps in my engineering education.

Although my area of technical expertise is pure software development and algorithms research, I was involved chiefly in building a gesture data collection process during the final technical sprint (April 1st–April 30th, 2022). I had initially volunteered for the task because gesture data collection seemed most similar to the proof-of-concept machine learning and signal processing work I was most familiar with. However, I soon realized that the job of building a data collection system more resembled the task of designing a large, integrated user-interaction system.

Managing the first iteration of STEP’s gesture data collection development was a very educational experience, albeit a very frustrating one. The team, including both my student coworkers and the advising faculty members, has much to be proud of. The gesture ring prototype built in the first iteration of STEP unearthed many insights in both design and execution, and has been useful for investigating the potential of screen-free and wearable technologies.

The effort cannot be called wholly successful, however. Any data collector, data volunteer, or user of the final model is quickly aware of how much better it should be. Inconsistencies and flaws in design and execution hampered the ability to elicit the maximum potential of useful design feedback in both user co-design sessions and final demo events. Furthermore, at the end of the course, the final dataset size was smaller than planned, the simple baseline model was not replaced by a more domain-appropriate algorithm, and points of interest and key research were unexplored due to time constraints.

After presenting the final set of demos to Olin’s Board of Trustees, I began to analyze the STEP experience to see what management and technical lessons were to be learned. In particular, I wanted to compare the problems and insights I had learned from scratch in STEP to the lessons detailed in Frederick P. Brooks’ The Mythical Man-Month (1975), a collection of essays on real-world large, integrated software project management. I had enjoyed reading it in the summer before I entered college, but just two years later I had struggled to remember and map those theoretical takeaways into my own large software design planning processes. In a way, this essay is a belated answer to Olin’s Board of Trustees member Dr. Richard Roca’s probing question on why the benefits of project-based learning are worth the costs and inefficiency.

In this quest, I have profited from long conversations with Dr. Sam Michalka, Dr. Paul Ruvolo, and Dr. Alessandra Ferzoco (Olin STEP advising faculty members), Dr. Steve Matsumoto, (Olin professor of computing with specialty in teaching large software design), and Dr. Erhardt Graeff (Olin professor of computing with specialty in teaching design for civic engagement). I refined my own personal values for improving care and justice in the technology design process after listening to Olin President Gilda Barabino’s inauguration speech (topic of engineering education for all) and Olin student Shreya Chowdhary’s research thesis (topic of femisinist ideals in the technology design process). I have compared conclusions with other STEP students, including Jerry of gesture ring data collection implementation, Adi and Sam of gesture ring hardware implementation, Eamon of whispered speech data collection, Kate of whispered speech project management, and Navi of screen-free interaction market planning.

My own conclusions and perspectives are embodied in the text that follows. Since I believe that criticism should be offered alongside creativity, I have also included a list of proposals for STEP v2. These conclusions are meant for faculty planning on running another iteration of a STEP-like course, engineering students grappling with the process of designing a process, and my future self before I set on my next large integrated software development project.

I strongly agree with The Mythical Man-Month’s stance that “large programming projects suffer management problems different in kind form small ones, due to division of labor”, and proper organization revolves around believing that “the critical needs to be the preservation of the conceptual integrity of the product itself”. The desire to maintain conceptual integrity should be supported by the organizational structure; the design and implementation of mechanical, electrical, and software components; and the process of efficiently eliciting and incorporating implementer and user feedback. Following large system design principles should make it easier to achieve coherent, positive change in the world. 

## STEP v1 – The Good, The Bad, The Ugly

As an organization, STEP encouraged working on technical and user research in several directions simultaneously. For example, in the gesture ring product, the mechanical team researched materials and ways to mitigate end-of-life impacts, the electrical team worked on making a custom circuit board with all the sensors but a smaller footprint, and the signal processing team worked on building a machine learning model for gesture classification. I cannot speak to the full extent of work happening within STEP. My perspective is limited to my work in the gesture ring vertical, within the signal processing horizontal, leading the gesture set development, data collection, and machine learning classification efforts. Yet, examining this single task and all its integration facets provided me with the lens to take a deep look into major challenges for engineering positive change. In the following section, I take a critical and comprehensive look at the resounding successes, iterative design attempts, and room for improvement I saw while leading gesture ring data collection efforts.

### On Tackling Uncertainty

#### Build one to throw away

My first attempt in designing a new, large, integrated data collection system for the real world user was not a polished work, but it was educational. First, I learned how to be okay with designing a first system in the face of uncertainty and trusting in the process to learn something from it. Second, I learned enough to pinpoint the most frustrating blockers of resolving said uncertainty and resolved to plan a better process for next time. Third, I learned about the technical and social aspects of wearable technology, grappled with the problem of precisely communicating with engineering disciplines I had less experience in, and developed more opinions on organizational practices to support the process of iterative engineering design.

When I discussed my experiences with Professor Steve Matsumoto, he told me to look into the concept of conceptual integrity, a design principle introduced in The Mythical Man-Month. This design principle refers to the idea of choosing each decision in an integrated engineering project based on how it improves the end user’s ease-of-use, defined in terms of both simplicity and straightforwardness.

#### Experiencing the breakdown of conceptual integrity

As an example of the discordant design decision failure mode, I offer the story of how we finalized the gesture data collection ring’s two-finger form factor. In retrospect, given the user research we had at the time, we knew users preferred a single-finger ring over a double-finger ring for natural, immersive, freedom of movement – the main pro of a ring-shaped wearable technology.

Yet, for immediate electrical and mechanical integration, the custom board could not fit within a low-profile, single-finger ring. The mechanical subteam offered prototypes of both single- and double-ring form factors in metal skeleton rings that an off-the-shelf mbientlab IMU could be rubber-banded onto. The signal processing subteam, composed of software expertise engineers with very little user research design experience, felt unqualified to make a unilateral decision on deciding which physical form factor should be used for data collection. Thus, the question turned into a gesture ring team-wide debate, and ultimately was decided as an electrical and mechanical integration question: for immediate integration, the double-finger ring was preferred, so the double-finger ring became the forerunner prototype.

The kicker is, signal processing ended up collecting data with the uncomfortable, non-adjustable skeleton rings with off-the-shelf mbientlab IMUs rubber-banded to them. The data collection form factor decision did not depend on the immediacy of custom electrical integration and conflating the two challenges resulted in a less straightforward user experience. Furthermore, not having full authority over the whole data collection design led to more indecision and inaction later on. The signal processing group felt unqualified to collect user feedback and relay needs for improvement pertaining to the physically unadjustable and uncomfortable design of the data collection skeleton rings, and the mechanical subteam also did not take the initiative to improve it as the job was a lower priority compared to the subteam’s internal research task list. 

The uncomfortable and non-adjustable skeleton rings curtailed nuanced user feedback and volunteers’ patience with the data collection setup. Ultimately, the decision and communication failure modes are the result of an unclear process for deciding who has say and responsibility for deciding and improving user look and feel design.

### Designing Management

#### We tried to explore alternative organization structures!… And we ran into problems!

For the first two months of STEP, we explored alternative ways of self-organization: everyone in STEP had an equal share in decision power and this promise stayed consistent throughout the entire three month experience. Starting in a flat structure, then evolved into an exploratory subteam structure in order to investigate the “what” we wanted to build together. Here, each subteam carried out user research or technical feasibility research. Even when organized in subteams, each member was still promised to have an equal share in the decision-making process.

This system had a few drawbacks: in encouraging members to switch subteams in every sprint, time and information was lost in onboarding; many of the insights reached were too broad to be actionable; and the lack of progress toward a goal made many people — myself included — feel frustrated and lost. To combat the time and information loss, I supported the idea of making a Meta-STEP task force. Many people liked the idea so it passed: I was uninterested in overhead management work, so I did not self-select into the Meta-STEP subteam. Unfortunately, at this point, I developed a fear that the final “what” our representatives in MetaSTEP would collectively choose to build would not reflect my specific areas of interest and expertise (machine learning and data analysis) because only 2-4 members of the 30+ members of STEP had that same priority of a learning goal and no members of the MetaSTEP subteam had that as a learning goal.

In an effort to be as clear about my priorities as possible, I started attending as many after-class discussions as possible and ended up radicalizing myself into a “I’m fine as long as it has machine learning, it must have machine learning” type of political party. I wanted to paint myself as someone who would not budge from this point of work, and I believe that adopting this position and value system contributed to the environmental and educational silo-ing effect I ran into later on in the STEP process. 

After Spring Break, the STEP’s structure had reformed into a belated hierarchy of subteams assigned to products assigned to professors, in response to student cries for real structure during the real technical sprint. STEP’s final organization structure was dedicated to the development of three “vertical” app control input devices: an ergonomic pocket-sized remote controller, a ring that controlled phone behavior though gesture classification, and a throat mic that controlled phone behavior though whispered speech classification. Each “vertical” was split into several engineering skill “horizonals”: mechanical, electrical, signal processing, and all the vertical products interfaced with the same unified audio-first software iPhone app.

Beyond the difficult problem of choosing “what” to build, giving everyone equal decision power also exacerbated confusion and reluctance for making decisions about “how” to build. Frequently, I or others working with me would stage democratic votes to feel okay about passing a tentative decision, and later ask others to defend or document a decision made on tentative reasoning. Nobody knew who had final say on a design, so we spent more time and mental energy trying to reach decisions by consensus rather than trying to find a process to support individually-motivated innovation and experimentation. Making decisions by committee frequently buried the details needed for coherent decision-making, resulting in late or discordant design decisions and preemptive dismissal of useful ideas, as in the case of deciding the form-factor for the physical body of the gesture ring.

The large communication overhead also made it difficult to ask basic questions such as what is going on in other groups, what is the priority task list, who needs help, where can I help, and who is available to train me. Frequently, people complained about feeling “silo-ed” in their own vertical or horizontal. STEP briefly tried to encourage an apprenticeship program to enable students to teach and learn from students with other specialties, but onboarding people without requisite knowledge or clear division of labor is difficult. The program was dropped as the technical sprint progressed and deadlines had to be met. A few times, I asked for more help in carrying out data collection, but everyone was busy with their own deadlines and there was no pressure to help with mine, so nobody volunteered their time to learn about the data collection process, and I had no authority to demand it. I also did not make a significant push for it because I was already struggling to distribute work within the people I had working with me.

The lack of decision ownership and follow-though had a very real negative effect on informing subsequent attempts for data-driven decision-making. When given a functional prototype, users have a tendency to focus feedback on obvious problems or breaks in cohesion, even if told to ignore those aspects. Not prioritizing prompt design improvements limited future co-design and the ability to glean specific, nuanced feedback.

#### I tried to work on a team like I usually do, and I ran into the same problems I usually run into… but now I know why they are happening?

Every time I have worked on an Olin class project with multiple software developers, I have run into a terrible conflict of interest: obeying a class’s call to maintain an equal division and freedom of labor while also being pushed into a supervising code design position by necessity of technical experience. I usually divide the project into subprojects with well-defined interfaces and spend a significant amount of time being frustrated by the design decisions and missed requirements on the subproject I do not design. This game is never fair on the other workers because requirements always shift as designs mature, yet I am sulky and frustrated because, logically, there should be a way for two competent people with the same skillset to work together without doing a worse job than one competent person working alone.

In the past, I’ve always blamed the mishap on my poor communication skills, and subsequently tried very hard to build better communication skills. However, this time, my partners said that my communication skills were quite good and almost definitely not the cause of frustration. Now I believe that the real culprit is the way I plan to divide design and implementation labor. Here is how it went:

Initially, the two members of the ring signal processing subteam planned to divide labor across tasks and languages: I did data analysis and machine learning work in Python, my coworker did data collection app implementation in Swift (a language neither of us knew). This made sense for our learning goals — I didn’t want to learn Swift, he did, and we were encouraged to build upon an existing Swift prototype for data collection. Yet, this was an extremely inefficient division of labor because the design for the data analysis work and the data collection app work were deeply intertwined – decisions for making the data collection app easier to use were made after analyzing the data and vice versa. 

I ended up having to plan the design across both tasks. This was fine, but I worried about overstepping on my coworker’s “fun” creative freedom. I resolved to not overly criticize design decisions that he made and implemented without consulting me as long as they didn’t interfere with my explicit requirements too much. This was probably a bad call, as every feature matters when designing a data collection user experience to take a limited amount of user time and exist entirely on the small screen of an iPhone. As a result, the app implementation process was rife with rework and unused features rather than productive experiments in design or implementation.

Furthermore, the split of design information and code ownership meant that there was no efficient way to redistribute work if one of us got busy or sick for an extended period of time (which happened several times during the technical sprint), and there was no efficient way to do product testing because that process could only progress when both of us were present. This resulted in major implementation issues (like essential features being “cleaned away”) being discovered during time allotted for user experience research. These urgent bug fixes not only made work-life balance sporadic and unpredictable, but it also wasted data collecting volunteers’ time and reflected badly on the perceived care going into the user co-design / data collection work. Accordingly, users were less patient and curious about engaging with the buggy technology, which demotivated team morale. 

Overall, the process felt tedious to work in and the pains overshadowed the joys of programming and continuous learning.

#### STEP Faculty: a decently good model for superior-subordinate communications**

The teaching time occupied an interesting role in the STEP hierarchy. By studying their interactions with the students, I have a better idea of how effective bosses interact with their subordinates. I interacted the most with professors Sam and Paul, and found that they had a fantastic way of confirming whether updates were status updates or requests for action. Personally, I have found that their lack of answers unless specifically asked for inspired a good deal of self-growth and a sense of agency within myself, and I will strive to maintain that sort of relationship with my mentees and subordinates going forward. 

My main positive takeaway is that bosses must communicate a master plan and be aware of how their subordinates are managing their own local level plans. Awareness comes easily in a culture rooted in regular and honest updates. Building a culture of regular and honest updates requires a level of restraint: (1) do not interfere if everything is going to plan, and (2) if a problem comes up, wait to see if the subordinates can solve it without you.  This approach ensures that subordinates feel like they have creative freedom.

My experiences within STEP also showed me how a lack of clear answers can brew turmoil and anxiety within a group if the plan is not clearly communicated. Information and respect must go up and down the communication ladder.

### (Bad) Documentation

#### Ambiguously Useless Roadmap

The teaching team put out a general roadmap with suggested deadlines and goals. This was a good start, but the stand-up and general team meetings did not focus on understanding how to follow the roadmap to make decisions about integration across teams; there wasn’t a clear immediate distinction between minimum viable product, attainable product, and reach goals; and the time until milestones always seemed slightly unclear and were never emphasized until the week of. The verticals were supposed to set internal deadlines but, at least in the gesture ring vertical, making decisions by committee led to underdeveloped, late, and fatigued decision-making.

#### Status Updates Without Long-Term Planning

Status updates were given at the biweekly standup meetings. Each subteam reported progress on self-chosen implementation and research efforts, which always sounded good, but we did not compare them to estimates for milestones. The most vocal and present members ran the agenda for discussions. This typically happened to be the electrical and mechanical subteams and decisions related to their integration problem. Personally, I felt like there was no clear room or urgency to make plans for data collection integration needs and testing. I did not have a clear comprehensive plan, was never questioned about my lack of clarity, and did not effectively advocate for my integration requirements.

#### Documentation Trash Fire

Regular progress notes were documented in the Notion space, but these were not edited to be concise or precise. Thus, reading the generated documentation was not worth the effort – it was far more effective to bug the person leading a given task (this was ironic, as managing communication overhead tended to eat a substantial portion of work time).

#### Black Hole of Problems & Proposals

Problems and proposals were frequently floated at off-hour discussion circles. These problems and proposals were ill-documented, missed by anyone not at the meeting, and not time-boxed because nobody had authority to make a decision alone – presumably, decisions had to be made by majority vote. Most problems and proposals never seemed to be acted upon. Since everyone had equal authority, much management and proposal work was redone or thrown out with no clear explanation, leading to discontentment.

### Mechanical Integration

#### Neglect in requesting comfortable AND adjustable ring + IMU holder

I depended on the in-house mechanical subteam to fabricate this but I struggled to communicate clear design requirements or priority level and did not manage to develop a design and realization timeline. We were still using uncomfortable and un-adjustable skeleton rings as of the second-to-last demo day, and using one-size delicate, untested 3D-printed rings on the final demo day. When given an uncomfortable and unadaptable skeleton ring prototype and prompted to give feedback on comfort and ease-of-use, many users focused on the obvious size discomfort squeeze, looseness, or rough feel of the ring (which we knew were problematic) rather than on the more nuanced discomfort of the short-term / long-term comfort of gestures. Wobbly and tight rings also forced users to do gestures in strange ways due to the fear of the ring falling off.

### Electrical Integration

#### Acquire dependable electrical board to test IMU sensor data stream

We made an intelligent decision to work with off-the-shelf mbientlab IMUs while the electrical team designed and debugged the custom board. This greatly simplified software and algorithm development as we could be certain that any bugs were due to the code and not due to buggy hardware.

### Software Data Collection App

#### Iterative attempts at designing easy-to-use AND easy-to-modify data collection process

I depended on a coworker, whose learning objective was learning Swift, to design and implement the iOS data collection app. The communication process was workable, but inefficient. We used the following data flow to save a series of gesture data with its corresponding series of gesture prompts: IMU sensor data stream → series of prompts in randomized order + continuous recording of time series data on Swift App running on iPhone → .json files.

Going through the filter of the Swift app’s data processing, the sensor data and metadata structures output by the app were poorly better documented (i.e. given an array of 4 quaternions and 3 acceleration at each time point, are the quaternions ordered x, y, z, w or w, x, y, z – these are both conventional orderings so which one did we use? What about the order of the x, y, and z acceleration? – these answers are hard to answer without knowing the operations and precision level of the Swift code.)

The data collection app went through three major iterations: v1 → v2 incorporated an automatic progression of gesture prompts and removed the user’s need to tap to delineate the start and end of gestures, thus reducing user error and fatigue. The change from v2 → v3 removed the STUDY phase from the prompt system as prompting the user to STUDY then MAKE then STOP led to too many false positive starts in the STUDY phase.

#### Iterative attempts to ask about user experience

I added user experience questions to my data collection methodology after the user experience subteam was disbanded, but it was an afterthought and not as scalable or consistent as would be useful. I tended to only note and work on improvements in the parts of the product that I owned – the gesture set and the machine learning considerations. We tried to ask questions about preference for turn-based activation gestures (how to start/pause classification), and recorded some basic wearability information (ring worn on which fingers, ring worn on left vs. right hand, whether hand is oriented normally or at user’s side) per trial. We did not brainstorm and examine other demographic-specific gesture, app, or ring design preferences.

#### Iterative attempts to capture user metadata

User opinions and trial metadata was tracked in three ways – a Google spreadsheet filled out by the collector, questions in the iPhone App, and a reflective Google forms survey filled out by the user after data collection. We needed a unique key to match the spreadsheet metadata with each trial of data collection. In the name of consistency, we chose Olin usernames as the key. This was a bad choice for several reasons, ranging from the loss of anonymity and privacy, to the inconvenience of some Olin usernames being very long and hard to spell, prompting user intervention and frustration. The data collection user experience left much to be desired. Typing a test session key on the iPhone App touchscreen while also answering questions from the collector while also getting in position to collect data while also trying to remember the gesture set was annoying at best or overwhelming and tedious at worst. Patience usually ran out before users got to the point of filling out the reflective Google forms survey.

### Software Signal Processing Logic and Model Training

#### Iterative attempts to verify the general robustness and usability of a gesture set

A simple baseline model consisting of sklearn's standard scalar, Principal Component Analysis (PCA) with the maximum number of components, and a Support Vector Classifier (SVC) was implemented. The baseline model was intentionally kept very simple in order to serve as a comparison baseline and use initial prototyping time to build out the data collection and software integration infrastructure. The baseline model was trained and evaluated on different combinations of gesture sets in order to find the gesture set where it could mostly easily differentiate between different gesture classifications based solely on 3-axis IMU acceleration patterns. The model-determined robustly differentiable gesture set was:

* swipe right along the x-axis
* counterclockwise circle in xy-plane
* poke movement along the z-axis
* tap movement along the y-axis

The baseline model confirmed the hypothesis that the most differentiable gestures had signal peaks on different axes. The odd gesture, drawing a circle, had signal peaks on multiple axes.

In data collection and user testing, the poke gesture was voted as uncomfortable and inconsistently performed by a significant portion of users because it moved in a different axis than the other three gestures. To make the gesture more intuitive, it was renamed as “flick” and performed along the y-axis, opposite in direction to the “tap” gesture.

#### Neglect to improve the accuracy and robustness of the model

The baseline model (~75% accuracy) had two obvious failure modes: (1) it confused the “tap” (formerly “poke”) gesture with the “downward swipe/tap” gesture, and (2) it was overly sensitive to translations or warps in acceleration patterns over time (i.e. doing a gesture too fast or slow) due to both PCA and SVM treating different time points as entirely different features. Implementation of different models was not pursued as there was not enough data or enough time to collect enough data given the current data collection system.

### Unified Software Demo

#### Successful integration of machine learning model into the unified software demo

The software subteam depended on us, the signal processing subteam, to provide the gesture classification model and then worked with us to integrate classification into the app. In this case, the software team did an exemplary job defining the desired user experience for the unified demo app, so the signal processing team just had to solve the defined integration implementation challenge. We used the following data flow to work model predictions into the App: IMU sensor data stream → windowing logic to isolate gesture time series on Swift App running on iPhone → send HTTP request with IMU data to Flask server → model prediction → send HTTP response with predicted gesture name to Swift App → control action in demo app. The biggest user-perceived downside was that passing data though so many mediums made the process laggy.

To implement the windowing logic, I told the software team my design requirements (i.e. send a signal 25 milliseconds before and 75 milliseconds after crossing a signal threshold) and suggested a circular buffer data structure for implementation. The software team understood my requirement and wrote an implementation using a Swift-specific sliding window. Integration worked on the first try! This was a great experience in seeing the success of setting clear user experience and interface requirements while letting implementers have full implementation problem-solving design freedom.

#### Iterative attempt to design and implement easy-to-use demo app experience

Software team designed an audio-first app that could be controlled by commands rather than screen swipes. They first worked with blind and visually impaired users to identify common use-cases. The design was a bit too noisy – an inappropriate amount and style of audio feedback made navigation and controls difficult to discover for someone who could not see the screen – and it overlapped heavily with the functionality within iOS’s existing accessibility mode for the blind. Overall, the demo was a useful product and a great first step for eliciting reactions and feedback from potential users of screen-free technology. Early on, the unified software demo app decided to expose 4 control actions UP, DOWN, LEFT, and RIGHT actions to navigate a 2D screen. Mapping between the gesture set and the action set used by the app was contrived.

* RIGHT — swipe right along the x-axis
* LEFT — counterclockwise circle in xy-plane
* UP — flick / poke movement along the z- or y-axis (this gesture was redesigned to be more comfortable for users)
* DOWN — tap movement along the y-axis

Mapping model-chosen gestures back onto predefined actions felt strange and unintuitive for several users, especially since opposite actions (RIGHT vs. LEFT) used completely different gestures (swipe vs. circle). Changing the app’s control actions would have been difficult because the same interface was used by the whispered speech vertical.

#### Neglect to create a way to teach gestures in blind / visually impaired-aware manner

In the second to last demo day, the gesture set was presented to a user with visual impairments. Up until that point, we had been teaching gestures only to seeing users. Without the visual aids, the user could not follow instructions – “move your hand counter clockwise as if you are tracing around a clock on the wall in front of you” is an especially unhelpful way to convey a gesture to someone who has never visually read a clock.

## Proposal For Building A Better Second System

These proposals are meant to capture my thoughts on improving upon every criticism conveyed in the previous text. I hope these insights are useful to others and my future self as we develop more accessibility, AR/VR, or machine-learning powered large integrated software projects.

### Designing Management

#### Are hierarchies the end all be all of organizing large groups of people? No, there’s more to it.

Over the last month, I have been doing qualitative research on strategies for managing organizational structures. In addition to reading The Mythical Man-Month, I also attended talks and discussions around Olin on reworking organizations to support more diverse and nuanced outlooks on problem-solving. This included everything from building support to ensure historically marginalized voices have the ability to contribute to decision making, to figuring out my own responsibility in choosing design values. I now understand why hierarchies of responsibility and decision-making are the most popular solution to building a cohesive product and product design process. Yet, I have also learned that hierarchies alone are not a full solution. I demand more from the management structures in which I choose to exist. Going forward, I have decided on a set of four promises I need from an organization before I can dedicate my time, work, and creative energy to it. These freedoms are directly tied to four joys that motivate me to continue to work as an engineer. 

#### Why I choose to be an engineer

* First is the joy of deciding how to make something and satisfaction of following through.
* Second is the joy of building something that will help improve the ease of people’s lives and have a net positive impact on the world
* Third is the joy of flexing technical competency by wrestling with a complex puzzle.
* Fourth is the joy of always learning new insights, the surprise of understanding a clever shortcut, and the awe of appreciating the complexities of reality.

#### Qualities I will check for in an organization before joining

First is the organization’s dedication to support both my ability to hustle and my desire for downtime. Here, “hustle” means my ability and desire to sit down and accomplish large amounts of implementation work in one continuous stretch. The organization should promise to provide workers with enough information to support quick decision making and enable sessions of high progress without interruptions and dependency hell. In particular, the key freedom promised to workers is the authority and support to decide a personal, granular-level, satisfying path towards completion of a task. This freedom enables workers to maintain work-life balance, allot time for communication and reflective work, meet and set reasonable deadlines, and minimize burnout.

Second is the organization’s dedication to enforcing conceptual integrity within its products. Here, “conceptual integrity” means a commitment to designing a “what” (product or user experience) where all feature and integration decisions are focused on improving the ease of use within user’s lives and have a cohesive positive impact on the world. The organization should promise to select and enable self-selection of workers and supporters who agree with the organization’s values in terms of “what” specific positive change to pursue, and then promise to manage the assignment of priority critical work items to realize the “how” of that positive change. People will feel satisfaction when they perceive progress happening in accordance with their values. The key deliverable to have here is a detailed, specific, and cohesive idea of what experience the positive change will enable for the end user (see, feel, do). This will definitely evolve more nuance as more user and implementation uncertainty is resolved, but promising to maintain the essence of it throughout the process is necessary for ensuring that workers and users do not feel hopelessly disappointed or betrayed by the directions of the system.

Third is the organization's dedication to support creative freedom for each member. The organization must evolve its concept of work beyond assignments, critical dependencies, and treating people like cogs within a machine. All the people in an organization have chosen to support the values and ideas of the organization when they first joined. To support the spirit and values of workers, the organization must promise to support organizationally-useful and personally-fulfilling paths that allow workers to test the assumptions of decided ideas and investigate uncertainty outside the critical work path. The organization can do this by guaranteeing workers the support, freedom, and authority to choose areas of uncertainty to focus on, discuss and choose design ideas, and iteratively experiment with multiple alternative designs.

Fourth is the organization’s dedication towards maintaining and improving internal infrastructure for concise and purposeful communication. The organization must be dynamic and flexible in understanding plan status and accommodating changes. In order to do this efficiently, the organization must provide structured avenues to convey information. This includes enforced guidelines for using concise and precise formats that help readers find the information they need without feeling inconvenienced or feeling like they are inconveniencing others. Crafting concise and precise documentation can also serve as a useful synthesis experience for the maker. There is a strong need for building a culture of care, trust, and justice within the organization. One solution is making sure there are visible and traceable avenues of raising problems, proposing solutions, and planning to review design plans before implementation and before finalization, so missing information can be identified and considered proactively and the organization can react in prompt yet respectful ways. For the managers of an organization, following the evolution of documentation and feedback can also serve as a motivational and benchmarking tool for evaluating the realization of conceptual integrity that the organization wants to create in the world.

#### Proposal for a better division of labor within a team

Taking into account the joys of hustle, conceptual integrity, creative freedom, and efficient communication, I have settled on a better method of organizing division of labor between one or more software developers working on a single integrated program. Borrowing heavily from the ideas of The Mythical Man-Month, I believe in the idea of a “surgical team”. In a surgery, only one person does the cutting; the remaining members perform essential support roles. To work with the sequential and integrated nature of software design, only one person should work on the design, implementation, and debugging of a program. The other members should work on supporting that work through participating in discussion on (but not deciding) design decisions, researching unexplored aspects of the design, proposing and implementing alternative techniques, and serving as insurance to the critical path of the work if anything happens to the surgeon. In this way, the tasks on the critical work path are completed in a timely manner, conceptual integrity is not distorted though decisions by committee, creative freedom is ensured for all members of the team, the division of labor communicates information helpfully and efficiently.

When tackling a large project that spans several teams and disciplines, coordination should happen between the few heads dedicated to designing the overall user experience and critical work implementation for each component. In this way, the conceptual integrity stays intact and communication networks remain sparse, enabling quick work. The overall decision plan should be visible and communicated to all members, who can then creatively support the main effort by researching unaddressed areas of uncertainty and investigating solutions.

### (Purposeful, Concise, and Precise) Documentation

#### Proposal for Documentation Levels

I believe that documentation should be written with the intent of conveying expected information. What expected information is depends on why the reader is reading it. Documentation is easy to use if it is clearly organized by intent; by scanning the format and contents, readers should be able to quickly surmise whether the information they are looking for is present or in some other document. A few categories of readers with intent within STEP are:

* STEP faculty trying to evaluate STEP as a learning experience
* STEP students trying to establish a list of coherent features and plan a critical work roadmap
* STEP students trying to take an idea, implement it, and iterate on the process
* Users trying to use a STEP product
* Users trying to trust a STEP product
* Users trying to adapt a STEP product

#### Proposal for Documentation Types

I believe that each person should be given a local journal or space to collect their own notes on experiments and progress, and only contribute their notes to the global product documentation when their notes are reviewed, concise, and precise.

***Global “Look Here Before You Decide What To Work on Today” Documents***

* **User Specification Manual** - maintained by user experience design group. Should detail every part of the user experience. Will be consulted by implementers, but should not dictate implementation decisions.
* **Critical Work Path** - maintained by bosses and product managers. This document should outline what tasks are a priority because of dependency chains and time estimates. If a task is in the critical work path, the priority of people skills and resources should be assigned to completing it, and regular milestones should be communicated
* **Changes** - quick daily updates on what has been changed in the User Specification Manual and Critical Work Path

***Global “Nice To Look Here If You Want To Quickly Catch Up On What Has Happened” Documents***

* **Formal Problems** — fully written up so that they can be presented even if someone can’t make it / stay for the whole meeting. Meant to encourage productive brainstorming of solution ideas at the meeting.
* **Formal Proposals** — fully written up so that they can be presented even if someone can’t make it / stay for the whole meeting. Should have some amount of technical proof of concept, experimentation, implementer approval, or user feedback supporting the proposal. Meant to be given to the user experience design team to be evaluated on whether it fits into the conceptual integrity of improving user ease-of-use. If it fits into the conceptual integrity framework, a user experience design member will add it to the User Specification Manual, making sure it coexists with all the other decisions in the User Specification Manual.
* **Design team meeting minutes** — written agenda of problems and proposals, ideas discussed, decisions made, tasks assigned
Implementation team meeting minutes — written agenda of problems and proposals, ideas discussed, decisions made, tasks assigned
Notion / Trello task cards and stand up minutes — this should be regularly compared to the deadlines on the Critical Work Path

***Local “Work Has Been Done Here Journals” Documentation for each Person for each Product***

* **(Code) Product Journal** — overview of the code structure and features for effective onboarding of new members and for communicating information to the user who wants to adapt the product.
* **Playpen & Experimentation Logs** — what changes were implemented, what designs were tried out, what insights were gained
Integration Change Logs — what version of code is currently deployed for user experience feedback and data collection

#### Proposal for Documentation Templates

To ensure documentation stays purposeful, concise and precise, I propose we lay out a set of standard documentation templates with sections that answer the following questions:
* What Purpose
  * Main problem the component is designed to solve
  * Requirements of conceptual integrity and ways of defining ease-of-use as a design pillar
* How to Use
  * Environment
  * Operating instructions
* How to Trust
  * Estimates of time, accuracy, durability, etc.
  * Examples and specifications for main, supported but rare, and definitely unsupported operation scenarios
  * Input and output descriptions
  * Options for customizability
* How to Adapt
  * Functions realized and algorithms used
  * Overview of component structure and integration points
  * Self-documenting data structures (think annotated tables and dicts rather than bare numpy arrays)
  * Notes on ideas and suggested changes considered by the original team

### Mechanical Components

#### Proposal for comfortable AND adjustable ring + IMU holder

Designing a data collection prototype fulfills a different need from designing a final product. Both must be polished and usable, but where a final product is sparse and possibly designed to custom fit one user’s lifestyle and aesthetic, a data collection prototype must be easily customizable to immediately fit a variety of different users within a short time frame.

Ring sizing was a consistent problem spot for the gesture ring data collection prototype. I propose using a soft and adjustable band style and material, similar to a watch band or drawstring, rather than traditional ring materials. 

I also propose investigating designs for adjustable metal rings to mitigate rings becoming stuck when fingers swell or loosening when users lose weight.

I also suggest further research going into the mechanical design of stabilizing the orientation of a ring. Normally, if a ring is loose and twists to the side of a finger that does not matter too much. However, on a ring with acceleration sensors, the ring’s orientation relative to the finger could impact how it reads finger motion acceleration from its coordinate system.

### Electrical Components

#### Keep using dependable electrical board to test IMU sensor data stream

The decision to use a fully tested board for initial prototyping was intelligent, so I suggest that we keep using it. I propose that we assign at least one person to understand the documentation (style, interface, patents, and content) of a popular and marketed board.

Researching pain points and user specifications of boards successfully on the market may help us integrate with other products in the future, thus potentially preparing us to solve real-world usability challenges. This information gathering may have already happened, but that information never got down to me.

### Software Data Collection App

#### Proposal for easy-to-use AND easy-to-modify data collection process

If I were to do it all again, I would shift all the data collection and signal processing work into a purely Python implementation. The IMU supported a wrapper around a Python API, so that was possible.

Coding in just one language would allow for cleaner design and implementation iteration across the project. Coding in a language that we were familiar with would enable us to implement our ideas for real-time user feedback and collector review (i.e. plotting signals, visualizing validation checks, interactively marking bad data points) and more options for toggling customization (i.e. adjusting threshold noise levels, easily extending or shortening data collection periods).

To make data easier to review and better understand instances of model class confusion, having a definite ground truth is ideal. For example, if there were video recordings of all the gestures in the data set, it would be possible to check whether misclassified gestures had certain traits or whether human fallibility resulted in performing the wrong gesture in response to prompts. Given ground truth, we could retroactively ask questions such as what makes a gesture signal noisy, what is the effect of ring looseness or tightness, and what subtle changes happen over time as users become more comfortable with the gestures, or what are common error modes of users.

As a very far out idea, data collection and user experience could be combined into one very interesting experience, where the data is collected from a user playing a game or exploring a narrative space in explicitly controlled environment, and gestures classifications are supplied in real time by a more robust but not wearable technology (i.e. collecting data for the ring, classifying based on video control). This could be proposed to users as an interesting and fun futuristic experience while in the back it collects data about how people use real, intentional gestures. We could set it up in a hallway or the mall (like one of the Kinect just-dance promotion demos).

If, as in our case, working in Swift to control an iOS iPhone was a final goal, one person could be assigned to port fully tested and finalized ideas into Swift. By that point, the engineering challenge is about efficiency and implementation decisions, rather than user experience investigation, so design rework is a smaller pain-point.

#### Proposal for process of designing user experience questions

Surveying the demographic and usability questions of a data collection user is a great opportunity to collect user experience information and opinions. Collecting demographic and intention information (i.e. gender, age, blind/visual impairment notes, tendency towards gesturing in normal life, arm and grip strength, comfort level of voluntary expended mental effort) could be useful in crafting personas and adapting the product to solve the needs of realistic, rather than “universal, genderless, average” users.

While the final survey should be crafted by a single person in order to be as coherent, concise, and precise as possible, brainstorming questions should be a group effort, involving as many people from different research and work experience backgrounds as possible.

A variety of questions makes sure that product uncertainty is explored from multiple angles (i.e. from “what are comfortable activation gestures?” to “what ring designs will people wear?”). Having an idea about the broad scope of user experience uncertainty can guide planning and frequency of user interviews, surveys, and co-design, as well as development and testing plans for functional prototypes.

#### Proposal for process of capturing user feedback

I think that the system of recording user experience through a spreadsheet and forms was reasonably efficient as question changes did not involve coding changes and feedback could be summarized easily. 

However, I do think a process of streamlining and anonymizing the collection of user demographic data would have inspired more volunteer trust in the process. One anonymization proposal is, instead of using Olin usernames as the unique table data merging key, using randomly generated unique short words.

### Software Signal Processing Logic and Model Training

#### Proposal for a generally robust and intuitive gesture set incorporating hand anatomy research

Based on hand anatomy research, holding a hand in front of the body for long periods of time is a fatiguing and unreasonable neutral position. Generally, supporting a part of the arm or letting arms lie neutrally at their side is a more comfortable position. The ideal neutral hand orientation is also a position of minimum strain. Awareness of this position prompts a need to redesign the gesture set. During data collection, when users were asked to position their arms in a more comfortable way (arms crossed or arms at their side) they systematically struggled to perform the downward tap motion. Their hands were already resting in a full downward tap position, so they could not easily tap down without first going up. This pattern of acceleration then closely resembled the up-down acceleration of the flick motion, leading to classification confusion.

A better gesture set would use knowledge of how the hand naturally moves from its neutral position to craft robustly different movement patterns. 
* The **“draw counterclockwise circle starting at the bottom”** is a good gesture because it starts in a downward resting neural position and ends in the downward resting position.
* The **“flick”** or **“move up”** gesture is good as long as there are no other gestures also moving up and down along the same axis of acceleration. Relaxing the hand naturally snaps it into the neutral position so returning is not a burden for the user.
* Interestingly, the **“left swipe”** and **“right swipe”** motions are robustly differentiable because unlike the **“up”** and **“down”** swipes, moving the hand **“left”** and **“right”** activates significantly different muscles so the acceleration patterns are significantly different. The **“left”** and **“right”** gestures should not be confused as long as care is taken to return back to the neutral position before starting the gesture. Relaxing the hand naturally snaps it into the neutral position so returning is not a burden for the user. This gesture set showed workable accuracy when used in the non-machine learning classifier.

Further research needs to be done into understanding how users can effectively use the ring without developing hand fatigue. One proposal is supporting a “repeat action mode” for easier navigation and repetitive selection. For some users, engaging with a gesture ring may reduce their struggles with pressing buttons and heavy handles. If this is a consideration, it must be clearly defined in the design proposal (i.e. priority to develop a button-less mode switch, like an activation gesture).

For choosing an activation gesture, I propose a “full twist and back” motion. A full twist appears as a robustly detectable IMU orientation shift and can be captured by checking for that specific pattern on gyroscope sensor data. Two simple logic checks can capture the full variation caused by performing a right or left handed full twist (due to the handedness of this gesture, it is easier to classify programmatically than by machine learning model). Most users are comfortable with this gesture, though some have expressed preference for a more natural “wake up” movement such a “wave” or “slap” motion. The verbal gesture descriptions have a great influence over a user’s perception of the intent of a gesture, so communication and design materials should keep that in mind.

#### Proposal for way to teach gestures in blind / visually Impaired-aware manner

For both visually impaired users and sighted users, learning gestures by looking at a sequence of images or by following a video can be difficult. This is because gestures are inherently located in a directed 3D orientation space, so conveying the full scope of instructions via a 2D medium is imperfect. Adding in the complexity of describing the full freedoms of hand orientation (the model is trained on acceleration patterns so orientation does not matter. You can do gestures with your arm at your side, with your arms crossed, lying in bed, etc.) and emphasizing the stipulation of returning the hand back to the neutral position as part of the gesture, conveying all the instructions can become complicated. Also, using the verbal directions “up”, “left”, and “right” to describe movements relative to the IMU-finger origin and not the user’s point-of-view origin can create mental dissonance.

I propose developing a lightweight tactile learning aid in the form of stiff paper or cardboard that can be folded into a cube. On each face of the cube is a gesture movement arrow, either embossed or cut as a slit. The change in texture allows users to follow the movement without line-of-sight. Slits can be cut or texture can be placed so that there is an obvious correct direction of movement.

By placing a tactile learning aid against a surface (walls and tables both work), and resting their hand against that same surface, the user gets an idea of what neutral positioning and gesture hand orientation should look like. All instructions are in terms of interacting with the tactile learning aid (i.e. The bottom of the box has a rough texture. Place the box against a surface and rest your hand against the same surface oriented so that your pointer finger is touching the box. Make sure that your palm and the rough face of the box are facing the same direction. Practice the gesture by tracing the pattern on the box with the finger wearing the gesture ring.)

#### Proposal for improving the accuracy and robustness of the model

Given enough data, implementing methods like Convolutional Neural Networks (CNN) + Dynamic Time Warping (DTW) could allow the model to learn how to match gestures to templates despite variations in gesture speed.

### Unified Software Demo

#### Proposal for faster integration of machine learning model into software demo

The app’s classification latency is probably due to the speed of sending data over the HTTP protocol. Investigating methods to port the model into the iOS app would probably help.

#### Proposal for easy-to-use demo app experience incorporating AR/VR research

An alternative design flow may have allowed the evolution of the gesture set design to influence the evolution of the screen-free software demo app’s action set. This may have resulted in a more intuitive and discoverable gesture interaction mapping. This effort could leverage research in augmented and virtual reality development to find comfortable mappings that moved beyond opposing action pairs to control a 2D screen. AR and VR research shows that users have a strong preference for an UNDO action, an INTERACT action, and navigation actions. I envision a new interactive demo-app that acts like AR without the intensive graphics. I propose the following mapping:
* **INTERACT** — move up
* **UNDO / EXIT** — counterclockwise circle
* **NAVIGATION** — move right
* **REVERSE NAVIGATION** — move left

Based on user research, supporting the following modes could be useful:
* **DEACTIVATE** (turn machine learning model off)
* **ACTIVATE** (listen for single gesture)
* **REPEAT** (repeat last action, avoid hand fatigue from repeatedly gesturing)

Toggling between modes could be done with a mechanical dial or button, or by checking for a series of activation gestures (i.e. gestures classified outside of the machine learning model).

As a simple pilot program to help explore the user experience of screen-less software design, I propose implementing a short (1-5 minute), audio- and haptic-only version of a narrative story game. This game can include navigation (go forward, go back), interaction actions (equip, talk, eat), and switching between menus and panels (setting screen, inventory screen, etc.). The potential for rich but comfortably familiar interactions (and making downscaled or upscaled complexity versions as appropriate) could provide a good testing ground for functionality. It could also increase user interest and engagement by appealing to aesthetic senses of cuteness or coolness.

I propose developing this immersive game-like experience in tandem with the common use-case experience that the software team has already started developing. Both programs explore different but complementary areas in the screen-less interaction user experience space.

## Concise and Precise Takeaways

* **Pitch Goals AND Limitations Before Recruitment** — clearly propose a concrete “what” before trying to select or encourage self-selection of a team to design the “how”, especially if people can’t leave the class or experience easily. Time, work, and care are valuable, don’t create a situation where that commitment can be betrayed.
* **Organization Management** — support hustle, conceptual integrity, creative freedom, and status/problem/proposal communication.
* **Team Management** — divide labor so that one person is working on the core component design and main implementation, and assign other people to work on critical path support and alternative research and multiple implementations development.
* **Integration Management** — design the user experience around ease-of-use, outline a critical work path for implementation, create expectations to prioritize deadlines and integration requirements, plan assignment of less urgent / parallelizable tasks around the critical work path, strictly control and document what goes into the work product deliverable.
* **Documentation Management** — write documentation to support different reader intents. Follow a consistent structure to enable an easier reading experience and aid thoughtful, concise, and precise communication. All documentation and decision-making processes should be visible to enable research into problems and proposal development.
* **Before Showing Users the Work Product** — plan for design feedback, component test, integration test, implementation feedback, and surrogate user tests under a variety of conditions before planning to show product / prototype / data collection process to real users. Value the time, work, and care of users.
* **Learning a Broader Outlook** — I now understand more about the development of AR/VR, screen-free, and wearable technology. I can critically consider aspects of user-experience beyond the look, feel, and speed of 2D software components.





