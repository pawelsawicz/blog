+++
date = "2013-09-17T23:24:02+01:00"
description = "New blogpost for today"
draft = false
title = "Terraform in game development - Don’t Starve Together"
weight = 20

+++

dont_starve_together_blogpost

A while ago you could read about Vagrant for Don't Starve Together (aka DST), this time let's discuss about similar tool named Terraform.

As in vagrant blogpost I am playing Terraform with DST, because it's quite good aspect and problem were you can check out tools like Terraform.

You may wondering what do you have with those all games ? Games for me were *THIS* thing that I started programming, it all has begun from Helbreath, were as a 13 years old I wanted to create my own server so I had to compile C++ source code, it was challenging for somebody with zero knowledge of C++, so that's why I still love doing private server and giving back to gamers community.

Terraform is a tool that allows you to describe your infrastructure as a code, it means that you can write json-like description and it tells to terraform what it has to do to spin up required parts of infrastructure to achieve fully working environment. This tool is ideal for any kind of cloud provider it supports, Amazon, AWS, DigitalOcean

I have created terraform configuration for DST. Main place where my infrastructure described is vm-dontstarve.tf

This block describes that we going to use AWS provider, with access_key, secret_key and specific region. ${var.aws_access_key}  and ${var.aws_secret_key} this is how you define variables.

provider "aws" {
    access_key = "${var.aws_access_key}"
    secret_key = "${var.aws_secret_key}"
    region = "eu-west-1"
}

In this part, we can see block that has information about AWS instance,  what AMI to use (image of OS), what type of instance, ssh key name, your custom security group and additionally how to connect with our VM during provisioning step.

resource "aws_instance" "dontstarve" {
    ami = "ami-cb4986bc"
    instance_type = "t1.micro"
    key_name = "dontstarve"
    security_groups = ["${aws_security_group.dontstarve_port.name}"]

    connection {
        user = "ubuntu"
        key_file = "~/Downloads/dontstarve.pem"
    }
}

Here we describe provisioning step, we can copy file, execute remote command, or execute command locally.

provisioner "remote-exec" {
      inline = "mkdir -p ~/tmp/provisioning"
    }

    provisioner "file" {
        source = "./provisioning/provision_user.sh"
        destination = "~/tmp/provisioning/provision_user.sh"
    }

    provisioner "file" {
        source = "./provisioning/provision_dontstarve.sh"
        destination = "~/tmp/provisioning/provision_dontstarve.sh"
    }

    provisioner "remote-exec" {
        inline = [
          "chmod +x ~/tmp/provisioning/provision_user.sh",
          "~/tmp/provisioning/provision_user.sh"
        ]
    }

    provisioner "file" {
        source = "./config/server_token.txt"
        destination = "/home/steam/.klei/DoNotStarveTogether/server_token.txt"
    }

    provisioner "file" {
        source = "./config/settings.ini"
        destination = "/home/steam/.klei/DoNotStarveTogether/settings.ini"
    }

    provisioner "remote-exec" {
        inline = [
          "chmod +x ~/tmp/provisioning/provision_dontstarve.sh",
          "~/tmp/provisioning/provision_dontstarve.sh"
        ]
    }

Next one aws_security_group block, which describe your topology of ports and connections between your machines. DST requires port 10999 UDP protocol as incoming traffic so we have to add ingrees rule, in "egress" we specify that we allow to any outside traffic on any port and protocol.

resource "aws_security_group" "dontstarve_port" {
  name = "dontstarve_port"
  description = "Allow communicate with dontstarve"

  ingress {
      from_port = 10999
      to_port = 10999
      protocol = "udp"
      cidr_blocks = ["0.0.0.0/0"]
  }

  egress {
      from_port = 0
      to_port = 0
      protocol = "-1"
      cidr_blocks = ["0.0.0.0/0"]
  }
}

Last security rule (ingress) open SSH connection over 22 port, require to connect with our machine and to do provisioning.

ingress {
      from_port = 22
      to_port = 22
      protocol = "tcp"
      cidr_blocks = ["0.0.0.0/0"]
}

For a long time I was wondering if tools like Vagrant, Terraform could be helpful in game development especially to maintain server infrastructure, and in my opinion they are! Soon I will blog more about hashicorp products and their usage in game development.