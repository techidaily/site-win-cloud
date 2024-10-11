---
title: Comprehensive Strategies for Implementing Effective Firewall Configurations via VBScript, PowerShell, and the Advanced Installer Tool
date: 2024-10-07T22:12:57.292Z
updated: 2024-10-10T18:30:06.196Z
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

As a best practice it’s also important to remove the firewall rule during the uninstallation. For that, it means we need another Custom Action and a different VBScrit to remove our rule. The VBScript code is:

Dim WshShell
Set WshShell = CreateObject("Wscript.Shell")
WshShell.Run "netsh.exe advfirewall firewall delete rule name=HelloWorld"

Copy

After that, follow the same exact steps as above and configure the custom action as following:

![configure the custom action](https://cdn.advancedinstaller.com/img/configure-firewall-rules-with-custom-actions/remfwCA.png "configure the custom action")  

<!-- affiliate ads begin -->
<a href="https://appsumo.8odi.net/c/5597632/2111982/7443" target="_top" id="2111982">
  <img src="//a.impactradius-go.com/display-ad/7443-2111982" border="0" alt="https://techidaily.com" width="728" height="90"/>
</a>
<img height="0" width="0" src="https://appsumo.8odi.net/i/5597632/2111982/7443" style="position:absolute;visibility:hidden;" border="0" />
<!-- affiliate ads end -->

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

<!-- affiliate ads begin -->
<a href="https://unicoeye.pxf.io/c/5597632/2134235/18498" target="_top" id="2134235">
  <img src="//a.impactradius-go.com/display-ad/18498-2134235" border="0" alt="https://techidaily.com" width="728" height="90"/>
</a>
<img height="0" width="0" src="https://unicoeye.pxf.io/i/5597632/2134235/18498" style="position:absolute;visibility:hidden;" border="0" />
<!-- affiliate ads end -->

To also add the remove firewall PowerShell script, follow the same steps as above and do the following configurations:

![remove firewall PowerShell script](https://cdn.advancedinstaller.com/img/configure-firewall-rules-with-custom-actions/remfwPSCA.png "remove firewall PowerShell script")  

### Firewall rules with Advanced Installer

If you don’t like to code, Advanced Installer made it much simpler to add firewall rules. First, navigate to the [Windows Firewall](https://tools.techidaily.com/advancedinstaller/products/)[page](https://tools.techidaily.com/advancedinstaller/products/).

Next, click on **New Rule**. This will open a new window in which you can define the necessary details for your exception:

![Windows Firewall page](https://cdn.advancedinstaller.com/img/configure-firewall-rules-with-custom-actions/advfw.png "Windows Firewall page")  

<!-- affiliate ads begin -->
<a href="https://aligracehair.sjv.io/c/5597632/1885943/19272" target="_top" id="1885943">
  <img src="//a.impactradius-go.com/display-ad/19272-1885943" border="0" alt="https://techidaily.com" width="300" height="90"/>
</a>
<img height="0" width="0" src="https://aligracehair.sjv.io/i/5597632/1885943/19272" style="position:absolute;visibility:hidden;" border="0" />
<!-- affiliate ads end -->

As you can see, you can easily choose the direction, display name, program path, protocol and other settings directly from the GUI. In our case we wanted to mimic the above usages of netsh and PowerShell and left everything as before in the GUI.

And that is it, Advanced Installer will automatically create the exception during the installation and during the uninstallation it will remove the exception from the firewall, not needing to create two separate actions for it.

![remove the exception from the firewall](https://cdn.advancedinstaller.com/img/configure-firewall-rules-with-custom-actions/advfw2.png "remove the exception from the firewall")  

All you have to do is build and install the MSI package. After the installation, if we check the Inbound rules, our rule is there:

![check the Inbound rules](https://cdn.advancedinstaller.com/img/configure-firewall-rules-with-custom-actions/inbound.png "check the Inbound rules")

<!-- affiliate ads begin -->
<a href="https://appsumo.8odi.net/c/5597632/2144280/7443" target="_top" id="2144280">
  <img src="//a.impactradius-go.com/display-ad/7443-2144280" border="0" alt="https://techidaily.com" width="600" height="90"/>
</a>
<img height="0" width="0" src="https://appsumo.8odi.net/i/5597632/2144280/7443" style="position:absolute;visibility:hidden;" border="0" />
<!-- affiliate ads end -->

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
<li><a href="https://vp-tips.techidaily.com/new-2024-approved-decoding-the-nuances-of-whatsapp-audio-messages/"><u>[New] 2024 Approved Decoding the Nuances of WhatsApp Audio Messages</u></a></li>
<li><a href="https://vp-tips.techidaily.com/new-2024-approved-selective-software-optimal-blu-ray-players-free-to-pay/"><u>[New] 2024 Approved Selective Software Optimal Blu-Ray Players (Free to Pay)</u></a></li>
<li><a href="https://vp-tips.techidaily.com/new-in-2024-hero5-black-or-yi-comparing-top-actions-cameras/"><u>[New] In 2024, Hero5 Black or YI Comparing Top Actions Cameras</u></a></li>
<li><a href="https://snapchat-videos.techidaily.com/new-lens-language-speaking-visually-with-snapchat-filters-for-2024/"><u>[New] Lens Language Speaking Visually with Snapchat Filters for 2024</u></a></li>
<li><a href="https://article-files.techidaily.com/updated-unleash-creativity-no-cost-high-quality-text-psds-for-2024/"><u>[Updated] Unleash Creativity No-Cost, High-Quality Text PSDs for 2024</u></a></li>
<li><a href="https://youtube-tips.techidaily.com/approved-exploring-advanced-techniques-in-video-thumbnail-creation/"><u>2024 Approved Exploring Advanced Techniques in Video Thumbnail Creation</u></a></li>
<li><a href="https://win-cloud.techidaily.com/best-10-anti-cyberattack-tools-for-both-windows-and-android-devices-a-comprehensive-guide/"><u>Best 10 Anti-Cyberattack Tools for Both Windows & Android Devices: A Comprehensive Guide</u></a></li>
<li><a href="https://win-cloud.techidaily.com/effective-strategies-for-removing-the-gstatic-threat-using-malwarefox-tool/"><u>Effective Strategies for Removing the Gstatic Threat Using MalwareFox Tool</u></a></li>
<li><a href="https://win-cloud.techidaily.com/fastconvert-pro-the-efficient-replacement-for-format-factory-on-pcs-and-macs/"><u>FastConvert Pro: The Efficient Replacement for Format Factory on PCs and Macs</u></a></li>
<li><a href="https://win-answers.techidaily.com/fixing-hearts-of-iron-4-malfunctions-proven-methods-and-fixes-for-enthusiasts/"><u>Fixing Hearts of Iron 4 Malfunctions: Proven Methods and Fixes for Enthusiasts</u></a></li>
<li><a href="https://win-cloud.techidaily.com/flv-to-wmv-conversion-guide-seamless-video-format-switching-tips-and-tricks/"><u>FLV to WMV Conversion Guide - Seamless Video Format Switching Tips and Tricks</u></a></li>
<li><a href="https://video-capture.techidaily.com/get-the-official-avid-dnxhd-codec-now-supporting-windows-1011-and-mac-os-x-systems/"><u>Get the Official Avid DNxHD Codec, Now Supporting Windows 10/11 & Mac OS X Systems</u></a></li>
<li><a href="https://win-cloud.techidaily.com/guide-to-mastering-pubg-mobile-battle-royale-on-your-computer/"><u>Guide to Mastering PUBG Mobile Battle Royale on Your Computer</u></a></li>
<li><a href="https://win-cloud.techidaily.com/how-to-convert-your-audio-recordings-into-written-format-at-no-cost-effective-techniques-and-tools/"><u>How to Convert Your Audio Recordings Into Written Format at No Cost: Effective Techniques and Tools</u></a></li>
<li><a href="https://sim-unlock.techidaily.com/in-2024-android-unlock-code-sim-unlock-your-itel-p55-5g-phone-and-remove-locked-screen-by-drfone-android/"><u>In 2024, Android Unlock Code Sim Unlock Your Itel P55 5G Phone and Remove Locked Screen</u></a></li>
<li><a href="https://unlock-android.techidaily.com/in-2024-how-to-remove-screen-lock-pin-on-honor-80-pro-straight-screen-edition-like-a-pro-5-easy-ways-by-drfone-android/"><u>In 2024, How To Remove Screen Lock PIN On Honor 80 Pro Straight Screen Edition Like A Pro 5 Easy Ways</u></a></li>
<li><a href="https://win-cloud.techidaily.com/top-free-avi-to-mp4-converters-of-2014-compare-and-choose-the-ideal-one/"><u>Top Free AVI to MP4 Converters of 2014: Compare and Choose the Ideal One!</u></a></li>
<li><a href="https://win-cloud.techidaily.com/ultimate-guide-thorough-file-deletion-techniques-for-your-iphone-6-or-6-plus/"><u>Ultimate Guide: Thorough File Deletion Techniques for Your iPhone 6 or 6 Plus</u></a></li>
<li><a href="https://win-cloud.techidaily.com/understanding-key-distinctions-icloud-backup-vs-itunes-backup-explained/"><u>Understanding Key Distinctions: ICloud Backup Vs. ITunes Backup Explained</u></a></li>
</ul></div>

