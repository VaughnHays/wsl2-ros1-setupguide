*This guide assumes you already have a working Ubuntu WSL2 set up.*

# Powershell Setup
Run each of the following commands in Powershell with elevated permissions. **REQUIRES WINDOWS 11 PRO**.  
If you do not have 11 Pro, look into Microsoft Activation Scripts (MAS).
```powershell
Get-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V-All

Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V -All

New-VMSwitch -Name "External Switch" -NetAdapterName "Wi-Fi" -AllowManagementOS $true

New-NetFirewallRule -DisplayName "WSL" -Direction Inbound -InterfaceAlias "vEthernet (WSL (Hyper-V firewall))" -Action Allow

New-NetFirewallRule -DisplayName "WSL" -Direction Outbound -InterfaceAlias "vEthernet (WSL (Hyper-V firewall))" -Action Allow

# IF THE ABOVE TWO FIREWALL COMMANDS FAIL
Get-NetAdapter

# LOOK FOR THE ADAPTER WITH THE NAME 'vEthernet (<Default/External/Something else> Switch)'

New-NetFirewallRule -DisplayName "WSL" -Direction Inbound -InterfaceAlias "THAT NAME FROM ABOVE" -Action Allow

New-NetFirewallRule -DisplayName "WSL" -Direction Outbound -InterfaceAlias "THAT NAME FROM ABOVE" -Action Allow
```

# WSL2 Config
Add the following configuration to your `$HOME/.wslconfig`:
```.wslconfig
[wsl2]
networkingMode=bridged
vmSwitch="External Switch"
guiapplications=true
```
You may also want to increase the size of the VM's memory and swapfile. (also in `.wslconfig`, under the same section)
```.wslconfig
swap=8589934592
memory=6442450944
```
