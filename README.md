# Prompt Iterations Log

This document tracks three prompt versions tested against both sample transcripts. Each version was driven by a specific observation from the previous one. The goal was a prompt that generalizes well across different interview styles and candidate roles, not one tuned to a single transcript.

---

## Iteration 1: Basic Prompt

### Prompt

```
You are a recruiter assistant. Read this interview transcript and give me:
1. Topics covered
2. Candidate profile (role and level)
3. A short summary of the candidate

Transcript:
{transcript}
```

### Input

Both full transcripts were passed in separately. Excerpts below represent what was substantively discussed.

Transcript 1 excerpt (Frontend Engineer):
> Interviewer: Can you walk me through how you'd structure a scalable Angular application for a large enterprise dashboard?
> Candidate: Sure. I would break it into feature modules, shared modules, and core modules. The core module handles singleton services like auth and HTTP interceptors...

Transcript 2 excerpt (Operations candidate):
> Interviewer: Tell me about a time you managed a complex vendor relationship.
> Candidate: Yes, so I was handling pan-India vendor onboarding. We enrolled around 22 vendors. I did the market research, evaluated the commercials, and handled contract negotiations...

### Output (Transcript 1)

```
============================================================
INTERVIEW SUMMARY
============================================================

TOPICS COVERED
AI-powered coding assistance: The candidate discussed using AI coding assistants throughout
the development life cycle, including generating initial models and components.
Ionic Framework: The candidate explained how to structure the UI, handle responsiveness, and
ensure performance for large datasets in an Ionic v7 mobile-first dashboard.
Capacitor and plugin ecosystem: The candidate described using Capacitor and plugins for
native functionalities, such as capturing images and storing them locally.
CSS framework utility classes: The candidate discussed using Tailwind for a highly responsive
design system, including structuring styling approaches for scalability.
Angular Framework: The candidate designed a scalable Angular application structure for a large
enterprise dashboard with multiple modules and complex data flow.
State management in React: The candidate explained how to design a state management strategy
for a large-scale React application with multiple shared states.
Mobile app development: The candidate mentioned working on service-based applications, risk
assessment management tools, and communication-based applications, mainly focusing on Ionic
Angular and Ionic Capacitor.

CANDIDATE PROFILE
The candidate fits the role of a Full Stack Engineer, mid-senior level. With around nine years
of experience in software development, the candidate has a strong background in Angular, React,
and Ionic framework, as well as experience with Node.js, Express, and .NET Core. The candidate's
answers demonstrated a good understanding of technical concepts, such as state management, API
integration, and responsive design. However, the candidate's code examples were sometimes
incomplete or lacking in detail, which may indicate a need for improvement in coding skills or
experience with specific technologies like Redux or Zustand.

CANDIDATE SUMMARY
The candidate has a professional background spanning around nine years in software development,
with a focus on full-stack engineering using technologies like Angular, React, and Ionic. The
candidate's technical strengths include a good understanding of state management, API
integration, and responsive design, as well as experience with AI-powered coding assistance
tools. However, the candidate's limited experience with certain technologies, such as Redux or
Zustand, and incomplete code examples are concerns that need to be addressed. Overall, the
candidate appears to be a solid mid-senior level full-stack engineer with a strong foundation
in web and mobile development, but may require additional training or experience to fill gaps
in their technical skillset. The candidate's communication skills and process thinking are
clear, but their leadership indicators are not explicitly evident in the transcript.
```

### Output (Transcript 2)

