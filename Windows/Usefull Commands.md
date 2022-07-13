#### Quick update Windows:
First, run this command:
```powershell-session
PS C:\htb> Set-ExecutionPolicy Unrestricted -Scope Process
```

Now that we have our ExecutionPolicy set, let us install the PSWindowsUpdate module and apply our updates
```powershell-session
PS C:\htb> Install-Module PSWindowsUpdate 

Untrusted repository 
You are installing the modules from an untrusted repository. If you trust this repository, 
change its InstallationPolicy value by running the Set-PSRepository cmdlet. 
Are you sure you want to install the modules from 'PSGallery'?
[Y] Yes [A] Yes to All [N] No [L] No to All [S] Suspend [?] Help (default is "N"): A
```

Once the module installation completes, we can import it and run our updates.

```powershell-session
PS C:\htb> Import-Module PSWindowsUpdate 

PS C:\htb> Install-WindowsUpdate -AcceptAll

X ComputerName Result KB Size Title
- ------------ ------ -- ---- -----
1 DESKTOP-3... Accepted KB2267602 510MB Security Intelligence Update for Microsoft Defender Antivirus - KB2267602...
1 DESKTOP-3... Accepted 17MB VMware, Inc. - Display - 8.17.2.14
2 DESKTOP-3... Downloaded KB2267602 510MB Security Intelligence Update for Microsoft Defender Antivirus - KB2267602...
2 DESKTOP-3... Downloaded 17MB VMware, Inc. - Display - 8.17.2.14 3 DESKTOP-3... Installed KB2267602 510MB Security Intelligence Update for Microsoft Defender Antivirus - KB2267602... 3 DESKTOP-3... Installed 17MB VMware, Inc. - Display - 8.17.2.14 

PS C:\htb> Restart-Computer -Force
```

The above Powershell example will import the `PSWindowsUpdate` module, run the update installer, and then reboot the PC to apply changes. Be sure to run updates regularly, especially if we plan to use this host frequently and not destroy it at the end of each engagement. Now that we have our updates installed let us get our package manager and other essential core tools.

#### Quick install tools with choco:

```powershell
# Choco build script

write-host "*** Initial app install for core tools and packages. ***"

write-host "*** Configuring chocolatey ***"
choco feature enable -n allowGlobalConfirmation

write-host "*** Beginning install, go grab a coffee. ***"
choco upgrade  wsl2 python git vscode openssh openvpn netcat nmap wireshark burp-suite-free-edition heidisql sysinternals putty golang neo4j-community openjdk

write-host "*** Build complete, restoring GlobalConfirmation policy. ***"
choco feature disable -n allowGlobalCOnfirmation
```

#### Windows Defender Exemptions for the Tools' Folders.

-   `C:\Users\your user here\AppData\Local\Temp\chocolatey\`
    
-   `C:\Users\your user here\Documents\git-repos\`
    
-   `C:\Users\your user here\Documents\scripts\`
    

These three folders are just a start. As we add more tools and scripts, we may need to add more exclusions. To exclude these files, we will we a PowerShell command.

#### Adding exclusions:

```powershell-session
PS C:\htb> Add-MpPreference -ExclusionPath "C:\Users\your user here\AppData\Local\Temp\chocolatey\"
```

