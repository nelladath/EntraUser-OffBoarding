# Entra ID User Offboarding Automation

A comprehensive PowerShell script for automated user offboarding from Microsoft Entra ID (Azure AD) environments.

## 📋 Overview

This PowerShell automation provides a complete, secure, and auditable solution for offboarding users from your organization's Microsoft 365/Entra ID environment. The script ensures departing users lose all organizational access while maintaining proper compliance documentation.

## ✨ Features

### Automated Offboarding Process
- **Account Disabling**: Immediately blocks user sign-in
- **Session Revocation**: Terminates all active user sessions across devices
- **Group Removal**: Removes user from all security and Microsoft 365 groups
- **Application Access Removal**: Revokes all enterprise application assignments
- **License Deprovisioning**: Reclaims all assigned Microsoft 365/Azure licenses

### Post-Offboarding Verification
- **Access Inventory**: Comprehensive checklist of remaining permissions
- **Directory Roles**: Lists any assigned Entra ID administrative roles
- **Group Memberships**: Verifies complete group removal
- **Enterprise Applications**: Confirms application access revocation
- **App Registrations**: Shows applications owned by the user

## 🔧 Prerequisites

- **PowerShell 5.1** or later
- **Microsoft.Graph PowerShell Module**
- **Administrative Privileges** in Entra ID with required permissions
- **Execution Policy** set to allow script execution

### Required Permissions
The executing account must have the following Microsoft Graph permissions:
- `User.ReadWrite.All`
- `Group.ReadWrite.All`
- `Directory.ReadWrite.All`
- `AppRoleAssignment.ReadWrite.All`

## 🚀 Installation

1. **Install Microsoft Graph PowerShell Module** (if not already installed):
   ```powershell
   Install-Module Microsoft.Graph -Scope CurrentUser
   ```

2. **Download the Script**:
   ```bash
   git clone <repository-url>
   cd UserOffboard
   ```

3. **Set Execution Policy** (if needed):
   ```powershell
   Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
   ```

## 📖 Usage

### Basic Usage
```powershell
.\User-OffBoarding.ps1 -userUPN "john.doe@company.com"
```

### Interactive Mode
If you run the script without parameters, it will prompt for the user UPN:
```powershell
.\User-OffBoarding.ps1
```

## 🔍 What the Script Does

### Phase 1: Authentication & Validation
1. Connects to Microsoft Graph with required scopes
2. Validates the target user exists
3. Displays user information for confirmation

### Phase 2: Access Revocation
1. **Disables Account**: Sets `accountEnabled = false`
2. **Revokes Sessions**: Calls `/revokeSignInSessions` endpoint
3. **Removes Groups**: Iterates through all group memberships and removes user
4. **Revokes Apps**: Removes all enterprise application role assignments
5. **Reclaims Licenses**: Removes all assigned Microsoft 365/Azure licenses

### Phase 3: Verification & Audit
1. **Access Inventory**: Generates comprehensive report showing:
   - Directory role assignments
   - Remaining group memberships (should be none)
   - Enterprise application access (should be none)
   - Owned app registrations

## 📊 Example Output

```
Found user: John Doe
Offboarding user: John Doe
User sign-in blocked
Sessions revoked
Removed from group: Sales Team
Removed from group: Office 365 Users
Removed from all groups
  Removed app assignment: SharePoint
  Removed app assignment: Microsoft Teams
Application access removed
Licenses removed
User has NO remaining access

===============================
 USER ACCESS CHECKLIST
===============================
User        : John Doe
UPN         : john.doe@company.com
Account     : Disabled
--------------------------------

[1] DIRECTORY ROLES
 - None

[2] GROUP MEMBERSHIPS
 - None

[3] ENTERPRISE APPLICATION ACCESS
 - None

[4] APP REGISTRATIONS OWNED
 - MyCustomApp

===============================
 CHECKLIST COMPLETE
===============================
```

## ⚠️ Important Notes

### Security Considerations
- **Immediate Effect**: User access is revoked immediately upon execution
- **No Undo**: This process cannot be easily reversed
- **License Recovery**: Licenses are immediately available for reassignment
- **Audit Trail**: All actions are logged to the console for compliance

### Limitations
- **Directory Roles**: The script does NOT automatically remove Entra ID directory roles - these must be removed manually
- **App Registrations**: Applications owned by the user are NOT deleted - ownership may need to be transferred
- **Personal Data**: This script does not handle personal data removal (GDPR compliance may require additional steps)

## 🛡️ Best Practices

1. **Test First**: Always test in a non-production environment
2. **Backup**: Ensure you have proper backups of user data if needed
3. **Documentation**: Save the output for compliance and audit purposes
4. **Verification**: Review the access checklist to confirm complete offboarding
5. **Manual Cleanup**: Address any remaining directory roles or app registrations manually

## 📝 Error Handling

The script includes comprehensive error handling:
- **User Not Found**: Exits gracefully if user doesn't exist
- **Permission Errors**: Continues with warnings for individual failures
- **API Failures**: Logs errors without stopping the entire process

## 🤝 Contributing

Contributions are welcome! Please ensure any changes maintain the security and audit integrity of the offboarding process.

## 👨‍💻 Author

**Sujin Nelladath**  
LinkedIn: [https://www.linkedin.com/in/sujin-nelladath-8911968a/](https://www.linkedin.com/in/sujin-nelladath-8911968a/)

## 📄 License

This project is provided as-is for educational and operational purposes. Please ensure compliance with your organization's policies and applicable regulations when using this script.

## ⚡ Quick Start Checklist

- [ ] Install Microsoft Graph PowerShell module
- [ ] Ensure proper permissions in Entra ID
- [ ] Test in non-production environment
- [ ] Prepare user UPN for offboarding
- [ ] Run script and save output for records
- [ ] Verify complete access removal
- [ ] Handle any remaining directory roles manually
- [ ] Transfer ownership of any app registrations if needed

---

**⚠️ DISCLAIMER**: This script performs irreversible changes to user accounts. Always test thoroughly and ensure proper authorization before running in production environments.
