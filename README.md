# Wazuh-SIEM-lab

## Objective

- The objective of this lab is to deploy and configure Wazuh, an open-source security monitoring platform, to enhance system security by 
  detecting vulnerabilities, identifying unauthorized processes, and monitoring changes to files and the Windows Registry. I will also 
  configure alearts and active responses for countermeasures to security-related attacks. This setup aims to provide real-time threat      detection and incident response capabilities, ensuring a robust security posture for systems under observation.

### Skills Learned

- Gain hands-on experience in installing and configuring Wazuh for effective security monitoring.
- How to identify and assess vulnerabilities in systems using Wazuh’s scanning capabilities.
- Understand how to monitor and manage unauthorized processes to maintain system integrity.
- Acquire skills in tracking changes to critical files and the Windows Registry to detect potential security threats.
- Configurations to alerts and active responses in Wazuh. 

### Tools Used

- Linode cloud computing provider
- Wazuh (Endpoint detection & response tool/SIEM)
- Ubuntu Linux VM -*Wazuh host*
- Windows 10 VM (VirtualBox) -*Wazuh agent*  
- Kali Linux VM (VirtualBox) -*Wazuh agent*


## Steps

- I will start this lab by heading over to Linode and creating the virtual machine that i will be hosting Wazuh on. I will select *Create* > * *Linode High performance SSD Linux servers*

   ![image](https://github.com/user-attachments/assets/b39e331a-30c2-4c7a-ad5c-bbf3c93e80a8)

- Linode makes it super simple to deploy VM's and also has a feature that lets you pre-install services inside of your VM in the creation process. To setup Wazuh in my VM, i will select *Market place* and search for Wazuh. I will select Wazuh and then scroll down for the configuration.

   ![image](https://github.com/user-attachments/assets/6ff980cf-d930-4427-9d7d-c149506f69fc)

- I will type in my email for the SSL certificate, a username for a limited sudo user account, a password, image(Ubuntu), the region i want to store this in, and finally, the Linode plan(CPU/memory). I am going to go with the 4GB to make things work smoothly.

   ![image](https://github.com/user-attachments/assets/c0530cf0-6c3b-4901-915b-fedce1a45f60)

- I will select create, and now its going to bake me a VM inside of the cloud.

   ![image](https://github.com/user-attachments/assets/d20f156f-3f03-4271-8622-868ca9d060b7)

- Once i see that it's running, i will copy the SSH Access key and paste it inside of my terminal, and then accept all fingerprints for the Key.

  ![image](https://github.com/user-attachments/assets/e9eb04e2-b423-4a82-ae1e-b44c2a088545)
  ![image](https://github.com/user-attachments/assets/2b10f71c-e038-48f3-babc-4aa6ad947e6b)

- I will add the sudo password.

   ![image](https://github.com/user-attachments/assets/260120eb-6740-48d0-85a6-74921ac1c85d)

- Once i am in, the installation may not quite be ready yet. To monitor that, i will type *htop* in the command line.

   ![image](https://github.com/user-attachments/assets/a25cb799-a503-4f3b-83fc-ed12dd13b29d)
   ![image](https://github.com/user-attachments/assets/22c2c13f-7147-487a-aef7-3085019dbd4c)

- After a few minutes, i should be all set. Once it stops, *CTL+C* to exit the process view. I will then type *ls -all* in the command line and navigate to the credentials by typing * cat /home/jakecyberlabs/.credentials* in order to log in to Wazuh.

   ![image](https://github.com/user-attachments/assets/f695a7e3-37c6-405b-a6ae-97b43727a5b6)

- Okay, now that i have the admin password, i will log in to Wazuh by heading back over to my Linode dashboard and scrolling down to the network config page and grabbing the reverse DNS and pasting it inside of my browser.

   ![image](https://github.com/user-attachments/assets/93f1c09a-c797-414c-9dd2-067d22b1db86)
   ![image](https://github.com/user-attachments/assets/6975cdd6-a82d-4e17-abf2-6eea4d91a6e9)

-I can now that admin password from the terminal and punch it in.

   ![image](https://github.com/user-attachments/assets/e6e292d1-5271-4b1d-b49c-ec574bd2053f)

- Here we go! successfull deployment of Wazuh inside of the Linode virtual machine. Now it is time to add some agents. The two agents i will be adding are the Kali linux and Windows 10 VM that i have here running inside of VirtualBox.

   ![image](https://github.com/user-attachments/assets/00f85fe4-7455-4ba8-93c2-41877823b955)

- I will do so by clicking on *Add agent* inside of the Wazuh dashboard.

   ![image](https://github.com/user-attachments/assets/03ef695f-e9fe-4d68-9cd2-fa1a466331ca)

- I will start with the Kali Linux VM. I will select the *DEB amd64*

- Where it says *Server address* this will simply be the address the agent uses to communicate with Wazuh. I will input that reverse DNS from the Wazuh server that i created.

    ![image](https://github.com/user-attachments/assets/ec94cc40-29b5-4fd7-bbef-2e09db0f2586)

- I will name the agent *Kali_Linux* and select the default group.
- I will then be given a generated command shown below to copy in the Kali terminal to install the agent.

   ![image](https://github.com/user-attachments/assets/39b408e2-977c-4c25-84a9-5a162152d396)
   ![image](https://github.com/user-attachments/assets/fcfd39e0-c9a2-4159-a988-8a2fda905d3a)

- I need enable this as a service. In the Wazuh dashboard, below the command to install the agent there will also be a command to start the agent. I will copy that and paste it into the Kali VM to start the agent.

   ![image](https://github.com/user-attachments/assets/ef2cc33a-0266-4e53-9507-ca2bfea37517)

- One more thing. Before i start the agent, I have to edit the config file so that it will be able to connect with the Wazuh server. I will open the *ossec.config* file in the terminal and add the ip address of the Wazuh server (AKA linode VM).

   ![image](https://github.com/user-attachments/assets/32794474-a09f-46f6-8b91-25f03655c66e)

- Now i can input the start service command into my terminal and fire it up. 

   ![image](https://github.com/user-attachments/assets/4060e6c7-a199-441b-a767-2841fbaa7294)

- I will head back to my Wazuh dashboard and select *agents* again so it will reload. 
- AHA! its there!

    ![image](https://github.com/user-attachments/assets/6574e8bf-db96-42e8-828c-a4a68a2828f1)

- Now it's time to add my Windows 10 VM as an agent.

- I will head back to the Wazuh Dashboard and select *Deploy new agent*

   ![image](https://github.com/user-attachments/assets/d81b00a7-58f1-410a-8e6c-b70b99529132)

- The process will be the same except i will select the *windows* deployment, set *Windos_10*, enter the Ip address of the Wazuh server, and it will generate me a code to input inside of Powershell.

   ![image](https://github.com/user-attachments/assets/58739007-9085-4595-9bb4-953afe8be304)
   ![image](https://github.com/user-attachments/assets/fbdc6b54-4dcb-4de0-8db9-ee03533b1a20)

- I will wait for the install to finish and then open the config file to add the Wazuh server ip for communication with the agent.

   ![image](https://github.com/user-attachments/assets/cb73ff41-7c36-42aa-be86-50c888b89fe4)

   
- I can now start the agent with this command that Wazuh provides.
  
    ![image](https://github.com/user-attachments/assets/a705a16d-da8d-4512-aa1c-ce135b3064f9)
    ![image](https://github.com/user-attachments/assets/4892c555-5817-429c-99f7-8646e1846282)

- I will head back over to the Wazuh Agent dashboard to check for the Windows VM to establish connection.

   ![image](https://github.com/user-attachments/assets/7d917395-80dd-493e-9b0e-6e447932b7ff)

- NICE! both VMs are now active agents inside of Wazuh. Now i will click on the Kali Linux VM and see what is going on.

   ![image](https://github.com/user-attachments/assets/64a1cf11-480e-4a6b-9cf3-91e4c2d42714)

- Here is my agent dashboard for this one machine. Notice that there is a lot going on here. So much to appreciate!! I will start by pointing out the MITRE visual in the bottom left corner of the screenshot. This is SUPER resourceful. This function in Wazuh integrates the MITRE & ATT&CK framework into the Wazuh security monitoring solution and maps detected security events/alerts to the techniques and tactics defined in the framework. This enhances the understanding of potential threats and their behaviors. Note that this VM is new install so there are not too many threats detected but it provides valuable insight as to OS hardening reccomendations. Lets check out what more we can see here.

- I can scroll down to see the secure configuration assessment(SCA). 

   ![image](https://github.com/user-attachments/assets/95844a18-4876-4a5f-9b03-7d5e7f86535b)

- I will click on *System audit for Unix based systems*

   ![image](https://github.com/user-attachments/assets/5bcce60a-4be6-45d9-959b-d50f2fac4946)
   ![image](https://github.com/user-attachments/assets/a5ec828e-6d32-45d9-a22c-b511f29029df)
   ![image](https://github.com/user-attachments/assets/ccbfd03e-7e59-4f1e-b5e1-f3c368f5eb4d)

- This feature in Wazuh is designed to evaluate and ensure that systems comply with the specified security configurations and best practices. It can help users and organizations assess the security posture of their infrastructure by checking configurations against predefined rules and standards, such as CIS benchmarks or custom policies. It also works seamlessly with other Wazuh features, such as log analysis and intrustion detection, to provide a comprehensive overview. Here, i can see that the SCA hass a score of 18%(not good) as as fresh install.

- You are able to use the search function drill down to the specifics. I will type in *ssh* and see what comes up.

    ![image](https://github.com/user-attachments/assets/3e721d0f-f800-494a-907c-b6cc08bba4ce)

- As shown below, I will click on the first security check *SSH Hardening: Port should not be 22* and it gives you the rationale, remediation, description, check, and Compliance information. This is very cool. Not only will it show what has failed the security check, but it will explain why, how to fix it, and how to be compliant with certain industry standards. It also shows all the MITRE techniques that can be used by having a certain misconfiguration!

   ![image](https://github.com/user-attachments/assets/c9081f90-5e76-49cf-b604-53ebad36941e)

- I can do the same with Windows by repeating the previous steps.

   ![image](https://github.com/user-attachments/assets/1f0b49cc-f7f4-4768-8de0-349280db5ff5)

- As you can see, there are also hardening techniques and remidiations for Windows.

- There is much more to discover! I will head to *security events* and see that i could look at a more holistic view such as total alerts, authentication success/failures, charts top 5 alerts, top 5 rule groups, top 5 compliance requirements, graphs for timeline monitoring and a list of all the security events at the bottom.

   ![image](https://github.com/user-attachments/assets/a6477052-1101-4efa-adae-8054e1cc78ef)

- If i want to monitor all agents at once, i can do so by unpinning the agent at the top of the dashboard.

   ![image](https://github.com/user-attachments/assets/05723296-78a1-4bb3-9df5-c2a483be3687)
   ![image](https://github.com/user-attachments/assets/13e2889d-4793-449b-8f5f-50823d4b0848)

- Here above, i can see 1601 authentication failures. I purposely left the firewall off for the Linode VM to demonstrate the insights one can gain from this situation. If i scroll down to the list of security alerts and click on the most recent, the first one is an *attempt to login using a non-existent user*

   ![image](https://github.com/user-attachments/assets/7469cf06-f0ca-4880-9f77-4b02f1a3c2a7)
   ![image](https://github.com/user-attachments/assets/05ec1153-8c7c-44a6-a6c9-96e746c8e94e)



- The security event shows that someone in the U.K. is attempting to brute force into the machine via SSH. The alert has the MITRE attack Tactics and techniques used by the attacker. It also reveals geolocation, timestamps, incompliance with pcidss. 



- Wazuh also has File integrity monitoring. This is very powerful for Windows. The default scan time is 24 hours, and since i just set this machine up, i will have to configure the timing on the scans to view results. I wll head to the config file. The same one i had to for the IP address. I will hit *CTL+F* and type in *syscheck* to find the file integrity monitoring.

  ![image](https://github.com/user-attachments/assets/59d9e566-dad6-4fef-8a79-1a7f365c3594)
  ![image](https://github.com/user-attachments/assets/8d7f922b-5a56-4ecf-9de4-e4f17daabc86)
  ![image](https://github.com/user-attachments/assets/7c0c6b6c-e740-4d04-9917-8c92d3a8acdf)

- In the congig file i will type *<directories realtime="yes" report_changes="yes" check_all="yes">* and specify which directory i will want to monitor with those options by closing the statement out with *C:\Users\Jacob\Desktop</directories>* . I can now save the file.

  ![image](https://github.com/user-attachments/assets/e0b45743-9483-40d4-9f7e-d8194ae8d99d)
  ![image](https://github.com/user-attachments/assets/6701ea48-3be8-4054-802f-463cddc3b2e5)

- I will restart the restart service once the config file is saved.

   ![image](https://github.com/user-attachments/assets/e1db8437-e38f-44a6-bb02-30162f733568)

- To show how powerful this tool is, i am going to change something on my desktop now. I will create a file on the desktop

   ![image](https://github.com/user-attachments/assets/74c763a5-6996-451b-bd9a-e661a8ee9c56)

- I will head back over to the Dashboard and click on *events* > *refresh* 

   ![image](https://github.com/user-attachments/assets/065e8ffd-a77f-47cd-8b99-d08cad1c2098)

-Here it is! it shows that i just created the file, deleted the file, and added the file.(I named the file in realtime).


- I can edit the file, save, and it now shows that i modified the file(the checksum has changed).

   ![image](https://github.com/user-attachments/assets/42daac18-8eca-492b-8d7b-8e95ac77464c)
   ![image](https://github.com/user-attachments/assets/493eb3e0-acbe-4790-8140-9642e4d38855)

- It will even tell me what i added in the file.

    ![image](https://github.com/user-attachments/assets/a3988847-ba5c-43cf-b720-bff7465c6836)

- You can also monitotor windows regristry. It will not be in realtime though. I will have to change the intervals of the scan to check for changes in the registry values.

-  Wazuh does have an inventory of registry keys that it automatically monitors, but we can add specific ones that i want to have seen. I will add a custom one to it. To do that, i will open up reg edit.

   ![image](https://github.com/user-attachments/assets/9ffddfd3-e73a-456f-9639-9be0c5df3187)
   

- I will add a new key to observe by navigating to local machine > SOFTWARE> classes> new key and mame it *NEW KEY*, open it and name it *something fun*.

     ![image](https://github.com/user-attachments/assets/fd6929fa-5c91-4157-adc5-7c65625bb43c)
     ![image](https://github.com/user-attachments/assets/24678afa-10f7-4f52-8627-8e083a12aefa)

  - Right click and copy key name.
    
      ![image](https://github.com/user-attachments/assets/984dc069-9ba1-4b15-a59e-df1558587495)

- I will head back to configuration and underneath the last registry entry (above registry entries to ignore) and type in *<windows_registry and paste my key in after. Close it up with </windows_registry> 

   ![image](https://github.com/user-attachments/assets/06f23f91-ad53-47ec-bedc-86f0a6ef201a)

- Head up to where it says frequency that syscheck is enabled
  
   ![image](https://github.com/user-attachments/assets/baf9d063-bfe6-490a-8829-c792394bd852)

- Change it from every 12 hours to 30 seconds

   ![image](https://github.com/user-attachments/assets/33ca855e-1c11-4b2d-92a0-156e01b3cc6c)

- I will save the file and go restart the Wazuh service.

   ![image](https://github.com/user-attachments/assets/aebb8869-f4fe-40b3-8ad6-9b87a46da07c)

- Back in the dashboard i will select File integrity monitoring and go to inventory > Windows Registry and there it is!.

   ![image](https://github.com/user-attachments/assets/d03cfcd3-fc7d-45bb-98c3-a7da52cb6923)


- Above, it shows that the registry has been modified with the new key to observe. I will go and delete the key and see what happens by going to the events.

   ![image](https://github.com/user-attachments/assets/5d668e4f-41a8-4a4d-b4c9-66e8bf9c8b22)

- Monitoring the windows registry is useful because Windows has the configuration settings for your machine. If things are changed here, things are changing everywhere else. Things to look out for in the windows registry is maleware. What maleware will do is add keys to your run folfer. *HKEY_LOCAL_MACHINE\SOFTWARE\Classes\Microsoft\Windows\CurrentVersion\run*. These keys are changed when you add a new startup application for your system. This is how you monitor that. I will go and add a new key to that folder.

   ![image](https://github.com/user-attachments/assets/88ac312c-7fca-42c7-b5d0-38896c873aba)

- I'll call this *badstuff* and then for the key string value I will add the file path *"C:\Program Files (x86)\Internet Explorer\iexplore.exe"* 

    ![image](https://github.com/user-attachments/assets/bb490701-2ea9-4903-900d-7a7961f00008)
    ![image](https://github.com/user-attachments/assets/7e80779d-296b-4715-bf6c-e6f46c9bdbcb)

- Now i will head back over to the dashboard and check the events in the file integrity monitoring.

   ![image](https://github.com/user-attachments/assets/b8fe6739-dee7-4617-b78a-1492a99ad84a)

- Here it is! i will open it up and it will show what has been modified(badstuff)

   ![image](https://github.com/user-attachments/assets/0de09912-c93b-4508-8a03-474d27773362)

- Something else cool in Wazuh are Active responses. This is also super powerful. The "Acctive response" feature can setup automated responses that trigger when certain alerts are generated. This helps in quickly mitigating threats without manual intervention. Here is how i will do that.

- I will demonstate this by creating a response that will trigger when failed authentication attempts happen. In my Kali VM, i will start generating a failed login attempt via SSH. Here we can see that i am getting a password prompt. I am not blocked from logging in via SSH.

   ![image](https://github.com/user-attachments/assets/73e88ec5-bf5f-4572-9631-c99561ef4e16)

- I will go back to my Wazuh dashboard to see this in *Security events* and here it will show the agent name(kali VM) and the username(hacker) that i tried to login with.

   ![image](https://github.com/user-attachments/assets/4a266b28-47cd-4fc8-8f55-6e8ba59a9fae)


- It also shows the MITRE tactic and technique used along with associated timestamps. 
  
   ![image](https://github.com/user-attachments/assets/87c2d7d7-1a3b-402d-b523-7ef540691d4d)

- Now, i will configure Wazuh actively respond to failed SSH login attempts and add it to the block list. I will do so by heading back to the dashboard in Wazuh server and head to the configuration settings and click on *Edit configuration*

   ![image](https://github.com/user-attachments/assets/fde96638-5b10-49d1-bf10-f7fa395efad8)
   ![image](https://github.com/user-attachments/assets/b6e222ec-3fe3-4032-95ef-a1e1b11c00d2)

- I will scroll down to where it says *active response* It will show you the format for the active response so you would essentially just match that with your configuration. I will do that and add the rule "5710" since that was the *rule ID* number that showed when we failed to authenticate via SSH. You can also set the duration for the response. I will set this to 180 seconds.

   ![image](https://github.com/user-attachments/assets/02795aed-f2d7-4b4a-bf2f-d7e95769b994)

- Click on *Save* and *Restart Manager*

   ![image](https://github.com/user-attachments/assets/41e97815-4f31-4b47-9b36-2cbf9114b095)

- Now i will head back over to the Kali machine and try to login via SSH again.

   ![image](https://github.com/user-attachments/assets/22caf5e1-7392-4844-a2a5-a410bc88ed70)

- I will head back over to the security events page for the Kali VM in my Wazuh dashboard and notice how the firewall has blocked it with the active-response feature.

   ![image](https://github.com/user-attachments/assets/e3fe2aea-02fe-40b3-aba4-83653661bc5e)

- I am BLOCKED. I cannot even ping this machine for 180s. How cool is that! These active responses are so powerful. You can configure these responses based on a variety of rules. It is so custom. You could even do it based on a certain command that was ran or a certain log came about in the system. 

  


   


  


  

  
  


   





   




   

    



   

  







  
 
     
     




  
   


   

   






    

  




  



  

  

    





  




  







   
