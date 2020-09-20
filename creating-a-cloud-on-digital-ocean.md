# Introduction


## Step 1: Make sure you have SSH configured

Go to Digital Ocean's dashboard, navigate to `Settings` > `Security`
Add your SSH keys there.

## Step 2: Create a Droplet

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

Go to Digital Ocean's dashboard, then navigate to `Droplets` and copy the IP address of your new droplet.

From a terminal:

```bash
# droplet's IP, e.g. 1.2.3.4

ssh root@1.2.3.4
```

Whenever prompted about a new host, confirm (Y) and continue.
You should be inside your droplet now.

## Step 4: 

