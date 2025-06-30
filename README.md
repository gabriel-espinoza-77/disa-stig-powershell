Applying STIGs with PowerShell

I will be using Tenable to scan a VM and remediate failed STIGs that become visible from the Scan. This is used to show my skills with Tenable and to help anyone trying to remeidate any failed STIGs on their endpoints.

I will be using the `io-test-vm` VM as the device for this demonstration. We intentially have made this VM vulnerable to the internet as we have disabled the Windoows Firewall. 

Once you have the device ready for scan, go to the Tenable Vulnerability Management site, click the "Scans" navigation item, CLick create scan at the top right, and then press `Advanced Network Scan`.
<p align="center">
  <img src="https://github.com/user-attachments/assets/c33b28c0-687d-4763-ab3c-bd71de8db2f0" alt="" width="500"/>
  <img src="https://github.com/user-attachments/assets/d20755d9-7c55-43ff-9f03-f4c1f4732a0d" alt="" width="500"/>
  <img src="https://github.com/user-attachments/assets/9e37b1a8-21c9-4bba-83e4-054f9dce86da" alt="" width="500"/>
</p>

Fill out the necessary information like the Name of the scan, Scanner Type, Scanner, and Targets (I have put the private IP address of the VM). 

<p align="center">
  <img src="https://github.com/user-attachments/assets/4547ab3b-47d8-4f40-8390-2ca1ac24019e" alt="" width="750"/>
</p>

