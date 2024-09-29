# تثبيت Windows على الـ POCO X3 Pro الخاص بك

## الملفات/الأدوات اللازمة

TWRP معدلة:

| إسم الملف                                       | Android إصدار |
|-------------------------------------------------|-----------------|
| [twrp-3.7.1_12-vayu.img](https://github.com/woa-vayu/POCOX3Pro-Guides/raw/main/Files/twrp-3.7.1_12-vayu.img) | Android 12/12.1/13/14 |
| [twrp-3.7.0_11-vayu.img](https://github.com/woa-vayu/POCOX3Pro-Guides/raw/main/Files/twrp-3.7.0_11-vayu.img) | Android 11 |

- [ملف Windows على ARM](https://arkt-7.github.io/woawin/)

- [التعريفات/الدرايفرات](https://github.com/woa-vayu/POCOX3Pro-Releases/releases/latest)

- [UEFI ملف](https://github.com/woa-vayu/POCOX3Pro-Releases/releases/latest)

### أقلع TWRP
>
> إذا قامت MIUI بإستبدال الركفري، أقلع fastboot واكتب الأمر التالي (إستبدل path/to/twrp.img بمسار الملف twrp.img على جهازك)

```cmd
fastboot flash recovery path\to\moddedtwrp.img reboot recovery
```

#### شغّل البرنامج النصي msc
>
> اذا قال لك البرنامج run the script again!، اكتب الأمر مرة ثانية

```cmd
adb shell msc
```

### Diskpart
>
> [!WARNING]
> **لا تحذف او تُنشئ او تعدل على ايٍّ من اقسام الذاكرة او البارتيشينات وانت بداخل هذه الأداة (Diskpart)!!!!!!! هذه قد تدمر كل ذاكرة التخزين التي في جهازك (UFS) او قد تمنعك من إقلاع fastboot!!!!!! هذا يعني ان جهازك سيتوقف عن العمل بدون حل! (بإستثناء اخذ الجهاز إلى Xiaomi او إصلاح الذاكرة عن طريق EDL، والذي سيكلفك بضعًا من الدراهم)**

```cmd
diskpart
```

#### إعرض قائمة بالأحياز (جمع حَيِّز 😅) المتصلة بحاسبك

```cmd
list volume
```

#### اختر الحَيِّزَ الخاص بـ Windows على هاتفك
>
> استعمل `list volume` لِتَجِدَه، إستبدل `$` بالرقم الفعلي الخاص بـ **WINVAYU**

```diskpart
select volume $
```

#### أعطهِ الحرف X

```diskpart
assign letter x
```

#### اختر الحَيِّزَ الخاص بـ ESP على هاتفك
>
> استعمل `list volume` لِتَجِدَه، إستبدل `$` بالرقم الفعلي الخاص بـ **ESPVAYU**

```diskpart
select volume $
```

#### أعطهِ الحرف Y

```diskpart
assign letter y
```

#### أُخرج من Diskpart

```diskpart
exit
```

### تثبيت Windows
>
> [!warning]
> لا تستعمل 24H2!!!

> إستبدل `path\to\install.esd` بالمسار الفعلي لملف install.esd (من الممكن ان يكون اسم الملف install.wim)


```cmd
dism /apply-image /ImageFile:path\to\install.esd /index:6 /ApplyDir:X:\
```

> إذا ظهر لك `Error 87`، تحقق من الترتيب الخاص بملف الـ esd الخاص بك باستخدام
>
>   `dism /get-imageinfo /ImageFile:path\to\install.esd`،
> 
>  ثم غير الرقم 6 في `index:6` بالربم الفعلي لـ **Windows 11 Pro** في ملف الـ esd الخاص بك


### استخراج الـ boot.img من الهاتف

```cmd
adb pull /dev/block/by-name/boot boot.img
```

### نسخ الـ boot.img الخاص بك إلى Windows

- اسحب وافلت ملف الـ **boot.img** من مجلد الـ platform-tools إلى قرص **WINVAYU** الظاهر في متصفح الملفات.

### Installing Drivers

- Unpack the driver archive, then open the `OfflineUpdater.cmd` file

> If it asks you to enter a letter, enter the drive letter of **WINVAYU** (which should be **X**), then press enter

#### Create Windows bootloader files
>
> If any error shows up, such as "Failure when attempting to copy boot files", open `diskpart` again and assign any new letter to **ESPVAYU**, then replace the letter `Y` in the next command with the letter that you just added.

```cmd
bcdboot X:\Windows /s Y: /f UEFI
```

#### Remove the drive letter for ESP
>
> If this does not work, ignore it and skip to the next command. This phantom drive will disappear the next time you reboot your PC.

```cmd
mountvol y: /d
```

### Reboot to fastboot

```cmd
adb reboot bootloader
```

#### Boot into the UEFI
>
> Replace `path\to\vayu-uefi.img` with the actual path of the UEFI image

```cmd
fastboot boot path\to\vayu-uefi.img
```

### Reboot to Android
Your device should reboot by itself after +- 10 minutes of waiting, after which you will be booted into Android, for the last step.

## [Last step: Setting up dualboot](4-dualboot.md)
