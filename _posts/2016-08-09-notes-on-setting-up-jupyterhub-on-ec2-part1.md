---
layout: post
title: Notes on setting up Jupyterhub on an EC2 instance (part 1 )
---

# Notes on setting up Jupyterhub on an EC2 instance ( part 1 )

I am working on setting up a shared Python programming environment for PyLadies London
beginner and intermediate programming workshops. My last programming workshop on 
generators and co-routines was less than successful and I think a lot of it had to with
the fact that attendees 

1. had to spend a lot of time setting up an environment
2. didn't get direct feedback about whether or not the code they had produced was successful
3. the material wasn't great ( let's be honest )

I hope that I can improve the situation for points 1. and 2. by making a unified PyLadies London Jupyterhub 
environment. Together with [nbgrader](https://github.com/jupyter/nbgrader), having a unified
environment should hopefully make future PyLadies London workshops more enjoyable for the attendees. 
In this post ( or really 'note-to-self' ), I'm going to be listing some notes on setting up a Jupyterhub on an
Amazon EC2 instance. 

I am broadly following the [Deploying JupyterHub on AWS guide](https://github.com/jupyterhub/jupyterhub/wiki/Deploying-JupyterHub-on-AWS).
Please note that there may be errors here. If in doubt, always use the official Amazon AWS guides in lieu of whatever I just wrote here. 

## Setting up an Ubuntu instance on Amazon AWS

1. Go to the [Amazon AWS console](https://aws.amazon.com/console/) 
2. In the 'Services' tab, select EC2
3. Create a new instance and select Ubuntu. I am using the AWS free tier to test drive this deployment, so I'll be using t2.micro instance type.
4. Download the private key (.pem file) you get from the 'Create Instance' wizard( or create your own ) and move to safe location on local machine
5. Once you move the .pem file to a safe location, you have to change the permissions on the file. Otherwise connecting to your EC2 machine via ssh may not work. 

```bash

chmod 400 <my-pem-filepath.pem>

```
6. Select your instance in the Instances tab and click on the 'Connect' button. This will bring up a dialogue with instructions with the ssh commands that you need to execute from your local machine's shell to connect to your Ubuntu server running on EC2

```bash

ssh -i <my-pem-filepath.pem> ubuntu@<ec2-instance-dns-name> 
```

7. Verify the fingerprint. This is, unfortunately, not as straightforward as comparing the fingerpring that appears on the ssh console with the key fingerpring you can find when you go to Network & Security -> Key Pairs tab in your management console. You can find more information about this by reading the answer to [this](http://serverfault.com/questions/603982/why-does-my-openssh-key-fingerprint-not-match-the-aws-ec2-console-keypair-finger) Stack Overflow question. In short, you have to generate a fingerprint from the .pem file by running the command

```bash
openssl pkcs8 -in <my-pemfilepath>.pem -nocrypt -topk8 -outform DER | openssl sha1 -c
```

8. You should now be able to ssh to your Ubuntu server. 

## Using Let's Encrypt to secure communications between browser and server

Let's Encrypt is a free certificate authority (CA), which means that it provides digital certificates to domains. This in turn helps to ensure secure communication between a browser and the domain it is connecting to (yes, this explanation is a bit waffly and that's because to this day I have been very very clueless about SSL and certificates - that is all about to change. Hopefully by the next blog post I will be a bit more in tune with what all of this means ). 

1. Let's Encrypt does not work with amazon.com domains, so I will have to point a domain that I registered with another domain provider
to my EC2 instance. 

2. The Deployment guide does this with Route53, an Amazon AWS DNS service. I opted to go down the Route53 route (heheh), although now
that I think about it, I should have just stuck with my own DNS provider. In the end, I decided to go back and use my domain's original nameservers and add the EC2 instance as a new record into the zone file. The TTL on that record is 14400, so I suppose I have 12 hours to wait and see if anything comes to fruition out of my random rambling (TTL is the amount of time I need to wait between updating a DNS record and the DNS record update being reflected in the DNS servers ). I'll save Route53 for another day. For now, I am too impatient ( and too much of a n00b ).






