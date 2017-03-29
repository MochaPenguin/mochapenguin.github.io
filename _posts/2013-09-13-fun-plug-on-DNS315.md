---
published: false
layout: post
title: Install fun_plug (0.7) on D-Link DNS-315 (NAS)
excerpt: fun_plug on DNS-315
categories: Articles
tags:
  - fun_plug
  - DNS-315
comments: true
---

- Table of Contents {:toc}

# Introduction

These are my notes on installing and configuring fun_plug (0.7) on D-Link DNS-315 (NAS). If you come across anything that doesn't come across clear please leave a comment and I shall attempt to add more explanation.

For me the primary motive for installing fun_plug was to enable SSH. This allows me to mount directories on my NAS onto my laptop / desktop.

# Resources

- NAS-TWEAKS.NET article

# Pre-requisites

## Remote Backup

Please ensure that the internal "Remote Backup" Services is deactivated. This server is shipped with newer NAS like the **DNS-320/DNS-320L/DNS-325/DNS-345** and conflicts with the SSH-Server of the fun_plug.
