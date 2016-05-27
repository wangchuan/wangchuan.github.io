---
layout: post
title: "Ubuntu Command"
category: programming
tags: [ubuntu, os]
use_math: true
---

### Ubuntu Command
#### Redirect to files
```bash
~/caffe$ ./build/tools/caffe train --solver=./examples/mst/lenet_solver.prototxt 2>&1 | tee ~/caffetrain.log
```
#### CMD to install .deb file
```bash
sudo dpkg -i DEB_PACKAGE
```

#### Date and Time
```bash
#!/usr/bin/env sh
mkdir snapshots
NOW=$(date +"%Y-%m-%d_%H-%M-%S")
EXPNAME=$NOW
../../../../build/tools/caffe train --solver=solver.prototxt 2>&1 | tee $EXPNAME.log
```

#### While screen shot
```bash
COUNTER=0  
while [  $COUNTER -lt 1000 ]; do  
    echo The counter is $COUNTER
	NOW=$(date +"%Y-%m-%d_%H-%M-%S")
	import -window root ~/Dropbox/LinuxScreen/screenshot$COUNTER-$NOW.jpg
	sleep 600
    let COUNTER=COUNTER+1   
done 
```

#### Check GPU Memory
```bash
nvidia-smi -l
```

#### Install `SmartGit` with dependency `Java`
```bash
sudo apt-get install default-jre
sudo add-apt-repository ppa:webupd8team/java
sudo apt-get update
sudo apt-get install oracle-java7-installer
sudo apt-get install oracle-java7-set-default
```

#### Appearance Enhancement: `Unity Tweak Tool`
```bash
sudo apt-get install unity-tweak-tool
```

#### Move directory
```bash
sudo mv fromPath/ toPath/
```

#### Add Path
```bash
PATH="$PATH:$HOME/bin"
```

#### Download files from URL and save
```bash
mkdir data
for i in `seq 1 500`; 
do wget http://app02.szaic.gov.cn/aiceqmis.webui/CheckCode.aspx?key=0 -O real-data/$i.gif; 
done
```

