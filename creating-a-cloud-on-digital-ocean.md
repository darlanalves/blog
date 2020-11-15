# Introduction

We will set up a new Droplet in Digital Ocean, then install a few packages on it and prepare to host multiple services and domains in one machine

## Step 1: Get a droplet up and running

Create a droplet and secure it following [the steps in this other guide](/digital-ocean-ssh.md)

## Step 2: Update packages and install dependencies

We'll install Node.js, Docker and Nginx

```bash

# Update dependency list
apt-get update && apt-get upgrade -y

# Node.js
snap install node --classic

# Docker
snap install docker

```

# Step 3: prepare the cloudy service

```bash
npm i -g @cloud-cli/cloudy

```
