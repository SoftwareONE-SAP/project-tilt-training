# How To Fix the MTU Entry

**Please Note - Editing the registry can cause problems with the machine.  Do not do this if you are not comfortable with the registry and its workings.**

The problem is that the NETSH command does not store a changed MTU value.  the value is replaced with a default that is (presumably) supplied by the driver program for the network card.  The following process will allow you to amend this behaviour.

1. Start an administrative command prompt

2. From within that run the command REGEDIT

3. Locate this key

   ```text
   HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\Control\Class\{4d36e965-e325-11ce-bfc1-08002be10318}
   ```

   There will be several of them. Search through them until you locate the one with the value of 'Class' = 'Net' on the right hand pane.

4. Now open it up.  In the subsequent list, there will be entries numbered 0000 - 9999,  On my machine there are ones from 0000 to 0017.  Look through them until you find one that has a value of 'DriverDesc' = 'My network card'.  When you locate it make a note of the GUID in the entry NetCfgInstanceId

   You will need to repeat this for any other network cards/WiFi that you want to affect.

5. Now locate this key

   ```text
   HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\Services\Tcpip\Parameters\Interfaces
   ```

   Below this there are a number of sub-keys with GUID names.  Find the one(s) with the value matching the GUID you noted in step (4).

   In the values for the Key you have located will be an MTU entry.  Amend this as needed.

**Note** that the value is in Hex so 1500 = 0x5DC.  On my machine there were some subkeys for the WiFi card, representing other networks that have been connected to, and I changed the MTU setting in those as well.

Once this is done then reboot the machine and then if you check the command

```text
netsh interface ipv4 show interfaces
```

(again from an administrator command prompt) this should then report the correct MTU value.

[Credit :link:](https://www.cbfive.com/how-to-find-a-network-adapter-in-the-registry/)
