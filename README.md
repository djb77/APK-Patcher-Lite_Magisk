# Magisk Module Template

**Update `README.md` if you want to submit your module to the online repo!** This `README.md` will be shown in a Webview dialog when a user taps your module in Magisk Manager, so make sure to place some information / changelog / notes here.

If you are not familiar with the Markdown syntax, you can start by experimenting on GitHub's online Markdown editor, which will let you preview before publishing. If you need more help, the [Markdown Cheat Sheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet) will be handy.

For more information about modules and repos, please check the [official documentation](https://topjohnwu.github.io/Magisk/)


# APK-Patcher Lite

Flashable Zip Template for Modifying System APKs On-Device

Based on [APK-Patcher](https://github.com/osm0sis/APK-Patcher) by osm0sis @ xda-developers


## Information

This is a modified version of APK-Patcher that will delete / inject files into System APK files instead of using a baksmali / apktool method.

The method used here is a similar method used in my ROMs to patch files, where instead of having to have like for example 4 SystemUI files, I only needed to keep the actual files that were changed. This method could also be used quite easily to apply OTA updates or addons on already pre-modified APK files.


## Usage

* Copy your pre-compiled resource files (including .dex files) to the patch folder, removing the .apk part of the filename (ie: SystemUI)

* Create a file in scripts with the same name (ie: SystemUI.sh) if you want to delete any existing files from the APK

* Make edits to envvar.sh, and also to extracmd.sh if needed


### Properties / Variables (envvar.sh)

* banner="";
* apklist="";
* apkbak=/data/media/0/APK-Backup;
* backup=1;
* cleanup=1;

**banner** is the name of your patch zip, usually suggestive of what it does, to be displayed at the beginning of the zip flash. You should include your name/handle here like "by osm0sis @ xda-developers" for credit purposes.

**apklist** is a string containing the list of APKs to be patched included in the patch zip, separated by spaces between the quotes. Each APK is automatically found recursively in /system, then copied to the working directory to be decompiled and acted on, then copied back to /system.

**apkbak** is the location to place backups of the untouched APKs in apklist if backup=1 is set.

**backup=1** will store backups of the untouched APKs in the location specified in apkbak.

**cleanup=0** will keep the zip from removing it's working directory in /tmp/apkpatcher - this can be useful if trying to debug in adb shell whether the patches worked correctly. cleanup=1 is necessary on multi-APK patching zips, so it's recommended each APK to be patched be tested on their own with cleanup=0 before combining into a single zip.


### envvar.sh

Modify the envvar.sh to add your banner, apklist, backup and cleanup options

Multiple files can be patched, put a space between the filenames (ie: apklist="file1.apk file2.apk";)


### extracmd.sh

Modify the extracmd.sh to add any additional commands to be performed at the end of the patching process that aren't patch-related (/data file changes,


### scripts/$apkname.sh

* fileremove="";

$apkname is the name of the folder that you put your resources files in. Copy scripts_sample.sh and rename it to your APK (ie: SystemUI.sh)

Multiple files can be deleted, put a space between the paths (ie: fileremove="/res/drawable/file1.png /res/drawable/file1.png";)


