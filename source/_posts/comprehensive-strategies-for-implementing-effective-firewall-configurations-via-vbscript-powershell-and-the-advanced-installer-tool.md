---
title: Comprehensive Strategies for Implementing Effective Firewall Configurations via VBScript, PowerShell, and the Advanced Installer Tool
date: 2024-10-02T19:53:31.209Z
updated: 2024-10-05T18:39:19.656Z
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

<!-- affiliate ads begin -->
<a href="https://appsumo.8odi.net/c/5597632/2118320/7443" target="_top" id="2118320">
  <img src="//a.impactradius-go.com/display-ad/7443-2118320" border="0" alt="https://techidaily.com" width="728" height="90"/>
</a>
<img height="0" width="0" src="https://appsumo.8odi.net/i/5597632/2118320/7443" style="position:absolute;visibility:hidden;" border="0" />
<!-- affiliate ads end -->

As a best practice it’s also important to remove the firewall rule during the uninstallation. For that, it means we need another Custom Action and a different VBScrit to remove our rule. The VBScript code is:

Dim WshShell
Set WshShell = CreateObject("Wscript.Shell")
WshShell.Run "netsh.exe advfirewall firewall delete rule name=HelloWorld"

Copy

After that, follow the same exact steps as above and configure the custom action as following:

