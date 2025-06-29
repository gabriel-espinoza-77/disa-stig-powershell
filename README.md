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

Go back to the Scan page, and launch the scan we just created. Make sure your device is on while the scan is going. Go to the page of the scan you created, 

<p align="center">
  <img src="https://github.com/user-attachments/assets/420a151b-04ce-49cf-9d97-f566f3dc7716" alt="" width="500"/>
  <img src="https://github.com/user-attachments/assets/765ee59e-e43c-4bcf-9eb4-0fb5d07dd64a" alt="" width="500"/>
  <img src="" alt="" width="500"/>
  <img src="" alt="" width="500"/>
  <img src="" alt="" width="500"/>
</p>

<p align="center">
  <img src="" alt="" width="500"/>
  <img src="" alt="" width="500"/>
  <img src="" alt="" width="500"/>
</p>

<p align="center">
  <img src="" alt="" width="500"/>
  <img src="" alt="" width="500"/>
  <img src="" alt="" width="500"/>
</p>

<p align="center">
  <img src="" alt="" width="500"/>
  <img src="" alt="" width="500"/>
  <img src="" alt="" width="500"/>
</p>
