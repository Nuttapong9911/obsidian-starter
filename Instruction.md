## Prerequisites

ใช้ Obsidian ร่วมกับ AI ได้ 2 ทาง เลือกทางใดทางหนึ่งก็พอ

### ทาง 1: Claude Code CLI

ต้องลงทั้งคู่:
- **Claude Code CLI** — ตัว AI agent หลัก รันผ่าน terminal
- **Obsidian community plugin: Terminal** — เปิด terminal ในตัว Obsidian ได้เลย ไม่ต้องสลับไปแอป terminal แยก

### ทาง 2: Claude Code (Desktop App)

ต้องลงแค่:
- **Claude Code Desktop App** — มี UI/terminal ในตัวอยู่แล้ว ไม่ต้องพึ่ง community plugin เพิ่ม

### Optional: Obsidian CLI

ใช้ทั้ง 2 ทางเหมือนกัน — ไม่บังคับตอน setup แรก แต่บาง feature (เช่น Report Skills) ต้องมีถึงจะติดตั้งได้ ถ้ายังไม่มีให้ทำ 2 steps นี้ก่อน (จาก https://obsidian.md/cli):

1. **Activate the CLI** — Enable "Command line interface" ใน Settings → General
2. **Register the CLI** — ทำตาม on-screen instructions เพื่อเพิ่ม CLI เข้า system PATH แล้ว restart terminal (บน macOS ขั้นนี้จะสร้าง symlink ที่ `/usr/local/bin/obsidian` ต้องใช้สิทธิ์ admin จะมี system dialog ถามขึ้นมา)

เช็คว่าลงสำเร็จด้วย `which obsidian` หรือ `obsidian version`

## 1. เริ่ม Claude ใน Vault

### ทาง 1: Claude Code CLI

1. เปิด vault นี้ใน Obsidian
2. เปิด panel ของ Terminal plugin (command palette → "Terminal: Open terminal") — cwd จะเป็น root ของ vault ให้อัตโนมัติ
3. รันคำสั่ง:
   ```
   claude
   ```

### ทาง 2: Claude Code (Desktop App)

1. เปิด Claude Code Desktop App
2. เลือก/เปิด project แล้วชี้ path ไปที่ vault ที่ clone มา
3. เริ่มแชทได้เลย

## 2. เริ่ม Setup

ปกติ Claude จะทักก่อนเองว่ามี AI skills ให้ติดตั้งใน `claude-setup/` (ตาม note ใน `CLAUDE.md`) ถ้าเปิด session แล้วไม่เห็น Claude พูดถึงเรื่องนี้ พิมพ์บอกตรงๆ ได้เลย:

```
start setup
```