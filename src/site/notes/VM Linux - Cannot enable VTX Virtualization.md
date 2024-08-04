---
{"dg-publish":true,"permalink":"/vm-linux-cannot-enable-vtx-virtualization/","tags":["gardenEntry"]}
---


#gns3 #vm #linux #windows #vtx 
```
C:\Windows\System32>bcdedit /enum {current}

Windows Boot Loader
-------------------
identifier              {current}
device                  partition=C:
path                    \Windows\system32\winload.efi
description             Windows 11
locale                  en-US
inherit                 {bootloadersettings}
recoverysequence        {6c4f4480-4834-11ef-b4f4-ccc5de64ca9f}
displaymessageoverride  Recovery
recoveryenabled         Yes
isolatedcontext         Yes
allowedinmemorysettings 0x15000075
osdevice                partition=C:
systemroot              \Windows
resumeobject            {f52e62f8-4833-11ef-b4f4-ccc5de64ca9f}
nx                      OptIn
bootmenupolicy          Standard
bootstatuspolicy        DisplayAllFailures

C:\Windows\System32>bcdedit /set hypervisorlaunchtype off
The operation completed successfully.

C:\Windows\System32>bcdedit /set hypervisorlaunchtype off

then reboot
```

![Pasted image 20240801125719.png](/img/user/Attachments/Pasted%20image%2020240801125719.png)