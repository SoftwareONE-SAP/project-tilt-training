# Connection Problems

This is intended to address a very specific problem you may encounter when

- trying to use SSH
- from a WSL 2 distrubution
- running under Windows

## How to Tell if you have the problem

The most obvious symptom is that when you try and perform any SSH related activities, the SSH program appears to hang.

This problem will occur with any SSH command that has to try and send a key across the network, so it could affect connections to any SSH server (e.g. Azure).

To check if you are having the specific connection problem this article attempts to address, do the following:

1. Start a WSL shell
2. Run the following command

   ```text
   ssh -T -vv git@github.com
   ```

3. If you are suffering from the connection issue you will see the following output:

   ```text
   openssH 8.2p1 Ubuntu-4, openssL 1.1.1f 31 mar 2828
   debug1: Reading configuration data /etc/ssh/ssh_config
   debug2: /etc/ssh/ssh_config line 19: include /etc/ssh/ssh_config.d/*.conf matched no files
   debug1: /etc/ssh/ssh_config line 21: Applying options for *
   debugl: Connecting to github.ccm [148.82.121.3] port 22.
   debugl: Connection established.
   ...
   ... Lots of messages here ...
   ...
   debugl: Authenticating to github.ccn:22 as 'git'
   debugl: . KEXINIT sent
   debugl: . SSH2 KEXINIT received
   debugl: kex: algoritfun: curve25519-sha256
   debugl: kex: host key algoritfun: ecdsa-sha2-nistp256
   debugl: kex: server->client cipher: chacha2ø-p01y13øS@penssh.ccm (implicit> compression:
   debugl: kex: client->server cipher: chacha2ø-p01y13øS@penssh.ccm 'u: (implicit> compression:
   debugl: expecting SSH2_KEX_ECDH_REPLY
   ```

## How to Fix the Problem - Preparation

The problem is caused by a mismatch in the configuration of the network cards in Windows and the 'virtual' network card in the WSL machine.  As preparation to fix the problem you first need to do the following:

1. Start a Windows command prompt, and run the following command.

   ```text
   netsh interface ipv4 show interfaces
   ```

2. This will produce a list such as this

   |Idx|   Met    |   MTU    |    State     |         Name                |
   |---|----------|----------|--------------|-----------------------------|
   |  1|        75|4294967295|     connected| Loopback Pseudo-Interface 1 |
   | 17|        50|      1300|  disconnected| WiFi                        |
   | 20|        25|      1300|     connected| Ethernet                    |
   | 13|        25|      1500|  disconnected| Local Area Connection* 3    |
   | 15|        25|      1500|  disconnected| Local Area Connection* 12   |
   | 54|        15|      1500|     connected| vEthernet (WSL)             |

3. Locate your active network interface(s), this will probably be called either 'Ethernet' or 'WiFi'.  You need to make a note of the values from the IDX column and the MTU column.
4. Now start a shell on the WSL machine.
5. Run the following command:

   ```text
   ip link
   ```

6. This will list the interfaces on the WSL machine, and produce something like this

   ```text
   1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN mode DEFAULT group default qlen 1000
      link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
   2: bond0: <BROADCAST,MULTICAST,MASTER> mtu 1500 qdisc noop state DOWN mode DEFAULT group default qlen 1000
      link/ether 8e:d4:8b:55:37:c8 brd ff:ff:ff:ff:ff:ff
   3: dummy0: <BROADCAST,NOARP> mtu 1500 qdisc noop state DOWN mode DEFAULT group default qlen 1000
      link/ether 06:63:1c:4d:3b:48 brd ff:ff:ff:ff:ff:ff
   4: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP mode DEFAULT group default qlen 1000
      link/ether 00:15:5d:2e:42:5f brd ff:ff:ff:ff:ff:ff
   5: tunl0@NONE: <NOARP> mtu 1480 qdisc noop state DOWN mode DEFAULT group default qlen 1000
      link/ipip 0.0.0.0 brd 0.0.0.0
   6: sit0@NONE: <NOARP> mtu 1480 qdisc noop state DOWN mode DEFAULT group default qlen 1000
      link/sit 0.0.0.0 brd 0.0.0.0
   ```

7. Locate the 'eth0' entry and note the MTU value.

So once, you have done all this you should have the following information

- For the network interface(s) that your laptop uses to connect to the internet
  - An IDX value
  - An MTU value

- For the network interface the WSL machine uses (there will only be one of these)
  - An MTU value

If the MTU values for the Windows environment does NOT match the one that the WSL machine sees, then you have found the cause of the SSH hanging problem.

## How to Fix the Problem

We need to make the MTU in Windows match the MTU in the WSL machine (or vice versa).  So to correct the situation you must do one of two things - either modify the MTU value in Windows, or modify the MTU value in the WSL environment.

### In Windows

You need to start a Windows command prompt (or Powershell).  The command prompt/power shell needs to be running as the Administrator.  Then you must run the following command:

```text
netsh interface ipv4 set subinterface XXX mtu=YYY store=persistent
```

Where you replace

- 'XXX' with index you noted down 'Preparation' section.
- 'YYY' with the MTU value from the WSL machine

### In WSL

You need to start a WSL Shell and then run the following commands:

```text
sudo apt install net-tools
sudo ifconfig eth0 mtu YYY
```

Where you replace

- 'YYY' with the MTU value from the Windows machine

## Conclusion

This will fix the problem with SSH hanging.  You can test that the fix has worked by running the command

```text
ssh -T git@github.com
```

:hand: PLEASE NOTE

The two methods outlined above will **not** remember the MTU changes if you reboot the machine.

In the WSL machine to persist these changes you can add the ifconfig command.

Under Windows you may need to set up a command file that you run after you reboot the machine.  There is an alternative, but this requires editing the registry and is quite complicated.  See [how-to-change-mtu-permanently-window-10](how-to-change-mtu-permanently-window-10.md) for more details.
