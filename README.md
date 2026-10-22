# Beetech Universal RFID SmartSdk - Official Releases

Official public release repository and package distribution for **Beetech Universal RFID & IoT Edge Gateway SmartSdk**.

[![Release](https://img.shields.io/github/v/release/anhdovan/smartsdk-releases?color=blue&logo=github)](https://github.com/anhdovan/smartsdk-releases/releases/latest)
[![PyPI](https://img.shields.io/pypi/v/beetech-smartsdk?color=green&logo=pypi)](https://pypi.org/project/beetech-smartsdk/)
[![NuGet](https://img.shields.io/nuget/v/Beetech.OmniAutoId.Client?color=blue&logo=nuget)](https://www.nuget.org/packages/Beetech.OmniAutoId.Client/)
[![NPM](https://img.shields.io/npm/v/@beetech-autoid/smartsdk-client?color=red&logo=npm)](https://www.npmjs.com/package/@beetech-autoid/smartsdk-client)
[![Docker](https://img.shields.io/docker/v/anhdv74/adv-rfid-gateway?color=blue&logo=docker)](https://hub.docker.com/r/anhdv74/adv-rfid-gateway)

The SmartSdk provides high-performance, cross-platform RFID reading, Bitwise ChaCha20 EPC Cryptography, GS1 SGTIN-96 & SSCC-18 translation, and multi-tier industrial edge automation.

---

## 1. Java / Android / Kotlin (Gradle & Maven)

The Java binding is self-contained: it embeds hardened unmanaged Native AOT engines for **Windows (x64)** and **Linux (x64)** with zero external driver prerequisites.

### Gradle (Groovy DSL)
Add to your `build.gradle`:

```groovy
repositories {
    mavenCentral()
    maven {
        // Direct public zero-friction repository (No credentials required)
        url = uri("https://raw.githubusercontent.com/anhdovan/smartsdk-releases/main/repository")
    }
}

dependencies {
    implementation 'com.beetech.adv:smartsdk:1.0.5'
}
```

### Gradle (Kotlin DSL)
Add to your `build.gradle.kts`:

```kotlin
repositories {
    mavenCentral()
    maven {
        url = uri("https://raw.githubusercontent.com/anhdovan/smartsdk-releases/main/repository")
    }
}

dependencies {
    implementation("com.beetech.adv:smartsdk:1.0.5")
}
```

### Maven (`pom.xml`)
```xml
<repositories>
    <repository>
        <id>beetech-releases</id>
        <url>https://raw.githubusercontent.com/anhdovan/smartsdk-releases/main/repository</url>
    </repository>
</repositories>

<dependencies>
    <dependency>
        <groupId>com.beetech.adv</groupId>
        <artifactId>smartsdk</artifactId>
        <version>1.0.5</version>
    </dependency>
</dependencies>
```

### Java Quickstart (Java 21+ FFM)
*JVM Flag required on Java 21: `--enable-preview --enable-native-access=ALL-UNNAMED`*

```java
import com.beetech.adv.smartsdk.SmartSdk;
import com.beetech.adv.smartsdk.SmartSdk.SmartSdkReader;
import com.beetech.adv.smartsdk.SmartSdk.TagReadEvent;

public class Main {
    public static void main(String[] args) {
        try (SmartSdk sdk = new SmartSdk()) {
            System.out.println("Hardware Fingerprint: " + sdk.getHardwareFingerprint());
            sdk.setDevLicenseBypass(true);

            // GS1 SGTIN-96 Encoding
            String sgtin = sdk.getSgtin("0614141107346", "1001", 7);
            System.out.println("SGTIN-96: " + sgtin);

            // ChaCha20 EPC Encryption
            String epc = sdk.encryptEpc(99887766L, "2000");
            System.out.println("Encrypted EPC: " + epc);

            // Connect to Reader (Impinj, Zebra, Urovo, Chainway, or Mock)
            try (SmartSdkReader reader = sdk.createReader("mock-reader", 1, "127.0.0.1")) {
                reader.addTagReadListener((TagReadEvent evt) -> {
                    System.out.printf("EPC: %s | RSSI: %d dBm | Ant: %d%n", evt.epc(), evt.rssi(), evt.antennaPort());
                });
                reader.connect();
                reader.startInventory();
            }
        }
    }
}
```

---

## 2. Python Package

Install directly from **[PyPI](https://pypi.org/project/beetech-smartsdk/)**:

```bash
pip install beetech-smartsdk
```

```python
import beetech_smartsdk as sdk

print("Fingerprint:", sdk.SmartSdk.get_hardware_fingerprint())
sdk.SmartSdk.set_dev_bypass(True)

# Encrypt EPC (ChaCha20)
encrypted = sdk.SmartSdk.encrypt_epc(9876543210, "2000")
print("Encrypted EPC:", encrypted)

# Connect to RFID reader
smart = sdk.SmartSdk()
with smart.create_reader("mock-reader", 1, "127.0.0.1") as reader:
    reader.on_tag_read(lambda evt: print(f"Tag: {evt.epc} | RSSI: {evt.rssi} dBm"))
    reader.start_inventory()
```

---

## 3. .NET / C# (NuGet)

```bash
dotnet add package Beetech.OmniAutoId.Client --version 1.0.3
```

---

## 4. Node.js / TypeScript (NPM)

```bash
npm install @beetech-autoid/smartsdk-client
```

---

## 5. Docker Edge Gateway

Run the preconfigured edge gateway container directly with Docker:

```bash
docker pull anhdv74/adv-rfid-gateway:latest

docker run -d \
  --name rfid-gateway \
  -p 19090:19090 \
  -p 19443:19443 \
  -v rfid-data:/app/data \
  -v rfid-spool:/app/spool \
  anhdv74/adv-rfid-gateway:latest
```

---

## Direct Download Assets (v1.0.5)

| Asset | Platform / Runtime | Description |
| :--- | :--- | :--- |
| **[`smartsdk-1.0.5.jar`](https://github.com/anhdovan/smartsdk-releases/releases/download/v1.0.5/smartsdk-1.0.5.jar)** | Java 21+ (All OS) | Standalone JAR containing embedded hardened native engines |
| **[`smartsdk-1.0.5.pom`](https://github.com/anhdovan/smartsdk-releases/releases/download/v1.0.5/smartsdk-1.0.5.pom)** | Maven / Gradle | POM dependency metadata |
| **[`beetech_smartsdk-1.0.5-py3-none-any.whl`](https://github.com/anhdovan/smartsdk-releases/releases/download/v1.0.5/beetech_smartsdk-1.0.5-py3-none-any.whl)** | Python 3.9+ | Self-contained Python wheel package with hardened binaries |
| **[`AdvSmartSdk.dll`](https://github.com/anhdovan/smartsdk-releases/releases/download/v1.0.5/AdvSmartSdk.dll)** | Windows x64 | Hardened Native AOT unmanaged engine (PE64) |
| **[`AdvSmartSdk.so`](https://github.com/anhdovan/smartsdk-releases/releases/download/v1.0.5/AdvSmartSdk.so)** | Linux x64 | Hardened Native AOT shared library (ELF64 stripped) |

---

## License

Copyright (c) 2026 Beetech AutoID Corp. All rights reserved.
Proprietary & Commercial. Unauthorized copying or redistribution is strictly prohibited.
