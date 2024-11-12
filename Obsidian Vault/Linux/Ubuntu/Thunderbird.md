To set up Thunderbird to sync to your Outlook.com account’s email:

1.       Open Thunderbird.
2.       From the Application menu, choose **Add Account…**
3.       Input your account information, choose **Options…** then **Account Settings…**
4.       Click the **Account Actions** button, then choose **Add Mail Account…**
5.       Input your account information.
6.       Click **Continue.**
7.       Click **Manual config.**
8.       Set the **Incoming** settings as follows:
          a.       **Server hostname**: imap-mail.outlook.com.
          b.      **Port:** 993.
          **c.       SSL:** SSL/TLS.
          **d.      Authentication:** Normal password

9.       Set the **Outgoing** settings as follows:  
          a.       **Server hostname**: smtp-mail.outlook.com.
          b.      **Port:** 587.
          **c.       SSL:** STARTTLS.
          **d.      Authentication:** Normal password.

==d.  Probably should be OAuth2 now.==
10.   Click **Done.**