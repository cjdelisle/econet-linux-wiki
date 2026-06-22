---
title: EcoNet Linux Software
description: Software and tools for supporting EcoNet devices
published: true
date: 2026-06-22T11:10:44.818Z
tags: 
editor: markdown
dateCreated: 2026-06-22T11:10:44.818Z
---

# EcoNet Linux Software
Here are some software and utilities which support EcoNet Linux devices.

## Embedded-Tools
https://github.com/cjdelisle/embedded-tools

This is a batch of utilities for controlling embedded devices, with some specific to EcoNet. This contains automated testing support wherein a device can be power cycled, a new firmware uploaded, flashed, and booted, then a number of tests are run as defined in `device_tests.js`.

## OpenWrt-EN751221-Builds
https://github.com/cjdelisle/OpenWrt-EN751221-Builds/

This repository has a github action which builds OpenWrt for various EcoNet devices. It also performs a test run using devices which are currently setup for testing (right now SmartFiber XP-8421B and ChinaMobile GS3101.

### Getting your OpenWrt PR tested
As soon as [#23533](https://github.com/openwrt/openwrt/pull/23533) (GS3101 support) lands, you will be able to test any OpenWrt branch by simply updating the repo and hash in [build.sh](https://github.com/cjdelisle/OpenWrt-EN751221-Builds/blob/main/build.sh#L3) and making a PR to OpenWrt-EN751221-Builds. For the moment, testing will only occur when your PR has been accepted, but there's generally no reason not to do that.

This is an example of a test run: https://github.com/cjdelisle/OpenWrt-EN751221-Builds/actions/runs/27863459590/job/82463380886 you should be able to see logs from the *Run device tests* job if you are signed in to Github.