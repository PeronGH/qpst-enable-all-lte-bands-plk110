# Enable all LTE/5G bands on Chinese OnePlus 15 (PLK110)

Chinese OnePlus variants ship with modem policy files that restrict which network bands are active. This repo contains the original and modified policyman files to unlock all hardware-supported bands.

## What it does

- Sets all RF band lists (`gw`, `lte`, `nr5g_sa`, `nr5g_nsa`, `nr5g_nrdc`) to `base="hardware"` instead of `base="current"`/`base="none"`
- Removes `generic_band_restrictions.xml` from the policy chain, which applies location-based band exclusions (e.g. disabling LTE B39/B41 in the US, restricting bands in Indonesia/Singapore/Japan/Taiwan)

## Requirements

- [.NET 10.0](https://dotnet.microsoft.com/download/dotnet/10.0) or later
- [ADB](https://developer.android.com/tools/adb) with root access (`su`)
- [EfsTools](https://github.com/PeronGH/EfsTools) (cross-platform, no Windows/QPST needed)

## Steps

### 1. Enable DIAG mode

If rooted (no reboot required):

```sh
adb shell 'su 0 setprop sys.usb.config diag,diag_mdm,qdss,qdss_mdm,serial_cdev,dpl,rmnet,adb'
```

If not rooted, reboot to FTM mode first:

```sh
adb reboot ftm
adb shell setprop sys.usb.config diag,diag_mdm,qdss,qdss_mdm,serial_cdev,dpl,rmnet,adb
```

### 2. Backup your current files

```sh
EfsTools readFile -i /policyman/global_defines.xml -o ./backup_global_defines.xml
EfsTools readFile -i /policyman/policies.xml -o ./backup_policies.xml
```

### 3. Write the modded files

```sh
EfsTools writeFile -i policyman/modded/global_defines.xml -o /policyman/global_defines.xml
EfsTools writeFile -i policyman/modded/policies.xml -o /policyman/policies.xml
```

### 4. Reboot

```sh
adb reboot
```

## Reverting

Write back your backup files:

```sh
EfsTools writeFile -i ./backup_global_defines.xml -o /policyman/global_defines.xml
EfsTools writeFile -i ./backup_policies.xml -o /policyman/policies.xml
adb reboot
```

## Repo structure

```
policyman/
  backup/   - original files from a stock Chinese PLK110
  modded/   - modified files with all bands unlocked
```

## Credits

- Band unlock method originally documented by [@dekefake](https://github.com/dekefake/qpst-enable-all-lte-bands-plk110) and [@rmendez011](https://xdaforums.com/t/updated-6-30-25-how-to-enable-n77-5g-oxygen-os-11-14-0-0-1901-5g-uw-icon-oxygen-os-13-14-only-9-series-le2115-le2125-verizon-network.4429489/)
- [EfsTools](https://github.com/PeronGH/EfsTools) — cross-platform Qualcomm EFS tool over USB (libusb)
