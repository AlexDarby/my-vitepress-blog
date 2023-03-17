---
title: "Sending Push Notifications with Python and Ntfy."
description: The story so far.
aside: false
date: 2022-11-01
tags:
  - Python
---


Over the last few days I encountered an excellent tool in the form of [Ntfy](https://ntfy.sh), an excellent anonymous message relay. There are quite a few compelling use-cases for a tool such as:
- Basic system monitoring and alerting.
- Home automation.
- Web scraping alerts.

In order to recieve the notifications its recommended yo use the official app on the [App Store](https://apps.apple.com/us/app/ntfy/id1625396347) / [Play Store](https://play.google.com/store/apps/details?id=io.heckel.ntfy). Bare in mind you don't need any kind of account or login for the app in order to recieve your notices.

I've elected to use a GUID as the message topic I am subscribing to, to prevent anyone stumbling upon my feed, though you can use anything like 'alexs-messages' if you wish. In the interest of security, the author of Ntfy does recommend the use of a personally hosted instance, should you want to be completely assured your messages are not being read/stored in their central system. While this is not in scope of this project I may cover it in a seperate post.

Create a GUID in the terminal with 'uuidgen'

```Bash
$uuidgen
67240364-be56-4a43-af4b-f4eae318ee94

```

Once you have the app installed on a device you should now be able to test you are recieving messages with a quick CURL command. Within the app, ensure you have added a new message subscription topic in the Ntfy app on your device and add the GUID as the topic name.

Now we can send a message to the topic using CURL to see if everything is working.

```Bash
curl -d "Test Message" ntfy.sh/67240364-be56-4a43-af4b-f4eae318ee94
```

With any luck you should have recvieved the test message. Great! ... OK time to crack open Python...

Lets set up a basic project from the Ntfy docs for Python...

```Python
# /notify.py
import requests

topic="67240364-be56-4a43-af4b-f4eae318ee94"

requests.post("https://ntfy.sh/" + topic, 
    data="Test Message".encode(encoding='utf-8'))
```

Running this script should generate the same message as our test CURL command above...

```Bash
python3 notify.py
```

Cool. Now we should update the script to allow us to send a title, priority status etc. and a few other elements as per the docs.


```Python
# /notify.py
import requests, json

topic="67240364-be56-4a43-af4b-f4eae318ee94"

requests.post("https://ntfy.sh/",
    data=json.dumps({
        "topic": topic,
        "priority": 5
        "message": "You left the house. Turn down the A/C?",
        "actions": [
            {
                "action": "view",
                "label": "Open portal",
                "url": "https://home.nest.com/",
                "clear": True
            }
        ]
    })
)
```

And there we go. We can now send ourselves (or any subscriber of the topic) notifications by calling the script.

Possible next steps:
- Add args to the script to allow you to parse options from the command line.
- Migrate the script to a library and integrate it within a Python project.
