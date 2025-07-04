# STIG Remediation with Tenable

This project demonstrates how STIG (Security Technical Implementation Guide) findings identified through Tenable scans can be manually and automatically remediated on a Windows virtual machine. The process displays practical skills in Tenable vulnerability scanning, STIG understanding, manual remediation, and scripted automation. It is ideal to perform such tasks to support system hardening and minimize the attack surface for Window environments.

For this demonstration, a virtual machine named `io-test-vm` running on Windows 10 was intentially configured to simulate a vulnerable state by disabling its Windows Firewall. Once prepared, the machine was scanned using **Tenable Vulnerability Management**.

---

## Tenable Scan Creation

To initiate the scan, an **Advanced Network Scan** was created within the Tenable portal.

<p align="center">
  <img src="https://github.com/user-attachments/assets/c33b28c0-687d-4763-ab3c-bd71de8db2f0" alt="Advanced Network Scan - Step 1" width="400"/>
  <img src="https://github.com/user-attachments/assets/d20755d9-7c55-43ff-9f03-f4c1f4732a0d" alt="Advanced Network Scan - Step 2" width="200"/>
  <img src="https://github.com/user-attachments/assets/9e37b1a8-21c9-4bba-83e4-054f9dce86da" alt="Advanced Network Scan - Step 3" width="450"/>
</p>

---

## Configurating the Scan

With the scan created, key fields such as scan name, scanner, and target IP were configured. The scan targeted the private IP address of the virtual machine `io-test-vm`.

<p align="center">
  <img src="https://github.com/user-attachments/assets/4547ab3b-47d8-4f40-8390-2ca1ac24019e" alt="Scan Target Configuration" width="750"/>
</p>

For full compliance auditing, authentication credentials were added. Under **Host > Windows**, credentials for the Windows account were supplied and all relevant settings under *Scan-wide Credential Type Settings* were enabled.

<p align="center">
  <img src="https://github.com/user-attachments/assets/11919a6a-89ff-4087-9bbc-4ef74ecb86a6" alt="Credential Setup 1" width="400"/>
  <img src="https://github.com/user-attachments/assets/9147bf50-bf1d-402c-baf5-25d0e33ba1d9" alt="Credential Setup 2" width="500"/>
  <img src="https://github.com/user-attachments/assets/b15c1894-7c91-44f7-8610-e7bc59113f98" alt="Credential Setup 3" width="500"/>
</p>

The **Compliance** section was configured next by selecting the `Windows 10 STIG v3r4` audit version.

<p align="center">
  <img src="https://github.com/user-attachments/assets/71323716-f503-4c6e-ae1b-08de36024766" alt="Compliance Audit Search" width="350"/>
  <img src="https://github.com/user-attachments/assets/9475498a-b9fd-45dc-baed-42a706166843" alt="Compliance Parameters" width="450"/>
  <img src="https://github.com/user-attachments/assets/7054b043-6983-4390-80c3-c8a921c49407" alt="Compliance Audit Save" width="250"/>
</p>

To focus exclusively on policy compliance, all plugin categories were disabled except **Policy Compliance**, and within that, only *Windows Compliance Checks* were enabled.

<p align="center">
  <img src="https://github.com/user-attachments/assets/85bca885-6ef5-43e7-9077-2ab22379b0be" alt="Plugin Configuration Overview" width="1000"/>
  <img src="https://github.com/user-attachments/assets/66569bd3-3e06-4c43-9b5b-da7921cbd4d9" alt="Policy Plugin Enabled" width="300"/>
  <img src="https://github.com/user-attachments/assets/331dc972-8e87-4732-8600-f0647eee1315" alt="Plugin Mixed Status" width="300"/>
</p>

---

## Investigating and Remediating Failed STIG `WN10-AU-000500`

Upon scan completion, the **Audit** tab within the scan details revealed many failed compliance checks. One particular finding, `WN10-AU-000500`, pertains to the size configuration of the Application Event Log and will serve as the primary focus in this remediation.

<p align="center">
  <img src="https://github.com/user-attachments/assets/420a151b-04ce-49cf-9d97-f566f3dc7716" alt="Audit Failure - WN10-AU-000500" width="500"/>
  <img src="https://github.com/user-attachments/assets/765ee59e-e43c-4bcf-9eb4-0fb5d07dd64a" alt="Failed Control Details" width="1000"/>
  <img src="https://github.com/user-attachments/assets/4603d299-0cb5-42b3-8fb3-99206bc4770f" alt="Audit Severity Breakdown" width="500"/>
