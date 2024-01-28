---
title: "APT install kali linux repos in Ubuntu 22.04"
date: 2024-01-28T15:11:20-05:00
draft: false
tags: [security, linux]
---

If you recently tried adding a new linux source repository in Ubuntu 22.04, then you might have seen this warning mesage:

```
Warning: apt-key is deprecated. Manage keyring files in trusted.gpg.d instead (see apt-key(8)).
```

Apt-key is a sub-command of apt used to manage asymmetric encryption keys in order to authenticate packages. It was found some time ago to be vulnerable to man-in-the-middle attacks (see [CVE-2011-3374](https://security-tracker.debian.org/tracker/CVE-2011-3374)). Now it has finally been deprecated and will be deleted at some point in the future.

In this post I will show you the proper cool new way of adding linux repositories to your apt lists. I will use the kali linux source repos as an example.

APT uses GPG to verify the authenticity and integrity of installed packages. Gnu Privacy Guard or GPG is a program that implements the OpenPGP standard. Package maintainers will use their gpg private key to generate a digital signature of their package and you as a user of that package will use their trusted public key to verify that signature to make sure the package hasn't been tampered with. If you want to learn more about public key cryptography and GPG I recommend this really well done series of [articles](https://tutonics.com/articles/gpg-encryption-guide-part-1/).

## Let's get down to business

First download and import the kali gpg public key:

```
wget -q -O - https://archive.kali.org/archive-key.asc | gpg --import
```

Unpack the gpg key to convert it from ASCII to binary format and add it to the keyrings folder. As an alternative you can add it to /etc/apt/keyrings.

```
cat archive-key.asc | gpg --dearmor > kali-keyring.gpg
cat kali-keyring.gpg | sudo tee /usr/share/keyrings/kali-keyring.gpg > /dev/null
```

Add the kali repository to the list of repositories that apt will go through when installing a new package.

```
echo 'deb [arch=amd64 signed-by=/usr/share/keyrings/kali-keyring.gpg] https://http.kali.org/kali kali-rolling main non-free contrib' | sudo tee /etc/apt/sources.list.d/kali.list
```

The important part here that changes from adding a source repository with apt-key is the use of the `signed-by` predicate which references the file holding our previously downloaded kali public key.

Next, set the preference for the kali repository. This allows apt to first go through the kali repos before checking other sources.

```
sudo sh -c "echo 'Package: *' > /etc/apt/preferences.d/kali.pref; echo 'Pin: release a=kali-rolling' >> /etc/apt/preferences.d/kali.pref; echo 'Pin-Priority: 50' >> /etc/apt/preferences.d/kali.pref"
```

Finally update apt. This tells apt to download the most up to date matadata for all configured sources.

```
sudo apt update
```

If all goes well you should now be able to install packages from the kali source repositories.

```
sudo apt install wordlists
```

And voila, you are now a humble hacker with your Ubuntu machine!

