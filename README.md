# Appx-Backup
PowerShell script to backup an installed Windows Store App to an installable Appx file.

This small script allows you to make a backup of installed Windows Store apps. This particularly important if you have installed apps that are no longer available from the Winddows Store. If you ever need to use the Recovery options to Reset your Windows 10 installation, you will not be able to reinstall those apps that are no longer available in the Windows store. An example of this is Disney's Wreck It Ralph. I had to reset one of my PC's and found that even though the game showed up in my Windows Store account, if I tried to install it, Windows Store tells me that it is no longer available.

Fortunately I had another computer with the game installed and and I found this way to make an installable Appx file and be able to reinstall it manually into the computer that I Reset to Factory.

You will need to have some tools installed for this to work. These tools are part of Microsoft Visual Studio Community 2015. It's kind of a big download just to be able to do this. If anyone knows of a smaller download, please let me know and I'll update these instructions. Visual Studio installed these tools by default in the 'C:\Program Files (x86)\Windows Kits\10\bin\x86' folder.

Example Usage (open a Powershell prompt):
```powershell
PS C:\temp> .\Appx-Backup.ps1 -WSAppPath "C:\Program Files\WindowsApps\Disney.Wreck-itRalph_1.0.0.12_x86__6rarf9sa4v8jt" -WSAppOutputPath "C:\Temp" -WSTools "C:\Program Files (x86)\Windows Kits\10\bin\x86"
Reading "C:\Program Files\WindowsApps\Disney.Wreck-itRalph_1.0.0.12_x86__6rarf9sa4v8jt\AppxManifest.xml"
  App Name : Disney.Wreck-itRalph
  Publisher: CN=58DECE39-D5D0-4293-AAA0-9AF8484F12E2
Creating "C:\Temp\Disney.Wreck-itRalph_1.0.0.12_x86__6rarf9sa4v8jt.appx".
  Done.
Creating self-signed certificates.
  Click NONE in the 'Create Private Key Passsword' pop-up.
  Done.
Converting certificate to pfx.
  Done.
Signing the package.
  Done.
Success!
  App Package: "C:\Temp\Disney.Wreck-itRalph_1.0.0.12_x86__6rarf9sa4v8jt.appx"
  Certificate: "C:\Temp\Disney.Wreck-itRalph_1.0.0.12_x86__6rarf9sa4v8jt.cer"
Install the '.cer' file to [Local Computer\Trusted Root Certification Authorities] before you install the App Package.
PS C:\temp>
```
NOTE: Although this can save the installable program, it cannot save anyuser information such as saved games or other user data. And if the app requires network support from the publisher that is also discontinued, then it's not likely that the reinstalled app will work. YMMV.

Also, this does not sign the app with the publisher's actual key or counter-sign it with the Windows Store key. It signs it with a custom self-signed key. To reinstall your app, you will need first install the *.cer file into your Trusted Root Certificates Store. Just double clicn on the *.cer file and manually add it to the correct store. This requires administrator privileges. Once that is done, you can install the app with the PowerShell command Add-AppxPackage.

# My Notes:
Install Visual Studio 2022 Community:<br />
https://visualstudio.microsoft.com/downloads/<br />
Direct Link:<br />
https://visualstudio.microsoft.com/thank-you-downloading-visual-studio/?sku=Community&channel=Release&version=VS2022&source=VSLandingPage&cid=2030&passive=false

Run the PowerShell script by right clicking Appx-Backup.ps1 and clicking Run with PowerShell

WSAppPath (App folder for your app):<br />
C:\Program Files\WindowsApps\2949193320E78.BestPlayer8.1_7.7.8.0_x64__pg6a145mvhp7p

WSAppOutputPath (Backup location):<br />
C:\Users\austn\Desktop\App Backup

WSTools (Visual Studio 2022 Community Tools folder):<br />
C:\Program Files (x86)\Windows Kits\10\bin\10.0.19041.0\x64

You must install the .cer file before installing the backup, or the Install button will be grayed out and it says:<br />
This app package’s publisher certificate could not be verified. Contact your system administrator or the app developer to obtain a new app package with verified certificates. The root certificate and all immediate certificates of the signature in the app package must be verified (0x800B010A)

Open the .cer and click Install Certificate...<br />
Local Machine<br />
Place all certificates in the following store<br />
Trusted Root Certification Authorities

Open the .aapx to install it
