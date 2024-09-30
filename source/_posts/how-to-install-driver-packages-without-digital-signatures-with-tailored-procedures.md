---
title: How to Install Driver Packages Without Digital Signatures with Tailored Procedures
date: 2024-09-27T17:11:03.729Z
updated: 2024-09-29T21:16:38.690Z
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

<!-- affiliate ads begin -->
<a href="https://ephamedtechinc.pxf.io/c/5597632/2137220/26400" target="_top" id="2137220">
  <img src="//a.impactradius-go.com/display-ad/26400-2137220" border="0" alt="https://techidaily.com" width="728" height="90"/>
</a>
<img height="0" width="0" src="https://ephamedtechinc.pxf.io/i/5597632/2137220/26400" style="position:absolute;visibility:hidden;" border="0" />
<!-- affiliate ads end -->

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
<a href="https://appsumo.8odi.net/c/5597632/2144278/7443" target="_top" id="2144278">
  <img src="//a.impactradius-go.com/display-ad/7443-2144278" border="0" alt="https://techidaily.com" width="728" height="90"/>
</a>
<img height="0" width="0" src="https://appsumo.8odi.net/i/5597632/2144278/7443" style="position:absolute;visibility:hidden;" border="0" />
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

Successfully signed and timestamped: C:\\toaster\\device\\example.cat

<!-- affiliate ads begin -->
<a href="https://dhgate.sjv.io/c/5597632/1175223/12108" target="_top" id="1175223">
  <img src="//a.impactradius-go.com/display-ad/12108-1175223" border="0" alt="https://techidaily.com" width="728" height="90"/>
</a>
<img height="0" width="0" src="https://dhgate.sjv.io/i/5597632/1175223/12108" style="position:absolute;visibility:hidden;" border="0" />
<!-- affiliate ads end -->

### Export the certificate from certstore manually

Run an administrator command: certmgr.exe

Export the certificate manually:

