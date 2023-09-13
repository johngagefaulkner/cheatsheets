## Microsoft / Intune / Device Filters

Below, you'll find some pre-written (and tested) device filters that you can either use in your own environment or as inspiration for your own!

---

### Filters


All Devices – Microsoft:
(device.manufacturer -eq "Microsoft")

All Devices – Dell:
(device.manufacturer -eq "Dell")

All Devices – Lenovo:
(device.manufacturer -eq "Lenovo")

All Devices – Rooted:
(device.isRooted -eq "True")

All Devices – Autopilot profile:
(device.enrollmentProfileName -startsWith "Autopilot Profile Name")

All Devices – Hybrid & Azure AD Joined:
(device.deviceTrustType -in ["Hybrid Azure AD joined","Azure AD joined"])

All Devices – Enterprise Edition:
(device.operatingSystemSKU -eq "Enterprise")

All Devices – Pro:
(device.operatingSystemSKU -eq "Professional")

All Devices – (Except Surface Hub) Windows 10 22H2:
(device.osVersion -startsWith "10.0.19045") and (device.model -notContains "Surface Hub")

All Devices – Azure AD joined:
(device.deviceTrustType -eq "Azure AD joined")

All Devices – Hybrid Azure AD joined or Azure AD joined:
(device.deviceTrustType -eq "Hybrid Azure AD joined") or (device.deviceTrustType -eq "Azure AD registered")

All Devices – Hybrid Azure AD joined – Windows 11:
((device.deviceTrustType -eq "Hybrid Azure AD joined") or (device.deviceTrustType -eq "Azure AD registered")) and (device.osVersion -startsWith "10.0.2")

All Devices – Windows 10:
(device.osVersion -startsWith "10.0.1")

All Devices – Windows 11:
(device.osVersion -startsWith "10.0.2")

Enrolled Devices – iOS profile:
(device.enrollmentProfileName -eq "iOS – Singapore")

Personal Devices macOS:
(device.deviceOwnership -eq "Personal")

Physical Devices – All Windows OS:
(device.osVersion -startsWith "10.0.") and (device.model -notContains "Virtual") and (device.model -notContains "Cloud PC")

Physical Devices – Windows 11:
(device.osVersion -startsWith "10.0.2") and (device.model -notContains "Virtual") and (device.model -notContains "Cloud PC")

Virtual Devices – All Windows OS:
(device.osVersion -startsWith "10.0.") and (device.model -contains "Virtual")

Virtual Devices – All Windows OS (Azure AD only):
(device.osVersion -startsWith "10.0.") and (device.model -eq "Virtual Machine")

Virtual Devices – Windows 10:
(device.osVersion -startsWith "10.0.1") and (device.model -contains "Virtual")

Virtual Devices – Windows 11:
(device.osVersion -startsWith "10.0.2") and (device.model -contains "Virtual")

Windows 365 Devices – All Windows OS:
(device.model -startsWith "Cloud PC")

Windows 365 Devices – Windows 10:
(device.osVersion -startsWith "10.0.1") and (device.model -startsWith "Cloud PC")

Windows 365 Devices – Windows 11:
(device.osVersion -startsWith "10.0.2") and (device.model -startsWith "Cloud PC")

Windows Out of Box (OOB) Devices:
(device.deviceName -startsWith "OOB")
