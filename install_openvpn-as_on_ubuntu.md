 Don’t forget to enable “Tun/Tap” functionality on your control panel

Log in to the system with next credentials

user : `
   root
  `

passwd : from the control panel

`
   apt-get update
  `

Get the package

`
   wget http://swupdate.openvpn.org/as/openvpn-as-2.0.17-Ubuntu14.amd_64.deb
  `

Install the package

`
   dpkg -i openvpn-as-2.0.17-Ubuntu14.amd_64.deb
  `

Thats all, its running

Set the passwd to server

`
   passwd openvpn
  `

Access to the server settings through web

`
   https://"server ip":943/admin
  `

Get file with settings

`
   https://"server ip":943
  `

