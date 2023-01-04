# vmware on asrock4x4 
 we are using the asrock 4800 with ryzen 7 and realtek nics \
 need to modify esxi installation by adding drivers for realtek nics \
 esxi7+ no longer support vmkernel so the realtek drivers can only be added to versions prior to esxi7 \
 we are using esxi6.5 on the asrock 4800 
 
 #get esxi license 
 https://my.vmug.com/s/login/?ec=302&startURL=%2Fs%2F 
 
 #use ocp assisted installer
 http://console.redhat.com
 
 
 #add realtek nic drivers to esxi6.7.0 software depot 
 
 change directory to location of esxi670 software depot (.zip) and net55-r8169 vib
 
 on linux run pwsh to enter the powershell emulator  (import the powercli into powershell)
 
 Add-EsxSoftwareDepot /home/aleberkn/github/vmwareonasrock4x4/ESXi670-201912001.zip
 
 Get-Module -ListAVailable VM* | Import-Module
 
 $env:PSModulePath.Split(';')
 
 Get-EsxImageProfile
 
 New-EsxImageProfile -CloneProfile "ESXi-6.7.0-20191204001-standard" -Name "ESXi-6.7.0-standard-RTL8111" -Vendor "knightlab"
 
 Set-EsxImageProfile -ImageProfile ESXi-6.7.0-standard-RTL8111 -AcceptanceLevel CommunitySupported
 
 Add-EsxSoftwareDepot /home/aleberkn/github/vmwareonasrock4x4/net55-r8168-8.045a-napi-offline_bundle.zip
 
 Get-EsxSoftwarePackage | Where {$_.Vendor -eq "Realtek"}
 
 Add-EsxSoftwarePackage -ImageProfile ESXi-6.7.0-standard-RTL8111 -SoftwarePackage net55-r8168
 
 Export-EsxImageProfile -ImageProfile ESXi-6.7.0-standard-RTL8111 -ExportToIso -filepath /home/aleberkn/github/vmwareonasrock4x4/VMware-ESXi-6.70-RTL8111.iso
