---
title: 1. Remove Unused Folders Automatically with PowerShell Scripts
date: 2024-10-10T01:45:03.664Z
updated: 2024-10-10T19:18:17.675Z
tags:
  - application-packaging-training
categories:
  - advancedinstaller
description: This Article Describes 1. Remove Unused Folders Automatically with PowerShell Scripts
thumbnail: https://thmb.techidaily.com/24b4a5d68fd5e6bea75410f8f6c4c82cdd5bcbea33115cb8218e3e0a99c10ef2.jpg
---

## 1. Remove Unused Folders Automatically with PowerShell Scripts

>  Disclaimer: This post includes affiliate links
>
>  If you click on a link and make a purchase, I may receive a commission at no extra cost to you.
>

## Delete empty directory

Technically, during the uninstallation process, based on what directories are defined in the Directory table, the MSI usually handles the deletion of all directories and assures a clean machine. However, the rules of erasing directories are:

* The directory must be empty
* The directory must appear in the Directory table

Looking at the requirements, it might be the case that the application creates another directory that isn’t present in the Directory table, and when the MSI is uninstalled, the directories will remain behind.

As discussed in our first book, you have the RemoveFile table. To ensure that a directory is deleted during MSI uninstallation, you need to modify the RemoveFile table in the MSI. Here's how you can achieve this:

* Open the MSI file using a compatible MSI editing tool like Orca, Wise Package Studio, or Advanced Installer.
* Locate and open the RemoveFile table within the MSI editing tool. This table specifies files and directories to be removed during uninstallation.
* Identify the directory you want to delete during uninstallation.
* Add a new row to the RemoveFile table with the following information: **Component**: Enter the component identifier for the component that contains the directory, **FileName**: Specify the name of the directory (not the full path), **DirProperty**: Enter the directory property associated with the directory.
* Save the modified MSI file.

By adding an entry in the RemoveFile table, you instruct the Windows Installer to remove the specified directory during the uninstallation process. The Windows Installer engine will automatically remove the directory if it exists and is empty. If the directory is not empty, the uninstallation will fail unless you have custom actions or scripts in place to handle the removal of files within the directory.

<!-- affiliate ads begin -->
<span id="1977028">
					<video width="128" height="480" style="cursor:pointer"
           poster="//a.impactradius-go.com/display-clicktoplayimage/1977028.png"
           onclick="if(!this.playClicked){this.play();this.setAttribute('controls',true);this.playClicked=true;}">
	   <source src="//a.impactradius-go.com/display-ad/22993-1977028">
	   <img src="//a.impactradius-go.com/display-clicktoplayimage/1977028.png" style="border: none; height: 100%; width: 100%; object-fit: contain">
	</video>
	<div style="width:80px;text-align:center"><a href="javascript:window.open(decodeURIComponent('https%3A%2F%2Fhomestyler.sjv.io%2Fc%2F5597632%2F1977028%2F22993'), '_blank');void(0);">Click here</a></div>
</span>
<img height="0" width="0" src="https://imp.pxf.io/i/5597632/1977028/22993" style="position:absolute;visibility:hidden;" border="0" />
<!-- affiliate ads end -->

### Delete empty directories with Custom Actions

**Delete with VBScript**

When it comes to removing emty directories you can go in two ways with VBScript:

* Use the navite functions VBScript provides
* Use the [RD utility](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/rd "RD utility") inside the OS

When it comes to native VBScript functions, we can use the following script:

Dim objFSO, objFolder
Dim folderPath
folderPath = "C:\Path\to\folder"
Set objFSO = CreateObject("Scripting.FileSystemObject")
' Check if the folder exists
If objFSO.FolderExists(folderPath) Then
    Set objFolder = objFSO.GetFolder(folderPath)
    ' Check if the folder is empty
    If objFolder.Files.Count = 0 And objFolder.SubFolders.Count = 0 Then
        ' Delete the empty folder
        objFolder.Delete
        WScript.Echo "Folder deleted successfully."
    Else
        WScript.Echo "Folder is not empty."
    End If
Else
    WScript.Echo "Folder does not exist."
End If
Set objFolder = Nothing
Set objFSO = Nothing

Copy

Replace "C:Pathtofolder" with the actual path to the folder you want to inspect. This script checks the folder's existence, counts the files and subfolders within it, and deletes the folder if it is empty. Let's look at how the script works:

* The script begins by declaring variables: objFSO, objFolder, and folderPath.
* folderPath is set to the path of the folder you want to check for emptiness and delete if empty.
* CreateObject("Scripting.FileSystemObject") creates an instance of the FileSystemObject to work with file system objects.
* objFSO.FolderExists(folderPath) checks if the folder specified by folderPath exists.
* If the folder exists, objFolder is set to represent the folder using objFSO.GetFolder(folderPath).
* The script then checks if the folder is empty by verifying that both objFolder.Files.Count (number of files) and objFolder.SubFolders.Count (number of subfolders) are zero.
* If the folder is empty, objFolder.Delete is called to delete the folder.
* If the folder is not empty or if it doesn't exist, appropriate messages are echoed using WScript.Echo.
* Finally, the script releases the object references by setting objFolder and objFSO to Nothing.

