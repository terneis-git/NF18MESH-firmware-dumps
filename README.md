# NF18MESH Firmware Dumps

**Device:** NF18MESH (Slingshot / ISP model)  
**SoC:** Broadcom BMIPS4350 (dual-core)  
**Bootloader:** CFE (accessible via serial)

## Overview

This repository contains extracted firmware and root filesystem images from the NF18MESH router.  
The purpose of this repo is **documentation, research, and analysis** of the device firmware layout.

- RAM booting is possible via CFE
- Root filesystem is JFFS2 (read-only)
- `/data` partition is JFFS2 (read-write)
- Dual firmware image layout is present

!!! **Not affiliated with the manufacturer or ISP.** !!!

---

## Contents

| File | Description |
|------|-------------|
| `rootfs.bin` | Active root filesystem |
| `rootfs_update.bin` | Backup root filesystem |
| `image.bin` | Kernel + root filesystem |
| `image_update.bin` | Backup kernel + root filesystem |

---

## Flash Layout

mtd0: rootfs
mtd1: rootfs_update
mtd2: data
mtd3: nvram
mtd4: image
mtd5: image_update


---

## Notes / Warnings

- **Do NOT flash these images** unless you fully understand the risks.
- Flashing incorrect images can permanently brick the device.
- RAM booting via CFE is safer for experimentation.
- Firmware images may contain **device-specific identifiers**.

---

## Accessing Serial & CFE

This documents **my process**. Your board revision may differ.

### 1️⃣ Finding the Serial Header

1. Remove the **four Phillips screws** from the back of the router.
2. Slide the PCB out of the plastic shell.
3. Locate the **4-pin header** labeled:

IN OUT GND 3.3V


!!! **Do not connect to the 3.3V.** !!!

---

### 2️⃣ Using an ESP32 as a USB-to-UART Adapter

I used a **generic ESP32-S3**, but most ESP32 boards will work.

#### Requirements
- ESP32 board
- Arduino IDE
- CP2102 or CH340 USB driver (depends on your ESP32)

#### Setup

1. Install Arduino IDE
2. Add ESP32 board support  
   - `File → Preferences → Additional Boards Manager URLs`
   - Add:
     ```
     https://raw.githubusercontent.com/espressif/arduino-esp32/gh-pages/package_esp32_index.json
     ```
3. Install the **ESP32 by Espressif Systems** package
4. Install CP2102 drivers if required

#### ESP32 Serial Bridge Code

Upload this sketch to the ESP32:

```
void setup() {
  Serial.begin(115200);
  Serial2.begin(115200, SERIAL_8N1, 16, 17); // RX=16, TX=17
}

void loop() {
  if (Serial.available()) {
    Serial2.write(Serial.read());
  }
  if (Serial2.available()) {
    Serial.write(Serial2.read());
  }
}

```

Wiring
ESP32	Router Serial
GND-GND
RX2(GPIO16)-OUT
TX2(GPIO17)-IN

    OUT = Router TX
    IN = Router RX

3️⃣ Connecting to Serial

    Plug in the router

    Open Arduino Serial Monitor

        Ctrl + Shift + M

        Baud rate: 115200

    Power on the router

    You should see boot output

4️⃣ Entering CFE

    Power on the router

    Watch for:  Press any key to stop auto run

    Press any key or Ctrl+C

    You should now be at the CFE prompt

5️⃣ Getting a Root Shell (Normal Boot)

    Allow the router to boot normally

    At the prompt, type:

login admin

Enter the password from the router sticker

Once logged in, type:

   sh 

If successful, the prompt will change to:

    #

You now have a root shell.

!!! License / Disclaimer !!!

This repository is provided as-is for educational and research purposes only.
Use entirely at your own risk.
