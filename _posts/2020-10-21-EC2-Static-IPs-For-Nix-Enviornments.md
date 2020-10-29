---
layout: post
title:  "EC2 - Static IP’s For Nix Environments (RHEL/CentOS)"
date:   2020-10-21
image: /images/posts/aws_masters.jpg
tags: [AWS, CentOS, RHEL, Administratrion, Cloud, EC2, Repost, Christopher L Medina]
---

## Repost

I was recently tasked with investigating how to assign persistent static IP’s (four) to a RHEL EC2 instance. As a longtime Unix/Linux Engineer in a previous life, I (much like many of you right now) assumed this would be simple. I regret to inform you we were sadly mistaken. After a bit of research, I stumbled into quite a few separate ways to accomplish this goal. Here I have outlined what I believe to be the easiest methodology.

<!--more-->

A lot of constructs in AWS rely on DHCP. It appears as if inherently, AWS really wants us to stay away from static IP’s in lieu of more modern infrastructure practices (which I agree with completely). Connecting to your instance via a public IP will require some sort of NAT, the auto IP address leasing in your VPC subnets will rely on DHCP, load balancing, failover, so on and so forth. It’s because of this philosophy that I assume the process is a little more complicated than normal. However with Amazon Linux, you simply add the desired IP’s in the ENI settings during configuration of an instance and it ‘automagically’ works.

Normally, an admin would look at this task and ‘splice’ the interface virtually. By creating additional network initialization scripts, we can turn the physical eth0 device into several virtual devices. Eth0 will be divided and function as eth0, eth0:1, eth0:2, so on and so forth. Each device listing with a separate designated IP address. This process is called IP address aliasing, and due to the AWS ‘automagic’, it does not appear to work as intended in older non AWS Linux 'nix' based EC2 instances. So where do we go from here?

Understanding the why is often as important as understanding the how. Everything IP related in an EC2 instance is governed by the Elastic Network Interface (ENI). AWS needs to know what addresses to reserve per ENI so DHCP will never lease out an IP that has been reserved already (even if not in use). This means that our EC2 instances will essentially ignore any static address we give it if it hasn’t been given permission to use it via our ENI.

There are a few ways to ‘reserve’ IP’s for our ENI’s to use. On EC2 launch we can specify ENI static addresses. I will be going through this example using 172.31.64.10-13.

{% include image_caption.html imageurl="/images/posts/static_ips_for_nix_environments_001.webp" title="ENI Config" %}

This comes with a few caveats:

Firstly, the IP’s being specified must be a valid address from the designated subnet of your VPC chosen earlier during instance launch.
You will be limited on the amount of ENI’s and IPv4 address per ENI based off your instance type. Reference ENI limits [here](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-eni.html#AvailableIpPerENI). 

When you’re finished configuring your ENI’s and designated addresses SSH into your instance. You’ll notice that only the primary IP address will be displayed for eth0.

{% include image_caption.html imageurl="/images/posts/static_ips_for_nix_environments_002.webp" title="ENI Config" %}

This is because by default, most of the nix-based images available don’t have an ec2-net-utils package available for them. Ec2-net-utils is what allows Amazon Linux to automatically detect ENI changes and reflect them in the OS. If you have no stringent requirements for your environment I would almost always recommend using Amazon Linux for this and other similar AWS compatibility reasons (built in aws cli, etc.).

So, what we need to do is manually add the IP’s we specified in our ENI into our OS level network interface. The following commands should work in RHEL/CentOS:

```bash
sudo ip addr add 172.31.64.111/20 dev eth0
sudo ip addr add 172.31.64.112/20 dev eth0
sudo ip addr add 172.31.64.113/20 dev eth0
```

Now when we look at our eth0 configuration we should see all four addresses.

{% include image_caption.html imageurl="/images/posts/static_ips_for_nix_environments_003.webp" title="ENI Config" %}

Finally, we need to work some magic to make this configuration persist after boot. There are a few options; however, based off the EC2 cloud-init programming it’s safe enough to just run this in rc.local. While we could probably manipulate the cloud-init itself, I imagine it’s more familiar to people just running this configuration after boot via the rc.local file. Modify /etc/rc.d/rc.local and append the following:

```bash
touch /var/lock/subsys/local
ip addr add 172.31.64.11/20 dev eth0
ip addr add 172.31.64.12/20 dev eth0
ip addr add 172.31.64.13/20 dev eth0
```

Now this configuration will remain intact and persist through reboots. Remember this will only work if those addresses are assigned via the ENI attached to the instance. If you configure the OS to add an address to an eth device the OS will not return any errors. However, traffic will have nowhere to pass through. This means all configurations using those addresses specified will have nowhere to go and fail or error.

A few things to note:

* f you forget what IP’s you have reserved for use by an ENI and your inside of the instance you can curl http://169.254.169.254/latest/meta-data/network/interfaces/macs/ENIMAC/local-ipv4s. 
* Remember to change ENIMAC with your ENI’s mac address.
* If you need to add an additional ENI on the same instance, remember to change the dev accordingly when you run ip addr add.

> <cite>- FIN -</cite>
> <cite>Christopher L Medina</cite>
> <cite>Solutions Architect - Masterthe.Cloud</cite>