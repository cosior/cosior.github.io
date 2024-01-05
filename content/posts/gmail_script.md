---
title: "Python Gmail Script"
date: 2019-01-06
description: "A simple python script that allows you to use your gmail account to send automated emails from your server without the need of setting up your own email server."
tags: ["Open source", "emails", "python"]
type: post
weight: 20
showTableOfContents: true
---

Use cases:

- You can use it to forward messages from a contact form in your website.
- Send notifications to your email when something happens to your server.
- Send reminders to yourself.
- etc.

{{< highlight python >}}
#!/usr/bin/python
import smtplib
from email.mime.text import MIMEText
import sys
import re

def send_mail():

    usage = '''usage: <%s>     ''' % (sys.argv[0])
    
    # Change the following according to your gmail credentials
    gmailUser = 'your_name@gmail.com' 
    gmailPassword = 'your_gmail_password'

    # Check input argument number
    if len(sys.argv) != 5:
        print usage
        exit(0)

    # Collect the arguments
    emailFrom = sys.argv[1]
    name    = sys.argv[2]
    subject = sys.argv[3]
    message = sys.argv[4]

    if not validateEmail(emailFrom):
        print 0
        exit(-1)

    subj = u'Subject: {0}'.format(subject)

    text = u'Name: {0}\nemail: {1}\n{2}\n{3}'.format(name, emailFrom, '-'*40, message)
    
    msg = MIMEText(text, 'plain', 'utf-8')
    msg['Subject'] = subj
    msg['From'] = emailFrom
    msg['To'] = gmailUser

    mailServer = smtplib.SMTP('smtp.gmail.com', 587)
    mailServer.ehlo()
    mailServer.starttls()
    mailServer.ehlo()
    mailServer.login(gmailUser, gmailPassword)
    mailServer.sendmail(gmailUser, gmailUser, msg.as_string())
    mailServer.quit()
    print 1
    exit(0)

def validateEmail(email):
    if len(email) > 7:
        if re.match("^.+@(\\[?)[a-zA-Z0-9\\-\\.]+\\.([a-zA-Z]{2,3}|[0-9]{1,3})(\\]?)$", 
        email) is not None:
            return 1
    return 0

if __name__ == "__main__":
    send_mail()
{{< /highlight >}}