```
============================================================
INTERVIEW SUMMARY
============================================================

TOPICS COVERED
Project and Program Management: Discussion on managing projects, vendor management, and
stakeholder engagement.
Fraud Detection and Prevention: Candidate's experience in building a fraud detection system
and preventing scams.
Vendor Management: Process of onboarding vendors, evaluating their performance, and handling
underperformance.
Leadership and Stakeholder Management: Interaction with CXOs, CEOs, and other high-level
stakeholders.
Operational Efficiency: Improving execution speed, upgrading BPO/calling processes, and
tracking KPIs.
Data Analysis and Presentation: Presenting data to higher-level stakeholders and handling
disagreements.
Communication and Conflict Resolution: Handling conflicting priorities and stakeholder
disagreements.
Career Background and Experience: Candidate's work experience in ed-tech companies and their
career progression.

CANDIDATE PROFILE
The candidate fits the role of a Project and Program Manager at a mid-level to senior level.
This is justified by the candidate's experience in managing end-to-end finance operations,
building a fraud detection system, and interacting with high-level stakeholders. The candidate
has also demonstrated their ability to handle complex projects, manage vendors, and improve
operational efficiency. Additionally, their experience in presenting data to stakeholders and
handling conflicts also supports their suitability for this role.

CANDIDATE SUMMARY
The candidate has demonstrated strong experience in project and program management, with a
background in managing complex operations and improving efficiency. Their ability to build a
fraud detection system and prevent scams showcases their analytical and problem-solving skills.
However, the candidate's tendency to use jargon and lack of confidence in communication may
be a limitation in leadership roles. Overall, the candidate has shown a strong understanding
of operational management and stakeholder engagement, but may need to work on their
communication skills to succeed in a senior leadership role. The candidate's willingness to
learn and take feedback is also a positive aspect, as seen in their response to the
interviewer's feedback on their communication style.
```

### What Worked / What Didn't

The model returned the right three sections for both transcripts without any format failures, which was the bare minimum and it cleared it. Topics were populated with reasonable labels, and on Transcript 2, the model caught the communication gap, which showed it was reading the transcript rather than just pattern-matching from the prompt.

The problems were significant though. On Transcript 1, the model labeled the candidate a "Full Stack Engineer" when the entire interview was clearly frontend-weighted. Node.js and .NET Core were mentioned but never substantively explored in the conversation. That misclassification matters because a hiring team would route this candidate differently based on that label alone. The Transcript 2 profile had the opposite problem: accurate in role but completely generic in justification. Sentences like "the candidate has demonstrated their ability to handle complex projects" could apply to anyone in any transcript.

The Transcript 2 summary added a claim about "willingness to learn and take feedback" that was a soft inference, not something clearly demonstrated in the transcript. With no instruction preventing it, the model filled gaps with generous assumptions. The Transcript 1 summary ending with "leadership indicators are not explicitly evident in the transcript" was an honest observation but read like a placeholder, not an actionable signal for a hiring team.

The prompt gave the model no guidance on what "specific" means, no constraint on role classification, and no instruction to stay grounded in evidence. Everything that went wrong here traced back to those three omissions.

---

## Iteration 2: Structured Prompt

### Prompt

```
You are a senior technical recruiter with 10 years of experience. Read the interview transcript
below and produce a structured candidate summary with three sections:

1. TOPICS COVERED: List the main themes discussed (4-8 topics), one per line.

2. CANDIDATE PROFILE: State the role and level (e.g. "Backend Engineer, mid-level"). Justify
in 2-3 sentences based on the transcript.

3. CANDIDATE SUMMARY: A paragraph of 4-5 sentences covering background, strengths, weaknesses,
and overall impression. Be honest and specific, not generic.

Do not invent skills not mentioned in the transcript. If something is unclear, say so.

Transcript:
{transcript}
```

### Input

Same both transcripts, passed in full.

### Output (Transcript 1)

```
============================================================
INTERVIEW SUMMARY
============================================================

TOPICS COVERED
AI-powered coding assistance: The candidate discussed using AI coding assistants throughout
the development life cycle, including generating initial models and components.
Ionic Framework: The candidate explained how to structure the UI, handle responsiveness, and
ensure performance for large datasets in an Ionic v7 mobile-first dashboard.
Capacitor and plugin ecosystem: The candidate described using Capacitor and plugins for
native functionalities, such as capturing images and storing them locally.
CSS framework utility classes: The candidate discussed using Tailwind for a highly responsive
design system, including utility-first approach and mobile-first responsiveness.
Angular Framework: The candidate designed a scalable Angular application structure for a large
enterprise dashboard with multiple modules and shared components.
State management in React: The candidate explained how to design a state management strategy
for a large-scale React application with multiple shared states.
Mobile app development: The candidate mentioned working on service-based applications, risk
assessment management tools, and communication-based applications using Ionic, Angular, and
React.

CANDIDATE PROFILE
The candidate fits the role of a Full Stack Engineer, mid-senior level. With around nine years
of experience in software development, the candidate has a strong background in Angular, React,
and Ionic framework, as well as experience with Node.js, Express, and .NET Core. The candidate's
answers demonstrated a good understanding of technical concepts, such as state management, API
integration, and responsive design. However, the candidate's lack of experience with certain
tools, such as Zustand, was noticeable.

CANDIDATE SUMMARY
Prasanna Kumar has around nine years of experience in software development, with a strong focus
on Angular, React, and Ionic framework. The candidate's technical strengths include designing
scalable application structures, implementing state management strategies, and using CSS
frameworks for responsive design. However, the candidate's lack of experience with certain
tools, such as Zustand, and some gaps in knowledge, such as the specifics of Redux slice, are
concerns. Overall, the candidate demonstrates a good understanding of technical concepts and
an ability to design and implement complex applications, but may require additional training or
support in certain areas. The candidate's communication skills are clear, and they are able to
explain technical concepts in a concise manner. Despite some limitations, the candidate appears
to be a strong fit for a mid-senior level full stack engineer role.
```

