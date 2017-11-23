 ![](<https://i.imgur.com/VC6iSYE.jpg>)

[Original article](<http://sandrinodimattia.net/posting-successful-ssh-logins-to-slack/>)

Integration by using **Incoming Web Hooks ** .


## Slack Setup  ##

 So first you would need to configure an **Incoming Web Hook ** in Slack:

`
   https:  / /YOUR_DOMAIN.slack.com/apps  /manage/custom  -integrations `

Configuring this will give you a **Webhook URL ** to which you can post your messages.


## Machine Setup  ##

 Now connect to your machine and create a script in your `
   ssh
  ` folder:

`
   sudo vim /etc/ssh/notify.sh
  `

Add the following code to the script which we'll configure to run each time a user signs in:

``
  #!/bin/sh  if  [ " $PAM_TYPE  "  != "close_session"  ]; then  url= "YOUR_SLACK_WEBHOOK_URL"  channel= "#ssh_logins"  host= "\`curl ident.me\`/site.com"  content= "\"attachments\": [ { \"mrkdwn_in\": [\"text\", \"fallback\"], \"fallback\": \"SSH login: $PAM_USER  connected to \\` $host  \\`\", \"text\": \"SSH login to \\` $host  \\`\", \"fields\": [ { \"title\": \"User\", \"value\": \" $PAM_USER  \", \"short\": true }, { \"title\": \"IP Address\", \"value\": \" $PAM_RHOST  \", \"short\": true } ], \"color\": \"#F35A00\" } ]"  curl -X POST --data-urlencode "payload={\"channel\": \" $channel  \", \"mrkdwn\": true, \"username\": \"ssh-bot\", $content  , \"icon_url\": \"\"}"  $url  fi   `` Here

`
  \"icon_url\": \"  ` part will leave bot icon (avatar) as it was configured in webhook

you can use

`
  \"icon_emoji\": \":computer:\"
 ` to use a standard emoji avatar for your bot.

Now make the script executable:

`
  sudo chmod +x /etc/ssh/notify.sh
 ` Finally add the following line to `
   /etc/pam.d/sshd
  ` :

`
  session  optional pam_exec.so seteuid /etc/ssh/notify.sh `
## Done  ##

![](<https://i.imgur.com/DETVPyk.png>)