If we want to use the RD utility together with VBScript, the following code can be used:

Dim objShell
Dim folderPath
folderPath = "C:\Path\to\folder"
Set objShell = CreateObject("WScript.Shell")
' Check if the folder exists
If objShell.FileSystemObject.FolderExists(folderPath) Then
    ' Execute the rd command to delete the folder
    objShell.Run "cmd /c rd /s /q """ & folderPath & """", 0, True
    WScript.Echo "Folder deleted successfully."
Else
    WScript.Echo "Folder does not exist."
End If
Set objShell = Nothing

Copy

Here's how the script works:

* The script begins by declaring variables: objShell and folderPath.
* folderPath is set to the path of the folder you want to delete.
* CreateObject("WScript.Shell") creates an instance of the WScript.Shell object, which allows us to execute shell commands.
* The script checks if the folder specified by folderPath exists using objShell.FileSystemObject.FolderExists.
* If the folder exists, objShell.Run is used to execute the rd command with the appropriate arguments (/s to delete all files and subdirectories, and /q to perform the deletion silently without prompts).
* The folder is enclosed in double quotes to handle any spaces or special characters in the folder path.
* The third argument of objShell.Run is set to True, which specifies that the script should wait for the command to complete before continuing.
* After the folder is deleted, a success message is echoed using WScript.Echo.
* If the folder does not exist, an appropriate message is echoed.
* Finally, the script releases the object reference by setting objShell to Nothing.

### Delete with PowerShell

For PowerShell, we can use the following script:

$folderPath = "C:\Path\to\folder"

<!-- affiliate ads begin -->
<a href="https://appsumo.8odi.net/c/5597632/2111967/7443" target="_top" id="2111967">
  <img src="//a.impactradius-go.com/display-ad/7443-2111967" border="0" alt="https://techidaily.com" width="728" height="90"/>
</a>
<img height="0" width="0" src="https://appsumo.8odi.net/i/5597632/2111967/7443" style="position:absolute;visibility:hidden;" border="0" />
<!-- affiliate ads end -->

## Check if the folder exists
if (Test-Path $folderPath) {
# Check if the folder is empty
    if ((Get-ChildItem $folderPath | Measure-Object).Count -eq 0) {
# Delete the folder
        Remove-Item $folderPath -Force -Recurse
        Write-Host "Folder deleted successfully."
    } else {
        Write-Host "Folder is not empty."

} else {
    Write-Host "Folder does not exist."

Copy

Replace "C:\\Path\\to\\folder" with the actual path of the folder you want to check. Here's how the script works:

* The script begins by setting the variable $folderPath to the path of the folder you want to delete.
* The script uses the Test-Path cmdlet to check if the folder specified by $folderPath exists.
* If the folder exists, the script uses the Get-ChildItem cmdlet to get all items (files and subfolders) within the folder.
* The Measure-Object cmdlet is used to count the number of items. If the count is equal to zero, it means the folder is empty.
* If the folder is empty, the script uses the Remove-Item cmdlet to delete the folder.
* The -Force parameter is used to force the deletion without prompting for confirmation.
* The -Recurse parameter is used to delete the folder and its contents recursively.
* After deleting the folder, the script writes a success message using Write-Host.
* If the folder is not empty, the script writes a message indicating that the folder is not empty.
* If the folder does not exist, the script writes a message indicating that the folder does not exist.

<!-- affiliate ads begin -->
<a href="https://appsumo.8odi.net/c/5597632/2049382/7443" target="_top" id="2049382">
  <img src="//a.impactradius-go.com/display-ad/7443-2049382" border="0" alt="https://techidaily.com" width="728" height="90"/>
</a>
<img height="0" width="0" src="https://appsumo.8odi.net/i/5597632/2049382/7443" style="position:absolute;visibility:hidden;" border="0" />
<!-- affiliate ads end -->

### Delete empty directories with Advanced Installer

Advanced Installer offers a much simpler method to remove folders generated by the application. If you navigate to the Files and Folders page and right-click on a desired folder, you will see the Uninstall Cleanup option:

![ai files folders uninstall cleanup](https://cdn.advancedinstaller.com/img/delete-empty-directory-with-custom-actions/ai-files-folders-uninstall-cleanup.png "ai files folders uninstall cleanup")  

The uninstall cleanup wizard launches, allowing you to specify which files and folders created by your application will be removed when you uninstall it. To launch this wizard, select the parent folder, the directory from which you want to delete files and folders, and then choose "Uninstall Cleanup..." from the context menu. Following the wizard's launch, simply follow the on-screen instructions to select the files you want to delete.

![ai uninstall cleanup step1](https://cdn.advancedinstaller.com/img/delete-empty-directory-with-custom-actions/ai-uninstall-cleanup-step1.png "ai uninstall cleanup step1")  

<!-- affiliate ads begin -->
<a href="https://appsumo.8odi.net/c/5597632/2118305/7443" target="_top" id="2118305">
  <img src="//a.impactradius-go.com/display-ad/7443-2118305" border="0" alt="https://techidaily.com" width="728" height="90"/>
</a>
<img height="0" width="0" src="https://appsumo.8odi.net/i/5597632/2118305/7443" style="position:absolute;visibility:hidden;" border="0" />
<!-- affiliate ads end -->

As you can see, you can delete the desired folder and all of its subfolders, or just the contents of the folder, including non-empty directories.

![ai uninstall cleanup step2](https://cdn.advancedinstaller.com/img/delete-empty-directory-with-custom-actions/ai-uninstall-cleanup-step2.png "ai uninstall cleanup step2")  

Furthermore, Advanced Installer allows you to ask the user if the specified folders should be deleted when the cleanup is performed, or you can perform the cleanup silently.

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
<li><a href="https://youtube-sure.techidaily.com/024-approved-effortless-fusion-of-youtube-audio-with-imovie-masterpieces/"><u>[New] 2024 Approved Effortless Fusion of YouTube Audio with iMovie Masterpieces</u></a></li>
<li><a href="https://android-location.techidaily.com/1721104635219-solved-cant-open-acpi-atk0100-kernel-mode-driver/"><u>[Solved] Can't Open ACPI ATK0100 Kernel Mode Driver</u></a></li>
<li><a href="https://youtube-blog.techidaily.com/ed-fresh-start-strategy-budget-channels-to-purchase/"><u>[Updated] Fresh Start Strategy Budget Channels to Purchase</u></a></li>
<li><a href="https://fox-http.techidaily.com/updated-in-2024-instantaneous-access-to-high-quality-passport-selfies-free-desktopweb-tool/"><u>[Updated] In 2024, Instantaneous Access to High-Quality Passport Selfies – Free Desktop/Web Tool</u></a></li>
<li><a href="https://win-cloud.techidaily.com/best-10-anti-cyberattack-tools-for-both-windows-and-android-devices-a-comprehensive-guide/"><u>Best 10 Anti-Cyberattack Tools for Both Windows & Android Devices: A Comprehensive Guide</u></a></li>
<li><a href="https://win-cloud.techidaily.com/effective-strategies-for-removing-the-gstatic-threat-using-malwarefox-tool/"><u>Effective Strategies for Removing the Gstatic Threat Using MalwareFox Tool</u></a></li>
<li><a href="https://win-cloud.techidaily.com/fastconvert-pro-the-efficient-replacement-for-format-factory-on-pcs-and-macs/"><u>FastConvert Pro: The Efficient Replacement for Format Factory on PCs and Macs</u></a></li>
<li><a href="https://win-cloud.techidaily.com/flv-to-wmv-conversion-guide-seamless-video-format-switching-tips-and-tricks/"><u>FLV to WMV Conversion Guide - Seamless Video Format Switching Tips and Tricks</u></a></li>
<li><a href="https://win-cloud.techidaily.com/guide-to-mastering-pubg-mobile-battle-royale-on-your-computer/"><u>Guide to Mastering PUBG Mobile Battle Royale on Your Computer</u></a></li>
<li><a href="https://win-cloud.techidaily.com/how-to-convert-your-audio-recordings-into-written-format-at-no-cost-effective-techniques-and-tools/"><u>How to Convert Your Audio Recordings Into Written Format at No Cost: Effective Techniques and Tools</u></a></li>
<li><a href="https://phone-solutions.techidaily.com/in-2024-how-to-use-snapchat-location-spoofer-to-protect-your-privacy-on-tecno-spark-20c-drfone-by-drfone-virtual-android/"><u>In 2024, How to use Snapchat Location Spoofer to Protect Your Privacy On Tecno Spark 20C? | Dr.fone</u></a></li>
<li><a href="https://iphone-transfer.techidaily.com/in-2024-iphone-transfer-transfer-contact-from-apple-iphone-14-pro-max-to-iphone-without-icloud-drfone-by-drfone-transfer-from-ios/"><u>In 2024, iPhone Transfer Transfer Contact from Apple iPhone 14 Pro Max to iPhone without iCloud | Dr.fone</u></a></li>
<li><a href="https://phone-solutions.techidaily.com/in-2024-life360-learn-how-everything-works-on-motorola-moto-g14-drfone-by-drfone-virtual-android/"><u>In 2024, Life360 Learn How Everything Works On Motorola Moto G14 | Dr.fone</u></a></li>
<li><a href="https://win-cloud.techidaily.com/top-free-avi-to-mp4-converters-of-2014-compare-and-choose-the-ideal-one/"><u>Top Free AVI to MP4 Converters of 2014: Compare and Choose the Ideal One!</u></a></li>
<li><a href="https://os-tips.techidaily.com/understanding-timely-notifications-in-ios-a-comprehensive-guide/"><u>Understanding Timely Notifications in iOS: A Comprehensive Guide</u></a></li>
</ul></div>

