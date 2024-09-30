---
title: Comprehensive Strategies for Implementing Effective Firewall Configurations via VBScript, PowerShell, and the Advanced Installer Tool
date: 2024-09-26T08:20:11.893Z
updated: 2024-09-30T09:33:49.478Z
tags:
  - application-packaging-training
categories:
  - advancedinstaller
description: This Article Describes Comprehensive Strategies for Implementing Effective Firewall Configurations via VBScript, PowerShell, and the Advanced Installer Tool
thumbnail: https://thmb.techidaily.com/1d09a35d2889f182293f6c4568acc826b5a70f4b0e7972cc64ae0b415a19b02f.jpg
---

## Comprehensive Strategies for Implementing Effective Firewall Configurations via VBScript, PowerShell, and the Advanced Installer Tool

>  Disclaimer: This post includes affiliate links
>
>  If you click on a link and make a purchase, I may receive a commission at no extra cost to you.
>

## Firewall

To make the environment more secure it’s important to properly define and configure the firewall of your machines. However, there might be times when a specific executable must be added as an exception to the Inbound or Outbound rules of the firewall in order to have access.

In this article, let’s have a look at how you can configure firewall rules via MSI with Advanced Installer, VBScript and Powershell.

<!-- affiliate ads begin -->
<a href="https://aligracehair.sjv.io/c/5597632/2016148/19272" target="_top" id="2016148">
  <img src="//a.impactradius-go.com/display-ad/19272-2016148" border="0" alt="https://techidaily.com" width="728" height="90"/>
</a>
<img height="0" width="0" src="https://aligracehair.sjv.io/i/5597632/2016148/19272" style="position:absolute;visibility:hidden;" border="0" />
<!-- affiliate ads end -->

### Firewall rules with VBScript