</p>

The STIG description recommends that the Application event log size must be configured to **32,768 KB or greater**. This setting can be managed through the Windows Registry Editor.

<p align="center">
  <img src="https://github.com/user-attachments/assets/70c1eeb5-f15d-4a20-83d5-3feb9cd65e03" alt="STIG Viewer - WN10-AU-000500 Explanation" width="1000"/>
</p>

Navigating to `HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows`in the Registry Editor, it was observed that the `EventLog\Application` subkey did not exist. Manual creation of the subkey was required, along with configuring the `MaxSize` DWORD value to `32768` (decimal).

<p align="center">
  <img src="https://github.com/user-attachments/assets/d8ff589c-180e-4b18-9262-2a24feb72e2e" alt="Registry Key Navigation" width="500"/>
  <img src="https://github.com/user-attachments/assets/7247c49a-dda3-46a3-8170-80d08c81e615" alt="Creating EventLog and MaxSize" width="500"/>
  <img src="https://github.com/user-attachments/assets/1792e852-5a42-434a-9829-7d397168abc2" alt="Registry Value Set" width="500"/>
  <img src="https://github.com/user-attachments/assets/72eb6b27-2349-446d-892e-943e153ea64d" alt="" width="300"/>
</p>

After this manual remediation, the scan was re-run and confirmed the STIG had passed.

<p align="center">
  <img src="https://github.com/user-attachments/assets/c766e0b7-de07-483a-8562-6799eecaf6b8" alt="STIG Passed after Registry Fix" width="500"/>
</p>

---

## Automating the Fix with PowerShell

The manual remediation was undone and the scan results once again showed a failed STIG status.

<p align="center">
  <img src="https://github.com/user-attachments/assets/ea2a42db-1c4e-4f24-bd1e-a598da9b7773" alt="STIG Failed after Reset" width="500"/>
</p>

To demonstrate automation, the registry key was programmatically recreated using PowerShell. To replicate the correct configuration, the `Application` registry key was exported and converted into a scriptable format using ChatGPT.

Below is the exported data from the registry key:
> ```
> Windows Registry Editor Version 5.00
> 
> [HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\EventLog\Application]
> "MaxSize"=dword:00008000
> ```

<p align="center">
  <img src="https://github.com/user-attachments/assets/c3b49cd1-37c0-4d22-a440-0acd48e5f26b" alt="Exporting Registry Key" width="250"/>
  <img src="https://github.com/user-attachments/assets/f25a7ebf-bcb7-4d4f-8941-559d3f5a334c" alt="Preparing for Automation" width="500"/>
</p>

The script was executed in **PowerShell ISE** running in Administrator mode. Upon verification, the registry keys were correctly created and populated.

<p align="center">
  <img src="https://github.com/user-attachments/assets/c9c4ad29-5e82-469e-af0e-dddb7cc0424f" alt="PowerShell Execution Success" width="700"/>
  <img src="https://github.com/user-attachments/assets/cdcc0ae9-13c5-4103-aaa5-20af02beb5e7" alt="Registry Key Recreated via Script" width="200"/>
</p>

A final scan was run to validate that the PowerShell-based remediation resolved the STIG, confirming the setting was properly enforced across the system.

<p align="center">
  <img src="https://github.com/user-attachments/assets/b6d5672e-3514-4610-b684-ee830e08495d" alt="Final Scan - STIG Passed via Script" width="250"/>
  <img src="https://github.com/user-attachments/assets/77799e3d-0cdc-4bf1-9e7b-2a047449ced7" alt="" width="500"/>
  <img src="https://github.com/user-attachments/assets/de17c7ba-9e37-42b7-8765-45395abb8757" alt="Audit Output - STIG Resolved" width="200"/>
</p>

---

## Conclusion

This project highlights how DISA STIG findings on Windows systems can be remediated both manually and through automation using available tools. Tenable was crucial in identifying compliance weaknesses, allowing targeted configuration changes through manual edits or PowerShell. This ensures Windows systems are configured in accordance with security best practices and remain compliant to DISA STIG standards.

---

**Author:** Gabriel Espinoza


























