# PSPrivateGallery

Based on the instruction found:
https://kurtroggen.wordpress.com/2016/06/06/powershell-private-gallery-your-internal-powershell-repository/

### Clone GitHub project locally (from https://github.com/PowerShell/PSPrivateGallery)
```
(New-Object System.Net.WebClient).DownloadFile('https://github.com/PowerShell/PSPrivateGallery/archive/master.zip', "${Env:Temp}\PSPrivateGallery-master.zip")
```

### Extract the contents of the zip file into the path “C:\PSPrivateGallery”.
```
Expand-Archive –Path "${Env:Temp}\PSPrivateGallery-master.zip" –DestinationPath “C:\” –Force # Requires PSv5
Rename-Item –Path “C:\PSPrivateGallery-master” –NewName “PSPrivateGallery”
```

### Unblock files (remove streams) using the Unblock-File cmdlet
```
Get-ChildItem –Path “C:\PSPrivateGallery” –Recurse | Unblock-File
```

### Copy PSGallery DSC Resources to the $env:PSModulePath – Copy Modules folder contents to $env:ProgramFiles\WindowsPowerShell\Modules
```
Copy-Item –Path “C:\PSPrivateGallery\Modules\*” –Destination “C:\Program Files\WindowsPowerShell\Modules” –Recurse
```

### Generate Credential files for both user and admin
### IMPORTANT: These passwords must meet password complexity requirements for machine/domain.
```
Get-Credential –Credential GalleryUser  | Export-Clixml 'C:\PSPrivateGallery\Configuration\GalleryUserCredFile.clixml'
Get-Credential –Credential GalleryAdmin | Export-Clixml 'C:\PSPrivateGallery\Configuration\GalleryAdminCredFile.clixml'
```

### Update Configuration Data to your needs (Optional):
```
# C:\PSPrivateGallery\Configuration\PSPrivateGalleryEnvironment.psd1
# C:\PSPrivateGallery\Configuration\PSPrivateGalleryPublishEnvironment.psd1
```

### Deploy the PowerShell Gallery, using PSPrivateGallery.ps1
```
Set-Location “C:\PSPrivateGallery\Configuration”
. .\PSPrivateGallery.ps1
```

### Open Private PowerShell Hallery
```
Invoke-Item 'http://localhost:8080'
```
