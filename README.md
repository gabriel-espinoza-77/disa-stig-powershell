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

Next go to the Credentials section, press Add Credentials, Expand Host option and press Windows

<p align="center">
  <img src="https://github.com/user-attachments/assets/11919a6a-89ff-4087-9bbc-4ef74ecb86a6" alt="" width="500"/>
</p>

<p align="center">
  <img src="https://github.com/user-attachments/assets/9147bf50-bf1d-402c-baf5-25d0e33ba1d9" alt="" width="500"/>
</p>

<p align="center">
  <img src="" alt="" width="500"/>
</p>

<p align="center">
  <img src="" alt="" width="500"/>
</p>

<p align="center">
  <img src="" alt="" width="500"/>
</p>


