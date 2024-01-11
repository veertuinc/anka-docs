---
date: 2019-12-12T00:00:00-00:00
title: "FAQS"
weight: 99
description: >
  Frequently Asked Questions
---

## How do we differ from competition?

#### Mature and Battle-tested Ecosystem

It's important to understand that virtualization on Intel and Silicon differ drastically behind the scenes.

 - For **Intel**, Anka has always provided the most feature-rich and performant solution for enterprise grade macOS virtualization. We built a custom virtualization on top of Hypervisor framework to meet our customers needs for maximum performance and flexibility.
 - For **Silicon**, Apple now requires all providers build on the [Virtualization Framework](https://developer.apple.com/documentation/virtualization) and this limits the features and performance available for everyone.

For us at Veertu, it therefore becomes more about focusing on taking a holistic approach to the tools your team needs and ensuring they all function in an optimal way to maximize your CI and Automation task performance. This is where each of our custom built and flexible components of the [Anka Build Cloud]({{< relref "what-is-anka.md" >}}) shine compared to other solutions.

{{< include file="_partials/what-is-anka/_short-what-is.md" >}}

#### World Class Support

Our users can also expect that when features are not available, we'll work with you to define them to meet your needs. Our attentive and friendly support team is a major benefit to your ability to get up and running quickly, as well as help solve problems when they arise.

---

## Ventura Host OS, Sonoma (14.x) VM creation requirements

On Silicon/ARM, creating macOS 14.x (or higher) VMs on Ventura Host OS requires Xcode 15 or greater. Alternatively, you can install the Device Support Image/dmg Apple provides.

---

## Nested Virtualization?

Apple’s `Virtualization.Framework` on Apple Silicon/ARM doesn’t support nested virtualization such as Android emulators or Docker inside macOS VMs.

---

## Keychain not accessible over SSH

Often users who want to use credentials in the VM's keychain for things like codesigning will experience errors when using SSH. This is due to SSH not unlocking the keychain by default. You can add the following to your `.zprofile`, or run it manually to unlock the keychain:
```bash
security set-key-partition-list -v -S apple-tool:,apple: -s -k <Login Keychain Password> <Login Keychain Path> > 2>&1 > /dev/null
```

---

## Unable to activate license

In order to activate your anka license with `sudo anka license activate {LICENSE}` the host must have access to two different urls in your firewall rules. Please reach out to us and we can provide them.

---