### Output (Transcript 2)

```
============================================================
INTERVIEW SUMMARY
============================================================

TOPICS COVERED
Program Management: Discussion on managing programs, vendor management, and stakeholder
engagement.
Vendor Management: Strategies for onboarding and managing vendors, including market research
and contract negotiations.
Operational Efficiency: Improving execution speed, reducing inefficiencies, and implementing
process upgrades.
Leadership and Stakeholder Management: Handling disagreements with stakeholders, presenting
data, and making decisions with multiple stakeholders.
Project Management: Approaches to managing projects, including breaking down tasks, setting
timelines, and tracking progress.
BPO and Call Center Operations: Optimizing call center efficiency, implementing auto dialers,
and tracking KPIs.
Data Analysis and Presentation: Presenting data to stakeholders, tracking KPIs, and handling
data discrepancies.

CANDIDATE PROFILE
The candidate fits the role of a Project and Program Manager at a mid-to-senior level. This
is justified by the candidate's experience in managing end-to-end finance operations, building
a fraud detection and prevention unit, and handling business operations and analytics. The
candidate has also demonstrated expertise in vendor management, having enrolled 22 vendors at
a pan-India level and having experience with contract negotiations. Additionally, the candidate
has shown strong problem-solving skills and the ability to communicate complex ideas, although
with some room for improvement in terms of confidence and communication style.

CANDIDATE SUMMARY
The candidate has a strong background in program management, vendor management, and operational
efficiency, with a proven track record of implementing process upgrades and improving execution
speed. One of the candidate's key strengths is their ability to analyze complex problems and
develop creative solutions, as seen in their approach to building a fraud detection and
prevention unit. However, the candidate could improve their communication style, as they
sometimes use jargon and lack confidence in their explanations. Overall, the candidate has a
solid foundation in project and program management, but may need to work on their presentation
skills to effectively communicate with high-level stakeholders. The candidate's experience and
skills make them a strong fit for a project and program management role, but they may require
some coaching to reach their full potential.
```

### What Worked / What Didn't

Iteration 2 improved on several fronts. The "do not invent skills not mentioned in the transcript" instruction had a measurable effect: the soft inference about "willingness to learn and take feedback" from the Transcript 2 Iteration 1 output disappeared. The Transcript 2 profile now cited a concrete data point, the 22 vendor pan-India onboarding, which was absent before. The summary also named the candidate directly on Transcript 1, which made it feel less like a template fill.

Two persistent issues stood out. The Transcript 1 role classification was still wrong: "Full Stack Engineer" again, when the interview covered Angular architecture, Tailwind, Ionic, and React state management with no backend depth at all. The 2-3 sentence justification constraint was too short to surface that distinction. The model listed technologies rather than analyzing which ones the interview actually tested, and with so little space to reason, it defaulted to the broadest label available.

On Transcript 2, the profile ended with the claim that the candidate had "the ability to communicate complex ideas," which is directly contradicted by the communication concern noted in the same paragraph. The model noticed the tension but did not resolve it, producing a profile that read as diplomatically evasive rather than analytically honest. The summary ending with "may require some coaching to reach their full potential" told a hiring team nothing actionable. It sounds cautious without being useful.

The prompt also gave no guidance on evaluating non-technical candidates. For Transcript 2, where there is no tech stack to assess, the model had to improvise its criteria, and it defaulted to generic operational buzzwords rather than examining communication clarity or process thinking with any rigor. That gap needed to be addressed explicitly.

---

