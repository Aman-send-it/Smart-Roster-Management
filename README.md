# Smart-Roster-Management


1. **How did you model employees, brands, processes, pods, and their relationships?**  
   I used SQLite to represent the team and its work. Each employee belongs to a pod, each pod has a lead, and each process belongs to a brand. An assignment links an employee to a specific brand and process. This lets one employee handle several processes and several employees share one process. I stored daily roster entries separately from regular assignments so that a backup can cover someone for a day without changing who normally owns the work.

2. **How does your system generate a roster and check the constraints?**  
   For the selected month, the scheduler determines which processes are open each day using their operating calendars and applicable holidays. It accounts for approved absences and locked manual entries, then places weekly offs while trying to protect coverage and avoid work streaks longer than six days. Available pod leads fill remaining gaps. Finally, the system compares the number of people covering each open process with its minimum staffing requirement and reports shortages, missed weekly offs, and preference overrides.

3. **How did you handle single-resource and multi-employee brands?**  
   I used the same scheduling rule for both: check who is assigned to each process and available that day. In the sample, 17 of the 21 brands have one dedicated employee, while four have three employees. At KoraSkin, Meera normally handles both Customer Support (CS) and Abandoned Cart Conversions (ACC). At VelvetVibe, CS requires two people, so having one person available still leaves that process short. Checking coverage per process captures both arrangements.

4. **What happens when a dedicated employee is unavailable on a required working day?**  
   The scheduler checks every process that employee would normally cover and looks for an available backup for each one. For example, when I recorded leave for Meera on December 1, KoraSkin lost its regular coverage for both CS and ACC. After regeneration, Aarav covered CS and Siddharth covered ACC. Each process showed **1 of 1 required**. If only one backup had been available, the other process would still have appeared as understaffed.

5. **How does your system handle conflicting requirements or insufficient staffing?**  
   I gave required coverage priority over preferred off-days and made the outcome visible to the manager. An understaffed process produces a hard conflict with the date, process, and staffing gap. Moving a preferred off-day or using a backup produces a soft warning. For example, on December 5, Ritika covers ZestyCrate’s CS process, but Retention still has **0 of 1 required**. The app reports that shortage and blocks an attempt to assign Ritika to both processes that day.

6. **Which decisions did you automate, and which did you leave under human control? Why?**  
   I automated calendar checks, weekly-off placement, backup allocation, and validation because the same rules need to be applied consistently across every employee and date. Managers enter approved absences, change regular assignments, review conflicts, and can select an available lead manually. A manual backup stays locked when the roster is regenerated. I left decisions such as asking a client to accept reduced coverage with the manager because they depend on business priorities the scheduler cannot infer.

7. **If the company doubled its team and brand count, what would you change?**  
   I would first measure where generation slows down. The current engine repeatedly counts available staff while trying different off-days and backups. Keeping a count for each process and date, and updating it when an assignment changes, would reduce that repeated work. If several managers were planning together, I would also move from browser-local storage to a shared database with edit history and version checks so one manager’s regeneration could not overwrite another’s changes.

8. **If you had one additional week, what would you improve first?**  
   I would improve how the system checks an employee’s capacity across multiple responsibilities. The current model works in whole days and assumes a working employee can handle all their regular assigned processes that day. That is reasonable for some dedicated roles, but it can overstate coverage when two duties need attention at the same time. I would add shift windows or workload limits and test two cases: one employee handling compatible duties successfully, and one employee being flagged when two assignments overlap.
