+++
date = 2014-10-22
draft = false
slug = "fix-glibc2-14-not-found-on-debian"
tags = ["Ghost"]
title = "Command line only on linux servers"
+++

* First you’ll need to find out the URL of the latest Ghost version. It should be something like ```http://ghost.org/zip/ghost-latest.zip.```

* Fetch the zip file with ```wget http://ghost.org/zip/ghost-latest.zip``` (or whatever the URL for the latest Ghost version is).

* Delete the old ```core``` directory from your install

* Unzip the archive with ```unzip -uo ghost-latest.zip -d path-to-your-ghost-install```

* Run ```npm install --production``` to get any new dependencies

* Finally, restart Ghost so that the changes will take effect

想不到Ghost更新还是挺简单的,几分钟就把Ghost更新到了0.5.3