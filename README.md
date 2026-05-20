# win-safe-cleanup
Clean your Windows PC by safely removing all temporary files to free up storage space.

# ⚙️ Windows 11 Safe Full Automation Cleanup Script

A safe and industry-standard PowerShell script to clean unnecessary files and free up disk space on Windows 11 without breaking system stability.

---

## 🚀 What This Script Does

This automation performs **safe system cleanup only**, including:

- 🧹 Temporary files (User + Windows Temp)
- 🗑️ Recycle Bin cleanup
- 🧰 Windows Update cache reset (safe)
- ⚡ Prefetch cleanup (safe, auto-rebuilds)
- 🌐 DNS cache flush
- 💾 Optional: Disable hibernation (frees disk space)

---

## ⚠️ Important

- Run **PowerShell as Administrator**
- Safe for daily use
- Does NOT delete personal files
- Windows will automatically rebuild required caches

---

## 💻 How to Use

1. Open **Start Menu**
2. Search **PowerShell**
3. Right-click → **Run as Administrator**
4. Paste the script below
5. Press **Enter**

---

## 🧾 Full Script

```powershell
# ================================
# SAFE WINDOWS 11 CLEANUP SCRIPT
# ================================

Write-Host "Starting system cleanup..." -ForegroundColor Green

# 1. Clear TEMP folders
Write-Host "Cleaning TEMP files..."
Remove-Item -Path "$env:TEMP\*" -Recurse -Force -ErrorAction SilentlyContinue
Remove-Item -Path "C:\Windows\Temp\*" -Recurse -Force -ErrorAction SilentlyContinue

# 2. Clear Recycle Bin
Write-Host "Emptying Recycle Bin..."
Clear-RecycleBin -Force -ErrorAction SilentlyContinue

# 3. Clear Windows Update cache
Write-Host "Cleaning Windows Update cache..."
Stop-Service wuauserv -Force -ErrorAction SilentlyContinue
Stop-Service bits -Force -ErrorAction SilentlyContinue

Remove-Item "C:\Windows\SoftwareDistribution\Download\*" -Recurse -Force -ErrorAction SilentlyContinue

Start-Service wuauserv -ErrorAction SilentlyContinue
Start-Service bits -ErrorAction SilentlyContinue

# 4. Clear DNS cache
Write-Host "Flushing DNS..."
ipconfig /flushdns | Out-Null

# 5. Clear Prefetch (safe, rebuilds automatically)
Write-Host "Cleaning Prefetch..."
Remove-Item "C:\Windows\Prefetch\*" -Force -ErrorAction SilentlyContinue

# 6. OPTIONAL: Disable Hibernation (frees GBs)
Write-Host "Disabling Hibernation (optional space gain)..."
powercfg -h off

Write-Host "Cleanup completed successfully!" -ForegroundColor Cyan
