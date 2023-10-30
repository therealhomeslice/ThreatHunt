From a Nessus scan on the Key Terrain the following level of vulnerabilities need prioritised by category

1.	Critical: This category includes vulnerabilities that pose a significant risk to the security of the scanned system. These vulnerabilities often have a high severity level and can potentially lead to unauthorized access, data breaches, or system compromise.
2.	High: High-risk vulnerabilities that may not be as severe as critical ones but still present a significant security concern. They can potentially lead to unauthorized access or system compromise if left unaddressed.
3.	Medium: Medium-risk vulnerabilities that have a moderate impact on the system's security. These vulnerabilities might not pose an immediate threat but should still be addressed to reduce the overall risk level.
4.	Low: Low-risk vulnerabilities that have minimal impact on the system's security. These vulnerabilities are typically less critical and may not require immediate attention. However, it's recommended to address them over time to maintain a secure environment.
5.	Informational: This category includes non-security-related findings that provide additional information about the scanned system, such as system configurations, software versions, or other details that can help with system management or compliance.
6.	Compliance: Nessus can also include a category specific to compliance-related vulnerabilities. These vulnerabilities are often tied to regulatory standards and industry best practices. The compliance category helps identify issues that may impact the system's adherence to specific standards, such as PCI DSS, HIPAA, or ISO 27001.

1.	Run Nessus
2.	Establish a baseline inventory of all production systems.
3.	Prioritize patches based on their criticality either for security or functional reasons.
4.	Automate the patch management process to decrease the time between the release and application of patches.
5.	Validate that patches work as expected and don’t break any of your systems or applications.




---------
DISA, “Security Readiness Review (SRR).”
https://public.cyber.mil/announcement/disa-releases-the-following-updated-security-guidance-security-readiness-review-scripts-supplemental-automation-content-and-benchmarks/
(STIG Download Link)
Confluence: Nessus Compliance Scan: https://confluence.90cos.cdl.af.mil/display/OJCCTM/Nessus
1. Download the appropriate DISA STIG checklists for the systems that you are going to scan (OS Type/version dependent)
1.1: For Manual Checks download STIG Viewer
1.1.1: Load appropriate checklist / go line by line on each item
1.1.2: Repeat process on each box
1.2: For Semi Manual (Per Machine) Checks download SCAP (Win Only)
1.2.1: Open SCAP on Local PC, load STIG checklist into SCAP
1.2.2: Run SCAP and go line by line on any failed checks
1.2.3: Repeat process on each box
2. Using the Nessus, build a few compliance scans for each OS Type/Ver
2.1: Add IP Range
2.2: Add System Credentials
2.3: Load STIG Checklist
2.4: Run Scan(s) / receive reports for all machines hit in each scan