## Iteration 3: Final Prompt (System + User)

### System Prompt

```
You are a senior technical recruiter and talent analyst with 15 years of experience evaluating
candidates across engineering, product, and operations roles. You read interview transcripts
carefully and produce structured, honest, and useful summaries for hiring teams.

Your summaries must be grounded strictly in what the transcript shows. Do not invent or assume
skills or experiences not evidenced in the conversation. If the transcript is short or vague on
a dimension, say so honestly rather than filling gaps with generic praise.

You write in clear, direct English. No filler phrases like "the candidate demonstrated a strong
passion for" or "showcases exceptional talent." Be specific. Reference actual moments from the
transcript when relevant.
```

### User Prompt

```
Read the following interview transcript carefully, then produce a structured candidate summary
with exactly three sections.

--- TRANSCRIPT START ---
{transcript}
--- TRANSCRIPT END ---

If speaker labels or names (e.g., Interviewer, Candidate) are present, use them naturally for
clarity. If not, proceed without assuming or inventing names.

Now write the structured summary using this exact format. Do not add extra sections or change
the section headers.

TOPICS COVERED
List the main themes and subject areas discussed in this interview. For each topic, write one
line: the topic name followed by a colon and a brief note on what was actually covered. Aim for
4 to 8 topics. Focus on what was substantively discussed, not just briefly mentioned.

CANDIDATE PROFILE
State the role and seniority level this candidate fits, for example "Backend Engineer, mid-level"
or "Operations Program Manager, mid-senior." Follow with 3 to 5 sentences of justification. Be
specific: cite years of experience, tools named, depth of answers, or gaps observed. If the
transcript is from a non-technical role, assess communication clarity, process thinking, and
leadership indicators.

CANDIDATE SUMMARY
Write a single paragraph of 4 to 6 sentences. Cover: (1) professional background and experience
span, (2) two or three concrete technical or functional strengths, (3) one or two honest concerns
or limitations visible in the transcript, (4) an overall hiring impression. Be balanced. A
candidate who is strong in some areas but weak in others should not read as universally impressive
or universally weak.
```

### Input

Both full transcripts, same as previous iterations.

### Output (Transcript 1)

```
============================================================
INTERVIEW SUMMARY
============================================================

TOPICS COVERED
AI-powered coding assistance: The candidate discussed using AI coding assistants throughout
the development life cycle, including generating initial models and components.
Ionic Framework: The candidate explained how to structure the UI, handle responsiveness, and
ensure performance for large datasets in an Ionic v7 mobile-first dashboard.
Capacitor and plugin ecosystem: The candidate described using Capacitor and plugins for
native functionalities, such as capturing images and storing them locally.
CSS framework utility classes: The candidate discussed using Tailwind for a highly responsive
design system, including a utility-first approach and mobile-first responsive design.
Angular Framework: The candidate designed a scalable Angular application structure for a large
enterprise dashboard with multiple modules and shared components.
State management in React: The candidate explained how to design a state management strategy
for a large-scale React application with multiple shared states.
Mobile app development: The candidate mentioned working with Ionic, Angular, and React for
mobile app development, including deploying applications to iOS and Android.

CANDIDATE PROFILE
The candidate fits the role of a Frontend Engineer, mid-senior level. With around nine years
of experience in software development, the candidate has a strong background in Angular, React,
and Ionic framework, as well as experience with Node.js, Express, and .NET Core. The candidate's
answers demonstrated a good understanding of technical concepts, such as state management and
CSS frameworks, and they were able to provide concrete examples of how they would approach
different problems. However, the candidate's lack of experience with certain tools, such as
Zustand, was noticeable, and they sometimes struggled to provide detailed code examples.

CANDIDATE SUMMARY
The candidate has a professional background spanning around nine years in software development,
with a focus on frontend technologies such as Angular, React, and Ionic framework. The
candidate's technical strengths include their experience with AI-powered coding assistance,
Ionic Framework, and state management in React, as demonstrated by their ability to design a
scalable Angular application structure and explain how to use Tailwind for a highly responsive
design system. However, the candidate's lack of experience with certain tools, such as Zustand,
and their sometimes limited ability to provide detailed code examples, are concerns that need
to be considered. Overall, the candidate appears to be a solid mid-senior level frontend
engineer with a strong foundation in frontend technologies, but may require additional training
or experience to fill gaps in their skillset. The candidate's communication style is clear and
concise, and they demonstrate a good understanding of technical concepts, making them a
promising candidate for a frontend engineering role.
```

