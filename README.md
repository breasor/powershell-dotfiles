# PowerShell Dotfiles

This repository contains my **PowerShell dotfiles**, including profiles, custom functions, aliases, and configurations for different terminal environments. The goal is to **keep my PowerShell setup consistent across all machines** and **automate syncing with GitHub**.

## 📌 Features

- **Auto-sync with GitHub** – Automatically pulls updates and pushes local changes.
- **Supports multiple environments** – Works with PowerShell 5, PowerShell 7, VS Code, Windows Terminal, and SSH.
- **Custom aliases and functions** – Streamlines daily workflows.
- **Consistent profile management** – Ensures the same configuration across devices.

## 📂 Directory Structure

```
~/.dotfiles/
├── PowerShell/
│   ├── Microsoft.PowerShell_profile.ps1   # General PowerShell Profile
│   ├── Microsoft.VSCode_profile.ps1       # VS Code-Specific Profile
│   ├── WindowsTerminal_profile.ps1        # Windows Terminal Profile
│   ├── SSH_profile.ps1                     # SSH-Specific Profile
│   ├── common.ps1                          # Shared Settings for All Terminals
│   ├── init.ps1                            # Bootstrap Script
├── .gitignore
├── README.md
```

## 🚀 Installation & Setup

### **1️⃣ Clone the Repository**

```powershell
git clone https://github.com/breasor/powershell-dotfiles.git $env:USERPROFILE\dotfiles
```

### **2️⃣ Symlink PowerShell Profile**

```powershell
New-Item -ItemType SymbolicLink -Path $PROFILE -Target "$env:USERPROFILE\dotfiles\PowerShell\Microsoft.PowerShell_profile.ps1" -Force
```

✅ Now your PowerShell session will always use the GitHub dotfile version!

### **3️⃣ Enable Auto-Sync**

This setup ensures that changes are **automatically pulled and pushed** whenever a new PowerShell session starts.

```powershell
# Auto-Update & Auto-Push PowerShell Dotfiles
$DotfilesRepo = "$env:USERPROFILE\dotfiles"
if (Test-Path $DotfilesRepo -and (Get-Command git -ErrorAction SilentlyContinue)) {
    Push-Location $DotfilesRepo
    $PullOutput = git pull
    if ($PullOutput -match "Already up to date.") {
        Write-Output "Dotfiles are up to date."
    } else {
        Write-Output "Updated dotfiles from GitHub."
    }
    # Auto-Commit & Push Local Changes
    $StatusOutput = git status --porcelain
    if ($StatusOutput) {
        git add .
        git commit -m "Auto-update: $(Get-Date -Format 'yyyy-MM-dd HH:mm:ss')"
        git push origin main
        Write-Output "Local changes have been pushed to GitHub."
    }
    Pop-Location
}
```

## ⚡ Usage

### **Edit Your PowerShell Profile**

```powershell
code $PROFILE  # Opens profile in VS Code
```

Make changes, save, and restart PowerShell!

### **Manually Push Changes**

If you make changes to your profile and want to push manually:

```powershell
cd $env:USERPROFILE\dotfiles
git add .
git commit -m "Updated PowerShell profile"
git push origin main
```

## 📖 Additional Customizations

- **Aliases & Functions** → `common.ps1`
- **VS Code Settings** → `Microsoft.VSCode_profile.ps1`
- **Windows Terminal Tweaks** → `WindowsTerminal_profile.ps1`

## 🛠️ Future Enhancements

- ✅ **OneDrive Backup Integration**
- ✅ **More Advanced Automation Scripts**
- ✅ **Cross-Platform PowerShell Support (Linux/macOS)**

## 📜 License

This repository is licensed under the **MIT License**. See the LICENSE file for details.
