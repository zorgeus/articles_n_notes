 <div>
  <div>
   ![](<https://i.imgur.com/Rwqzbzb.png>)

[by  Vivek Gite  on  January 6, 2007 last updated January 6, 2007](<http://www.cyberciti.biz/faq/howto-drop-block-all-ping-packets/>)

A. Generally you can use [iptables](<http://www.cyberciti.biz/tips/linux-iptables-9-allow-icmp-ping.html>) to block or allow ping requests.

You can setup kernel variable to drop all ping packets. Type the following command at shell prompt:  **# echo “1” > /proc/sys/net/ipv4/icmp_echo_ignore_all **

This instructs the kernel to simply ignore all ping requests (ICMP type 0 messages). To enable ping request type the command:  **echo “0” > /proc/sys/net/ipv4/icmp_echo_ignore_all **

You can add following line to /etc/sysctl.conf file:  **# vi /etc/sysctl.conf **  Append following line:  **net.ipv4.icmp_echo_ignore_all = 1 **

Save and close the file.

Sometimes ping request can be handy for testing your own server. You can disable ICMP type 0 [messages in the firewall](<http://www.cyberciti.biz/tips/linux-iptables-9-allow-icmp-ping.html>) so that local administrators to continue to use ping command for their own server. Following command block all ICMP packets including ping request:  `
     # iptables -A INPUT -p icmp -j DROP
    `

</div>
 </div>
