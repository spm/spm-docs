# Sendmail  
Send a mail message (attachments optionals) to an address.  

* **Recipient** (enter text)  
User to receive mail.  

* **Subject** (enter text)  
The subject line of the message. %DATE% will be replaced by a string containing the time and date when the email is sent.  

* **Message** (enter text)  
A string containing the message to send.  

* **Attachments** (select files)  
List of files to attach and send along with the message.  

* **Parameters**   
Preferences for your e-mail server (Internet SMTP server) and your e-mail address. MATLAB tries to read the SMTP mail server from your system registry. This should work flawlessly. If you encounter any error, identify the outgoing mail server for your electronic mail application, which is usually listed in the application's preferences, or, consult your e-mail system administrator, and update the parameters. Note that this function does not support e-mail servers that require authentication.  

    * **SMTP Server** (enter text)  
    Your SMTP server. If not specified, look for sendmail help.  

    * **E-mail** (enter text)  
    Your e-mail address. Look in sendmail help how to store it.  

    * **Zip attachments** (choose from the menu)  
    Zip attachments before being sent along with the message.  
