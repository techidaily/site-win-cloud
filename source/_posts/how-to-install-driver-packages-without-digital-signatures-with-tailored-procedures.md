---
title: How to Install Driver Packages Without Digital Signatures with Tailored Procedures
date: 2024-10-05T01:24:49.389Z
updated: 2024-10-11T00:16:04.179Z
tags:
  - application-packaging-training
categories:
  - advancedinstaller
description: This Article Describes How to Install Driver Packages Without Digital Signatures with Tailored Procedures
thumbnail: https://thmb.techidaily.com/0c8f696950ea736c2174f2d7e8a74906124afdbd8faac5e2796b198a9b431fdb.jpg
---

## How to Install Driver Packages Without Digital Signatures with Tailored Procedures

>  Disclaimer: This post includes affiliate links
>
>  If you click on a link and make a purchase, I may receive a commission at no extra cost to you.
>

## Install Unsigned Drivers

There are cases when we try to install drivers on Windows and the following windows appears:

![signerror when try to install drivers on Windows](https://cdn.advancedinstaller.com/img/install-unsigned-drivers-using-custom-actions/signerror.png "signerror when try to install drivers on Windows")  

And we must click : "Install this driver software anyway". 

This is happening because that particular driver is unsigned. In the IT Pros world we need to make sure that these types of drivers are installed silently and no interaction to the user is necessary. We can install the driver without any prompt in following way :

Tools that you need: (most are from the Windows Driver Kit – the latest version of [Windows Driver Kit W11 22H2](https://learn.microsoft.com/en-us/windows-hardware/drivers/download-the-wdk "Windows Driver Kit W11 22H2")):

[Inf2Cat.exe](https://learn.microsoft.com/en-us/windows-hardware/drivers/devtest/inf2cat "Inf2Cat.exe") (To generate the unsigned catalog file from our INF)

[Makecert.exe](https://learn.microsoft.com/en-us/windows/win32/seccrypto/makecert "Makecert.exe") (Used to create our certificate)

[Signtool.Exe](https://learn.microsoft.com/en-us/windows/win32/seccrypto/signtool "Signtool.Exe") (Sign our catalog file with an Authenticode digital signature)

[Certmgr.exe](https://learn.microsoft.com/en-us/dotnet/framework/tools/certmgr-exe-certificate-manager-tool "Certmgr.exe") (Used to add and delete our certificate to the system root)

Let's have a look at each step that you must take to get your unsigned certificates installed silently. 

### Create a digital certificate by using the MakeCert tool

Open an **x86/x64 Free Build Environment** command prompt with administrator permissions, by right-clicking **x86 Free Build Environment** on the **Start menu**, and then selecting **Run as administrator**.

At the **x86/x64 Free Build Environment** command prompt, type the following command on a single line (it appears here on multiple lines for clarity and to fit space limitations):

makecert -r -n "CN=Name"
		 -ss CertStore
		 -sr LocalMachine

Copy

Ex: makecert -r -n CN="TestCert" -ss Root -sr LocalMachine

The meaning of each parameter is as follows:

**\-r**

Specifies that the certificate is to be "self-signed," rather than signed by a CA. Also called a "root" certificate.

**\-n** "CN= Name "

Specifies the name associated with this new certificate. It is recommended that you use a certificate name that clearly identifies the certificate and its purpose.

**\-ss** CertStore

Specifies the name of the certificate store in which the new certificate is placed.

**\-sr** LocalMachine

Specifies that the certificate store created by the -ss option is in the per computer store, instead of the default per user store.

![Specifies that the certificate store](https://cdn.advancedinstaller.com/img/install-unsigned-drivers-using-custom-actions/makecert.png "Specifies that the certificate store")  

<!-- affiliate ads begin -->
<a href="https://unicoeye.pxf.io/c/5597632/2134493/18498" target="_top" id="2134493">
  <img src="//a.impactradius-go.com/display-ad/18498-2134493" border="0" alt="https://techidaily.com" width="728" height="90"/>
</a>
<img height="0" width="0" src="https://unicoeye.pxf.io/i/5597632/2134493/18498" style="position:absolute;visibility:hidden;" border="0" />
<!-- affiliate ads end -->

The command returns the message "Succeeded" when the store and certificate are created.

### Create a .cat (catalog) file for the driver

We notice that some drivers don't contain a cat file, so we'll need to generate one. 

Open the .INF file in a text editor. Ensure that under the \[version\] section that you have an entry specifying a .cat file. 

If it's not there, add the lineat the end of the section. For example:

[version]Signature=xxxxxxProvider=xxxxxxCatalogFile=MyCatalogFile.cat

Copy

"MyCatalogFile.cat" is the name of the cat file that we want to generate. Not having a line specifying this will result in an "error 22.9.4 - Missing 32-bit catalog file entry" when we run Inf2Cat.exe. 

Command line:

Inf2Cat.exe /driver:"<Path to folder containing driver files

Copy

Ex : Inf2cat.exe /driver:\[PathToINFwithoutFile\] /os:10\_x64,10\_x86

The meaning of each parameter is as follows:

\- **/driver**: c:\\toaster\\device

Specifies the location of the .inf file for the driver package. You must specify the complete folder path. A '.' character does not work here to represent the current folder.

\- **/os**: 10\_x86 or 10\_x64

Identifies the 32-bit version of Windows 10 as the operating system. Run the command inf2cat /? for a complete list of [supported operating systems and their codes](https://learn.microsoft.com/en-us/windows-hardware/drivers/devtest/inf2cat "supported operating systems and their codes").

![Identifies the 32-bit version of Windows 10](https://cdn.advancedinstaller.com/img/install-unsigned-drivers-using-custom-actions/inf2cat.png "Identifies the 32-bit version of Windows 10")  

<!-- affiliate ads begin -->
<a href="https://appsumo.8odi.net/c/5597632/2123729/7443" target="_top" id="2123729">
  <img src="//a.impactradius-go.com/display-ad/7443-2123729" border="0" alt="https://techidaily.com" width="600" height="90"/>
</a>
<img height="0" width="0" src="https://appsumo.8odi.net/i/5597632/2123729/7443" style="position:absolute;visibility:hidden;" border="0" />
<!-- affiliate ads end -->

### Sign the catalog file using SignTool

signtool sign /v /sm /s Root /n "TestCert" /t http://timestamp.digicert.com path\example.cat

Copy

The meaning of each parameter is as follows:

\- **/sm**

Specifies that a machine store, instead of a user store, is used

\- **/s** CertStore

Specifies the name of the certificate store in which SignTool searches for the certificate specified by the parameter /n. In our case we look in the Root

\- **/n** “Name ”

Specifies the name of the certificate to be used to sign the package. You must include enough of the name to allow SignTool to distinguish it from others in the store. If this name includes spaces, then you must surround the name with double quotes.

\- **/t** path to time stamping service

Specifies the path to a time stamping service at an approved certification authority. If you purchase your certificate from a commercial vendor, they should provide you with the appropriate path to their service.

\- example.cat

Specifies the path and file name of the catalog file to be signed.

Signtool indicates completion with the following message:

![Successfully signed and timestamped](https://cdn.advancedinstaller.com/img/install-unsigned-drivers-using-custom-actions/signtool.png "Successfully signed and timestamped")  

<!-- affiliate ads begin -->
<a href="https://aligracehair.sjv.io/c/5597632/2016170/19272" target="_top" id="2016170">
  <img src="//a.impactradius-go.com/display-ad/19272-2016170" border="0" alt="https://techidaily.com" width="728" height="90"/>
</a>
<img height="0" width="0" src="https://aligracehair.sjv.io/i/5597632/2016170/19272" style="position:absolute;visibility:hidden;" border="0" />
<!-- affiliate ads end -->

Successfully signed and timestamped: C:\\toaster\\device\\example.cat

### Export the certificate from certstore manually

Run an administrator command: certmgr.exe

Export the certificate manually:

![Export the certificate manually](https://cdn.advancedinstaller.com/img/install-unsigned-drivers-using-custom-actions/exportcert.png "Export the certificate manually")  

<!-- affiliate ads begin -->
<a href="https://aligracehair.sjv.io/c/5597632/1959764/19272" target="_top" id="1959764">
  <img src="//a.impactradius-go.com/display-ad/19272-1959764" border="0" alt="https://techidaily.com" width="728" height="90"/>
</a>
<img height="0" width="0" src="https://aligracehair.sjv.io/i/5597632/1959764/19272" style="position:absolute;visibility:hidden;" border="0" />
<!-- affiliate ads end -->

### Install the certificate to Root and TrustedPublisher

Command line:

certutil.exe -addstore "Root" [PathToCertificatewithFile]
certutil.exe -addstore "TrustedPublisher" [PathToCertificatewithFile]

Copy

![Install the certificate to Root and TrustedPublisher](https://cdn.advancedinstaller.com/img/install-unsigned-drivers-using-custom-actions/addcertificate.png "Install the certificate to Root and TrustedPublisher")  

for remove :

certutil.exe -delstore "Root" [PathToCertificatewithFile]
certutil.exe -delstore "TrustedPublisher" [PathToCertificatewithFile]

Copy

Now we can install the driver without the prompt.

### Build the MSI

Now that we have understood how you sign a driver we also need to learn how you add the actions in the MSI. Basically all you need are two steps:

1. Install the certificate
2. Install the driver

For the second step we already had a look [a chapter earlier](https://tools.techidaily.com/advancedinstaller/products/) on how to achieve that, so basically all we have to do is create a script that performs the certutil commands previously mentioned. Let’s assume that the driver .inf name is HP.inf and let’s assume that we are going to place the certificate directly into the C:\\Windows\\DPInst.

For VBScript:

Option Explicit
On Error Resume Next
Dim strCmd,WshShell,strInstalldir
Set WshShell = CreateObject("WScript.Shell")
strInstalldir = WshShell.ExpandEnvironmentStrings("%SYSTEMROOT%")
strCmd = "certutil.exe -addstore " & chr(34) & "Root" & chr(34) & " " & chr(34) & strInstalldir & "\DPInst\TestCer.cer" & chr(34)
WshShell.Run strCmd

Copy

The script performs the following actions:

* Option Explicit: This statement enforces variable declaration in the script, ensuring that all variables are explicitly declared before they are used.
* On Error Resume Next: This statement allows the script to continue running even if an error occurs, bypassing the error and continuing with the next line of code.
* Dim strCmd, WshShell, strInstalldir: Declares three variables: strCmd to hold the command to be executed, WshShell to access the Windows Script Host Shell object, and strInstalldir to store the expanded value of the %SYSTEMROOT% environment variable.
* Set WshShell = CreateObject("WScript.Shell"): Creates an instance of the Windows Script Host Shell object, which allows the script to run shell commands and interact with the Windows environment.
* strInstalldir = WshShell.ExpandEnvironmentStrings("%SYSTEMROOT%"): Retrieves the value of the %SYSTEMROOT% environment variable using the ExpandEnvironmentStrings method of the WshShell object. The %SYSTEMROOT% variable represents the path to the Windows installation directory.
* strCmd = "certutil.exe -addstore " & chr(34) & "Root" & chr(34) & " " & chr(34) & strInstalldir & "\\DPInst\\TestCer.cer" & chr(34): Constructs the command to be executed. It uses the certutil.exe utility to add a certificate (TestCer.cer) to the "Root" certificate store. The path to the certificate file is obtained by combining strInstalldir (Windows installation directory) with the relative path \\DPInst\\TestCer.cer. The chr(34) is used to insert double quotes into the command string.
* WshShell.Run strCmd: Executes the command stored in the strCmd variable using the Run method of the WshShell object. This runs the command in a separate process.

In summary, the script uses VBScript to execute the certutil.exe command and add a certificate (TestCer.cer) to the "Root" certificate store. The script retrieves the Windows installation directory, constructs the command with the appropriate paths and options, and then runs it using the WshShell object.

Once the script is created, navigate to the Custom Actions Page and add the **Launch attached file** predefined custom action into the sequence, select the installation vbscript file that was previously created and configure the Custom Action as such:

![add certificate](https://cdn.advancedinstaller.com/img/install-unsigned-drivers-using-custom-actions/addcertAI.png "add certificate")  

<!-- affiliate ads begin -->
<a href="https://appsumo.8odi.net/c/5597632/2082542/7443" target="_top" id="2082542">
  <img src="//a.impactradius-go.com/display-ad/7443-2082542" border="0" alt="https://techidaily.com" width="728" height="90"/>
</a>
<img height="0" width="0" src="https://appsumo.8odi.net/i/5597632/2082542/7443" style="position:absolute;visibility:hidden;" border="0" />
<!-- affiliate ads end -->

![Important](https://cdn.advancedinstaller.com/svg/common/IconMessageInfo.svg)Make sure that the script which installs the certificate is placed before the script which installs the driver under the “Install Execution Stage” section.

With PowerShell it's even easier because we can use the [Import-Certificate cmdlet](https://learn.microsoft.com/en-us/powershell/module/pki/import-certificate?source=recommendations&amp;view=windowsserver2022-ps "Import-Certificate cmdlet"):

$CerLocation = $env:SystemRoot + "\DPInst\TestCer.cer"
Import-Certificate -FilePath $CerLocation -CertStoreLocation Cert:\LocalMachine\Root
Import-Certificate -FilePath $CerLocation -CertStoreLocation Cert:\LocalMachine\TrustedPublisher

Copy

Once we have the script ready, navigate to the Custom Actions Page and add the **Run PowerShell script file** predefined custom action into the sequence, select **Attached Script** and select the file that was previously created and configure the Custom Action as such:

![PowerShell script file - Attached Script](https://cdn.advancedinstaller.com/img/install-unsigned-drivers-using-custom-actions/addcertAIPS.png "PowerShell script file - Attached Script")  

<!-- affiliate ads begin -->
<span id="1328683">
					<video width="200" height="200" style="cursor:pointer"
           poster="//a.impactradius-go.com/display-clicktoplayimage/1328683.png"
           onclick="if(!this.playClicked){this.play();this.setAttribute('controls',true);this.playClicked=true;}">
	   <source src="//a.impactradius-go.com/display-ad/15852-1328683">
	   <img src="//a.impactradius-go.com/display-clicktoplayimage/1328683.png" style="border: none; height: 100%; width: 100%; object-fit: contain">
	</video>
	<div style="width:125px;text-align:center"><a href="javascript:window.open(decodeURIComponent('https%3A%2F%2Fthefitville.pxf.io%2Fc%2F5597632%2F1328683%2F15852'), '_blank');void(0);">Click here</a></div>
</span>
<img height="0" width="0" src="https://imp.pxf.io/i/5597632/1328683/15852" style="position:absolute;visibility:hidden;" border="0" />
<!-- affiliate ads end -->

Next, build the package and during installation/uninstallation the PowerShell scripts will run and install/uninstall the driver without asking if we want to install an unsigned driver.

### Installing unsigned drivers with Advanced Installer

As you can see, handling unsigned certificates is not an easy task and it’s definitely time consuming. Advanced Installer makes it easier to install unsigned drivers with just a click of a button..and yes that is not a figure of speech.

Navigate to the Drivers page and click on **New Driver**. A window will open for you to select the .inf file which must be present in the package.

![Installing unsigned drivers with Advanced Installer](https://cdn.advancedinstaller.com/img/install-unsigned-drivers-using-custom-actions/advinstdriverinst.png "Installing unsigned drivers with Advanced Installer")  

Next, all you have to do is just select “Install unsigned driver packages and driver packages that have missing files” and Advanced Installer does everything for you!

![Install unsigned driver packages and driver packages that have missing files](https://cdn.advancedinstaller.com/img/install-unsigned-drivers-using-custom-actions/unsigneddriversAI.png "Install unsigned driver packages and driver packages that have missing files")  

And that is it! Next, just build and install the package and you should have a clean installation without any warning messages from the OS.

<ins class="adsbygoogle"
     style="display:block"
     data-ad-format="autorelaxed"
     data-ad-client="ca-pub-7571918770474297"
     data-ad-slot="1223367746"></ins>

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-7571918770474297"
     data-ad-slot="8358498916"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<span class="atpl-alsoreadstyle">Also read:</span>
<div><ul>
<li><a href="https://article-helps.techidaily.com/new-2024-approved-the-ultimate-video-upgrade-with-enhancer-22/"><u>[New] 2024 Approved The Ultimate Video Upgrade with Enhancer 2.2</u></a></li>
<li><a href="https://youtube-docs.techidaily.com/asy-process-extracting-youtube-media-directly/"><u>[New] Easy Process Extracting YouTube Media Directly</u></a></li>
<li><a href="https://some-knowledge.techidaily.com/new-identifying-tech-giants-iphone-x-and-samsungs-face-recognition/"><u>[New] Identifying Tech Giants IPhone X & Samsung's Face Recognition</u></a></li>
<li><a href="https://win-cloud.techidaily.com/advanced-techniques-for-handling-package-dependencies-within-microsofts-installer-framework/"><u>Advanced Techniques for Handling Package Dependencies Within Microsoft's Installer Framework</u></a></li>
<li><a href="https://win-cloud.techidaily.com/annotated-list-of-the-deadliest-digital-plagues-unraveling-the-dark-side-of-technology-with-a-review-of-historic-malware-foxes/"><u>Annotated List of the Deadliest Digital Plagues: Unraveling the Dark Side of Technology with a Review of Historic Malware Foxes</u></a></li>
<li><a href="https://win-cloud.techidaily.com/antivirus-limitations-and-advanced-safeguards-necessary-for-optimal-security-online/"><u>Antivirus Limitations and Advanced Safeguards Necessary for Optimal Security Online</u></a></li>
<li><a href="https://facebook-video-files.techidaily.com/basics-of-effective-fb-ad-cta-design-for-2024/"><u>Basics of Effective FB Ad CTA Design for 2024</u></a></li>
<li><a href="https://win-cloud.techidaily.com/discover-the-best-video-shrinking-apps-for-your-iphone-a-trilogy-of-solutions/"><u>Discover the Best Video Shrinking Apps for Your iPhone: A Trilogy of Solutions!</u></a></li>
<li><a href="https://win-cloud.techidaily.com/essential-unknown-capabilities-of-ios-11-a-must-know-list-for-tech-enthusiasts/"><u>Essential Unknown Capabilities of iOS 11: A Must-Know List for Tech Enthusiasts</u></a></li>
<li><a href="https://fox-access.techidaily.com/find-your-favorites-ranking-of-8-preferred-mp3-extractors-android-for-2024/"><u>Find Your Favorites Ranking of 8 Preferred MP3 Extractors (Android) for 2024</u></a></li>
<li><a href="https://win-howtos.techidaily.com/how-to-fix-the-specified-module-could-not-be-found/"><u>How To Fix The Specified Module Could Not Be Found</u></a></li>
<li><a href="https://some-guidance.techidaily.com/professionelle-dvd-rip-und-verbesserung-mit-winxdvd-ultimative-losung-fur-hd-videoubertragung-auf-mobilgerate/"><u>Professionelle DVD Rip Und Verbesserung Mit WinXDVD - Ultimative Lösung Für HD Videoübertragung Auf Mobilgeräte</u></a></li>
<li><a href="https://win-cloud.techidaily.com/step-by-step-guide-wireless-and-usb-file-sharing-between-your-pc-and-galaxy-s4/"><u>Step-by-Step Guide: Wireless and USB File Sharing Between Your PC & Galaxy S4</u></a></li>
<li><a href="https://win-cloud.techidaily.com/the-ultimate-troubleshooting-for-retrieving-deleted-conversations-following-the-ios-12-upgrade/"><u>The Ultimate Troubleshooting for Retrieving Deleted Conversations Following the iOS 12 Upgrade</u></a></li>
<li><a href="https://some-knowledge.techidaily.com/44or44k944kz44oz5lik44gn44gu44ot44oh44kq5zue6lui5oqa6kgtic0g54sh5paz44ks44kk44oj/"><u>パソコン上でのビデオ回転技術 - 無料ガイド</u></a></li>
</ul></div>

