+++
date = 2016-04-29
draft = false
slug = "how-to-uninstall-jdk-on-mac-os-x"
tags = ["Mac"]
title = "How to Uninstall JDK on Mac OS X"
+++


#### Remove the Java Runtime
```bash
sudo rm -fr /Library/Internet\ Plug-Ins/JavaAppletPlugin.plugin 
sudo rm -fr /Library/PreferencePanes/JavaControlPanel.prefpane
```

#### Removing the Java JDK
```bash
cd /Library/Java/JavaVirtualMachines
sudo rm -rf jdk*.jdk
```