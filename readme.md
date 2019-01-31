THIS IS A FORK FROM VPNDemon (https://github.com/primaryobjects/vpndemon)
=========
### for Linux

VPNDemon monitors your VPN connection and kills a target program upon disconnect. It's the safest and easiest way to help prevent DNS leaks and enhance your security while connected over a VPN.

The purpose of this fork is to remove the GUI and to simply work in a terminal.

It's as simple as this:

- Run vpndemon.sh.
- Enter the name of the target process to kill when the VPN disconnects.
- Or you can also set the target process as an argument.

That's it!

Technical Details
---

VPNDemon monitors the VPN connection by listening to events from the linux [NetworkManager](https://wiki.archlinux.org/index.php/NetworkManager). When a VPN connect/disconnect event is received, the signal is checked to see which state it relates to. If it's a connect state, the status is simply displayed in the main window. If it's a disconnect state, VPNDemon immediately issues a kill command for all processes that match the target process name:

```sh
for i in `pgrep $killProgramName`
do
    kill $i
done
```

Since the NetworkManager is being listened to, directly via the [dbus-monitor](http://dbus.freedesktop.org/doc/dbus-monitor.1.html), disconnect events are detected almost instantly. Likewise, the target process is killed almost instantly.

VPNDemon should be compatible with any linux system that uses NetworkManager for VPN connections.

Troubleshooting
---

1. A log file is saved to /tmp/vpndemon, which contains the list of VPN connect/disconnect events and a list of processes terminated. The log is cleared each time the app is run. However, you can review the log during or after running the app, to help determine any troubleshooting issues.

Preventing DNS Leaks with IPv6
---

If you want even tighter privacy, you can disable IPv6. This is easy to do. IPv6 incorporates hardware MAC addresses, and since many VPN services do not yet route IPv6 traffic, it creates a potential leak for network activity.

To disable IPv6, edit the file /etc/sysctl.conf and add the following lines:
```sh
net.ipv6.conf.all.disable_ipv6 = 1
net.ipv6.conf.default.disable_ipv6 = 1
net.ipv6.conf.lo.disable_ipv6 = 1
```
After making these changes, refresh the file by running:
```sh
sudo sysctl -p
```
To verify IPv6 is actually disabled, run ifconfig and verify that "inet6" is not present in the output:
```sh
ifconfig | grep inet6
```

License
----

MIT

Author
----
Original : Kory Becker
http://www.primaryobjects.com/kory-becker

Fork : Robin Colombier
https://www.robin-colombier.com