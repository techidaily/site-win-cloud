---
title: Advanced Techniques for Handling Package Dependencies Within Microsoft's Installer Framework
date: 2024-10-05T02:27:59.947Z
updated: 2024-10-10T21:59:08.692Z
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
<span id="1936838">
					<video width="374" height="48" style="cursor:pointer"
           poster="//a.impactradius-go.com/display-clicktoplayimage/1936838.png"
           onclick="if(!this.playClicked){this.play();this.setAttribute('controls',true);this.playClicked=true;}">
	   <source src="//a.impactradius-go.com/display-ad/18409-1936838">
	   <img src="//a.impactradius-go.com/display-clicktoplayimage/1936838.png" style="border: none; height: 100%; width: 100%; object-fit: contain">
	</video>
	<div style="width:234px;text-align:center"><a href="javascript:window.open(decodeURIComponent('https%3A%2F%2Fcoinrule.sjv.io%2Fc%2F5597632%2F1936838%2F18409'), '_blank');void(0);">Click here</a></div>
</span>
<img height="0" width="0" src="https://imp.pxf.io/i/5597632/1936838/18409" style="position:absolute;visibility:hidden;" border="0" />
<!-- affiliate ads end -->

### Advanced Installer Prerequisites page

Open your Advanced Installer project and go to the Prerequisites page. Advanced Installer provides a wide range of options for configuring prerequisites, including the ability to specify a specific version of the prerequisite, the installation path, and the location of prerequisite files.

This view enables you to incorporate existing installers in your package, enable Windows features, and configure specific Windows Server roles.

![ai prerequisites](https://cdn.advancedinstaller.com/img/adding-prerequisites-to-packages/ai-prerequisites.png "ai prerequisites")  

All prerequisite setup files can be bundled with your package or placed online and accessed via a URL. If the prerequisite is not found during installation, it will be installed automatically.

<!-- affiliate ads begin -->
<a href="https://appsumo.8odi.net/c/5597632/2118312/7443" target="_top" id="2118312">
  <img src="//a.impactradius-go.com/display-ad/7443-2118312" border="0" alt="https://techidaily.com" width="728" height="90"/>
</a>
<img height="0" width="0" src="https://appsumo.8odi.net/i/5597632/2118312/7443" style="position:absolute;visibility:hidden;" border="0" />
<!-- affiliate ads end -->

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

You can also select the extract folder for each selected prerequisite, keep the prerequisite files after installation, hide installed prerequisites and check the launch conditions before searching for the prerequisites.

<!-- affiliate ads begin -->
<a href="https://unicoeye.pxf.io/c/5597632/2134243/18498" target="_top" id="2134243">
  <img src="//a.impactradius-go.com/display-ad/18498-2134243" border="0" alt="https://techidaily.com" width="728" height="90"/>
</a>
<img height="0" width="0" src="https://unicoeye.pxf.io/i/5597632/2134243/18498" style="position:absolute;visibility:hidden;" border="0" />
<!-- affiliate ads end -->

### How to Enable Windows Features with Advanced Installer

Advanced Installer also allows you to enable Windows Features.Some Windows programs and features must be enabled before applications can use them. Other features are enabled by default, but you can disable them if your application does not require them.

Use the \[Windows Feature bundle\] toolbar button or the "New Windows Feature bundle" context menu item to add a Windows Features bundle then select the Target Operating System. Once you have chosen the OS, the Available Windows Features will be populated with all the options available for those particular OSes.

![ai prerequisites windows features](https://cdn.advancedinstaller.com/img/adding-prerequisites-to-packages/ai-prerequisites-windows-features.png "ai prerequisites windows features")  

<!-- affiliate ads begin -->
<a href="https://appsumo.8odi.net/c/5597632/1062450/7443" target="_top" id="1062450">
  <img src="//a.impactradius-go.com/display-ad/7443-1062450" border="0" alt="https://techidaily.com" width="600" height="90"/>
</a>
<img height="0" width="0" src="https://appsumo.8odi.net/i/5597632/1062450/7443" style="position:absolute;visibility:hidden;" border="0" />
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
<li><a href="https://youtube-web.techidaily.com/024-approved-strategies-for-effective-video-markup-on-youtube/"><u>[New] 2024 Approved Strategies for Effective Video Markup on YouTube</u></a></li>
<li><a href="https://article-knowledge.techidaily.com/updated-2024-approved-bring-out-canons-best-enjoy-10-free-luts-and-beyond-selection/"><u>[Updated] 2024 Approved Bring Out Canon's Best Enjoy 10 Free LUTs and Beyond Selection</u></a></li>
<li><a href="https://buynow-info.techidaily.com/blades-blh4100-rtf-high-performance-outdoor-rc-helicopter-in-depth-review/"><u>Blades BLH4100 RTF High-Performance Outdoor RC Helicopter - In Depth Review</u></a></li>
<li><a href="https://data-safeguard.techidaily.com/cookiebot-enabled-customization-for-targeted-advertising-strategies/"><u>Cookiebot-Enabled Customization for Targeted Advertising Strategies</u></a></li>
<li><a href="https://easy-unlock-android.techidaily.com/how-to-bypass-android-lock-screen-using-emergency-call-on-realme-by-drfone-android/"><u>How to Bypass Android Lock Screen Using Emergency Call On Realme?</u></a></li>
<li><a href="https://win-cloud.techidaily.com/mastering-the-art-of-removing-red-eye-from-images-a-comprehensive-tutorial/"><u>Mastering the Art of Removing Red-Eye From Images - A Comprehensive Tutorial</u></a></li>
<li><a href="https://ai-video-editing.techidaily.com/new-how-to-put-a-background-on-a-green-screen-for-2024/"><u>New How to Put a Background on A Green Screen for 2024</u></a></li>
<li><a href="https://win-cloud.techidaily.com/quick-and-simple-methods-for-shifting-pdf-files-from-pc-to-ios-devices/"><u>Quick & Simple Methods for Shifting PDF Files From PC to iOS Devices</u></a></li>
<li><a href="https://mondly-stories.techidaily.com/semaine-francaise-la-palette-des-jours/"><u>Semaine Française: La Palette Des Jours</u></a></li>
<li><a href="https://extra-lessons.techidaily.com/significant-tenets-of-interactive-storytelling/"><u>Significant Tenets of Interactive Storytelling</u></a></li>
<li><a href="https://win-cloud.techidaily.com/step-by-step-user-manual-creating-and-editing-with-apowersoft-gif/"><u>Step-by-Step User Manual: Creating & Editing with Apowersoft GIF</u></a></li>
<li><a href="https://win-cloud.techidaily.com/the-art-of-deception-exploring-how-and-where-digital-thieves-conceal-harmful-software/"><u>The Art of Deception: Exploring How & Where Digital Thieves Conceal Harmful Software</u></a></li>
<li><a href="https://win-cloud.techidaily.com/uncover-shocking-cyberbullying-realities-and-their-impact-essential-strategies-for-prevention-and-intervention/"><u>Uncover Shocking Cyberbullying Realities & Their Impact: Essential Strategies for Prevention and Intervention</u></a></li>
<li><a href="https://ai-video-tools.techidaily.com/updated-android-video-editing-made-easy-top-10-free-and-paid-apps-for-2024/"><u>Updated Android Video Editing Made Easy Top 10 Free and Paid Apps for 2024</u></a></li>
</ul></div>

