# Overview

This package is designed to reset the Bitlocker passphrase in the background using Tanium. You might want to do this in cases of an Insider Threat issue where you need to deny access to the end user of their device drive upon reboot, but keep that drive in a forensically-sound state for an investigation. Couple this with a UEFI/BIOS locked-by-default setup and disallow booting from anything but the internal drive without unlocking the UEFI/BIOS, and you'll have a setup that foils nearly any non-destructive unlock attempts your end users will throw at their company-issued laptops. 

# Considerations

I wrote this for my corp environment that only has a single type of BitLocker KeyProtector set up, which is "TPM And PIN" - no PIN or or TPM only or other weird configurations. When testing this thoroughly in your own environment, you should have something similar otherwise it will just not work. Also, our endpoint sysvol drives are always C:, so if that isn't the case for you, change that in the script. 

If you run **manage-bde -protectors -get c:** it will ensure you have the proper keyprotectors for this to work. If you run that and don't see "4" or get complaints about the drive, this script probably isn't gonna work.  

More info on KeyProtectors: https://learn.microsoft.com/en-us/windows/win32/secprov/getkeyprotectors-win32-encryptablevolume

More info on Manage-BDE: https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/manage-bde-protectors


# Usage

- Change **InsertSomeDeviceLockedPassphraseHere** in BitlockerPassReset.ps1 to something your org wants to keep fairly close-hold

- Create a Tanium package with the "package command.txt" as the command to run.

- Add BitlockerPassReset.ps1 as a package resource to the Tanium package 

- Deploy the package to your device of choice by using the Computer Name sensor or similar targeting mechanism

- Optionally send a reboot to the endpoint if you need this to take effect NOW (which seems to always be the case for our org when this gets deployed)

- Bitlocker password on reboot will be **InsertSomeDeviceLockedPassphraseHere** unless you wisely changed the password in the script before deploying


