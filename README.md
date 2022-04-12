#!/bin/bash

open -a virtualbox ; sleep 10 && sudo kill `ps -ef | grep VirtualBox | awk '{print $2}'`

echo " Quelle est la version récente de VirtualBox que vous souhaitez installer comme mise à jour ? "; read nv ; v="$nv"

echo " Quel est le n° de la build récente de VirtualBox que vous souhaitez installer comme mise à jour ? "; read nb ; b="$nb"

cd /Users/$(logname)/Downloads
sudo rm -iRv ./VirtualBox\ 6.*
mkdir VirtualBox\ "$nv"\ Build\ "$nb"
cd ./VirtualBox\ "$nv"\ Build\ "$nb"/

wget https://download.virtualbox.org/virtualbox/"$v"/VirtualBox-"$v"-"$b"-OSX.dmg https://download.virtualbox.org/virtualbox/"$v"/VBoxGuestAdditions_"$v".iso https://download.virtualbox.org/virtualbox/"$v"/Oracle_VM_VirtualBox_Extension_Pack-"$v"-"$b".vbox-extpack

MOUNTDIR=$(echo `hdiutil mount /Users/$(logname)/Downloads/VirtualBox\ "$v"\ Build\ "$b"/VirtualBox-"$v"-"$b"-OSX.dmg | tail -1 | awk '{$1=$2=""; print $0}'` | xargs -0 echo)

sudo installer -pkg "${MOUNTDIR}"/*.pkg -target /

diskutil unmount "${MOUNTDIR}"

cd

open -a virtualbox ; vboxmanage --version
sudo vboxmanage extpack install /Users/$(logname)/Downloads/VirtualBox\ "$v"\ Build\ "$b"/Oracle_VM_VirtualBox_Extension_Pack-"$v"-"$b".vbox-extpack --replace
