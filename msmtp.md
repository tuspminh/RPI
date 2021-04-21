# MSMTP
msmtp is a very simple and easy to configure SMTP client for sending emails.
Its default operating mode consists of forwarding e-mails to the SMTP server that you have indicated in its configuration. The latter will be responsible for distributing the emails to their recipients.
It is fully compatible with sendmail, supports TLS secure transport , multiple accounts, various authentication methods, and delivery notifications.

# Prerequisites
 Have fully functional Internet connectivity.
 Enabling less secure apps to access Gmail https://www.google.com/settings/security/lesssecureapps to get pass

	
# Installation
To install this software, just install the msmtp and msmtp-mta packages .
Either in command line:
`sudo apt install msmtp msmtp-mta`

# Configuration
To configure the sending of emails at the system level, open the`/etc/msmtprc` file for modification then fill in the connection parameters to your email account using the example below:

```
# Default values for all accounts. 
defaults 
auth on 
tls on 
tls_starttls on 
tls_trust_file /etc/ssl/certs/ca-certificates.crt 
logfile /var/log/msmtp.log 

# Example for a Gmail 
account gmail account 
auth plain 
host smtp.gmail.com 
port 587 
from username @ gmail.com 
user username 
password XXXXXXXXXX 
# Example for a GMX 
account gmx account 
host mail.gmx.com 
port 587 
from username@gmx.com 
user username@gmx.com 
password XXXXXXXXXX 
tls_nocertcheck
# Example for an OVH 
account account ovh 
host XXXXXX.ovh.net 
port 465 
from username@example.com 
user username@example.com 
password XXXXXXXXXX 
# Example for an Infomaniak 
account infomaniak account 
host mail.infomaniak.com 
port 587 
from username @ example .com 
user username@example.com 
password XXXXXXXXXX 
# Example for a test server MailHog 
account mailhog 
auth off 
tls off 
tls_starttls off 
from username@example.com
host           localhost
port 1024
# Define the default 
account account default: gmail
```
For OVH in the general settings, remember to disable starttls:
 `tls_starttls   off `
 
If you want to configure the sending of emails at the user level, create the `.msmtprc` file in the personal folder of the affected user. 
This file should only be accessible for reading and writing to the user:
`chmod 600 .msmtprc`

It is necessary to check that the system uses msmtp well for sending mail:
`ls -la /usr/sbin/sendmail`

Who should return:
`lrwxrwxrwx 1 root root 12 nov.  28  2016 /usr/sbin/sendmail -> ../bin/msmtp`

If not, reinstall the msmtp-mta package .

## Function test

`echo 'message' | mail user@domain.com`

If that doesn't work, try:
`echo 'message' | msmtp user@domain.com`

Check his inbox.
If the command maildoes not work, install the bsd-mailx package :
`sudo apt install bsd-mailx`

If you receive this error message: 
send-mail: impossible d'écrire dans le journal /var/log/msmtp.log : erreur d'ouverture de fichier: Permission non accordée 
the solution is summarized on this page in French. and specified on its source in English Successfully tested on Ubuntu 20.04.


# Uninstallation
To remove this application, just remove its package . 
Depending on the method chosen, the global configuration of the application is kept or deleted. 
System logs, and users' preference files in their personal folders are always kept.


