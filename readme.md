# How to enable all LTE bands on the Oneplus 15 chinese variant

## Pros and cons of purchasing a Chinese unit

### Pros

- **Lower Price** – Imported models are often **much** cheaper than European versions.
- **More Storage/RAM Options** – China variants sometimes offer higher-end configurations (the 1To variant is China-exclusive)
- **Early Availability** – Chinese releases often come out before European models.

---

### Cons

- **No Google Services by Default** - Chinese devices run on ColorOS which is specific to the Chinese market, but they can be imported with OxygenOS pre-installed from [TradingShenzhen (affilied link)](https://tradingshenzhen.com/en/oneplus-15?affp=257557)
- **Missing 4G/5G Network Bands** – But can be solved with EFS edits (more on that later)
- **Warranty Issues** – Repairs may require sending the phone back to China.
- **No eSIM support** – Chinese Oneplus phones can only carry nano SIMs.

## Convert a Chinese unit to OxygenOS

If you imported an unit with ColorOS installed, follow this great guide from @koaaN at XDA -> https://xdaforums.com/t/plk110-10-dec-coloros-to-oxygenos-eu-in-16-0-2-401-glo-16-0-1-303.4767949/

## Enable all missing 4G/5G network bands

Most people who import a device from China stop after installing the global ROM, but the device often performs below its full potential. Network connectivity is usually weaker, and battery life tends to suffer as a result.

This can be easily solved on Oneplus devices with Qualcomm Snapdragon SOCs, by modifying the modem policy configuration with the Qualcomm QPST tool.

Many people believe it requires unlocking the device bootloader and rooting the device, but it does **NOT**. Here is how to achieve it, on the Oneplus 15 as an example:

- Boot your **Windows PC**
- Enable developer options in the settings.
- In developer options, enable USB debugging.
- Install [QPST](https://qpsttool.com/qpst-tool-v2-7-496)
- Install [ADB](https://www.xda-developers.com/install-adb-windows-macos-linux/)
- In a terminal/cmd prompt, type: `adb reboot ftm`. The device will reboot and show one line in Chinese on the screen.
- Now that the device is up, type `adb shell` and in the device shell, `setprop sys.usb.config diag,diag_mdm,qdss,qdss_mdm,serial_cdev,dpl,rmnet,adb`. This will set the phone in Qualcomm DIAG mode.
    - *Note that if your device is rooted, you can type `adb shell`, `su`, and then the setprop command without rebooting to FTM mode.*
- Follow [this great guide](https://xdaforums.com/t/updated-6-30-25-how-to-enable-n77-5g-oxygen-os-11-14-0-0-1901-5g-uw-icon-oxygen-os-13-14-only-9-series-le2115-le2125-verizon-network.4429489/) from @rmendez011 at XDA on how to setup Qualcomm drivers. You will need to set up the drivers on the three unknown devices named after your device (in that case, `Oneplus 15`)
- Open QPST configurator, EFS explorer, and extract `policyman/policies.xml`, as well as `policyman/global_defines.xml`
- Apply [these changes](https://github.com/dekefake/qpst-enable-all-lte-bands-plk110/commit/8aa309194acc5a35722a3677704272ce055cc8cf) to your two files.
- Drag and drop the edited files to the EFS Explorer window.
- Once the files are replaced on the phone, close EFS Explorer.
- In a terminal/cmd prompt, type: `adb reboot`.
- Enjoy.

The phone will now be able to connect to all LTE bands your carrier uses. With this mod, my Chinese Oneplus 15 happily connects to French LTE B28.

## [Purchase a Chinese Oneplus 15 (affilied link)]([TradingShenzhen](https://tradingshenzhen.com/en/oneplus-15?affp=257557))
