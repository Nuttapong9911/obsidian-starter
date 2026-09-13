---
type: Note
date: 2026-09-13
project:
  - "[[farming-web-app-game|Farming Web App Game]]"
area: "[[software-development]]"
---

Tech stack ของ [[farming-web-app-game|Farming Web App Game]]:

- Frontend: React + Phaser (canvas render ตัวเกม, React คุม UI รอบนอกเช่น inventory/shop)
- Backend: [[farm-api|farm-api]] — REST + WebSocket (sync เวลาพืชโตแบบ real-time)
- DB: PostgreSQL เก็บ state ฟาร์ม, Redis cache session ผู้เล่นที่ออนไลน์

งาน setup CI/pipeline พวกนี้เป็นงานประจำของ [[software-development|Software Development]] ไม่ใช่ของ project นี้โดยตรง — เลย link แค่ใน body ไม่ใส่ใน property `project`
