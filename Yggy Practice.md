Welcome to Tempnet IRIS!
Timeframe for incident: 3 March 2023 - 8 March 2023
NOTE: Do not hunt outside the time frame unless specified to. This will cause confusion!
- This instance is to be used to practice utilizing the tools for the evaluations. There may be questions that ask to hunt for specific IOCs, these IOCs may not always show up. Practice queries that will get you the information you are looking for



1. Hunt Plan Development
	This section will measure your practical knowledge of various parts of a basic hunt plan. The hunt plan should include the following areas to meet the expected critera for grading of Q.
	
		Atleast 3 Tactical Task
		Measures of Performance for each task
		How you intend on determining success of overall objective given for the evaluation
		Prioritization of Task
		Atleast 1 contingency
		Include given intellegence and information provide in FRAGO
		There is currently a json file on your desktop which may be imported into Mitre Attack Matrix for additional Intelligence.
	
	Timeframe - +/- 20mins

2. Validate Endpoints 
	Using any tool, validate all endpoints on the Treasury Network.
	
	Constraints NMAP has been restricted due to bandwith limitations on mission partner network
	
	Restraints
	
	Based on the network map, are the endpoints shipping logs into the SIEM?
	
	Is every endpoint shipping logs?
	
	Are there any endpoints logging we do not know about?
	
	Is the information on the Treasury Network Map accurate? (IP Address, Device function)
	
	Update targets file on your desktop to only contain devices within the Workstation subnet unless otherwise directed to be used for agent deployment of Endgame, Velicoraptor for agent deployment and to access vulnerabilities using Nessus on the next shift.
	
	Annotate finding in the comments log and report them to your CCL upon completion.

3. Baselining and Data Collection Validation
	1. Based on the devices decovered. Perform the following actions.
	Query https://www.zerodayinitiative.com/advisories/published/ for Zero days. 
	
	1. Is the most current advisor applicable to the environment based on information currently know?
	
	2. Confirm remote and local access to the following tool on each device in scope.
	
	Windows Event Viewer
	Task Scheduler
	Regedit
	Windows Management Instrumentation
	Powershell
	
	3. Report findings in text document in the following format below to include systemic issues that could lead to vulnerability. (r) remote (rl) remote and local (l) local (x) inaccessible
	
	
	Host Name: example.com
	IP Address: 123.123.123.123
	    1. Windows Event Viewer : (r)
	    2. Task Scheduler : (rl)
	    3. Regedit : (x)
	    4. Windows Management Instrumentation : (x)
	    5. Powershell :(rl)
	    6. Bash(rl)
	    7. SSH: (x)
	
	4. Send file to CCL via rocketchat for plan considerations 

4. Investigate Endpoint
	1. **This task is for the investigation of an endpoint.**
	**Constraints** Use of RDP for endpoints restricted by Mission Partner.
	
	**Restraints** 
	
	
	Using your planned hunt methodology IAW unit standards and MITRE ATT&CK matrix provided, identify and provide **AT LEAST 2** additional indicators of compromise to escalate the malicous nature of observed activity.
	
	Provide the scope of other host with same indicators from above.
	
	Compare Hashes of executables with known bad list of hashes (NIST, NSRL, Bit9)
	
	
	
	Update task log with any findings and Attach artifacts under Evidences within this case. Uses the naming convention provided. (File-DDMMYYYY-TaskID)
	
5. Hunt Lateral Movement
	This Task is to be used If Network team suspects lateral movement. 
	
	Continue your investigation for signs of lateral movement and perform an initial endpoint triage against the host of interest.
	
	Update task log with any artifacts or findings.

6. Identify Exfiltrated Data
	Mission Partner stated they are concerned with exfiltration of data from the Treasury network. This ticket is for investigation associated with Exfiltration.
		
	Use any alternative method to RDP to identify artifacts and indicators of staging and exfiltration is Pre-Approved by the mission partner.
		
	Intel stated the most likely method of data exfilitration will involve common directories a user.
		
	Add findings to the task log and attached artifacts to Evidences under this case. Naming conventions may be passed if warranted.

7. Recommendations & Remediation's
	Based on the artifacts identified, submit recommendations to the mission partner for hardening their systems and vulnerabilities.
	
	The Mission partner would like assistance with identifying the TTPs of the adversary in the future.
	
	Task Provide 1 method of finding any of the artifacts you collected which would satisfy this request with your reccomendations.
	
	Given the current landscape and adversary activity what would be considered if the mission partner wanted to act to remove the adversary from the network.

8. Mission Report - Request Network Support
	Utilizing everything you just discovered from previous tasks, combine it all into a single report for future operators to investigate next shift or CCL/MC to report on.
	
	Use whichever format you would like, summary, bullet form, timestamps, whomever. Just paint the entire picture of what you found or believe happened.
	
	Adversary movement
	What did you do
	2 Known IOCs discovered
	2 Documents generated and where are they stored
	 

