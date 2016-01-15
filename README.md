#!/bin/bash

#
# This script will update your OpenHAB 2 installation and add any OpenHAB 1 addons that you want to bring across
# The script assumes that the OpenHAB 2 is already installed.  Please shut it down and then run the script
# If OpenHAB 2 is not already installed, create the directories ahead of time, and it will install OpenHAB 2
#
# This script will also update or install Smarthome Designer if desired.  Please make sure your work is saved
#  before updating.
#
# v2 Greatly simplified for the new OpenHAB2 Karaf online distribution
# v1.1 Jim Howard
#

# The location for archive downloads
DOWN="/opt/DOWN"
# The location of the openHAB installation
HAB="/opt/openHAB"
# The location of SmartHome Designer
SMD="/opt/ohdesign"

cd $DOWN

wget -N https://openhab.ci.cloudbees.com/job/openHAB-Distribution/lastSuccessfulBuild/artifact/distributions/openhab-online/target/openhab-online-2.0.0-SNAPSHOT.zip

# Uncomment the following line if Habmin2 is desired
wget -N --content-disposition --no-check-certificate https://raw.githubusercontent.com/cdjackson/HABmin2/master/output/org.openhab.ui.habmin_2.0.0.SNAPSHOT-0.0.15.jar

# Uncomment the following line if SmartHome Designer is desired
wget -N http://download.eclipse.org/smarthome/nightly-snapshots/eclipsesmarthome-incubation-0.8.0-SNAPSHOT-designer-linux64.zip

cd $HAB
rm -rf $HAB/runtime

unzip -o $DOWN/openhab-online-2.0.0-SNAPSHOT.zip

# clear up some windows clutter not needed with *nix systems
rm $HAB/*.bat

cd $HAB
#add any OpenHAB 2 bindings to the list in the following line.  Make sure any wildcards select only the binding you want
#unzip -o $DOWN/distribution-2.0.0-SNAPSHOT-addons.zip *xmpp*.jar *http*.jar *network*.jar *insteon*.jar *rrd*.jar *logging*.jar *action.mail*.jar *binding.weather*.jar *zwave*.jar *yahoo*.jar *sonos*.jar *hue*.jar *nest*.jar
# This is not needed any longer as all addons can be installed with the PaperUI.  For defaulting addons, exit the service/addons.cfg

# uncomment the following line if the habmin2 jar is to be copied in
# this will need to be updated when the new habmin is available
cp $DOWN/org.openhab.ui.habmin_2.0.0.SNAPSHOT-0.0.15.jar $HAB/addons

# uncomment the following 4 lines if Eclipse Designer is also to be updated.  Be sure to save your work before updating.

cd $SMD
rm -rf $SMD/p2/* $SMD/plugins 
unzip -o $DOWN/eclipsesmarthome-incubation-0.8.0-SNAPSHOT-designer-linux64.zip