![configure the custom action](https://cdn.advancedinstaller.com/img/configure-firewall-rules-with-custom-actions/remfwCA.png "configure the custom action")  

<!-- affiliate ads begin -->
<a href="https://aligracehair.sjv.io/c/5597632/1997648/19272" target="_top" id="1997648">
  <img src="//a.impactradius-go.com/display-ad/19272-1997648" border="0" alt="https://techidaily.com" width="728" height="90"/>
</a>
<img height="0" width="0" src="https://aligracehair.sjv.io/i/5597632/1997648/19272" style="position:absolute;visibility:hidden;" border="0" />
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

To also add the remove firewall PowerShell script, follow the same steps as above and do the following configurations:

![remove firewall PowerShell script](https://cdn.advancedinstaller.com/img/configure-firewall-rules-with-custom-actions/remfwPSCA.png "remove firewall PowerShell script")  

### Firewall rules with Advanced Installer

If you don’t like to code, Advanced Installer made it much simpler to add firewall rules. First, navigate to the [Windows Firewall](https://tools.techidaily.com/advancedinstaller/products/)[page](https://tools.techidaily.com/advancedinstaller/products/).

Next, click on **New Rule**. This will open a new window in which you can define the necessary details for your exception:

![Windows Firewall page](https://cdn.advancedinstaller.com/img/configure-firewall-rules-with-custom-actions/advfw.png "Windows Firewall page")  

<!-- affiliate ads begin -->
<a href="https://aligracehair.sjv.io/c/5597632/2080312/19272" target="_top" id="2080312">
  <img src="//a.impactradius-go.com/display-ad/19272-2080312" border="0" alt="https://techidaily.com" width="300" height="90"/>
</a>
<img height="0" width="0" src="https://aligracehair.sjv.io/i/5597632/2080312/19272" style="position:absolute;visibility:hidden;" border="0" />
<!-- affiliate ads end -->

As you can see, you can easily choose the direction, display name, program path, protocol and other settings directly from the GUI. In our case we wanted to mimic the above usages of netsh and PowerShell and left everything as before in the GUI.

And that is it, Advanced Installer will automatically create the exception during the installation and during the uninstallation it will remove the exception from the firewall, not needing to create two separate actions for it.

![remove the exception from the firewall](https://cdn.advancedinstaller.com/img/configure-firewall-rules-with-custom-actions/advfw2.png "remove the exception from the firewall")  

All you have to do is build and install the MSI package. After the installation, if we check the Inbound rules, our rule is there:

![check the Inbound rules](https://cdn.advancedinstaller.com/img/configure-firewall-rules-with-custom-actions/inbound.png "check the Inbound rules")

<!-- affiliate ads begin -->
<a href="https://aligracehair.sjv.io/c/5597632/2027181/19272" target="_top" id="2027181">
  <img src="//a.impactradius-go.com/display-ad/19272-2027181" border="0" alt="https://techidaily.com" width="728" height="90"/>
</a>
<img height="0" width="0" src="https://aligracehair.sjv.io/i/5597632/2027181/19272" style="position:absolute;visibility:hidden;" border="0" />
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
<li><a href="https://youtube-blog.techidaily.com/024-approved-unmatched-youtube-success-10-unique-tips-for-your-shorts/"><u>[New] 2024 Approved Unmatched YouTube Success 10 Unique Tips for Your Shorts</u></a></li>
<li><a href="https://youtube-tips.techidaily.com/ed-in-2024-charting-a-path-to-success-exploring-15-top-youtube-beginnings/"><u>[Updated] In 2024, Charting a Path to Success Exploring 15 Top YouTube Beginnings</u></a></li>
<li><a href="https://fox-access.techidaily.com/2024-approved-the-essential-guide-to-podcast-creation-in-garageband/"><u>2024 Approved The Essential Guide to Podcast Creation in GarageBand</u></a></li>
<li><a href="https://win-cloud.techidaily.com/best-iphone-screen-mirroring-applications-ranked/"><u>Best iPhone Screen Mirroring Applications Ranked</u></a></li>
<li><a href="https://win-cloud.techidaily.com/beware-of-phishing-understanding-the-email-confirmation-trojan-disguised-as-a-shipping-update/"><u>Beware of Phishing: Understanding the 'Email Confirmation' Trojan Disguised as a Shipping Update</u></a></li>
<li><a href="https://some-techniques.techidaily.com/garageband-strategies-for-perfect-podcast-editing-for-2024/"><u>GarageBand Strategies for Perfect Podcast Editing for 2024</u></a></li>
<li><a href="https://apple-account.techidaily.com/how-to-delete-icloud-account-from-apple-iphone-x-without-password-by-drfone-ios/"><u>How to Delete iCloud Account From Apple iPhone X without Password?</u></a></li>
<li><a href="https://buynow-tips.techidaily.com/in-depth-nest-audio-evaluation-the-ultimate-sound-experience-for-audiophiles/"><u>In-Depth Nest Audio Evaluation: The Ultimate Sound Experience for Audiophiles</u></a></li>
<li><a href="https://win-cloud.techidaily.com/quick-tips-how-to-shrink-mp3-audio-on-your-computer-and-phone-with-ease/"><u>Quick Tips: How to Shrink MP3 Audio on Your Computer and Phone with Ease</u></a></li>
<li><a href="https://win-cloud.techidaily.com/seamless-syncing-techniques-iphone-data-migration-to-windows-11/"><u>Seamless Syncing Techniques: IPhone Data Migration to Windows 11</u></a></li>
<li><a href="https://win-cloud.techidaily.com/step-by-step-guide-adding-protective-watermarks-to-your-instagram-images/"><u>Step-by-Step Guide: Adding Protective Watermarks to Your Instagram Images</u></a></li>
<li><a href="https://win-cloud.techidaily.com/top-6-ideal-mobile-applications-for-enhancing-your-images-with-perfect-backgrounds/"><u>Top 6 Ideal Mobile Applications for Enhancing Your Images with Perfect Backgrounds</u></a></li>
<li><a href="https://youtube-clips.techidaily.com/transformative-content-blocks-enhanced-youtube-decks/"><u>Transformative Content Blocks Enhanced Youtube Decks</u></a></li>
<li><a href="https://techno-recovery.techidaily.com/ultimate-guide-resolving-ntdlldll-issues-on-your-pc-windows-1087/"><u>Ultimate Guide: Resolving ntdll.dll Issues on Your PC (Windows 10/8/7)</u></a></li>
<li><a href="https://win-cloud.techidaily.com/understanding-the-impact-of-malware-operation-consequences-and-prevention-strategies/"><u>Understanding the Impact of Malware: Operation, Consequences, & Prevention Strategies</u></a></li>
</ul></div>

