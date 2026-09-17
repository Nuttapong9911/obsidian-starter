# Installer

> สำหรับ Claude: ไฟล์นี้คุยกับ user ทีละ feature ต่อไปนี้ ต่อ 1 feature ให้ทำตามลำดับ:
> 1. อธิบายสั้นๆ ว่า feature นี้คืออะไร
> 2. ถามว่าอยากติดตั้งไหม
> 3. ถ้าไม่ → ข้ามไป feature ถัดไป
> 4. ถ้าใช่ → ถาม confirm อีกรอบ แล้วค่อยโชว์คำสั่งติดตั้งให้ user เป็นคนรันเอง (Claude ไม่รันเอง เว้นแต่ user ขอให้รันให้)
>
> ข้อยกเว้น: **Feature 1** ไม่ทำตามลำดับข้างบน — โชว์คำสั่งติดตั้งไปพร้อม explanation ในข้อความเดียวเลย ไม่ต้องถามก่อนว่าจะติดตั้งไหม ไม่ต้อง confirm ซ้ำ (ดูรายละเอียดใน section ของมันเอง)
>
> คุยกับ user เป็น**ภาษาไทย**ตลอด installer นี้ (ยกเว้นชื่อไฟล์/คำสั่ง/code block ที่ต้องคงเป็นภาษาอังกฤษตามจริง)

## Step 0: Vault Name

**อธิบายให้ user ฟัง พร้อมโชว์ตัวอย่างคำสั่งในข้อความเดียวกัน:**

ชื่อโฟลเดอร์นี้ (ชื่อปัจจุบัน: ดูจากชื่อโฟลเดอร์จริง) คือชื่อ vault ที่ Obsidian ใช้จริง และ skill บางตัวใน Feature 2 (`/daily-report` ฯลฯ) จะฝัง path/vault name นี้ไว้ตอนติดตั้ง — ถ้าอยากเปลี่ยนชื่อ vault ควร rename **ก่อน** setup ต่อ

**ถาม user:** อยาก rename ไหม?

- **ไม่** → ไปต่อ Feature 1 ได้เลย
- **ใช่** → บอก user ให้ออกจาก session นี้ก่อน แล้ว rename โฟลเดอร์เอง (Claude ไม่รันให้ เพราะจะทำให้ path ของ session ปัจจุบันหลุด) ตัวอย่างคำสั่ง:

  ```
  mv <path เดิม> <path ใหม่ที่มีชื่อโฟลเดอร์ที่ต้องการ>
  ```

  แล้วเปิด Claude Code ใหม่จาก path ใหม่ พิมพ์ `start setup` อีกครั้งเพื่อ resume ต่อ

## Feature 1: Obsidian Skill (by Obsidian's CEO)

**อธิบายให้ user ฟัง พร้อมโชว์คำสั่งติดตั้งในข้อความเดียวกันเลย** (ไม่ต้องถามว่าจะติดตั้งไหมก่อน):

