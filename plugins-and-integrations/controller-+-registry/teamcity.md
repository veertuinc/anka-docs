---
date: 2019-07-03T22:24:47-05:00
title: "Using Teamcity and the Anka Build Cloud"
linkTitle: "Teamcity"
weight: 3
description: >
  Instructions on how to use Teamcity with the Anka Build Cloud
---

## VM Template & Tag Requirements

1. **In the VM**, install the proper JDK/JAVA version. We recommend [Zulu](https://www.azul.com/downloads/?package=jdk#download-openjdk).

## Usage

1. [Download the latest plugin zip.](https://veertu.com/downloads/ankabuild-tc-latest/).
2. Upload it to your Teamcity.
3. Edit your project and under Cloud Profiles add a new Profile. For Cloud type, choose Anka Build Cloud and specify the URL for your Controller.
4. Choose the template you created for Teamcity and specify any other required fields.
5. Run your job and then watch the Controller's Instances page to ensure an instance is starting.

---

## Release Notes

Release notes for the Anka Github Actions can be found on github using the above URLs.
