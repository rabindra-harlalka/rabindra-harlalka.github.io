---
layout: post
title: "SSH and X11 configuration"
categories: tools
---

## Aim
View graphical applications running on Ubuntu VM hosted on cloud (Azure/AWS/GCP) locally on the Windows 10 PC.

## Steps
1. Install an X implementation on your Windows PC. [X410](https://x410.dev/) is an example.
2. Run the X server application installed in the previous step.
3. Open a shell to run ssh. [WSL terminal](https://docs.microsoft.com/en-us/windows/wsl/wsl-config) is a good choice.
4. Set `DISPLAY` environment variable: `export DISPLAY=localhost:0.0`
5. SSH into the remote Ubuntu VM, make sure that the `/etc/ssh/ssh_config` (not `sshd_config`) has the following lines as shown and uncommented:

```
   ForwardX11 yes
   ForwardX11Trusted yes
```
Restart ssh if the file is modified: `sudo systemctl restart ssh`.

6. Disconnect the SSH session and now connect again with `-X` option to request X11 forwarding: `ssh -X ...`.
7. Once connected, we should see something similar to the following:

```
$ echo $DISPLAY
localhost:10.0
```

8. Now run the graphical program. For example, we can run matlab: `matlab`.
9. The application will open on the local Windows PC inside the X server's window.

### Example
![](/_images/ssh-x11.png?raw=true)

## References
- [https://fabianlee.org/2018/10/14/ubuntu-x11-forwarding-to-view-gui-applications-running-on-server-hosts/](https://fabianlee.org/2018/10/14/ubuntu-x11-forwarding-to-view-gui-applications-running-on-server-hosts/)
- [https://unix.stackexchange.com/q/12755](https://unix.stackexchange.com/q/12755)
- [https://stackoverflow.com/a/60285940](https://stackoverflow.com/a/60285940)
