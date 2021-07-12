---
title: How to setup Pentaho on MacOS BigSur
author: Jacky Tang
date: 2021-07-11 14:56:00 +0800
categories: [Blogging, MacOS, Pentaho]
tags: [pentaho]
---

I’ve struggled several hours trying to figure out how to get the pentaho data integration app running on my macOS BigSur 11.2.3. When I ran spoon.sh or spoon.command in the terminal, the interface will eventually show up for a second but then will crash immediately.

![](https://cdn.jsdelivr.net/gh/jackytang/jackytang.github.io@gh-pages/assets/img/post/2021/pentaho-1.png)

I was finally able to get it to run with Eris’s method. It’s seems that we need to tell Pentaho application specifically where java is located. So starting over, my final steps are:

## Install Pentaho

```bash
brew install --cask data-integration
```

```bash
==> Downloading https://downloads.sourceforge.net/pentaho/pdi-ce-9.1.0.0-324.zip
==> Downloading from https://versaweb.dl.sourceforge.net/project/pentaho/Pentaho
######################################################################## 100.0%
==> Verifying SHA-256 checksum for Cask 'data-integration'.
==> Installing Cask data-integration
==> Moving App 'Data Integration.app' to '/Applications/Data Integration.app'.
🍺  data-integration was successfully installed!
```

The .app file will be moved to the application folder when using brew cask install.

```bash
## To find out where the folder is located:
brew info data-integration
```

```bash
data-integration: 9.1.0.0-324
https://sourceforge.net/projects/pentaho/
/usr/local/Caskroom/data-integration/9.1.0.0-324 (1,422 files, 1.8GB)
From: https://github.com/Homebrew/homebrew-cask/blob/HEAD/Casks/data-integration.rb
==> Name
Pentaho Data Integration
==> Description
End to end data integration and analytics platform
==> Artifacts
data-integration/Data Integration.app (App)
==> Analytics
install: 37 (30 days), 136 (90 days), 651 (365 days)
```

## Install Java

```bash
brew tap adoptopenjdk/openjdk
brew cask install adoptopenjdk8
```

## Modify JavaApplicationStub

```
cd "/Applications/Data Integration.app/Contents/MacOS"
nano JavaApplicationStub
```

Replace the content with following:

```bash
#!/bin/sh

# PROG_DIR=$(cd "$(dirname "$0")"; pwd)
# PROG_DIR is in .app/Contents/MacOS
# BASE_DIR="$PROG_DIR"/../../../
# cd "$BASE_DIR"
# . "spoon.command" "$BASE_DIR"

BASE_DIR="/usr/local/Caskroom/data-integration/9.1.0.0-324/data-integration"
cd "$BASE_DIR"
. "spoon.command" "$BASE_DIR"
```

## Setup correct pentaho env in spoon.sh

```bash
cd /usr/local/Caskroom/data-integration/9.1.0.0-324/data-integration 
code ./spoon.sh
```

```bash
# Add environment variables
export PENTAHO_JAVA_HOME="/Library/Java/JavaVirtualMachines/adoptopenjdk-8.jdk/Contents/Home/"
```

## Download Standard Widget Toolkit For Mac OS X (Cocoa)

[https://mvnrepository.com/artifact/org.eclipse.platform/org.eclipse.swt.cocoa.macosx.x86_64](https://mvnrepository.com/artifact/org.eclipse.platform/org.eclipse.swt.cocoa.macosx.x86_64)

## Replace Standard Widget Toolkit For Mac OS X (Cocoa)

```bash
cp ~/Downloads/swt.jar /usr/local/Caskroom/data-integration/9.1.0.0-324/data-integration/libswt/osx64/swt.jar
```

## Run Data Integration app or spoon.sh

![](https://cdn.jsdelivr.net/gh/jackytang/jackytang.github.io@gh-pages/assets/img/post/2021/pentaho-2.png)

## References

[https://j3ang.com/data/2020/04/10/sql-window_function.html](https://j3ang.com/data/2020/04/10/sql-window_function.html)

