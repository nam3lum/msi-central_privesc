# CVE-2022-38532

### Local privilege escalation in MSI Center desktop application.

![CVE-2022-38532](https://github.com/nam3lum/msi-central_privesc/raw/main/media/MSI%20Center.png)

The vulnerability exist in "C_Features" of MSI.CentralServer.exe. MSI.CentralServer.exe is an application that gathers information about your system, it collaborates with MSI.TerminalServer.exe. The ExecuteTask function which we can call it in "CMD_AutoUpdateSDK" gives us a chance to run an exectable with custom parameters under Administrative privileges. You can see the related port only from localhost. 

![Vulnerable process & port](https://github.com/nam3lum/msi-central_privesc/raw/main/media/MSI.CS-ps.jpg)

#### The vulnerability
You can easily disassemble the MSI.CentralServer.exe using any .NET disassembler. Central Server itself listens on 32682 port from localhost, we can find the source code of the handler in "C_Features". Just look at the CMD_AutoUpdateSDK feature to see the vulnerability. We abuse this feature (it is automatic updater of MSI Center). It receives the user-given payload, splits it into multiple parts to execute the command with custom parameters.
![Vulnerable feature](https://github.com/nam3lum/msi-central_privesc/raw/main/media/Vulnerable%20function.png)

This is main function which our feature uses it to execute given PE with custom arguments:
![Main function](https://github.com/nam3lum/msi-central_privesc/raw/main/media/Main%20function.png)

### The port which MSI Central Server listens is updated in 1.0.59.0 version. It is 32683.

#### POC
You can generate your own payload, hex it and run the script in the local computer. The POC creates hacker user with "hacker123" password and adds it to the Administrators group.

**Proof-of-Concept video:**
https://user-images.githubusercontent.com/64528432/188067866-f30fe089-db76-4cc0-81ce-f74871769b33.mp4
