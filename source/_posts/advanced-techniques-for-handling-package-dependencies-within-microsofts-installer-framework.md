---
title: Advanced Techniques for Handling Package Dependencies Within Microsoft's Installer Framework
date: 2024-10-04T19:07:49.841Z
updated: 2024-10-05T19:11:36.917Z
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

### Prerequisites

This is where the Prerequisites feature comes in handy and lets you quickly create a bundle which contains your application and the desired dependencies.

Prerequisites are software components that must be installed before the installation of an application to ensure its proper functioning. Advanced Installer provides a wide range of prerequisites that can be easily added to MSI packages, including .NET Framework, Visual C++ Redistributable, and SQL Server Express.

Adding prerequisites to MSI packages is important for several reasons. Firstly, it ensures that the application will run correctly on target systems, reducing the risk of compatibility issues and user frustration. Secondly, it simplifies the deployment process by automating the installation of required software components. Finally, it can improve the performance of the application by ensuring that it has access to the latest software components.

Adding prerequisites with Advanced Installer is a straightforward process that can be done in just a few steps. Here's how to do it:

<!-- affiliate ads begin -->
<a href="https://laganoo.pxf.io/c/5597632/1484910/16446" target="_top" id="1484910">
  <img src="//a.impactradius-go.com/display-ad/16446-1484910" border="0" alt="https://techidaily.com" width="300" height="90"/>
</a>
<img height="0" width="0" src="https://laganoo.pxf.io/i/5597632/1484910/16446" style="position:absolute;visibility:hidden;" border="0" />
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
<a href="https://appsumo.8odi.net/c/5597632/2105863/7443" target="_top" id="2105863">
  <img src="//a.impactradius-go.com/display-ad/7443-2105863" border="0" alt="https://techidaily.com" width="728" height="90"/>
</a>
<img height="0" width="0" src="https://appsumo.8odi.net/i/5597632/2105863/7443" style="position:absolute;visibility:hidden;" border="0" />
<!-- affiliate ads end -->

You can also select the extract folder for each selected prerequisite, keep the prerequisite files after installation, hide installed prerequisites and check the launch conditions before searching for the prerequisites.

<!-- affiliate ads begin -->
<a href="https://aligracehair.sjv.io/c/5597632/1902289/19272" target="_top" id="1902289">
  <img src="//a.impactradius-go.com/display-ad/19272-1902289" border="0" alt="https://techidaily.com" width="300" height="90"/>
</a>
<img height="0" width="0" src="https://aligracehair.sjv.io/i/5597632/1902289/19272" style="position:absolute;visibility:hidden;" border="0" />
<!-- affiliate ads end -->

### How to Enable Windows Features with Advanced Installer

Advanced Installer also allows you to enable Windows Features.Some Windows programs and features must be enabled before applications can use them. Other features are enabled by default, but you can disable them if your application does not require them.

Use the \[Windows Feature bundle\] toolbar button or the "New Windows Feature bundle" context menu item to add a Windows Features bundle then select the Target Operating System. Once you have chosen the OS, the Available Windows Features will be populated with all the options available for those particular OSes.

