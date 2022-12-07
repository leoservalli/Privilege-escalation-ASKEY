# Privilege-escalation-ASKEY-Router-RTF3505VW-N1


Privilege escalation vulnerability on ASKEY routers

Device: Mitrastar RTF3505VW-N1

Firmware: BR_g3.5_100VNZ0b33 (not tested in other version)

Exploit:

Mitrastar RTF3505VW-N1 devices are provided with access through ssh into a restricted default shell:

image

The restricted shell has CLI Version “Reduced_CLI”, and the environment is restricted to avoid execution of most linux/unix commands.

image

The command “tcpdump” present in the restricted shell do not handle correctly the -z flag, so it can be used to escalate privileges through the creation a local file in the /tmp directory on the router, and injecting packets through port 80 (used for the router's Web GUI) with the string ";/bin/bash" in order to be executed. By using “;/bin/bash” as injected string we can spawn a busybox/ash console, as seen on the next image:

image

So it is possible to escalate privileges by spawning a full interoperable console with root privileges (see next image):

image

Through this escalation we can change the content of /etc/passwd (/var/passwd), create new users, or change any other system resource permanently.

The user “support” is provided printed on the back of the router. In some cases, this routers use default credentials.
