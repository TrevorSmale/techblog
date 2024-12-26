+++
author = "Hugo Authors"
title = "ProLUG Admin Course Unit 15 🐧"
date = "2024-12-25"
description = "ProLUG Admin Course Unit 15"
draft = "false"
tags = [
  "Tech", "Linux", "Administration", "Engineering", "ProLUG Course", "Operating Systems", "Trouble Shooting"
]
categories = [
    "Learning",
]
series = ["Learning"]

+++

<!--more-->

# Engineering Troubleshooting

Systems engineering troubleshooting involves diagnosing and resolving complex issues within interconnected systems to ensure seamless operation and optimal performance. It requires a methodical approach to identify root causes, integrate solutions, and maintain system functionality while addressing both technical and process-related challenges.

---

## Discussion Post 1: 

Your management is all fired up about implementing some Six Sigma processes around the company. You decide to familiarize yourself and get some basic understanding to pass along to your team.

### Quoted from the book

5S is a Japanese Lean approach to organizing a workspace, so that by making a process more effective and efficient, it will become easier to identify and expunge muda. 5S relies on visual cues and a clean work area to enhance efficiencies, reduce accidents, and standardize workflows to reduce defects. The method is based on five steps:

- Sort (Seiri)
- Straighten (Seiton)
- Shine (Seiso)
- Standardize (Seiketsu)
- Sustain (Shitsuke)

### What about the “5S” methodology might help us as a team of system administrators? 

- Identify and categorize common troubleshooting problems such as typos, illogical configurations, or vulnerabilities to establish clarity and prioritize issues. (Seiri)
- Organize and catalog processes and procedures for addressing both routine and uncommon problem scenarios for quick access and consistency. (Seiton)
- Validate and test all processes and procedures to ensure they function effectively and reliably. (Seiso)
- Promote team familiarity by regularly practicing and drilling procedures, similar to incident response training, to build confidence and efficiency. (Seiketsu)
- Apply the processes and procedures in real-world scenarios, evaluate their effectiveness, make necessary adjustments, and document improvements for future use. (Shitsuke)

By applying the 5S methodology to troubleshooting, the team can develop a shared understanding of how to consistently address issues, identify system failure points, and create opportunities for incremental improvement, fostering a sense of flow and efficiency.

### What are the four layers of process definition? How would you explain them to your junior engineers?

input - anything entering the process or is required to enter the process to drive the creation of an output
outputs - service or product that is created by this process
events - predefined criteria or actions that cause a process to begin working.
tasks - activities are the heart. a unit of action within the process.
4.1 - Decisions are possible made during or for tasks.

The four layers of a process—inputs, outputs, events, and tasks—function similarly to how a computer program uses functions to process variables and produce results. However, in a Six Sigma process, these elements are more dynamic, encompassing both virtual and physical components, as well as steps that may be driven by either human actions or automated systems. This flexibility allows Six Sigma to address complex workflows that combine diverse inputs and tasks to achieve consistent and efficient outputs.

Looking at our operation as a series of processes with layers like this can help us to identify, refine and standardize processes into Standard Operating Procedures (SOP's). 

---

## Discussion Post 2: Your team looks at a lot of visual data. You decide to write up an explanation for them to explain what they look at.

### What is a high water mark? Why might it be good to know in utilization of systems?

The phrase “high water mark” originates from marking a riverbank to indicate the highest water level reached during a season. This mark serves as a warning, signaling potential danger if the water rises beyond it in the future.
In the context of systems, the high water mark represents historically safe operational loads. If metrics indicate that this threshold has been exceeded, it should alert administrators to a potential issue.
For example, if the high water mark for daily memory usage was 14/18 GiB of RAM, and we observe the system suddenly using 16/16 GiB, this warrants attention as a potential problem requiring further investigation.

### What is an upper and lower control limit in a system output? While this isn’t exactly what we’re looking at, why might it be good to explain to your junior engineers?

Control limits are essential tools for monitoring the stability of a process over time. The upper and lower control limits define the normal range within which a process output should remain when the process is operating correctly. If the output exceeds these boundaries, it signals a potential issue, indicating that the process may be out of control and requiring investigation or troubleshooting.

---

## Definitions/Terminology

- **Incident** An unplanned event that disrupts normal operations.
- **Problem** The underlying cause of one or more incidents.
- **FMEA** (Failure Mode and Effects Analysis): A method for identifying and prioritizing potential failure points in a process.
- **Six Sigma** A data-driven methodology focused on improving processes by reducing defects and variability.
- **TQM** (Total Quality Management): A management approach emphasizing continuous improvement and customer satisfaction across all organizational processes.
- **Post Mortem** A retrospective analysis of an event to identify successes and areas for improvement.
- **Scientific Method** A systematic process of forming hypotheses, testing them, and analyzing results to draw conclusions.
- **Iterative** A repetitive approach to refining a process or solution through successive cycles.
- **Discrete Data** Data that can only take specific, distinct values.
- **Ordinal** Data with a meaningful order but no consistent interval (e.g., satisfaction ratings).
- **Nominal** (Binary/Attribute): Categorical data without order, such as “yes/no” or “male/female.”
- **Continuous Data** Data that can take any value within a range, such as temperature or time.
- **Risk Priority Number (RPN)** A score in FMEA used to prioritize risks, calculated as Severity × Occurrence × Detection.
- **5 Whys** A technique to identify the root cause of a problem by repeatedly asking “why” until the root cause is found.
- **Fishbone Diagram (Ishikawa)** A visual tool used to identify and categorize potential causes of a problem.
- **Fault Tree Analysis (FTA)** A deductive analysis method used to identify the root causes of system failures.
- **PDCA (Plan-Do-Check-Act)** A cyclical process for continuous improvement in workflows or systems.
- **SIPOC** A high-level process map identifying Suppliers, Inputs, Processes, Outputs, and Customers.

---

### ProLUG Links ⛓️

Discord: https://discord.com/invite/m6VPPD9usw
Youtube: https://www.youtube.com/@het_tanis8213
Twitch: https://www.twitch.tv/het_tanis
ProLUG Book: https://leanpub.com/theprolugbigbookoflabs
KillerCoda: https://killercoda.com/het-tanis

