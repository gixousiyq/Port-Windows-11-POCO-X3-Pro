# إعادة تثبيت Windows على الـ POCO X3 Pro الخاص بك

## الملفات/الأدوات اللازمة

- [ADB و Fastboot](https://developer.android.com/studio/releases/platform-tools)

TWRP معدله:

| إسم الملف                                       | Android إصدار |
|-------------------------------------------------|-----------------|
| [twrp-3.7.1_12-vayu.img](https://github.com/woa-vayu/POCOX3Pro-Guides/raw/main/Files/twrp-3.7.1_12-vayu.img) | Android 12/12.1/13/14 |
| [twrp-3.7.0_11-vayu.img](https://github.com/woa-vayu/POCOX3Pro-Guides/raw/main/Files/twrp-3.7.0_11-vayu.img) | Android 11 |

### أقلع TWRP
> إذا قامت MIUI بإستبدال الركفري، أقلع fastboot واكتب الأمر التالي (إستبدل path/to/twrp.img بمسار الملف twrp.img على جهازك)

```cmd
fastboot flash recovery path\to\twrp.img reboot recovery
```

#### تهيئة قسم Windows (بالعربي كده فورمات لويندوز، ده مش بيعمل فورمات لأندرويد فمتقلقش)

```cmd
adb shell format
```

## [الخطوة القادمة: تثبيت Windows](/en/3-install.md)