Next go to the Credentials section, press Add Credentials, Expand Host option and press Windows (This is only applicable if the machine you're working with has a Windows OS). Authentication method will be "Password", then Fill out the Username and Password information, this will be the credentials used for the machine that will be scanned. Then check everything under "Scan-wide Credential Type Settings". Save the credentials.

<p align="center">
  <img src="https://github.com/user-attachments/assets/11919a6a-89ff-4087-9bbc-4ef74ecb86a6" alt="" width="500"/>
  <img src="https://github.com/user-attachments/assets/9147bf50-bf1d-402c-baf5-25d0e33ba1d9" alt="" width="500"/>
  <img src="https://github.com/user-attachments/assets/b15c1894-7c91-44f7-8610-e7bc59113f98" alt="" width="500"/>
</p>

Now for the Compliance section press Add Compliance Audits, in the search tab do "Windows 10 STIG" and choose the v3r4. Th next page shows the parameters you could have in your environment but we wont touch that so leave it for now and press Save.

<p align="center">
  <img src="https://github.com/user-attachments/assets/71323716-f503-4c6e-ae1b-08de36024766" alt="" width="500"/>
  <img src="https://github.com/user-attachments/assets/9475498a-b9fd-45dc-baed-42a706166843" alt="" width="500"/>
  <img src="https://github.com/user-attachments/assets/7054b043-6983-4390-80c3-c8a921c49407" alt="" width="500"/>
</p>

For this scan we will be disabling ALL plugins except Policy Compliance, only for the purpose of making this scan fast. Click on Policy Compliance and within Policy Compliance, enable only “Windows Compliance Checks”. Now the Policy COmpliance plugin will have a "Mixed" status. Then click Save. Now the scan will only check Windows Compliance Checks

<p align="center">
  <img src="https://github.com/user-attachments/assets/85bca885-6ef5-43e7-9077-2ab22379b0be" alt="" width="1000"/>
  <img src="https://github.com/user-attachments/assets/66569bd3-3e06-4c43-9b5b-da7921cbd4d9" alt="" width="500"/>
  <img src="https://github.com/user-attachments/assets/331dc972-8e87-4732-8600-f0647eee1315" alt="" width="500"/>
</p>

Go back to the Scan page, and launch the scan we just created. Make sure your device is on while the scan is going. Go to the page of the scan you created, go to the audits tab and you can see the the security configurations that either passed, failed or have a warning. For this lab we will only be working on one of these security configurations which is "WN10-AU-000500". To find a fix, we can use Google for help in addition to the description of the failed STIG.

<p align="center">
  <img src="https://github.com/user-attachments/assets/420a151b-04ce-49cf-9d97-f566f3dc7716" alt="" width="500"/>
  <img src="https://github.com/user-attachments/assets/765ee59e-e43c-4bcf-9eb4-0fb5d07dd64a" alt="" width="1000"/>
  <img src="https://github.com/user-attachments/assets/4603d299-0cb5-42b3-8fb3-99206bc4770f" alt="" width="500"/>
  <img src="" alt="" width="500"/>
  <img src="" alt="" width="500"/>
</p>

If we use the `stigaview` website, it says why this STIG exists, it has a section on how to check if the configurations are fixed for the STIG, and how to fix it. For this STIG were working on, we want to increase the capacity of the application event logs on this device to hold more logs so the fix could be recognized by tenable.

<p align="center">
  <img src="https://github.com/user-attachments/assets/70c1eeb5-f15d-4a20-83d5-3feb9cd65e03" alt="" width="1000"/>
  <img src="" alt="" width="500"/>
  <img src="" alt="" width="500"/>
</p>

First, were going to check if the registry value needed to these event logs exists. Open run view, search "regedit", the Registry Editor application will open. We want to navigate through `HKEY_LOCAL_MACHINE` until you get here `\SOFTWARE\Policies\Microsoft\Windows`. If you see the "EventLog" directory nothing needs to be done here. But on my machine there seems to be no EventLog directory so i will create one.

<p align="center">
  <img src="https://github.com/user-attachments/assets/d8ff589c-180e-4b18-9262-2a24feb72e2e" alt="" width="500"/>
  <img src="https://github.com/user-attachments/assets/69cf6930-d7ad-4f3b-9eb0-1ad2e3edffab" alt="" width="500"/>
  <img src="https://github.com/user-attachments/assets/ee36da23-60f5-49a7-8d23-5062774eacf9" alt="" width="500"/>
</p>

Now create a new key named "EventLog", and inside the EventLog create a new key called "Application", and inside Application create a DWORD value called MaxSize. Now for the MaxSize value we will give it a decimal value of 32768. Now the applications logs size is the size that it should be. The STIG has now manuelly been remediated.

<p align="center">
  <img src="https://github.com/user-attachments/assets/7247c49a-dda3-46a3-8170-80d08c81e615" alt="" width="500"/>
  <img src="https://github.com/user-attachments/assets/5065160d-e2b4-424c-ac83-0c7dbc2562ac" alt="" width="500"/>
  <img src="https://github.com/user-attachments/assets/1792e852-5a42-434a-9829-7d397168abc2" alt="" width="500"/>
  <img src="https://github.com/user-attachments/assets/76b95b11-eea9-48c6-9aa8-5dfdd65d502d" alt="" width="500"/>
  <img src="https://github.com/user-attachments/assets/65182428-487f-4892-b636-e308095b7926" alt="" width="500"/>
</p>

<!--
The description of the STIG says "The Application event log size must be configured to 32768 KB or greater." So now that we know the fix using the `stigaview` site, we get on the device were working on, open the Run view, and type "eventvwr.msc" to open up the event viewer.Now we expland Windows Logs, right click on Application, press properties and adjust the Maximum log size 

<p align="center">
  <img src="https://github.com/user-attachments/assets/7bbbec45-bcfb-44aa-8bd1-cbe0b3f50e0d" alt="" width="500"/>
  <img src="https://github.com/user-attachments/assets/6ec594f7-ab4e-481b-bbe0-c90b03587d7a" alt="" width="500"/>
</p>
-->

To verify the remediation of the WN10-AU-000500 STIG, scan the device again using Tenable. As you can see, the STIG has passed after manuelly remediating it.

<p align="center">
  <img src="https://github.com/user-attachments/assets/c766e0b7-de07-483a-8562-6799eecaf6b8" alt="" width="500"/>
</p>

Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\EventLog\Application]
"MaxSize"=dword:00008000

Now we want to implement the fix using PowerShell. Undo the manuel remediation by deleting the EventLog directory we created in Registry Editor. Im going to rescan the device to show that the STIG is failing again.

<p align="center">
  <img src="https://github.com/user-attachments/assets/ea2a42db-1c4e-4f24-bd1e-a598da9b7773" alt="" width="500"/>
  <img src="" alt="" width="500"/>
  <img src="" alt="" width="500"/>
  <img src="" alt="" width="500"/>
</p>



<p align="center">
  <img src="" alt="" width="500"/>
  <img src="" alt="" width="500"/>
  <img src="" alt="" width="500"/>
  <img src="" alt="" width="500"/>
</p>

<p align="center">
  <img src="" alt="" width="500"/>
  <img src="" alt="" width="500"/>
  <img src="" alt="" width="500"/>
  <img src="" alt="" width="500"/>
</p>

<p align="center">
  <img src="" alt="" width="500"/>
  <img src="" alt="" width="500"/>
  <img src="" alt="" width="500"/>
  <img src="" alt="" width="500"/>
</p>

<p align="center">
  <img src="" alt="" width="500"/>
  <img src="" alt="" width="500"/>
  <img src="" alt="" width="500"/>
  <img src="" alt="" width="500"/>
</p>
