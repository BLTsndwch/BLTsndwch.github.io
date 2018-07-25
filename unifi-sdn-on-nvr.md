## Setting up UniFi SDN on the UVC-NVR

*Note: this can be done before or after setting up UniFi Video. Depending on whether your NVR needs an UFV repository update, doing this first before setting up UFV could update UVF so that you begin with the latest version.* 

*Step 6 will not work without an internet connection, but if you really need to setup UniFi beforehand, you could look into uploading the latest [UniFi SDN Linux package](https://www.ubnt.com/download/unifi/default/default/unifi-sdn-controller-5824-debianubuntu-linux) to the NVR with SFTP after step 4.*

1. Connect a PC to same LAN as the NVR.
2. Get the NVR's IP address with the Ubiquity Device Discovery tool. (Chrome or Java)
3. Click on the NVR's IP address to go to the NVR management page in a web browser.
4. [Follow UBNT's steps to enable SSH on the NVR](https://help.ubnt.com/hc/en-us/articles/222970307-UniFi-Video-How-to-Enable-SSH-on-the-UVC-NVR-Hardware-NVR-).  Recommend changing device management credentials to something secure.  The SSH login info is the same as the management credentials, so a strong password is very important. (Switching to key based SSH auth and  using non-root login  after setup  recommended.)
5. SSH into the NVR with your favorite SSH client. PuTTY is a popular basic client; bash on Ubuntu on Windows 10 works well with setting up SSH key auth. Username is `ubnt`, password is what you set on the web management pane. With PuTTY, all you should have to do is put in the NVR's IP address, check that the port is at the default `22`, check SSH connection type, and click open. UBNT has an [intro to SSH](https://help.ubnt.com/hc/en-us/articles/218850057).
6. [Follow UBNT's steps to install UniFi via APT](https://help.ubnt.com/hc/en-us/articles/220066768-UniFi-How-to-Install-Update-via-APT-on-Debian-or-Ubuntu). The NVR is just a Debian 7 system that comes preloaded and configured for UFV. Since logging in with username `ubnt` logs you into root by default, you do not need sudo. That said, best practice is using a non-root account.
7. Navigate to `https://<NVR_IP_ADDR>:8443` in a web browser to setup UniFi. You'll get a warning about the self-signed SSH certificate, this is unfortunately normal. UBNT may be adding an easy way to get a free LetsEncrypt cert in the future. The URL for UFV is `https://<NVR_IP_ADDR>:7443`.

From then on, UniFi SDN can be upgraded with `apt-get update && apt-get upgrade`. Every now and then UniFi video repository might change, so check the UniFi Video Updates Blog before you upgrade UFV. UBNT has a [short article on UVC-NVR maintenance](https://help.ubnt.com/hc/en-us/articles/204952214-UniFi-Video-How-to-Update-the-NVR-Operating-System).

Note that for an NVR behind most routers, SSH will only work on the local network. For remote management, setting up a VPN is preferred. Otherwise, you can look into dynamic dns and setting up port forwarding with a high port number. If you open a port, make sure you are using key based SSH auth or a extremely strong password because there will be login attempts from the internet.