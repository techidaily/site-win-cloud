---
title: 1. Remove Unused Folders Automatically with PowerShell Scripts
date: 2024-09-24T03:39:40.628Z
updated: 2024-09-29T22:33:33.548Z
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
<a href="https://aligracehair.sjv.io/c/5597632/2080312/19272" target="_top" id="2080312">
  <img src="//a.impactradius-go.com/display-ad/19272-2080312" border="0" alt="https://techidaily.com" width="300" height="90"/>
</a>
<img height="0" width="0" src="https://aligracehair.sjv.io/i/5597632/2080312/19272" style="position:absolute;visibility:hidden;" border="0" />
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

<!-- affiliate ads begin -->
<a href="https://ephamedtechinc.pxf.io/c/5597632/2137219/26400" target="_top" id="2137219">
  <img src="//a.impactradius-go.com/display-ad/26400-2137219" border="0" alt="https://techidaily.com" width="728" height="90"/>
</a>
<img height="0" width="0" src="https://ephamedtechinc.pxf.io/i/5597632/2137219/26400" style="position:absolute;visibility:hidden;" border="0" />
<!-- affiliate ads end -->

### Delete with PowerShell

For PowerShell, we can use the following script:

$folderPath = "C:\Path\to\folder"
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

### Delete empty directories with Advanced Installer

Advanced Installer offers a much simpler method to remove folders generated by the application. If you navigate to the Files and Folders page and right-click on a desired folder, you will see the Uninstall Cleanup option:

![ai files folders uninstall cleanup](https://cdn.advancedinstaller.com/img/delete-empty-directory-with-custom-actions/ai-files-folders-uninstall-cleanup.png "ai files folders uninstall cleanup")  

The uninstall cleanup wizard launches, allowing you to specify which files and folders created by your application will be removed when you uninstall it. To launch this wizard, select the parent folder, the directory from which you want to delete files and folders, and then choose "Uninstall Cleanup..." from the context menu. Following the wizard's launch, simply follow the on-screen instructions to select the files you want to delete.

![ai uninstall cleanup step1](https://cdn.advancedinstaller.com/img/delete-empty-directory-with-custom-actions/ai-uninstall-cleanup-step1.png "ai uninstall cleanup step1")  

<!-- affiliate ads begin -->
<a href="https://appsumo.8odi.net/c/5597632/2151890/7443" target="_top" id="2151890">
  <img src="//a.impactradius-go.com/display-ad/7443-2151890" border="0" alt="https://techidaily.com" width="728" height="90"/>
</a>
<img height="0" width="0" src="https://appsumo.8odi.net/i/5597632/2151890/7443" style="position:absolute;visibility:hidden;" border="0" />
<!-- affiliate ads end -->

As you can see, you can delete the desired folder and all of its subfolders, or just the contents of the folder, including non-empty directories.

![ai uninstall cleanup step2](https://cdn.advancedinstaller.com/img/delete-empty-directory-with-custom-actions/ai-uninstall-cleanup-step2.png "ai uninstall cleanup step2")  

<!-- affiliate ads begin -->
<a href="https://jalbum-affiliate-program.sjv.io/c/5597632/1584040/17916" target="_top" id="1584040">
  <img src="//a.impactradius-go.com/display-ad/17916-1584040" border="0" alt="https://techidaily.com" width="728" height="90"/>
</a>
<img height="0" width="0" src="https://jalbum-affiliate-program.sjv.io/i/5597632/1584040/17916" style="position:absolute;visibility:hidden;" border="0" />
<!-- affiliate ads end -->

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
<li><a href="https://win-cloud.techidaily.com/tunein-radio/"><u>「TuneIn Radio」から音声コンテンツの録音＆保存ガイド：手軽な方法</u></a></li>
<li><a href="https://extra-guidance.techidaily.com/2024-approved-nft-mastermakers-essential-tools-for-digital-artists/"><u>2024 Approved NFT Mastermakers Essential Tools for Digital Artists</u></a></li>
<li><a href="https://sound-issues.techidaily.com/expert-advice-fixing-your-broken-logitech-g430-microphone-in-simple-steps/"><u>Expert Advice: Fixing Your Broken Logitech G430 Microphone in Simple Steps</u></a></li>
<li><a href="https://techidaily.com/how-to-get-out-of-dfu-mode-on-apple-iphone-14-plus-drfone-by-drfone-ios-system-repair-ios-system-repair/"><u>How To Get Out of DFU Mode on Apple iPhone 14 Plus? | Dr.fone</u></a></li>
<li><a href="https://win-cloud.techidaily.com/is-it-worth-the-expense-unveiling-the-capabilities-and-value-of-the-keychron-q5-on-zdnet/"><u>Is It Worth the Expense? Unveiling the Capabilities and Value of the Keychron Q5 on ZDNET</u></a></li>
<li><a href="https://tech-renaissance.techidaily.com/mastering-digital-twin-integration-strategies-for-conquering-7-common-business-challenges/"><u>Mastering Digital Twin Integration: Strategies for Conquering 7 Common Business Challenges</u></a></li>
<li><a href="https://win-cloud.techidaily.com/new-tech-on-the-block-microsofts-windows-11-se-and-cost-effective-surface-laptop-se-for-schools-spotlighted-by-zdnet/"><u>New Tech on the Block: Microsoft's Windows 11 SE and Cost-Effective Surface Laptop SE for Schools, Spotlighted by ZDNet</u></a></li>
<li><a href="https://win-cloud.techidaily.com/the-case-against-expensive-antivirus-programs-discover-how-to-protect-your-devices-without-the-cost/"><u>The Case Against Expensive Antivirus Programs: Discover How to Protect Your Devices Without the Cost</u></a></li>
<li><a href="https://win-cloud.techidaily.com/top-rated-mkv-to-dvd-conversion-software-for-pc/"><u>Top Rated MKV to DVD Conversion Software for PC</u></a></li>
<li><a href="https://win-cloud.techidaily.com/transforming-mp3s-into-professional-m4b-audiobooks-with-ease/"><u>Transforming MP3s Into Professional M4B Audiobooks with Ease</u></a></li>
<li><a href="https://win-blog.techidaily.com/ultimate-guide-to-overcome-recurring-outlook-program-hiccups/"><u>Ultimate Guide to Overcome Recurring Outlook Program Hiccups</u></a></li>
<li><a href="https://buynow-marvelous.techidaily.com/unveiling-the-potential-of-eero-pro-a-detailed-look-at-a-full-house-wifi-solution/"><u>Unveiling the Potential of Eero Pro - A Detailed Look at a Full-House WiFi Solution</u></a></li>
<li><a href="https://techno-recovery.techidaily.com/what-to-do-when-your-apple-watch-touchscreen-wont-respond/"><u>What to Do When Your Apple Watch Touchscreen Won't Respond?</u></a></li>
</ul></div>

