# vendor_google_pixel-additional
Additional Pixel stuffs

## Note for Google Play system updates support

### Reminder
You should fix VINTF entry missing errors, dlopen failures, system app crashes before you proceed.
<br>Otherwise, Google Play system updates will rollbacks all updates or even causing bootloop.
<br>Mostly, rollbacking/bootlooping occurs if you missed adding [VINTF](https://source.android.com/docs/core/architecture/vintf) entries in [Frameworks Compatibility Matrix](https://source.android.com/docs/core/architecture/vintf/comp-matrices) or Device Manifest (or both)
<br>that is not marked as optional or important to operate on your system,
<br>or dlopen failed due to missing blobs/symbols,
<br>or frequent system app crashes (e.g. MIUI Camera app).

### Add support for Google Play system updates
First, you need to add commit [build: Enable MODULE_BUILD_FROM_SOURE even if the apex is prebuilt](https://github.com/TheParasiteProject/build/commit/2e74d275c52d65a522db13a06494137ffd098f67) to build repo.<br>

After that, you need to include the `config.mk`'s path to your `device.mk`

```M
$(call inherit-product-if-exists, vendor/google/pixel-additional/config.mk)
```

If your device support Google Pixel's Now Playing feature,
<br>you could enable it by setting `TARGET_SUPPORTS_NOW_PLAYING := true` in your `device.mk`.
<br>

If you don't want to/can't support Google Play system updates,
<br>Set `TARGET_SUPPORTS_PREBUILT_UPDATABLE_APEX := false` in your `device.mk`.
<br>This will allows you to use AOSP's source build APEX.

```M
TARGET_SUPPORTS_PREBUILT_UPDATABLE_APEX := false
```

## Note for including CarrierSettings
You need to set `TARGET_INCLUDE_CARRIER_SETTINGS` to `true` in your device tree
<br>For example, if [PixelExperience](https://github.com/PixelExperience), you should add this flag to `aosp_(device-code-name).mk`

```M
TARGET_INCLUDE_CARRIER_SETTINGS := true
```

And then include the `config.mk`'s path to your `device.mk`

```M
$(call inherit-product-if-exists, vendor/google/pixel-additional/config.mk)
```

## Note for including more telephony components

* `TARGET_INCLUDE_PIXEL_IMS`: Pixel IMS
* `TARGET_INCLUDE_PIXEL_EUICC`: Pixel eUICC
* `TARGET_INCLUDE_CARRIER_SERVICES`: Google Carrier Services

## Note for including additional GApps and customizations

This repo also includes several additional GApps packages, such as 
* `TARGET_INCLUDE_CAMERA_GO`: Camera from Google (Formerly, Camera Go or GCam Go)
* `TARGET_SUPPORTS_LILY_EXPERIENCE`: Enabling Android (Go Edition) device specific features
* `TARGET_SUPPORTS_GOOGLE_BATTERY`: Build TurboAdapter with dummy GoogleBatteryService wheh flag is false
* `TARGET_GBOARD_KEY_HEIGHT`: Resize GBoard ime key height to `TARGET_GBOARD_KEY_HEIGHT` (Must be float. e.g. 1.2)

## Credits
* [DerpFest AOSP](https://github.com/DerpFest-AOSP)
* [hentaiOS](https://github.com/hentaiOS)
* [Jarl-Penguin](https://github.com/JarlPenguin)
* [PixelExperience](https://github.com/PixelExperience)
* [StatiX](https://github.com/StatiXOS)