![ai prerequisites windows features](https://cdn.advancedinstaller.com/img/adding-prerequisites-to-packages/ai-prerequisites-windows-features.png "ai prerequisites windows features")  

<!-- affiliate ads begin -->
<a href="https://appsumo.8odi.net/c/5597632/2068407/7443" target="_top" id="2068407">
  <img src="//a.impactradius-go.com/display-ad/7443-2068407" border="0" alt="https://techidaily.com" width="728" height="90"/>
</a>
<img height="0" width="0" src="https://appsumo.8odi.net/i/5597632/2068407/7443" style="position:absolute;visibility:hidden;" border="0" />
<!-- affiliate ads end -->

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
<li><a href="https://facebook-record-videos.techidaily.com/new-channel-titles-that-shine-how-to-innovate-for-2024/"><u>[New] Channel Titles That Shine How to Innovate for 2024</u></a></li>
<li><a href="https://instagram-clips.techidaily.com/new-elevating-your-engagement-game-the-instagram-edge-guide-for-2024/"><u>[New] Elevating Your Engagement Game The Instagram Edge Guide for 2024</u></a></li>
<li><a href="https://youtube-tips.techidaily.com/n-2024-top-10-fearful-video-blogs-overcoming-each-challenge/"><u>[New] In 2024, Top 10 Fearful Video Blogs Overcoming Each Challenge</u></a></li>
<li><a href="https://vp-tips.techidaily.com/new-ultimate-list-of-mac-friendly-video-to-mp4-codecs-for-2024/"><u>[New] Ultimate List of Mac-Friendly Video to MP4 Codecs for 2024</u></a></li>
<li><a href="https://extra-information.techidaily.com/updated-chronicle-crafters-collective-select-seventeen/"><u>[Updated] Chronicle Crafters Collective - Select Seventeen</u></a></li>
<li><a href="https://fox-boxes.techidaily.com/updated-in-2024-insiders-guide-to-pro-windows-10-expertise/"><u>[Updated] In 2024, Insider's Guide to Pro WINDOWS 10 Expertise</u></a></li>
<li><a href="https://win-cloud.techidaily.com/botnet-basics-unveiled-how-these-networks-operate-demystified-for-the-everyday-user/"><u>Botnet Basics Unveiled: How These Networks Operate Demystified for the Everyday User</u></a></li>
<li><a href="https://win-cloud.techidaily.com/comprehensive-analysis-top-rated-free-screen-capture-apps-without-watermarks/"><u>Comprehensive Analysis: Top-Rated Free Screen Capture Apps Without Watermarks</u></a></li>
<li><a href="https://win-cloud.techidaily.com/convert-your-wlmp-videos-to-avi-files-without-cost-step-by-step-tutorial/"><u>Convert Your WLMP Videos to AVI Files Without Cost - Step-by-Step Tutorial</u></a></li>
<li><a href="https://win-dash.techidaily.com/find-and-install-updated-toshiba-drivers-a-step-by-step-guide-for-windows-owners/"><u>Find & Install Updated Toshiba Drivers: A Step-by-Step Guide for Windows Owners</u></a></li>
<li><a href="https://win-cloud.techidaily.com/native-heic-picture-manager-for-windows-enjoy-seamless-viewing-without-changing-image-types/"><u>Native HEIC Picture Manager for Windows: Enjoy Seamless Viewing Without Changing Image Types!</u></a></li>
<li><a href="https://pokemon-go-android.techidaily.com/pokemon-go-no-gps-signal-heres-every-possible-solution-on-realme-v30t-drfone-by-drfone-virtual-android/"><u>Pokemon Go No GPS Signal? Heres Every Possible Solution On Realme V30T | Dr.fone</u></a></li>
<li><a href="https://fake-location.techidaily.com/prevent-cross-site-tracking-on-honor-play-8t-and-browser-drfone-by-drfone-virtual-android/"><u>Prevent Cross-Site Tracking on Honor Play 8T and Browser | Dr.fone</u></a></li>
<li><a href="https://win-cloud.techidaily.com/silence-unwanted-sound-discover-the-best-6-apps-for-removing-audio-interferences-on-your-smartphone-android-and-ios/"><u>Silence Unwanted Sound: Discover the Best 6 Apps for Removing Audio Interferences on Your Smartphone (Android & iOS)</u></a></li>
<li><a href="https://win-cloud.techidaily.com/step-by-step-guide-to-blocking-ads-and-protecting-privacy-across-all-your-devices/"><u>Step-by-Step Guide to Blocking Ads and Protecting Privacy Across All Your Devices</u></a></li>
<li><a href="https://win-cloud.techidaily.com/top-three-methods-for-converting-flac-files-to-itunes-compatible-formats-on-pc-and-mac/"><u>Top Three Methods for Converting FLAC Files to iTunes Compatible Formats on PC & Mac</u></a></li>
</ul></div>

