# NF18MESH System Information

This page provides detailed hardware and system information for the **NetComm NF18MESH** router. It’s based on direct inspection of the device and `/proc` dumps.

---

## CPU / SoC

- **CPU Core:** Broadcom BMIPS4350 V8.0 (dual-core)  
- **SoC:** Broadcom BCM63168  
- **Architecture:** MIPS32  
- **BogoMIPS:** 397 / 409  

**CPU Details from `/proc/cpuinfo`:**


processor : 0
cpu model : Broadcom BMIPS4350 V8.0
BogoMIPS : 397.31

processor : 1
cpu model : Broadcom BMIPS4350 V8.0
BogoMIPS : 409.60


> This SoC is commonly used in dual-core Broadcom router designs with integrated networking features.

---

## Memory

- **RAM:** 256 MB  
- **Swap:** 0 MB  
- **Approx. Free RAM:** 85 MB (after kernel and slab usage)  

Slab information (from `/proc/slabinfo`) is available in the repository under `proc-files/slabinfo.txt`.

---

## Flash and Filesystems

| Partition | Size | Used | Free | Type |
|-----------|------|------|------|------|
| rootfs    | 61 MB| 35 MB| 26 MB | JFFS2 (read-only) |
| data      | 4 MB | 0.5 MB| 3.5 MB | JFFS2 (read-write) |

- The **writable area** is `/data` — ideal for storing scripts, logs, or small custom binaries.  
- Root filesystem is mostly read-only to protect firmware integrity.

---

## Users

| Username | UID | GID | Description |
|----------|-----|-----|------------|
| admin    | 0   | 0   | Administrator |
| support  | 0   | 0   | Technical Support |
| user     | 0   | 0   | Normal User |
| nobody   | 0   | 0   | FTP / unprivileged user |

> Note: On this router, all regular users except `nobody` have full root privileges.

---

## Proc Dumps

For reference, the repository includes raw dumps from `/proc`:

- **CPU info:** `proc-files/cpuinfo.txt`  
- **Memory info:** `proc-files/meminfo.txt`  
- **Flash / partitions:** `proc-files/mtd.txt`  
- **Kernel version:** `proc-files/version.txt`  
- **Slab / memory caches:** `proc-files/slabinfo.txt`  

These files are included for research, debugging, or comparison with other NF18MESH units.

---

## Notes

- This page is for **documentation and research purposes**.  
- Sensitive files like device keys or passwords are **not included**.  
- It is intended for developers, hardware enthusiasts, and anyone interested in the inner workings of Broadcom BMIPS4350-based routers.

---

*Compiled and documented by terneis-git.*
