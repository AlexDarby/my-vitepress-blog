---
title: "Remote Development With Tailscale, VSCode and DigitalOcean"
description: The story so far.
aside: false
date: 2022-06-26
tags:
  - Tailscale
  - VSCode
---

I use multiple devices and operating systems and while my development needs are not particularly heavy, and I can largely use the same tools across multiple systems, I don’t like the thought that I can’t seamlessly move between machines. I don’t trust that I haven’t forgotten to push local commits from one machine to remote and then I’m stuck doing the merge dance if I want to make any changes from another device. I use Linux, OSX and Windows all relatively often and I could conceivably wont to work on some projects on any of them.

I have also had very mixed experiences getting project dependancies up and running across different OS’s and devices. I’ve frequently used Jekyll for small web projects and especially on OSX, Ruby can be a mare to set up and configure. The appeal of having a pre-cooked environment on a remote machine that I can hook up my local VSCode instance to and carry on as normal is very compelling.

Recently I discovered that there is an immensely popular (over 11 million downloads at time of writing) add-on for VSCode that allows you to work with a remote file-system over SSH. Interesting. And particularly so as I was also reminded that the excellent Tailscale had recently enabled SSH over their tool. As it turns out there are some really simple steps we can take to get our secure, remote dev environment up and running.

Firstly, you will need to install Tailscale on any client machine you want to be able to access the development VM from, there are guides for virtually every OS on Tailscale’s site, including iOS and Android which is particularly useful if you are working on any web app projects and want to see how it looks on a mobile device.

I’m setting up my OSX machine as a client for now, so all I need to do is download the client from the App Store, create an account or sign in and thats the client node stet up.

Now thats done, we need our remote development machine to work from. Any device you have ownership of will do. By all means if you would prefer to use this method to connect to your ‘main’ computer from a laptop on the go, that works too. My use is split fairly evenly across several computers, so a VM running on a cloud provider works for me.

I decided to spin up a DigitalOcean Droplet as my dev machine, however there are a plethora of cloud hosting providers that you could also use, often with trial credits as well, allowing you to try this setup without paying a penny.

I navigated to the new Droplet page in the DigitalOcean console, selected a VM size and operating system I was comfortable with (Ubuntu 20) and configured remote access via SSH key for the initial setup. There are plenty of guides out there for generating your SSH keys if you haven’t done so already. Once you are happy with the settings for the machine we can create the Droplet and wait for it to be created, which shouldn’t take long.

On our Droplet, all I need to do is remote into the machine via DigitalOcean’s web terminal tool (Click on the newly created Droplet and select Access > Launch Droplet Console) and a new window will load with a console session to the new VM.

Now that we have access to our new machine, we will need to ensure there is a non-root user to do your development with. You can create a new user…
```Bash
adduser username
```

Once you have filled in some basic information for the new user you can and add the new user to the sudoer’s list…
```Bash
usermod -aG sudo username
```

We can test this has worked by changing user to the newly created account and running whoami with sudo…
```Bash
su - username

sudo whoami
```

If the output says ‘root’ the new account has access to sudo and we can move to the next steps. If you are having trouble with this step it would be worth reviewing the /etc/sudoers file and ensure your new user account appears in the config with similar permissions…
```Bash
username ALL=(ALL:ALL) ALL
```

Now we have access to our new machine and we have su-ed to our new developer account, we need to get Tailscale up and running… as before, refer back to the installation docs for the specific OS you have chosen for the development machine. Ubuntu is here.

Once installed we can run
```Bash
sudo tailscale up --ssh
```

This will fire up the service with the extra SSH option. Once you have entered this command you will be prompted to authenticate the request by copying a URL from the terminal into a browser. Once authenticated, the Tailscale session will start and you should be able to SSH into the machine on the terminal using ‘ssh user@devbox’. You can check the Tailscale daemon is running using…
```Bash
service tailscaled status
```

Right. Now we can now add the SSH plugin to VSCode. Once this is done, add our devbox as a new remote host and connect.

This will give you the ability to run dev web servers remotely and the VSCode plugin will tunnel the connection to your local machine so you are able to view your project ‘locally’ via http://127.0.0.1:3000.

There we go.

Potential next steps culd be ensuring backups are enabled on your VM’s hosing provider, and setting up things like GitHub CLI, and any remote development packages you may need.

Enjoy!