Skill ชุดนี้เขียนโดย Steph Ango (kepano) — CEO ของ Obsidian เอง ([kepano/obsidian-skills](https://github.com/kepano/obsidian-skills)) เป็น Claude Code skill 5 ตัวสำหรับทำงานกับไฟล์ Obsidian โดยตรง:

- `obsidian-markdown` — เขียน/แก้ syntax เฉพาะของ Obsidian (wikilink, callout, properties, embed)
- `obsidian-bases` — สร้าง/แก้ไฟล์ `.base`
- `json-canvas` — สร้าง/แก้ไฟล์ `.canvas`
- `obsidian-cli` — คุยกับ Obsidian ที่เปิดอยู่ผ่าน `obsidian` CLI (ต้องลง CLI แยกต่างหาก — ดู "Optional: Obsidian CLI" ใน `instruction.md`)
- `defuddle` — ดึง markdown สะอาดๆ จากหน้าเว็บ ประหยัด token แทนการ fetch ตรงๆ

vault นี้ใช้ skill ชุดนี้อยู่แล้ว — note, `.base`, `.canvas` ตัวอย่างทั้งหมดถูกสร้างด้วย skill พวกนี้

ถ้าอยากติดตั้ง รันคำสั่งนี้เอง (Claude Code slash command รันในเซสชัน Claude Code เอง ไม่ใช่ shell command):

```
/plugin marketplace add kepano/obsidian-skills
/plugin install obsidian@obsidian-skills
```

หลังติดตั้งเสร็จ Claude Code จะเขียน `enabledPlugins` ลง `.claude/settings.json` ของ vault นี้ให้อัตโนมัติ

**ถาม user:** อยากไปต่อ feature ถัดไปไหม?

## Step 1.5: Template Config

vault นี้ออกแบบให้ template folder ต้องเป็น `_Templates` เสมอ (repo pattern ตายตัว ไม่ใช่ optional) — เช็คก่อนเข้า Feature 2 ว่าตั้งไว้ถูกไหม จาก `.obsidian/core-plugins.json` (ต้องมี `"templates": true`) และ `.obsidian/templates.json` (ต้องมี `{"folder": "_Templates"}`)

- **ถูกอยู่แล้ว** → ข้ามไป Feature 2 ได้เลย ไม่ต้องถามอะไร
- **ยังไม่ถูก/ไม่มีไฟล์** → **ถาม user เป็น confirmation prompt เดียว:** "vault นี้ต้องเปิด Templates core plugin ชี้ไปที่ `_Templates` ให้ Claude แก้ไฟล์ `.obsidian/core-plugins.json` และ `.obsidian/templates.json` ให้เลยไหม (เทียบเท่า user เข้า Settings → Core plugins → เปิด Templates → ตั้ง Template folder location เป็น `_Templates`)"
  - **ใช่** → set `"templates": true` ใน `.obsidian/core-plugins.json`, เขียน `{"folder": "_Templates"}` ใน `.obsidian/templates.json` (สร้างไฟล์ถ้ายังไม่มี) → รัน `obsidian reload vault=__VAULT_NAME__` ทันที แล้วไปต่อ Feature 2
  - **ไม่** → เตือน user ว่า `/daily-report` ใน Feature 2 จะไม่ทำงานถ้าไม่ตั้งอันนี้ ถามอีกรอบว่าจะข้ามไป Feature 2 แบบรู้ความเสี่ยง หรือกลับไปตั้งเองผ่าน Settings ก่อนแล้วเช็คซ้ำ

## Feature 2: Daily / Weekly / Monthly Report Skills

**อธิบายให้ user ฟัง:**

Skill ส่วนตัว 3 ตัวสำหรับเขียนรายงานงานประจำ — ไม่ได้มาจาก marketplace เป็นไฟล์ custom ที่มากับ starter kit นี้เลย:

- `/daily-report` — สร้าง/เปิด daily report ของวันนี้จาก template พร้อมเปิด report ก่อนหน้าเทียบกันแบบ split pane
- `/weekly-report` — รวบรวม daily report ของสัปดาห์นี้ สรุปเป็น bullet สั้นๆ พร้อมเอาไปพูดในที่ประชุม
- `/monthly-report` — รวบรวม daily report ทั้งเดือน สรุปเป็น draft ให้หัวข้อ ไม่บีบเนื้อหาแรงเท่า weekly เพราะเป็น raw material ให้ user เอาไปเรียบเรียงต่อเอง

ทั้ง 3 ตัวพึ่ง `obsidian` CLI (ต้องเปิด Obsidian อยู่ตอนรัน) และ `/daily-report` พึ่ง template folder `_Templates` ที่เช็ค/ตั้งไว้แล้วใน Step 1.5

**เช็ค prerequisite ก่อนถาม install:** รัน `which obsidian` หรือ `obsidian version` ดูก่อนว่ามี CLI พร้อมใช้ไหม

- **ไม่มี** → ชี้ user ไปทำตาม "Optional: Obsidian CLI" ใน `instruction.md` (หมวด Prerequisites) ก่อน แล้วค่อยกลับมาเช็คซ้ำ (`which obsidian`) ก่อนไปต่อ

**ถาม user:** อยากติดตั้งไหม?

- **ไม่** → ข้ามไป feature ถัดไป
- **ใช่** → confirm อีกครั้งพร้อมบอกรายละเอียดว่าจะทำอะไร เช่น "ยืนยันติดตั้ง Report Skills ใช่ไหม? จะ copy skill 3 ตัว (daily-report, weekly-report, monthly-report) จาก `claude-setup/skills/` ไปติดตั้งเป็น local-level skill ใน `.claude/skills/` ของ vault นี้ พร้อม copy template `daily_report.md` ไปที่ `_Templates/` ด้วย" แล้วทำตามนี้ (ไม่ต้องโชว์คำสั่งให้ user รันเอง เพราะเป็นแค่การ copy ไฟล์ + แก้ path ในไฟล์ ทำให้ user เลยได้):

  1. หา vault path ปัจจุบัน (`pwd` หรือ root ของ vault ที่ user เปิดอยู่) และ vault name (ปกติคือชื่อโฟลเดอร์ vault — ถ้าไม่ชัวร์ให้ถาม user)
  2. Copy 3 ไฟล์นี้จาก `claude-setup/skills/<name>/SKILL.md` ไปยัง `.claude/skills/<name>/SKILL.md` (daily-report, weekly-report, monthly-report) — ระหว่าง copy ให้แทนที่ `__VAULT_PATH__` ด้วย vault path จริง และ `__VAULT_NAME__` ด้วย vault name จริงในทุกจุดที่เจอ
  3. Copy `claude-setup/templates/daily_report.md` ไปยัง `_Templates/daily_report.md`
  4. สร้างโฟลเดอร์ `Reports/Daily`, `Reports/Weekly`, `Reports/Monthly` ใน vault (`mkdir -p`) ให้ครบทั้ง 3 ก่อนที่ skill จะถูกเรียกใช้จริง
  5. รัน `obsidian reload vault=__VAULT_NAME__` อีกครั้งเพื่อให้แน่ใจว่า Obsidian เห็นโฟลเดอร์ใหม่และ config ล่าสุดครบ (กัน bug เดิมที่ session ค้าง config เก่า)
  6. บอก user ว่าเสร็จแล้ว ลองพิมพ์ `/daily-report` ได้เลย

  หมายเหตุ: staged source ใน `claude-setup/skills/` และ `claude-setup/templates/` เก็บไว้เป็น reference ต่อ ไม่ต้องลบทิ้งหลังติดตั้ง

## Final Step: Cleanup

หลังถามครบทุก feature ข้างบนแล้ว (ไม่ว่า user จะเลือกติดตั้งอะไรบ้าง) ให้เก็บกวาดไฟล์ setup เอง:

**สำคัญ:** ขั้นนี้มี 2 คำถามให้ user ตอบ (setup files + example data ข้างล่าง) — ให้ถามทั้ง 2 ข้อ**พร้อมกันในครั้งเดียว** แบบ multiple-choice ที่ user เลือกตอบทีเดียวทั้ง 2 ข้อ (เช่นใช้ AskUserQuestion tool ส่ง 2 questions ในการเรียกครั้งเดียว) ห้ามถามทีละข้อแยกกัน

**ถาม user:** ไฟล์ setup (`instruction.md` กับทั้งโฟลเดอร์ `claude-setup/` ที่มีไฟล์นี้อยู่) ตอนนี้ใช้งานเสร็จแล้ว อยากให้ทำยังไง?

1. **ลบทิ้ง** — เอาออกจาก vault ให้สะอาด
2. **ย้ายไป `_Archives/`** — เก็บไว้อ้างอิงย้อนหลังได้ (เช่น กลับมาดูว่า feature ไหนติดตั้งไปแล้วบ้าง)
3. **ให้ user เลือกเอง** — ถาม user ว่าอยากเก็บไฟล์ไหนไว้บ้าง (เช่น เก็บแค่ `installer.md` ไว้ดูย้อนหลัง) แล้วไฟล์ที่เหลือเอาไปไว้ที่ไหน (`_Archives/` หรือลบ) — ทำตามที่ user ตอบเป๊ะๆ ไม่ต้องเดาแทน

ทำตามที่ user เลือก:

- ตัวเลือก 1: ลบ `instruction.md` และโฟลเดอร์ `claude-setup/` ทั้งหมด
- ตัวเลือก 2: ย้าย `instruction.md` และโฟลเดอร์ `claude-setup/` (ทั้งโฟลเดอร์) เข้าไปใน `_Archives/`
- ตัวเลือก 3: ตามที่ user ระบุ

ไม่ต้องถามเรื่อง `CLAUDE.md` ทั้งไฟล์ — ไฟล์นั้นอธิบายโครงสร้าง vault ให้ Claude อ่านต่อไปเรื่อยๆ ไม่ใช่ไฟล์ setup ชั่วคราว เก็บไว้เหมือนเดิม แต่ให้ลบเฉพาะ block "On first message in a fresh session" (อยู่บนสุดของ `CLAUDE.md` พูดถึง `claude-setup/`) ออกไปด้วย เพราะหลัง cleanup โฟลเดอร์นั้นไม่มีอยู่แล้ว ทิ้ง note ไว้จะชี้ไปที่ที่ไม่มีอยู่จริง

**ถาม user อีกข้อ:** example data ที่มากับ starter kit ตอนนี้อยากทำยังไง? ไฟล์ที่เกี่ยวข้องทั้งหมด:
- `_Projects/_examples/`, `_Areas/_examples/`, `_Keywords/_examples/`, `_Microservices/_examples/`
- `_Templates/note-project-farm-app.example.md`
- `Notes/farming-web-app-example/` ทั้งโฟลเดอร์ (ไฟล์ `.example.md` ทั้งหมด)
- `base-farming-web-app-game.example.base`
- `bases-explained.example.canvas`

1. **เก็บไว้เหมือนเดิม** — ไม่แตะอะไร
2. **ย้ายไป `_Archives/`** — เอาออกจาก working folder แต่ยังเปิดดูอ้างอิงได้ (ย้ายทุกไฟล์ข้างต้นทั้งหมด รวม base/canvas ด้วย คงโครง path เดิมไว้ใต้ `_Archives/`)
3. **ลบทิ้ง** — ลบทุกไฟล์ข้างต้นทั้งหมด รวม base/canvas ด้วย ให้ vault สะอาด
4. **ให้ user เลือกเอง** — ถาม user ว่าอยากเก็บไฟล์ไหนไว้บ้าง แล้วไฟล์ที่เหลือ (ที่ไม่ได้เลือกเก็บ) จะเอาไปไว้ที่ไหน (`_Archives/` หรือ ลบทิ้ง) — ทำตามที่ user ตอบเป๊ะๆ ไม่ต้องเดาแทน
