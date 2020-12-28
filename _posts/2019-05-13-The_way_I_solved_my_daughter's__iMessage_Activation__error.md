---
date: 2019-05-13 03:17
tags: Home computing iPhone iMessage bugs
title: The way I solved my daughter's "iMessage Activation" error
---

Writing these notes in case they help someone.

My daughter recently tried to add her Apple ID to her iPhone'sApple Messages
app. (Settings > Messages > Send & Receive > Add Apple ID)

When she tried this, she got a dialog box where she could type in her Apple ID
and password. After a 15 second delay she got an error dialog box:

```
iMessage Activation
  An error occurred during activation.
        Try again
```

She got a similar error message if she tried to activate FaceTime:

```
FaceTime Activation
  An error occurred during activation.
        Try again
```


I searched the Internet, and tried the various remedies that Apple and others
suggested:

* Reboot Phone.
* Make sure the phone can receive SMS messages.
* Enable/disable iMessage and FaceTime.
* Log the iPhone out of iCloud and log back in again.
* Visit icloud.com and check the iCloud account to see if there's any warnings or errors.
* Update the phone OS to the latest release version of iOS.
* Try to register with WiFi enabled but Cell disabled.
* Wait 24 hours and try again.

Having exhausted the home remedies, I contacted Apple Support by phone. Apple
Support listened sympathetically, and ran me through the same steps, it
roughly the same order. They also did a little bit of checking on their own of
the Apple ID account to see if there were any issues.

They couldn't find anything wrong, and they suggested that I contact my
carrier.

I contacted my carrier, T-Mobile, and they ran me through the same steps as
Apple. They also had me check out my phone's ability to connect to the
Internet for Data and SMS. They had me turn off my phone for a minute, so they
could update my phone's SIM. Unfortunately, nothing they tried helped.

In the end, what worked was to admit defeat. I had my daughter create a new
Apple ID. She was able to use the new Apple ID to log into iMessage without
any problem.

So far the drawback of this solution seem to be:

* My daughter lost everything she had stored in iCloud.
  * For many people this would be a serious issue.
  * Luckily my daughter does not use iCloud at all.
  * She uses Google services, like gmail, Google Docs and Google Photos, that do not depend on iCloud.
* My daughter has to re-add all her Contacts to her new Apple ID.
* My daughter lost some game progress in some of her Game Center aware video games.

My guess is that the problem was on Apple's end. The symptoms and cure seem to
indicate that my daughter's old Apple ID account was messed up in some small
way, and Apple's diagnostic systems were not detailed enough to detect or
correct the issue.

So, anyway, I wrote this up to offer one more home remedy for people suffering
from the "An error occurred during activation" message when trying to log in
to iMessage. That message could have one of a number of different causes. If
you are seeing it, please try the simpler remedies first, and please contact
Apple Support (and your phone carrier's support) for help. But if everything
else fails, be aware that for some users such as my daughter, one solution was
to switch to a new Apple ID.
