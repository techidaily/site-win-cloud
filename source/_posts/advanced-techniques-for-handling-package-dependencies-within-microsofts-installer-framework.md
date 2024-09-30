---
title: Advanced Techniques for Handling Package Dependencies Within Microsoft's Installer Framework
date: 2024-09-28T16:18:32.864Z
updated: 2024-09-30T02:34:52.239Z
tags:
  - application-packaging-training
categories:
  - advancedinstaller
description: This Article Describes Advanced Techniques for Handling Package Dependencies Within Microsoft's Installer Framework
thumbnail: https://thmb.techidaily.com/13fc80b5d967307ffa59212655cd04104762f09d1a6a2f19734cf5d213e7a224.jpg
---

## Advanced Techniques for Handling Package Dependencies Within Microsoft's Installer Framework

>  Disclaimer: This post includes affiliate links
>
>  If you click on a link and make a purchase, I may receive a commission at no extra cost to you.
>

## Working with Dependencies

While conditional statements give you the possibility to clearly define the needed environment for your installer, it doesn’t let you modify the system in order to make the proper changes for your application to be installed. As seen in the chapter above, conditional statements for software applications can be easily added into the MSI by using Advanced Installer, but what if you want to check if that particular dependency is installed and if not, install it yourself?

<!-- affiliate ads begin -->
<a href="https://bluettius.sjv.io/c/5597632/2139112/17108" target="_top" id="2139112">
  <img src="//a.impactradius-go.com/display-ad/17108-2139112" border="0" alt="https://techidaily.com" width="250" height="90"/>
</a>
<img height="0" width="0" src="https://bluettius.sjv.io/i/5597632/2139112/17108" style="position:absolute;visibility:hidden;" border="0" />
<!-- affiliate ads end -->

### Prerequisites

This is where the Prerequisites feature comes in handy and lets you quickly create a bundle which contains your application and the desired dependencies.

Prerequisites are software components that must be installed before the installation of an application to ensure its proper functioning. Advanced Installer provides a wide range of prerequisites that can be easily added to MSI packages, including .NET Framework, Visual C++ Redistributable, and SQL Server Express.

Adding prerequisites to MSI packages is important for several reasons. Firstly, it ensures that the application will run correctly on target systems, reducing the risk of compatibility issues and user frustration. Secondly, it simplifies the deployment process by automating the installation of required software components. Finally, it can improve the performance of the application by ensuring that it has access to the latest software components.

Adding prerequisites with Advanced Installer is a straightforward process that can be done in just a few steps. Here's how to do it:

<!-- affiliate ads begin -->
<a href="https://appsumo.8odi.net/c/5597632/2068433/7443" target="_top" id="2068433">
  <img src="//a.impactradius-go.com/display-ad/7443-2068433" border="0" alt="https://techidaily.com" width="728" height="90"/>
</a>
<img height="0" width="0" src="https://appsumo.8odi.net/i/5597632/2068433/7443" style="position:absolute;visibility:hidden;" border="0" />
<!-- affiliate ads end -->

### Advanced Installer Prerequisites page

Open your Advanced Installer project and go to the Prerequisites page. Advanced Installer provides a wide range of options for configuring prerequisites, including the ability to specify a specific version of the prerequisite, the installation path, and the location of prerequisite files.

This view enables you to incorporate existing installers in your package, enable Windows features, and configure specific Windows Server roles.

