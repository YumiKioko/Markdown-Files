
# 📱 Mobile Hacking Cheat Sheet (Android + iOS)

> ⚠️ For ethical hacking, pentesting, and research in authorized environments only.

---

## 🔍 1. Reconnaissance

### 🔹 APK/IPA Enumeration

**Android**
```bash
aapt dump badging app.apk
apktool d app.apk
jadx-gui app.apk
```

**iOS**
```bash
unzip app.ipa -d extracted/
class-dump --arch arm64 -H extracted/Payload/app_name -o headers/
```

### 🔹 Metadata Extraction

**Android**
```bash
grep -i 'permission\|activity\|intent' AndroidManifest.xml
```

**iOS**
```bash
plutil -p Info.plist
```

---

## 🧩 2. Static Analysis

### 🛠️ Android Tools
- `apktool` – Decompile resources
- `jadx` – Decompile Java code
- `MobSF` – Static & dynamic scanner
- `AndroBugs` – Vulnerability scanner

### 🍏 iOS Tools
- `class-dump` – Extract Objective-C headers
- `Hopper`, `Ghidra`, `IDA Pro` – Reverse engineering
- `MobSF` – IPA and source analysis
- `objection` – Runtime analysis (Frida wrapper)

---

## 🧪 3. Dynamic Analysis

### 🔧 Android
```bash
frida -U -n com.target.app -l script.js
adb logcat | grep com.target.app
adb shell am start -n com.app/.MainActivity
```

**Useful Tools**
- `Frida`
- `Xposed`
- `Burp Suite` (with mitmproxy cert)
- `Drozer`

### 🍎 iOS
```bash
truststorectl --add cert.pem
frida -n AppName -U
keychain_dumper
```

**Useful Tools**
- `Frida`
- `Cycript`
- `objection`
- `SSL Kill Switch 2`

---

## 🎯 4. Exploitation

### Android Attack Vectors
- Insecure WebViews
- Exported Activities/Services
- Backup/Debuggable Apps
- Content Provider Leaks

```bash
adb shell am start -n com.app/.ExportedActivity
```

### iOS Attack Vectors
- Jailbreak detection bypass
- Insecure URL schemes
- Unencrypted plist files
- Keychain misconfigurations

---

## 🧠 5. Post-Exploitation

### Android
- Shared prefs: `/data/data/com.app/shared_prefs/`
- Databases: `/data/data/com.app/databases/`
- Look for tokens, keys, secrets

### iOS
- App data: `/var/mobile/Containers/Data/Application/`
- Hook sensitive funcs:
```bash
frida-trace -n AppName -m "*send*"
```

---

## 🧰 Tools Summary

| Platform | Tools |
|----------|-------|
| **Android**  | apktool, jadx, Frida, MobSF, Drozer, Burp, Xposed |
| **iOS**      | Frida, Cycript, Objection, class-dump, Ghidra, MobSF |

---

## ⚠️ Legal & Ethical Notice

This cheat sheet is for **authorized use only**. Do **not** use these techniques on systems or apps you do not own or have explicit permission to test. Unauthorized access is **illegal**.
