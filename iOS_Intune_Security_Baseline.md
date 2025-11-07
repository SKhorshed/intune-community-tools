# iOS Intune Security Baseline

This document outlines recommended security baseline for iOS/iPadOS devices managed with Microsoft Intune. Adjust these recommendations based on your organization’s security requirements and regulatory obligations.

## Compliance Policy

- **Block jailbroken devices**: configure the compliance policy to mark devices as non compliant if they are jailbroken or rooted ([Device Compliance settings for iOS/iPadOS in Intune](https://learn.microsoft.com/en-us/intune/intune-service/protect/compliance-policy-create-ios#:~:text=Device%20Health)).
- **Defender risk level**: integrate with Microsoft Defender for Endpoint and require the device’s threat level to be below “Medium” for compliance ([Device Compliance settings for iOS/iPadOS in Intune](https://learn.microsoft.com/en-us/intune/intune-service/protect/compliance-policy-create-ios#:~:text=Device%20Health)).
- **Passcode requirements**: enforce a passcode with complexity (minimum 6‑digit numeric or alphanumeric), require Face ID/Touch ID, and set auto‑lock to a short duration (e.g., 5 minutes).
- **Minimum OS version**: set a minimum iOS/iPadOS version and require that devices are up to date with the latest security updates.
- **Encryption**: ensure storage encryption remains enabled (iOS devices use hardware‑backed encryption by default).
- **Email and device enrollment**: require the device to be associated with a managed Azure AD account and enrolled in Intune.

## App Protection Policy (AppPP)

App protection policies safeguard corporate data at the app level. Microsoft’s documentation groups settings into three categories: **data relocation**, **access requirements**, and **conditional launch** ([iOS/iPadOS App Protection Policy Settings - Microsoft Intune](https://learn.microsoft.com/en-us/intune/intune-service/apps/app-protection-policy-settings-ios#:~:text=iOS%2FiPadOS%20App%20Protection%20Policy%20Settings,In%20this%20article)).

- **Data relocation**: block backup of app data to iCloud; restrict cut/copy/paste between managed and unmanaged apps; enforce “Open‑in” restrictions so data only flows between managed apps; block printing and sending attachments to personal accounts.
- **Access requirements**: require a PIN or biometric to open managed apps; specify minimum PIN length and disable simple PINs; require re‑authentication after a period of inactivity; allow Touch ID/Face ID as biometric methods; require the device to be compliant.
- **Conditional launch**: define behavior when conditions aren’t met (e.g., block or wipe corporate data if a device is jailbroken) ([iOS/iPadOS App Protection Policy Settings - Microsoft Intune](https://learn.microsoft.com/en-us/intune/intune-service/apps/app-protection-policy-settings-ios#:~:text=iOS%2FiPadOS%20App%20Protection%20Policy%20Settings,In%20this%20article)); enforce minimum OS version and minimum app version; set maximum offline grace periods; warn or block access if requirements aren’t satisfied.
- Assign app protection policies to user groups and target them to approved apps using the Intune admin center.

## Conditional Access

- Require devices to be marked as compliant by Intune before they can access corporate resources (Exchange Online, SharePoint, Teams, etc.).
- Enforce multi‑factor authentication (MFA) for user sign‑in.
- Require the use of approved apps (with app protection policies) to access cloud resources.
- Incorporate risk‑based signals (e.g., sign‑in risk from Azure AD Identity Protection) to block or challenge access.

## Microsoft Defender for Endpoint

- Enroll iOS devices in Microsoft Defender for Endpoint.
- Use Defender’s risk level to gate access: block or restrict devices that report a high or medium threat level ([Device Compliance settings for iOS/iPadOS in Intune](https://learn.microsoft.com/en-us/intune/intune-service/protect/compliance-policy-create-ios#:~:text=Device%20Health)).
- Enable web protection and network threat detection to block phishing, malicious websites, and MITM attacks.
- Configure notifications to alert users and administrators when malware or jailbreak is detected.

## Configuration Profiles

- **Device restrictions**: disable AirDrop, screen capture, screen recording, unmanaged profile installation, and other features that could lead to data exfiltration; restrict Safari autofill and password sharing.
- **Passcode policies**: enforce complex passcodes, requiring a combination of characters, and set expiration and history requirements; require biometric authentication.
- **Software updates**: configure automatic OS updates and enforce installation deadlines to keep devices patched.
- **App configuration**: pre‑configure corporate accounts (email, calendar, contacts) and prevent users from modifying these settings.
- **Web content filter**: deploy a web filter to block adult or malicious content and restrict untrusted domains.

## VPN

- Set up per‑app VPN or device VPN profiles using secure protocols (IKEv2 or SSL/TLS).
- Use certificate‑based authentication (via SCEP or PKCS) rather than shared secrets.
- Limit VPN traffic to corporate apps and exclude personal traffic; enable “On‑Demand” or “Always On” VPN for corporate‑owned devices.
- Leverage conditional access to block VPN connections from non‑compliant devices.

## Wi‑Fi Profiles

- Deploy Wi‑Fi profiles so devices automatically join trusted corporate networks.
- Use WPA2/WPA3 Enterprise security with EAP‑TLS authentication.
- Authenticate using certificates provisioned via SCEP/PKCS instead of pre‑shared keys.
- Disable auto‑join to unknown networks to reduce risk from rogue access points.

## Certificates & SCEP

- Configure Simple Certificate Enrollment Protocol (SCEP) or PKCS profiles to provision user and device certificates for Wi‑Fi, VPN, and email authentication.
- Use separate certificate templates for user and device identities; define certificate validity and renewal periods.
- Secure the Network Device Enrollment Service (NDES) server and restrict its use to Intune‑initiated requests.
- Implement certificate revocation procedures for lost or decommissioned devices.

## Exporting & Implementing This Baseline

1. Review these recommendations and adjust them to fit your organization’s requirements.
2. In the Intune admin center, create or edit compliance policies, app protection policies, conditional access policies, and configuration profiles according to this baseline.
3. Assign policies to appropriate device and user groups.
4. Use Intune’s reporting and export features to document compliance status and verify that devices adhere to the baseline.
