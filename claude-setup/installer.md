# Installer

> สำหรับ Claude: ไฟล์นี้คุยกับ user ทีละ feature ต่อไปนี้ ต่อ 1 feature ให้ทำตามลำดับ:
> 1. อธิบายสั้นๆ ว่า feature นี้คืออะไร
> 2. ถามว่าอยากติดตั้งไหม
> 3. ถ้าไม่ → ข้ามไป feature ถัดไป
> 4. ถ้าใช่ → ถาม confirm อีกรอบ แล้วค่อยโชว์คำสั่งติดตั้งให้ user เป็นคนรันเอง (Claude ไม่รันเอง เว้นแต่ user ขอให้รันให้)

## Feature 1: Obsidian Skill (by Obsidian's CEO)

**อธิบายให้ user ฟัง:**

Skill ชุดนี้เขียนโดย Steph Ango (kepano) — CEO ของ Obsidian เอง ([kepano/obsidian-skills](https://github.com/kepano/obsidian-skills)) เป็น Claude Code skill 5 ตัวสำหรับทำงานกับไฟล์ Obsidian โดยตรง:

- `obsidian-markdown` — เขียน/แก้ syntax เฉพาะของ Obsidian (wikilink, callout, properties, embed)
- `obsidian-bases` — สร้าง/แก้ไฟล์ `.base`
- `json-canvas` — สร้าง/แก้ไฟล์ `.canvas`
- `obsidian-cli` — คุยกับ Obsidian ที่เปิดอยู่ผ่าน `obsidian` CLI (ต้องลง CLI แยกต่างหาก — https://help.obsidian.md/cli)
- `defuddle` — ดึง markdown สะอาดๆ จากหน้าเว็บ ประหยัด token แทนการ fetch ตรงๆ

vault นี้ใช้ skill ชุดนี้อยู่แล้ว — note, `.base`, `.canvas` ตัวอย่างทั้งหมดถูกสร้างด้วย skill พวกนี้

**ถาม user:** อยากติดตั้งไหม?

- **ไม่** → ข้ามไป feature ถัดไป
- **ใช่** → confirm อีกครั้ง ("ยืนยันติดตั้ง Obsidian Skill ใช่ไหม?") แล้วโชว์คำสั่งนี้ให้ user รันเอง:

  ```
  /plugin marketplace add kepano/obsidian-skills
  /plugin install obsidian@obsidian-skills
  ```

  หมายเหตุให้ user: นี่คือ Claude Code slash command รันในเซสชัน Claude Code เอง ไม่ใช่ shell command หลังติดตั้งเสร็จ Claude Code จะเขียน `enabledPlugins` ลง `.claude/settings.json` ของ vault นี้ให้อัตโนมัติ — commit ไฟล์นั้นไว้ด้วยเพื่อให้คนอื่นที่ clone repo ต่อได้ enable เหมือนกัน

## Feature 2: Daily / Weekly / Monthly Report Skills

**อธิบายให้ user ฟัง:**

Skill ส่วนตัว 3 ตัวสำหรับเขียนรายงานงานประจำ — ไม่ได้มาจาก marketplace เป็นไฟล์ custom ที่มากับ starter kit นี้เลย:

- `/daily-report` — สร้าง/เปิด daily report ของวันนี้จาก template พร้อมเปิด report ก่อนหน้าเทียบกันแบบ split pane
- `/weekly-report` — รวบรวม daily report ของสัปดาห์นี้ สรุปเป็น bullet สั้นๆ พร้อมเอาไปพูดในที่ประชุม
- `/monthly-report` — รวบรวม daily report ทั้งเดือน สรุปเป็น draft ให้หัวข้อ ไม่บีบเนื้อหาแรงเท่า weekly เพราะเป็น raw material ให้ user เอาไปเรียบเรียงต่อเอง

ทั้ง 3 ตัวพึ่ง `obsidian` CLI (ต้องเปิด Obsidian อยู่ตอนรัน) และพึ่ง template `daily_report.md` ใน `_Templates/` สำหรับตัว `/daily-report`

**เช็ค prerequisite ก่อนถาม install:** รัน `which obsidian` หรือ `obsidian version` ดูก่อนว่ามี CLI พร้อมใช้ไหม

- **ไม่มี** → ชี้ user ไปทำตาม "Optional: Obsidian CLI" ใน `instruction.md` (หมวด Prerequisites) ก่อน แล้วค่อยกลับมาเช็คซ้ำ (`which obsidian`) ก่อนไปต่อ

**ถาม user:** อยากติดตั้งไหม?

- **ไม่** → ข้ามไป feature ถัดไป
- **ใช่** → confirm อีกครั้ง ("ยืนยันติดตั้ง Report Skills ใช่ไหม?") แล้วทำตามนี้ (ไม่ต้องโชว์คำสั่งให้ user รันเอง เพราะเป็นแค่การ copy ไฟล์ + แก้ path ในไฟล์ ทำให้ user เลยได้):

  1. หา vault path ปัจจุบัน (`pwd` หรือ root ของ vault ที่ user เปิดอยู่) และ vault name (ปกติคือชื่อโฟลเดอร์ vault — ถ้าไม่ชัวร์ให้ถาม user)
  2. Copy 3 ไฟล์นี้จาก `claude-setup/skills/<name>/SKILL.md` ไปยัง `.claude/skills/<name>/SKILL.md` (daily-report, weekly-report, monthly-report) — ระหว่าง copy ให้แทนที่ `__VAULT_PATH__` ด้วย vault path จริง และ `__VAULT_NAME__` ด้วย vault name จริงในทุกจุดที่เจอ
  3. Copy `claude-setup/templates/daily_report.md` ไปยัง `_Templates/daily_report.md` (ไฟล์นี้ไม่มี path ต้องแทน copy ตรงๆ ได้เลย)
  4. บอก user ว่าเสร็จแล้ว ลองพิมพ์ `/daily-report` ได้เลย

  หมายเหตุ: staged source ใน `claude-setup/skills/` และ `claude-setup/templates/` เก็บไว้เป็น reference ต่อ ไม่ต้องลบทิ้งหลังติดตั้ง

## Final Step: Cleanup

หลังถามครบทุก feature ข้างบนแล้ว (ไม่ว่า user จะเลือกติดตั้งอะไรบ้าง) ให้เก็บกวาดไฟล์ setup เอง:

**ถาม user:** ไฟล์ setup (`instruction.md` กับทั้งโฟลเดอร์ `claude-setup/` ที่มีไฟล์นี้อยู่) ตอนนี้ใช้งานเสร็จแล้ว อยากให้ทำยังไง?

- **ย้ายไป `_Archives/`** — เก็บไว้อ้างอิงย้อนหลังได้ (เช่น กลับมาดูว่า feature ไหนติดตั้งไปแล้วบ้าง)
- **ลบทิ้ง** — เอาออกจาก vault ให้สะอาด

ทำตามที่ user เลือก:

- ย้าย: ย้าย `instruction.md` และโฟลเดอร์ `claude-setup/` (ทั้งโฟลเดอร์) เข้าไปใน `_Archives/`
- ลบ: ลบ `instruction.md` และโฟลเดอร์ `claude-setup/` ทั้งหมด

ไม่ต้องถามเรื่อง `CLAUDE.md` ทั้งไฟล์ — ไฟล์นั้นอธิบายโครงสร้าง vault ให้ Claude อ่านต่อไปเรื่อยๆ ไม่ใช่ไฟล์ setup ชั่วคราว เก็บไว้เหมือนเดิม แต่ให้ลบเฉพาะ block "On first message in a fresh session" (อยู่บนสุดของ `CLAUDE.md` พูดถึง `claude-setup/`) ออกไปด้วย เพราะหลัง cleanup โฟลเดอร์นั้นไม่มีอยู่แล้ว ทิ้ง note ไว้จะชี้ไปที่ที่ไม่มีอยู่จริง