### Output (Transcript 2)

```
============================================================
INTERVIEW SUMMARY
============================================================

TOPICS COVERED
Program Management: Discussion on managing end-to-end finance operations, sales, and revenue,
with a focus on fraud detection and prevention.
Vendor Management: The process of onboarding vendors, including market research, evaluating
services and commercials, and contract management.
Leadership Engagement: Interacting with CBOs, CEOs, CXOs, and other high-level stakeholders,
including presenting data and handling disagreements.
Operational Efficiency: Improving execution speed, upgrading BPO/calling processes, and
tracking KPIs for operation success.
Stakeholder Management: Handling multiple stakeholders with conflicting priorities and
explaining decisions to those whose requests are not prioritized.
Data Analysis: Presenting data to higher-level stakeholders, including executive dashboards
and analytics reports.
Project Management: Approaching misaligned departments, breaking down projects into phases,
and tracking progress.

CANDIDATE PROFILE
The candidate fits the role of an Operations Program Manager, mid-senior level. This
assessment is based on the candidate's 3-4 years of experience in operations and program
management, as evident from their discussion on managing end-to-end finance operations and
implementing fraud detection systems. The candidate demonstrates a good understanding of
tools such as CRM and experience with data analysis, which is a plus for this role. However,
the candidate's communication style, which includes using jargon and Hindi phrases, may need
improvement for effective leadership and stakeholder management.

CANDIDATE SUMMARY
Krishna has a professional background spanning around 4 years, primarily in operations and
program management within the ed-tech sector. The candidate's strengths include experience
in managing end-to-end finance operations, implementing fraud detection and prevention
systems, and improving operational efficiency through process upgrades. Additionally, Krishna
shows a good grasp of data analysis and presentation, which is crucial for stakeholder
management. However, a concern arises from the candidate's tendency to use jargon and Hindi
phrases, which may hinder effective communication with high-level stakeholders. Overall,
Krishna presents as a mid-senior level operations program manager with notable strengths in
process management and data analysis, but requiring refinement in communication skills for
higher-level roles.
```

### What Worked / What Didn't

Iteration 3 produced the strongest and most consistent results across both transcripts. The most significant improvement on Transcript 1 was the role classification: the model correctly identified the candidate as a "Frontend Engineer" rather than "Full Stack Engineer." This happened because the 3-to-5 sentence justification requirement, combined with the instruction to cite specific depth of answers and gaps observed, pushed the model to analyze what the interview actually covered. When it had to write more sentences justifying the level, it had to engage with the substance of the conversation rather than list resume technologies, and that substance pointed clearly to frontend work.

On Transcript 2, the output named Krishna directly, used specific language about CRM and pan-India operations, and called out the Hindi code-switching as a concrete communication issue rather than soft-pedaling it as "room for improvement." The explicit instruction to handle non-technical roles by assessing communication clarity, process thinking, and leadership indicators gave the model a defined evaluation framework rather than leaving it to improvise with operational buzzwords.

Splitting into a system prompt and a user prompt also made a structural difference. The system prompt set a consistent evaluative voice and gave the model explicit prohibitions on filler language that carried through both outputs, reducing phrases like "showcases exceptional talent" and "proven track record of delivering results" that had slipped through in earlier iterations. The user prompt handled per-call formatting and content requirements separately, which kept the instructions clean and easier for the model to follow without conflicting signals.

One remaining limitation is worth noting. The Transcript 1 summary still ended with "making them a promising candidate for a frontend engineering role," which slides back toward the kind of generic closer the system prompt was meant to prevent. The prohibition on filler phrases suppressed most of it but did not eliminate it entirely. A stronger negative example in the system prompt, showing a weak closing sentence next to a stronger one, would likely address this in a future iteration. The Transcript 2 profile justification also leaned on CRM as a tool reference without probing the depth of how data analysis was actually used in practice. That reflects the limits of the source material more than the prompt, but an instruction like "if the transcript does not provide sufficient evidence for a dimension, state that clearly rather than inferring from limited data" would make edge cases with thinner transcripts more honest.

---

*All three prompts were tested against both provided sample transcripts before selecting Iteration 3 as the final version.*