![Export the certificate manually](https://cdn.advancedinstaller.com/img/install-unsigned-drivers-using-custom-actions/exportcert.png "Export the certificate manually")  

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
<a href="https://aligracehair.sjv.io/c/5597632/1934292/19272" target="_top" id="1934292">
  <img src="//a.impactradius-go.com/display-ad/19272-1934292" border="0" alt="https://techidaily.com" width="728" height="90"/>
</a>
<img height="0" width="0" src="https://aligracehair.sjv.io/i/5597632/1934292/19272" style="position:absolute;visibility:hidden;" border="0" />
<!-- affiliate ads end -->

![Important](https://cdn.advancedinstaller.com/svg/common/IconMessageInfo.svg)Make sure that the script which installs the certificate is placed before the script which installs the driver under the “Install Execution Stage” section.

With PowerShell it's even easier because we can use the [Import-Certificate cmdlet](https://learn.microsoft.com/en-us/powershell/module/pki/import-certificate?source=recommendations&amp;view=windowsserver2022-ps "Import-Certificate cmdlet"):

$CerLocation = $env:SystemRoot + "\DPInst\TestCer.cer"
Import-Certificate -FilePath $CerLocation -CertStoreLocation Cert:\LocalMachine\Root
Import-Certificate -FilePath $CerLocation -CertStoreLocation Cert:\LocalMachine\TrustedPublisher

Copy

Once we have the script ready, navigate to the Custom Actions Page and add the **Run PowerShell script file** predefined custom action into the sequence, select **Attached Script** and select the file that was previously created and configure the Custom Action as such:

![PowerShell script file - Attached Script](https://cdn.advancedinstaller.com/img/install-unsigned-drivers-using-custom-actions/addcertAIPS.png "PowerShell script file - Attached Script")  

Next, build the package and during installation/uninstallation the PowerShell scripts will run and install/uninstall the driver without asking if we want to install an unsigned driver.

<!-- affiliate ads begin -->
<a href="https://ephamedtechinc.pxf.io/c/5597632/2137202/26400" target="_top" id="2137202">
  <img src="//a.impactradius-go.com/display-ad/26400-2137202" border="0" alt="https://techidaily.com" width="728" height="90"/>
</a>
<img height="0" width="0" src="https://ephamedtechinc.pxf.io/i/5597632/2137202/26400" style="position:absolute;visibility:hidden;" border="0" />
<!-- affiliate ads end -->

### Installing unsigned drivers with Advanced Installer

As you can see, handling unsigned certificates is not an easy task and it’s definitely time consuming. Advanced Installer makes it easier to install unsigned drivers with just a click of a button..and yes that is not a figure of speech.

Navigate to the Drivers page and click on **New Driver**. A window will open for you to select the .inf file which must be present in the package.

![Installing unsigned drivers with Advanced Installer](https://cdn.advancedinstaller.com/img/install-unsigned-drivers-using-custom-actions/advinstdriverinst.png "Installing unsigned drivers with Advanced Installer")  

Next, all you have to do is just select “Install unsigned driver packages and driver packages that have missing files” and Advanced Installer does everything for you!

![Install unsigned driver packages and driver packages that have missing files](https://cdn.advancedinstaller.com/img/install-unsigned-drivers-using-custom-actions/unsigneddriversAI.png "Install unsigned driver packages and driver packages that have missing files")  

<!-- affiliate ads begin -->
<a href="https://appsumo.8odi.net/c/5597632/2144308/7443" target="_top" id="2144308">
  <img src="//a.impactradius-go.com/display-ad/7443-2144308" border="0" alt="https://techidaily.com" width="600" height="90"/>
</a>
<img height="0" width="0" src="https://appsumo.8odi.net/i/5597632/2144308/7443" style="position:absolute;visibility:hidden;" border="0" />
<!-- affiliate ads end -->

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
<li><a href="https://youtube-tips.techidaily.com/024-approved-understanding-youtubes-monthly-revenue-options/"><u>[New] 2024 Approved Understanding YouTube's Monthly Revenue Options</u></a></li>
<li><a href="https://instagram-video-recordings.techidaily.com/new-in-2024-shatter-the-illusion-true-viewers-of-your-insta-posts-revealed/"><u>[New] In 2024, Shatter the Illusion True Viewers of Your Insta Posts Revealed</u></a></li>
<li><a href="https://some-skills.techidaily.com/new-palette-strategies-utilizing-color-science-in-art/"><u>[New] Palette Strategies Utilizing Color Science in Art</u></a></li>
<li><a href="https://youtube-sure.techidaily.com/ed-in-2024-cutting-edge-methods-to-reduce-youtube-video-size/"><u>[Updated] In 2024, Cutting-Edge Methods to Reduce YouTube Video Size</u></a></li>
<li><a href="https://facebook-video-recording.techidaily.com/updated-mastering-the-art-of-extracting-fb-gifs-for-all-platforms-for-2024/"><u>[Updated] Mastering the Art of Extracting FB GIFs for All Platforms for 2024</u></a></li>
<li><a href="https://fox-cloud.techidaily.com/updated-mastery-over-cloud-expenses-cheapest-options-showcased/"><u>[Updated] Mastery Over Cloud Expenses Cheapest Options Showcased</u></a></li>
<li><a href="https://win-cloud.techidaily.com/1-easy-guide-streaming-flv-files-on-iphone-and-ipad-without-hitches/"><u>1. Easy Guide: Streaming Flv Files on iPhone & iPad Without Hitches</u></a></li>
<li><a href="https://win-cloud.techidaily.com/1726027438379-dvdmp4/"><u>店頭で入手したDVDをMP4フォーマットに直すコツ</u></a></li>
<li><a href="https://win-cloud.techidaily.com/mkvgif/"><u>高画質MKVファイルから鮮やかなGIFへの完全変換ガイド</u></a></li>
<li><a href="https://win-able.techidaily.com/age-of-empires-ii-windows-10-compatibility-fixed-start-playing-without-hitches/"><u>Age of Empires II Windows 10 Compatibility Fixed - Start Playing Without Hitches</u></a></li>
<li><a href="https://win-cloud.techidaily.com/effortless-transformation-upload-your-mkv-videos-as-no-cost-dvd-discs/"><u>Effortless Transformation: Upload Your MKV Videos as No-Cost DVD Discs</u></a></li>
<li><a href="https://win-cloud.techidaily.com/fixing-the-unplayable-video-issue-overcoming-error-code-150-in-google-slides-presentations/"><u>Fixing the Unplayable Video Issue: Overcoming Error Code 150 in Google Slides Presentations</u></a></li>
<li><a href="https://win-cloud.techidaily.com/huawei-users-rejoice-learn-the-easy-way-to-convert-and-watch-dvds-on-your-phonetablet-without-cost/"><u>Huawei Users Rejoice! Learn the Easy Way to Convert & Watch DVDs on Your Phone/Tablet Without Cost</u></a></li>
<li><a href="https://easy-unlock-android.techidaily.com/in-2024-how-to-unlock-poco-x6-pin-codepattern-lockpassword-by-drfone-android/"><u>In 2024, How to Unlock Poco X6 PIN Code/Pattern Lock/Password</u></a></li>
<li><a href="https://win-cloud.techidaily.com/1726028974432-movmp4/"><u>MOVファイル大量バッチをMP4に転化する究極ガイド</u></a></li>
<li><a href="https://win-cloud.techidaily.com/pctiktoksns/"><u>PC版TikTok生中継の記録・保存テクニック！SNS動画フォロー強化</u></a></li>
<li><a href="https://win-cloud.techidaily.com/successfully-streaming-webm-files-on-your-ios-device-tips-and-tricks/"><u>Successfully Streaming WebM Files on Your iOS Device: Tips & Tricks</u></a></li>
<li><a href="https://techidaily.com/three-solutions-to-hard-reset-infinix-smart-8-hd-drfone-by-drfone-reset-android-reset-android/"><u>Three Solutions to Hard Reset Infinix Smart 8 HD? | Dr.fone</u></a></li>
<li><a href="https://technical-tips.techidaily.com/user-guide-to-downloading-and-using-the-latest-snappy-driver-installer-v113/"><u>User Guide to Downloading and Using the Latest Snappy Driver Installer V1.13</u></a></li>
</ul></div>

