# Privilege-escalation-ASKEY-Router-RTF3505VW-N1

CVE-2022-47040

Privilege escalation vulnerability on ASKEY routers

Device: ASKEY RTF3505VW-N1

Firmware: BR_SV_g000_R3505VMN1001_s32_7 (not tested in other version)

CLI Version: Reduced_CLI_HGU_v13


Exploit:

ASKEY RTF3505VW-N1 devices are provided with access through ssh into a restricted default shell:

![image](https://user-images.githubusercontent.com/90664730/206090394-c52e705c-d442-41d3-9188-fd7661f552df.png)

The restricted shell has access to a "Reduced_CLI”, and the environment is restricted to avoid execution of most linux/unix commands.

![image](https://user-images.githubusercontent.com/90664730/206090510-1b509d9b-81c6-4537-a7ea-9a5a482f2026.png)

The command “tcpdump” is present in the restricted shell and do not handle correctly the -z flag, so it can be used to escalate privileges through the creation of a local file in the /tmp directory of the router, and injecting packets through port 80 (used for the router's Web GUI) with the string ";/bin/bash" in order to be executed by "-z sh". By using “;/bin/bash” as injected string we can spawn a busybox/ash console.

As seen on the next images, we set a listen "nc" on port 4444, and run a Bash/Expect script with the exploit:

![image](https://user-images.githubusercontent.com/90664730/206090862-0c60a484-1ac4-4a50-a7cb-7e36e6eb0d9a.png)

The reverse shell is created in order of get a stable connection with the router:
![image](https://user-images.githubusercontent.com/90664730/206090678-ed4c8bed-5eb4-4b45-a9e3-3be4e07eb886.png)

So it is possible to escalate privileges by spawning a full interoperable console with root privileges (see next image):

![image](https://user-images.githubusercontent.com/90664730/206091157-2e02ba89-4b9f-4c58-85dd-0f77a2b54b8a.png)

Through this escalation we can change the content of /etc/passwd (/var/passwd), create new users, access restricted data/files, or change any other system resource permanently.

The user “support” is provided printed on the back of the router. In some cases, this routers use default credentials.
