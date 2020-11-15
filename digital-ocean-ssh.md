# Digital Ocean with SSH access

In this post we will create a new machine (Droplet) in your Digital Ocean account and secure it with SSH access

## Step 1: Make sure you have SSH configured

Go to Digital Ocean's dashboard, navigate to `Settings` > `Security`

Add your SSH keys there.

## Step 2: Create a droplet

Go to Digital Ocean's dashboard, click the "Create" button on top of the page and select "Droplet"

|option|value|
|--|--|
|Choose an image | Whatever Latest LTS Ubuntu is there, e.g. `18.04 (LTS) x64` |
|Choose a plan | `Basic`, `$5/month` |
|Choose a datacenter | any location you want |
| VPC | No VPC|
| Options | [ x ] IPv6  [ ] User data  [ x ] Monitoring |


Click [CREATE]


## Step 3: Login with SSH

Let's make sure you have ssh access to your new machine. 

First let's get you a root password: go to Dashboard, click on your droplet's name, go to `Access` and ask for a new root password.
Once that's done you'll receive your password via email.

Now go to Digital Ocean's dashboard, then navigate to `Droplets` and copy the IP address of your new droplet.

From a terminal, let's access your server.

Whenever prompted about a new host, confirm (Y) and continue.

You should be inside your droplet now. You will need to change your password the first time you access it.

```bash
ssh root@1.2.3.4

...
...

Changing password for root.

(current) UNIX password: [paste-root-password-here]
Enter new UNIX password: [new-password]
```

Enter a new password and repeat it.

Once done, let's confirm everything is fine, by closing our session and connecting again:

```bash
root@1.2.3.4:~# exit
logout
Connection to homebots.io closed.

$ ssh root@1.2.3.4
```

Ok, now let's remove password access from your machine.

```bash
nano /etc/ssh/sshd_config
```
Locale the line with `PermitRootLogin` and replace `yes` with `prohibit-password`

```
...
PermitRootLogin prohibit-password
...
```

Now only ssh access with your keys is allowed.

Make sure you don't lose your keys! 😅

> Read more about [security-related changes](https://www.cyberciti.biz/faq/how-to-disable-ssh-password-login-on-linux/)

