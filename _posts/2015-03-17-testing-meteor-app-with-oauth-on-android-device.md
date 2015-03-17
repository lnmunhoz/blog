---
layout: post
title:  "Testing Meteor Application with OAuth on Android device"
date:   2015-03-17 19:01:00
categories: meteor cordova
---

## Facebook Setup

1. On your facebook developer account, go to `My Apps` menu and select your app.
2. On `General` section, change the `Mobile Site URL` to `http://<your-app-name>.meteor.com`.
3. Now go to the `Advanced` section, scroll down a little bit. You will find a label called `Valid OAuth redirect URIs`, add the same url of step two

## Building and Testing

Connect your android device on the computer. After that, go to your terminal.

```
$ cd your-app-folder
```

```
$ meteor run android-device --mobile-server http://<your-app-name>.meteor.com
```

If the gods are with you, the application will pop up at your phone.
Good luck!
