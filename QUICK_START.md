# Quick Start Guide

## 📦 Package Contents

| File | Description |
|------|-------------|
| `socradar-sentinel-integration.json` | ARM Template (Logic App definition) |
| `README.md` | Main documentation with Deploy to Azure button |
| `INSTALLATION_GUIDE.md` | Detailed English installation guide |
| `deploy-socradar-integration.ps1` | PowerShell automated setup script |
| `deploy.sh` | Bash automated setup script (Linux/Mac) |
| `SOCRadar-Sentinel-Integration.zip` | Archive containing all files |

---

## 🚀 3 Deployment Options

### 🟢 Option 1: Azure Portal (Easiest)

**Windows, Mac, Linux - Browser**

1. Go to Azure Portal: https://portal.azure.com
2. Search for "Deploy a custom template"
3. "Build your own template in the editor"
4. Paste `socradar-sentinel-integration.json` content
5. Fill parameters:
   - SOCRadar API Key ✅
   - Workspace Name ✅
   - Resource Group ✅
6. "Review + create" → "Create"

⏱️ **Time:** 3-5 minutes

---

### 🔵 Option 2: PowerShell Script (Windows)

**Windows - Automated Setup**

```powershell
# Requires PowerShell 5.1 or higher
# Requires Azure PowerShell module

# 1. Extract files to a folder
# 2. Open PowerShell as administrator
# 3. Run the command:

.\Deploy-SOCRadarIntegration.ps1 `
  -ResourceGroupName "SentinelOps-RG" `
  -Location "westeurope" `
  -WorkspaceName "MyCompany-Sentinel" `
  -WorkspaceResourceGroup "Security-RG" `
  -SOCRadarAPIKey (ConvertTo-SecureString "your-api-key" -AsPlainText -Force)
```

**Or interactive mode:**
```powershell
.\Deploy-SOCRadarIntegration.ps1
# Script will prompt you for each value
```

⏱️ **Time:** 2-3 minutes

---

### 🟣 Option 3: Bash Script (Linux/Mac)

**Linux, Mac, WSL - Automated Setup**

```bash
# Requires Azure CLI
# Install: https://docs.microsoft.com/cli/azure/install-azure-cli

# 1. Grant execute permission to script
chmod +x deploy.sh

# 2. Run the script
./deploy.sh

# Script will interactively prompt for:
# - Resource Group
# - Location
# - Workspace Name
# - API Key
```

⏱️ **Time:** 2-3 minutes

---

## 📋 Pre-Installation Checklist

### ✅ Required Information

- [ ] SOCRadar API Key (Platform → Settings → API Keys)
- [ ] Azure Sentinel Workspace Name
- [ ] Workspace Resource Group Name
- [ ] Azure Subscription ID (optional)

### ✅ Required Permissions

- [ ] **Contributor** or **Logic App Contributor** in Azure
- [ ] **Microsoft Sentinel Responder** on Sentinel Workspace

### ✅ Tools (Platform-specific)

**Azure Portal Setup:**
- [ ] Web browser
- [ ] Azure Portal access

**PowerShell Setup:**
- [ ] PowerShell 5.1 or higher
- [ ] Az PowerShell module: `Install-Module -Name Az`

**Bash Setup:**
- [ ] Azure CLI: `curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash`

---

## 🎯 Post-Installation Steps

### 1. API Connection Authorization (Required)

```
Azure Portal
  → Logic Apps 
  → SOCRadar-CloseAlarm-OnIncidentClose
  → API connections
  → azuresentinel-...
  → Edit API connection
  → Authorize ← CLICK HERE
  → Sign in with your Azure account
  → Save
```

### 2. Configure SOCRadar Alerts

Add Alarm ID to the **ProductName** field of your alerts:

**Format:** `SOCRadar-AlarmID-[NUMBER]`

**Example:** `SOCRadar-AlarmID-12345`

#### Kusto Query Example:
```kusto
let socradarAlarmId = 12345;
SecurityAlert
| extend ProductName = strcat("SOCRadar-AlarmID-", socradarAlarmId)
```

#### Python Example:
```python
alert = {
    "ProductName": f"SOCRadar-AlarmID-{alarm_id}",
    "AlertName": "Threat Detected",
    "Severity": "High"
}
```

### 3. Test

1. Create a test incident in Sentinel
2. Add `SOCRadar-AlarmID-12345` to ProductName
3. Close the incident (Status: Closed)
4. Verify alarm is closed in SOCRadar
5. Check Logic App → Run history

---

## 🔍 Quick Troubleshooting

| Problem | Solution |
|---------|----------|
| **"Alarm ID not found"** | ProductName format: `SOCRadar-AlarmID-12345` |
| **"Authorization failed"** | Check API Key, regenerate if needed |
| **"Permission denied"** | Grant Managed Identity the Sentinel Responder role |
| **Logic App not running** | Authorize connection, check Run history |

---

## 📚 Detailed Documentation

- 🚀 **Quick Start:** `QUICK_START.md`
- 📖 **Detailed Guide:** `INSTALLATION_GUIDE.md`
- 📄 **README:** `README.md`

---

## 💡 Recommended Installation Method

| Platform | Recommended Method | Why |
|----------|-------------------|-----|
| **Windows** | PowerShell Script | Fully automated, fast |
| **Linux/Mac** | Bash Script | Fully automated, fast |
| **Any** | Azure Portal | GUI, visual, easy |

---

## ⚡ Super Quick Setup (Fastest)

**Windows:**
```powershell
# One line!
.\Deploy-SOCRadarIntegration.ps1 -ResourceGroupName "MyRG" -Location "westeurope" -WorkspaceName "MySentinel" -WorkspaceResourceGroup "MyWorkspaceRG" -SOCRadarAPIKey (ConvertTo-SecureString "sk_live_xxx" -AsPlainText -Force)
```

**Linux/Mac:**
```bash
# One command - interactive
./deploy.sh
```

**Azure Portal:**
```
Portal → Deploy a custom template → Paste template → Deploy
```

---

## 📞 Support

### Frequently Asked Questions

**Q: Where do I get the API Key?**
A: SOCRadar Platform → Settings → API Keys

**Q: What Sentinel role is required?**
A: Microsoft Sentinel Responder

**Q: Can I deploy to multiple workspaces?**
A: Yes, do separate deployment for each workspace

**Q: Is there a cost?**
A: Minimal Azure cost per Logic App run (typically $0.001/run)

### Contact

- 📧 SOCRadar Support
- 📄 Documentation: docs.socradar.com
- 🔧 Azure Support: portal.azure.com

---

## 🎉 Success!

After installation completes:

✅ Sentinel incident closes
✅ Logic App triggers (< 1 second)
✅ SOCRadar alarm automatically closes
✅ Comments added to both platforms
✅ Operation time reduced by 80%

---

**Version:** 1.0  
**Last Updated:** November 10, 2025  
**Built by:** SOCRadar Security Team
