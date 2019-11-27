# Ping Apt Tool Package  Creation
This walkthrough takes you step by step on how to create an apt package from your .NET core application the same can be easily installed on user's client machine.

# Deployment Steps
<b>Step 1:</b> Click on the <b>'Deploy to Azure'</b> button below </br>

<a href="https://azuredeploy.net/" target="_blank"><img src="http://azuredeploy.net/deploybutton.png"/></a>

<b>Step 2:</b> Fill template form as shown below, fill the KeyVault Name with only letters between 3-24 with no numbers or special characters, ObjectId is a GUID string obtained from Azure CLI in portal, in PowerShell mode of Azure CLI type 'Get-AzADUser -UserPrincipalName foo@domain.com' repalcing foo@domain.com with your own Azure Portal email and pasting the Object Id string in the form below as shown, click <b>'Next' </b>button </br>
<img src="https://komalsandboxdiag.blob.core.windows.net/pingarmtemplatereadmefiles/1%20new.png">
<b>Azure CLI</b> How to obtain <b>'ObjectId'</b> field value from Azure CLI</br>
<img src="https://komalsandboxdiag.blob.core.windows.net/pingarmtemplatereadmefiles/0.5.png">





<b>Step 2:</b> After ssh into the VM, you type the following to install</br>
<b>
$ wget -q https://packages.microsoft.com/config/ubuntu/18.04/packages-microsoft-prod.deb</br>
$ sudo dpkg -i packages-microsoft-prod.deb</br>
$ sudo apt-get install apt-transport-https</br>
$ sudo apt-get update</br>
$ sudo apt-get install dotnet-sdk-3.0</br>
$ sudo dotnet --version</br>


$ git clone https://airband@dev.azure.com/airband/Komal/_git/Komal</br>
$ sudo dotnet build
$ sudo dotnet publish -c Release --self-contained -r ubuntu.18.04-x64</br>


#run this
$ sudo dotnet tool install -g dotnet-symbol</br>
#okif this doesnt run as the .NET SDK is 2.1
$ sudo dotnet-symbol --symbols bin/Release/netcoreapp3.0/ubuntu.18.04-x64/publish/</br>


#run the following
$ sudo dotnet tool install -g dotnet-symbol</br>

#skip this
$ sudo dotnet-symbol --symbols bin/Release/netcoreapp3.0/ubuntu.18.04-x64/publish/</br>

#RUN THIS!!
$ sudo apt install gnupg pbuilder ubuntu-dev-tools apt-file dh-make bzr-builddeb</br>


$ export DEBEMAIL="v-kosyed@microsoft.com"</br>

$ export DEBFULLNAME="PingAsync Tool"</br>

$ sudo dotnet clean</br>

$ sudo rm -rf bin obj</br>

$ cd ..</br>

$ sudo mv PingAsync pingasync-2.0</br>

$ sudo tar cvzf pingasync-2.0.tar.gz pingasync-2.0</br>

$ sudo cd pingasync-2.0</br>

$ sudo dh_make -f ../pingasync-2.0.tar.gz -s -c mit -n</br>

$ cd debian </br>
$ sudo rm *ex *EX </br>
$ sudo rm README README.Debian README.source pingasync-docs.docs </br>

Files Changes

changelog

"
pingasync (1.0-0ubuntu1) bionic; urgency=medium

  * Initial Release.

 -- Karam Yaaqba <v-kayaaq@microsoft.com>  Tue, 29 Oct 2019 15:49:48 -0700
"

control 

"
Source: pingasync
Section: unknown
Priority: optional
Maintainer: root <v-kosyed@microsoft.com>
Build-Depends: debhelper (>= 10)
Standards-Version: 4.1.2
Homepage: <insert the upstream URL, if relevant>
#Vcs-Git: https://anonscm.debian.org/git/collab-maint/pingasync.git
#Vcs-Browser: https://anonscm.debian.org/cgit/collab-maint/pingasync.git

Package: pingasync
Architecture: any
Depends: ${shlibs:Depends}, ${misc:Depends}
Description: <insert up to 60 chars description>
 <insert long description, indented with spaces>

"

rules

"
#!/usr/bin/make -f
#export DH_VERBOSE = 1

%:
	dh $@ --with=systemd


override_dh_auto_build:
	dotnet publish -c Release --self-contained -r ubuntu.18.04-x64
	# auto-build disabled
override_dh_auto_install:
	# install application
	mkdir -p debian/pingasync/opt/kyaaqba/pingtool
	install -D -m 755 bin/Release/netcoreapp3.0/ubuntu.18.04-x64/publish/* debian/pingasync/opt/kyaaqba/pingtool
	rm debian/pingasync/opt/kyaaqba/pingtool/*.pdb #delete pdb

"

sudo vim pingasync.service

"
[Unit]
Description=Ping Async
After=network.target

[Service]
Type=simple
ExecStart=/opt/ksyed/pingtool/pingtool
ExecReload=/bin/kill -HUP $MAINPID

[Install]
WantedBy=multi-user.target

"

$ cd ..</br>

$ sudo dpkg-buildpackage -b --no-sign</br>


$ cd ..
$ sudo dpkg -i pingasync_2.0-0ubuntu1_amd64.deb

$ sudo apt-get install gnupg rng-tools
$ gpg --gen-key
#copy the key above

# position in directory /Komal/Asyn-r-code/repo
$ sudo apt-get install reprepro
$ mkdir repo && cd repo
$ mkdir conf && cd conf
$ touch distributions

$ sudo vim distributions


#change your file name and put the key SignWith below
# this is the pub key id given earlier step 
Origin: PingAsync Tool
Label: pingasync
Codename: bionic
Architectures: amd64
Components: main
Description: Personal repository
SignWith: B501DE17DA19A16F 



 #give path of repo and also of the deb file if you in the folder where both
 #the repo folder and the deb file are on same level then dont need to give it a path
$ sudo reprepro --basedir repo includedeb bionic pingasync_4.0-0ubuntu1_amd64.deb

sudo reprepro --basedir repo includedeb bionic pingasync*.deb


$ reprepro --basedir repo list bionic


$ gpg --output PUBLIC.KEY --armor --export v-kosyed@microsoft.com
$ mv PUBLIC.KEY repo

$ cd repo
$ sudo mv PUBLIC.KEY
$ sudo mv db
$ sudo mv conf
$ sudo mv pool


$ git clone ---- distination url of an online empty repo
$ git add --all
$ git commit -m "Package repository"
$ git push

#enter username and password for your git repo


</b>
<b> At this stage the Ping Utilty is installed in your system ready to be used! </b></br>
<img src="https://komalsandboxdiag.blob.core.windows.net/pingarmtemplatereadmefiles/26.png" >





