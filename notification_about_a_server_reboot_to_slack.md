 ![](<https://i.imgur.com/VC6iSYE.jpg>)

Create a script file in your home directory:

`
   touch reboot_notification.sh
  `

Open it with vim

`
   vim boot_notification.sh
  `

Hit "Insert" on the keyboard and put the next text in the file:

`
  #!/bin/sh
 ` `
  url="https://hooks.slack.com/services/your/hook"
 ` `
  channel="#ssh_logins"
 ` ``
  host="`curl ident.me`/site.com"
  `` ``
  content="\"attachments\": [ { \"mrkdwn_in\": [\"text\", \"fallback\"], \"fallback\": \"Server reboot on \`$host\`\", \"text\": \"Server reboot on \`$host\`\", \"fields\": [ { \"title\": \"Your server has just booted up\", \"value\": \"$PAM_RHOST\", \"short\": true } ], \"color\": \"#F35A00\" } ]"
  `` `
  curl -X POST --data-urlencode "payload={\"channel\": \"$channel\", \"mrkdwn\": true, \"username\": \"ssh-bot\", $content, \"icon_url\": \"\"}" $url
 ` save it by pressing "Esc" then ":wq".

Open crontab:

`
   sudo crontab -e
  `

Hit "Insert" on the keyboard and

At the end of this file add the next two rows:

`
   # Boot notifier
  `  `
   @reboot /home/user/boot_notification.sh
  `

save it by pressing "Esc" then ":wq".

Done

![](<https://i.imgur.com/aJwbGXL.png>)

