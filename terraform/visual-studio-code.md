---
description: VS Code integration with Terraform
---

# Visual Studio Code

## Introduction

Visual Studio Code runs on Windows.  In order to use Terraform \(installed on the Linux subsystem\) it has to be [installed](https://docs.microsoft.com/en-us/windows/wsl/install-win10#install-the-windows-subsystem-for-linux).  The VS code [Terraform extension by Mikael Olenfalk](https://marketplace.visualstudio.com/items?itemName=mauve.terraform) also needs to be installed.  Terraform itself be [installed in Linux](https://askubuntu.com/questions/983351/how-to-install-terraform-in-ubuntu) on the Windows 10 operating system, as the application.  Lastly the AWS \(or Azure\) [CLI is required to be installed \(using pip3\)](https://docs.aws.amazon.com/cli/latest/userguide/install-linux-al2017.html) on the Linux subsystem so that Terraform can reach the cloud platform to provision infrastructure.

## Create a Basic Resource File



