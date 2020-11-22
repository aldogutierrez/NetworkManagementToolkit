# 1. Why is postmortem analysis important?

- Postmortem analysis helps us formulate the procedure of error detection, activity, length of problem, and actions taken

# 2. What is blameless postmortem culture?

- Blameless postmortems occur when the team holds themselves accountable for delayed responses and bad practices without pointing the finger to a specific team member.
    - They help with _team growth_ and _avoids imposter syndrome_, which diminishes team performance

- A blameless postmortem must focus on identifying the contributing causes of the incident without indicting any individual or team for bad or inappropriate behavior

# 3. What is the difference between the problem summary and the root cause?

- **Problem summary**: Discusses the circumstances of the problem, along with downtime and query that caused the problem to arise. High level description from the client's perspective.
    - From the _Site Reliability Engineering Book_:
        - **Summary**: Shakespeare Search down for 66 minutes during period of very high inter‐ est in Shakespeare due to discovery of a new sonnet.

- **Root cause**: It is the primary defect that needs to be fixed in order to stop the chaos. Once fixed we can ensure that this event won’t happen again in the same way. More technical.
    - From the _Site Reliability Engineering Book_:
        - **Root Causes**: Cascading failure due to combination of exceptionally high load and a resource leak when searches failed due to terms not being in the Shakespeare corpus. The newly discovered sonnet used a word that had never before appeared in one of Shakespeare’s works, which happened to be the term users searched for. Under normal circumstances, the rate of task failures due to resource leaks is low enough to be unnoticed.

# 4. What are the key elements of good action items?

- **Action Item**: Description of taskt to care of
    - _Use flux capacitor to balance load between clusters_
- Type: Describes the endgame for the action, whether it is to prevent, fix, or rebuild.
    - _mitigate_, _prevent_, _process_
- Owner: Lists the person or people reponsible for such action item
- Bug: Lists the bug ID from the issue report, and the bug's status
    - _Bug 5554823_ **TODO**
    - _Bug 5554825_ **DONE**

# 5. Why is ownership of the postmortem and action items important?

- **_Ownership_**: Describes and assigns a problem to a single or a group of people, that way we have a structure of who's doing what.

- **_Action Items_**: These are important because they are a log of the tasks that were taken in order to fix the problem's root cause.

# 6. What does "data-driven" mean?

- The team working on a problem must be data-driven, which means that the log data and monitoring data is what dictates te workflow of the actions, teams do not let themselves be influenced by rumors or fear from other sections.
    - **Data-driven** decisions win over decisions based on feelings, hunches, or the opinion of the most senior employee in the room

## 6.1 What is a pre-requisite to a data-driven postmortem?

- The biggest pre-requisite would be just to have some kind of monitoring running, otherwise the source and credibility of the data may be compromised. And data-driven postmortems would not be as effective

# 7. What are the 4 basic principles of incident response?

- **Incident Command**: In a incident response team, there should be some kind of leader. The _incident commander_ selects the task force needed to mitigate the failure, and assings tasks depending on need and priority

- **Operational Work**: According to the incident commander, there should be an _operations manager_ that will overlook changes and fixes to the system during an incident. These people are the ONLY group modifying the system.

- **Communication**: There exists a person in the task force team that is reponsible of _maintaining periodic communication_ between the team and the stakeholders.

-  **Planning**: The planning role _supports_ the operations team _by dealing with longer-term issues_, such as filing bugs, ordering dinner, arranging handoffs, and tracking how the system has diverged from the norm so it can be reverted once the incident is resolved.

# 8. What is a runbook?

- A runbook _resembles FAQs_ about recurring problems in system administration. Instead of trying to figure out the same problems again and again, we can just refer to the _runbook as a standardized guide_ of what to do then a problem arises.

# 9. What are the 3 C's of incident management?

- **Coordinate**: Split team into sectors, each sector is responsible for an action. This organizes the team's response

- **Control**: Maintaining order and discipline when managing an incident

- **Communication**: Maintain constant contact within the team, the stakeholders, and the public

**_NOTE_**: When something goes wrong with incident response, the culprit is likely in one of these areas. _Mastering_ the 3Cs is **essential** for effective incident response. 

# 10. What is DiRT and why is it important?

- The **Disaster Recovery Testing** is a way to test the companies or teams efficiency when dealing with urgent **incidents**.
    - **DiRT** is used in practice when a group of engineers plan and execute **_real and fictitious_** outages for a defined period of time to test the effective response of the involved teams.