![ai prerequisites](https://cdn.advancedinstaller.com/img/adding-prerequisites-to-packages/ai-prerequisites.png "ai prerequisites")  

All prerequisite setup files can be bundled with your package or placed online and accessed via a URL. If the prerequisite is not found during installation, it will be installed automatically.

### Download and Install Prerequisites

Advanced Installer can also download and install prerequisites from a remote location, such as a web server or network share. This ensures that the most recent versions of the prerequisite are always used, lowering the possibility of compatibility issues.

In the main Prerequisites page you can see on the right hand menu that you have a ton of predefined prerequisites that you can choose from. Some of these prerequisites are:

* .NET Framework
* .NET Core
* .NET Runtime
* SQL Server Compact
* SQL Server Express
* MySQL Server
* SQL Server OLE DB
* Adobe products
* JRE
* JDK
* Silverlight
* Python
* Internet Explorer
* DirectX
* XNA
* Access Runtimes
* Visual C++ Redistributables
* VSTO
* IIS
* Apache Tomcat
* MSML
* Windows Installer
* PowerShell
* And many more

When you select a predefined prerequisite, Advanced Installer asks you if you want to download the package next to your project and include it in the package, making the overall process much simpler.

![ai prerequisites download](https://cdn.advancedinstaller.com/img/adding-prerequisites-to-packages/ai-prerequisites-download.png "ai prerequisites download")  

<!-- affiliate ads begin -->
<a href="https://appsumo.8odi.net/c/5597632/2105860/7443" target="_top" id="2105860">
  <img src="//a.impactradius-go.com/display-ad/7443-2105860" border="0" alt="https://techidaily.com" width="728" height="90"/>
</a>
<img height="0" width="0" src="https://appsumo.8odi.net/i/5597632/2105860/7443" style="position:absolute;visibility:hidden;" border="0" />
<!-- affiliate ads end -->

You can also select the extract folder for each selected prerequisite, keep the prerequisite files after installation, hide installed prerequisites and check the launch conditions before searching for the prerequisites.

<!-- affiliate ads begin -->
<a href="https://appsumo.8odi.net/c/5597632/2037358/7443" target="_top" id="2037358">
  <img src="//a.impactradius-go.com/display-ad/7443-2037358" border="0" alt="https://techidaily.com" width="728" height="90"/>
</a>
<img height="0" width="0" src="https://appsumo.8odi.net/i/5597632/2037358/7443" style="position:absolute;visibility:hidden;" border="0" />
<!-- affiliate ads end -->

### How to Enable Windows Features with Advanced Installer

Advanced Installer also allows you to enable Windows Features.Some Windows programs and features must be enabled before applications can use them. Other features are enabled by default, but you can disable them if your application does not require them.

Use the \[Windows Feature bundle\] toolbar button or the "New Windows Feature bundle" context menu item to add a Windows Features bundle then select the Target Operating System. Once you have chosen the OS, the Available Windows Features will be populated with all the options available for those particular OSes.

![ai prerequisites windows features](https://cdn.advancedinstaller.com/img/adding-prerequisites-to-packages/ai-prerequisites-windows-features.png "ai prerequisites windows features")  

Advanced Installer also offers multiple Windows Server roles from which you can choose to include in your package. Roles selected in this view only apply when running the package on a Windows Server. They will be ignored on any other OS type.

To include a server role in your package, select the Windows Server Roles tree item, then check the roles you want to include. The roles are organized by the earliest supported version of the target server's operating system.

![ai prerequisites windows server roles](https://cdn.advancedinstaller.com/img/adding-prerequisites-to-packages/ai-prerequisites-windows-server-roles.png "ai prerequisites windows server roles")  

Of course, you can add your own packages and create a suite installation, [a chapter we have already covered in the first MSI Packaging fundamentals ebook](https://tools.techidaily.com/advancedinstaller/products/).

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
<li><a href="https://facebook-video-share.techidaily.com/new-decoding-youtubes-procedure-after-a-video-is-uploaded-for-2024/"><u>[New] Decoding YouTube's Procedure After a Video Is Uploaded for 2024</u></a></li>
<li><a href="https://facebook-video-recording.techidaily.com/new-digitally-liberated-fb-tunes/"><u>[New] Digitally Liberated FB Tunes</u></a></li>
<li><a href="https://facebook-video-share.techidaily.com/updated-2024-approved-elevate-engagement-amplify-audience-youtube-marketing/"><u>[Updated] 2024 Approved Elevate Engagement, Amplify Audience (YouTube Marketing)</u></a></li>
<li><a href="https://win-cloud.techidaily.com/h264avi/"><u>「画質維持でH264動画をAVIに変換：専門家のコツとテクニック」</u></a></li>
<li><a href="https://win-cloud.techidaily.com/easy-ways-to-transform-bik-videos-into-popular-file-types-such-as-mp4-avi-and-mkv-for-universal-playback-compatibility/"><u>Easy Ways to Transform BIK Videos Into Popular File Types Such as MP4, AVI & MKV for Universal Playback Compatibility</u></a></li>
<li><a href="https://win-cloud.techidaily.com/effortless-techniques-transforming-your-twitch-stream-into-an-mp4-file/"><u>Effortless Techniques: Transforming Your Twitch Stream Into an MP4 File</u></a></li>
<li><a href="https://win-cloud.techidaily.com/get-your-free-trial-of-the-ultimate-flv-video-converter-flv-converter-factory-pro-no-risks-with-secure-online-buy/"><u>Get Your Free Trial of the Ultimate FLV Video Converter: FLV Converter Factory Pro - No Risks with Secure Online Buy</u></a></li>
<li><a href="https://win-cloud.techidaily.com/how-to-safely-and-legally-access-free-mp4-music-videos-a-step-by-step-guide/"><u>How to Safely and Legally Access Free MP4 Music Videos: A Step-by-Step Guide</u></a></li>
<li><a href="https://android-transfer.techidaily.com/in-2024-how-to-transfer-apps-from-samsung-galaxy-a23-5g-to-another-drfone-by-drfone-transfer-from-android-transfer-from-android/"><u>In 2024, How to Transfer Apps from Samsung Galaxy A23 5G to Another | Dr.fone</u></a></li>
<li><a href="https://android-transfer.techidaily.com/in-2024-how-to-transfer-data-from-oppo-find-n3-to-other-android-devices-drfone-by-drfone-transfer-from-android-transfer-from-android/"><u>In 2024, How to Transfer Data from Oppo Find N3 to Other Android Devices? | Dr.fone</u></a></li>
<li><a href="https://fox-glue.techidaily.com/in-2024-the-complete-untapped-potential-of-dji-phantom-4/"><u>In 2024, The Complete Untapped Potential of DJI Phantom 4</u></a></li>
<li><a href="https://pokemon-go-android.techidaily.com/in-2024-why-does-the-pokemon-go-battle-league-not-available-on-honor-x7b-drfone-by-drfone-virtual-android/"><u>In 2024, Why does the pokemon go battle league not available On Honor X7b | Dr.fone</u></a></li>
<li><a href="https://win-cloud.techidaily.com/master-the-art-of-hd-video-editing-with-uncracked-factory-pro-software-a-full-guide/"><u>Master the Art of HD Video Editing with Uncracked Factory Pro Software – A Full Guide</u></a></li>
<li><a href="https://data-wizards.techidaily.com/restoring-malfunctioned-mp4-data-on-smartphones/"><u>Restoring Malfunctioned MP4 Data on Smartphones</u></a></li>
</ul></div>