Although you can use the **HNetCfg.FwAuthorizedApplication** object with VBScript to define firewall rules, the easiest method is to call the [**netsh.exe**](https://learn.microsoft.com/en-us/windows-server/networking/technologies/netsh/netsh-contexts "netsh.exe") utility that it’s included in Windows. This command-line utility allows you to modify the network configuration of a certain machine that is currently running. One of the commands available for netsh is **advfirewall** which allows you to change to the netsh advfirewall context. Jumping further into the context, you can type 

netsh advfirewall firewall

Copy

Into a cmd window and this will give you the following options:

?          	- Displays a list of commands.
add        	- Adds a new inbound or outbound firewall rule.
delete     	- Deletes all matching firewall rules.
dump       	- Displays a configuration script.
help       	- Displays a list of commands.
set        	- Sets new values for properties of a existing rule.
show       	- Displays a specified firewall rule.

Copy

So basically if we want to add a firewall rule we can use:

netsh.exe advfirewall firewall add rule name=FRIENDLYNAME dir=IN/OUT action=ALLOW/DENY program=PATHTOEXE enable=YES/NO profile=domain

Copy

If we want to remove a firewall rule we can use:

netsh.exe advfirewall firewall delete rule name=FRIENDLYNAME

Copy

Now that we are aware of how netsh is working with firewall rules, let’s assume we have a HelloWorld.exe that we want to add to the inbound firewall and we want to allow everything. With VBScript we can produce the following:

Dim WshShell
Dim programPath2, programfiless, programfiles
Set WshShell = CreateObject("Wscript.Shell")
programfiless=WshShell.ExpandEnvironmentStrings("%ProgramFiles(x86)%")
programfiles=WshShell.ExpandEnvironmentStrings("%ProgramW6432%")
ProgramPath2 = programfiless & "\Program Files (x86)\Caphyon\Firewall App\HelloWorld.exe"
WshShell.Run "netsh.exe advfirewall firewall add rule name=HelloWorld dir=in action=allow program=" & chr(34) & ProgramPath2 & chr(34) & " enable=yes profile=domain ", 0, False

Copy

This VBScript performs the following actions:

* Dim WshShell: Declares a variable named WshShell to hold a reference to the Windows Script Host Shell object.
* Dim programPath2, programfiless, programfiles: Declares variables to store the paths of program files.
* Set WshShell = CreateObject("Wscript.Shell"): Creates an instance of the Windows Script Host Shell object.
* programfiless = WshShell.ExpandEnvironmentStrings("%ProgramFiles(x86)%"): Retrieves the path of the "Program Files (x86)" folder using the %ProgramFiles(x86)% environment variable.
* programfiles = WshShell.ExpandEnvironmentStrings("%ProgramW6432%"): Retrieves the path of the "Program Files" folder using the %ProgramW6432% environment variable.
* ProgramPath2 = programfiless & "\\Program Files (x86)\\Caphyon\\Firewall App\\HelloWorld.exe": Concatenates the program file path with the specific file name to create the full path of the executable file "HelloWorld.exe".
* WshShell.Run "netsh.exe advfirewall firewall add rule name=HelloWorld dir=in action=allow program=" & chr(34) & ProgramPath2 & chr(34) & " enable=yes profile=domain ", 0, False: Runs the netsh.exe command to add a firewall rule named "HelloWorld" with the specified properties. The command allows incoming traffic (dir=in), allows the specified program (program=) with the path of "HelloWorld.exe", enables the rule (enable=yes), and applies the rule to the domain profile.

Next, open Advanced Installer and navigate to the Custom Actions Page. In here, search for the **Launch attached file** and select the location of the VBScript. Next, configure the custom action to execute as shown below:

![Launch attached file](https://cdn.advancedinstaller.com/img/configure-firewall-rules-with-custom-actions/addfwCA.png "Launch attached file")  

<!-- affiliate ads begin -->
<a href="https://aligracehair.sjv.io/c/5597632/2135361/19272" target="_top" id="2135361">
  <img src="//a.impactradius-go.com/display-ad/19272-2135361" border="0" alt="https://techidaily.com" width="728" height="90"/>
</a>
<img height="0" width="0" src="https://aligracehair.sjv.io/i/5597632/2135361/19272" style="position:absolute;visibility:hidden;" border="0" />
<!-- affiliate ads end -->

As a best practice it’s also important to remove the firewall rule during the uninstallation. For that, it means we need another Custom Action and a different VBScrit to remove our rule. The VBScript code is:

Dim WshShell
Set WshShell = CreateObject("Wscript.Shell")
WshShell.Run "netsh.exe advfirewall firewall delete rule name=HelloWorld"

Copy

After that, follow the same exact steps as above and configure the custom action as following:

![configure the custom action](https://cdn.advancedinstaller.com/img/configure-firewall-rules-with-custom-actions/remfwCA.png "configure the custom action")  

### Firewall rules with PowerShell

While **netsh** is still available and widely used by the community, starting with Windows 8.1 you can use the buit-in **NetSecurity** PowerShell module to manage firewall operations.

In general, there are 85 commands available in this module that you can use in Windows 10/11, but we are only interested in two of them. To add a firewall rule you can simply do:

$HelloWorldLocation = ${env:ProgramFiles(x86)} + "\Caphyon\Firewall App\HelloWorld.exe"
New-NetFirewallRule -Program $HelloWorldLocation -Action Allow -Profile Domain -DisplayName “HelloWorld” -Description “Block Firefox browser” -Direction Inbound

Copy

To remove a firewall rule is even simpler as we only use the [Remove-NetFirewallRule](https://learn.microsoft.com/en-us/powershell/module/netsecurity/remove-netfirewallrule?view=windowsserver2022-ps "Remove-NetFirewallRule") PowerShell cmdlet:

Remove-NetFirewallRule -DisplayName "HelloWorld"

Copy

Next, open Advanced Installer and navigate to the Custom Actions Page. In here, search for the **Run PowerShell script file** and select the location of the PowerShell script. Next, configure the custom action to execute as shown below:

![Run PowerShell script file](https://cdn.advancedinstaller.com/img/configure-firewall-rules-with-custom-actions/addfwPSCA.png "Run PowerShell script file")  

To also add the remove firewall PowerShell script, follow the same steps as above and do the following configurations:

![remove firewall PowerShell script](https://cdn.advancedinstaller.com/img/configure-firewall-rules-with-custom-actions/remfwPSCA.png "remove firewall PowerShell script")  

<!-- affiliate ads begin -->
<a href="https://unicoeye.pxf.io/c/5597632/2134498/18498" target="_top" id="2134498">
  <img src="//a.impactradius-go.com/display-ad/18498-2134498" border="0" alt="https://techidaily.com" width="720" height="90"/>
</a>
<img height="0" width="0" src="https://unicoeye.pxf.io/i/5597632/2134498/18498" style="position:absolute;visibility:hidden;" border="0" />
<!-- affiliate ads end -->

### Firewall rules with Advanced Installer

If you don’t like to code, Advanced Installer made it much simpler to add firewall rules. First, navigate to the [Windows Firewall](https://tools.techidaily.com/advancedinstaller/products/)[page](https://tools.techidaily.com/advancedinstaller/products/).

Next, click on **New Rule**. This will open a new window in which you can define the necessary details for your exception:

![Windows Firewall page](https://cdn.advancedinstaller.com/img/configure-firewall-rules-with-custom-actions/advfw.png "Windows Firewall page")  

As you can see, you can easily choose the direction, display name, program path, protocol and other settings directly from the GUI. In our case we wanted to mimic the above usages of netsh and PowerShell and left everything as before in the GUI.

And that is it, Advanced Installer will automatically create the exception during the installation and during the uninstallation it will remove the exception from the firewall, not needing to create two separate actions for it.

![remove the exception from the firewall](https://cdn.advancedinstaller.com/img/configure-firewall-rules-with-custom-actions/advfw2.png "remove the exception from the firewall")  

<!-- affiliate ads begin -->
<a href="https://ephamedtechinc.pxf.io/c/5597632/2136619/26400" target="_top" id="2136619">
  <img src="//a.impactradius-go.com/display-ad/26400-2136619" border="0" alt="https://techidaily.com" width="728" height="90"/>
</a>
<img height="0" width="0" src="https://ephamedtechinc.pxf.io/i/5597632/2136619/26400" style="position:absolute;visibility:hidden;" border="0" />
<!-- affiliate ads end -->

All you have to do is build and install the MSI package. After the installation, if we check the Inbound rules, our rule is there:

![check the Inbound rules](https://cdn.advancedinstaller.com/img/configure-firewall-rules-with-custom-actions/inbound.png "check the Inbound rules")

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
<li><a href="https://youtube-web.techidaily.com/n-2024-create-impactful-youtube-videos-top-20-font-picks/"><u>[New] In 2024, Create Impactful YouTube Videos Top 20 Font Picks</u></a></li>
<li><a href="https://youtube-webster.techidaily.com/n-2024-echoing-ethos-with-closing-credits/"><u>[New] In 2024, Echoing Ethos with Closing Credits</u></a></li>
<li><a href="https://extra-support.techidaily.com/new-m1-nexus-smooth-transitions-unmatched-editing-velocity/"><u>[New] M1 Nexus Smooth Transitions, Unmatched Editing Velocity</u></a></li>
<li><a href="https://facebook-videos.techidaily.com/updated-unveiling-the-secrets-of-facebook-video-content-success-for-2024/"><u>[Updated] Unveiling the Secrets of Facebook Video Content Success for 2024</u></a></li>
<li><a href="https://win-cloud.techidaily.com/1-easy-guide-streaming-flv-files-on-iphone-and-ipad-without-hitches/"><u>1. Easy Guide: Streaming Flv Files on iPhone & iPad Without Hitches</u></a></li>
<li><a href="https://fox-friendly.techidaily.com/2024-approved-fine-tuning-focus-a-compreeher-guide-for-videoleap-users/"><u>2024 Approved Fine-Tuning Focus A Compreeher Guide for Videoleap Users</u></a></li>
<li><a href="https://win-cloud.techidaily.com/1726027438379-dvdmp4/"><u>店頭で入手したDVDをMP4フォーマットに直すコツ</u></a></li>
<li><a href="https://win-cloud.techidaily.com/1726030101997-gifgif/"><u>覚えやすくて楽しい GIFアニメ化ガイド:カワイコト・オシャレな動画を遊び心溢れるGIFに変える</u></a></li>
<li><a href="https://video-capture.techidaily.com/become-a-pro-at-xbox-video-recording-in-minutes-for-2024/"><u>Become a Pro at Xbox Video Recording in Minutes for 2024</u></a></li>
<li><a href="https://games-able.techidaily.com/discover-valves-new-upgrades-advanced-game-distribution-in-families-and-improved-parental-management-systems-on-steam/"><u>Discover Valve's New Upgrades: Advanced Game Distribution in Families and Improved Parental Management Systems on Steam</u></a></li>
<li><a href="https://win-cloud.techidaily.com/1726028974432-movmp4/"><u>MOVファイル大量バッチをMP4に転化する究極ガイド</u></a></li>
<li><a href="https://win-cloud.techidaily.com/1726030027717-pc3/"><u>PCに付属のオーディオ転送ソフトウェア3つ！長時間ボイスレコードへ</u></a></li>
<li><a href="https://driver-install.techidaily.com/standard-procedure-uninstalling-wacom-on-various-windows-editions/"><u>Standard Procedure: Uninstalling Wacom on Various Windows Editions</u></a></li>
<li><a href="https://win-cloud.techidaily.com/1726030406704-step-by-step-strategies-for-superior-video-upgrades-using-vlc-player-comprehensive-guide/"><u>Step-by-Step Strategies for Superior Video Upgrades Using VLC Player (Comprehensive Guide )</u></a></li>
<li><a href="https://win-cloud.techidaily.com/successfully-streaming-webm-files-on-your-ios-device-tips-and-tricks/"><u>Successfully Streaming WebM Files on Your iOS Device: Tips & Tricks</u></a></li>
<li><a href="https://tech-recovery.techidaily.com/ultimate-guide-resolving-ac1st16dll-file-not-found-issues/"><u>Ultimate Guide: Resolving 'Ac1st16.dll' File Not Found Issues</u></a></li>
<li><a href="https://win-cloud.techidaily.com/44ot44oh44kq44kr44oh44op44gl44kj44gu5yuv55s744ks44or44k944kz44oz5lik44gn44ov44k244go57eo6zug44gz44kl6ieq55sx44k944ov44oi6yg444gz44o75oyh5y2x/"><u>ビデオカメラからの動画をパソコン上でワザと編集する自由ソフト選び・指南</u></a></li>
</ul></div>

