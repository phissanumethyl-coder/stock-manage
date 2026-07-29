# 📦 ระบบจัดการสต๊อกผ้าม่าน (Stock Manager)

ระบบเบิก/จัดการสต๊อกผ้าม่านแบบ Real-time ทำงานบน **Firebase Realtime Database** ล้วน ๆ
ไม่ต้องใช้ Google Sheet และไม่ต้องใช้ Google Apps Script อีกต่อไป — เป็นเว็บ static ที่อัพขึ้น GitHub Pages ได้เลย

---

## 📁 ไฟล์ในโปรเจค

| ไฟล์ | หน้าที่ |
|------|---------|
| `index.html` | ตัวเว็บหลัก (ระบบทั้งหมดอยู่ในไฟล์เดียว) |
| `setup.html` | หน้าตั้งค่าครั้งแรก — ใช้สร้างบัญชี Admin คนแรก |
| `firebase-rules.json` | ตัวอย่าง Security Rules ของ Firebase |
| `README.md` | คู่มือนี้ |

---

## 🔥 ขั้นตอนที่ 1 — สร้างโปรเจค Firebase ใหม่

1. ไปที่ **https://console.firebase.google.com**
2. กด **Add project / เพิ่มโปรเจค**
3. ตั้งชื่อโปรเจค เช่น `stock-manager-2026` แล้วกด Continue
4. ปิด Google Analytics ได้ (ไม่จำเป็น) แล้วกด **Create project**
5. รอสักครู่จนสร้างเสร็จ แล้วกด Continue

---

## 🗄️ ขั้นตอนที่ 2 — เปิด Realtime Database

1. ในเมนูซ้าย เลือก **Build → Realtime Database**
2. กด **Create Database**
3. เลือก location เป็น **Singapore (asia-southeast1)** *(สำคัญ — ให้ตรงกับ databaseURL)*
4. เลือกโหมด **Start in test mode** แล้วกด Enable
5. คัดลอก **URL ของ database** ไว้ (จะขึ้นด้านบน เช่น `https://stock-manager-2026-default-rtdb.asia-southeast1.firebasedatabase.app`)

### ตั้ง Security Rules
ไปที่แท็บ **Rules** แล้ววางค่านี้ (ช่วงพัฒนา/ใช้งานภายใน):

```json
{
  "rules": {
    ".read": true,
    ".write": true
  }
}
```

กด **Publish**

> ⚠️ Rules แบบนี้เปิดให้ใครก็อ่าน/เขียนได้ เหมาะกับใช้งานภายใน หากต้องการความปลอดภัยสูงขึ้น ควรตั้ง Firebase Authentication เพิ่ม

---

## 🔑 ขั้นตอนที่ 3 — เอา Firebase Config

1. ในเมนูซ้าย กดรูปเฟือง ⚙️ ข้าง Project Overview → **Project settings**
2. เลื่อนลงมาที่หัวข้อ **Your apps** กดปุ่ม **</>** (Web)
3. ตั้งชื่อ app เช่น `stock-web` แล้วกด **Register app** (ไม่ต้องติ๊ก Hosting)
4. จะเห็นโค้ด `firebaseConfig` หน้าตาแบบนี้:

```javascript
const firebaseConfig = {
  apiKey: "AIzaSy...............",
  authDomain: "stock-manager-2026.firebaseapp.com",
  databaseURL: "https://stock-manager-2026-default-rtdb.asia-southeast1.firebasedatabase.app",
  projectId: "stock-manager-2026",
  storageBucket: "stock-manager-2026.firebasestorage.app",
  messagingSenderId: "123456789012",
  appId: "1:123456789012:web:abcdef123456"
};
```

**คัดลอกทั้งบล็อกนี้ไว้**

> หมายเหตุ: ถ้าในโค้ดที่ Firebase ให้มาไม่มีบรรทัด `databaseURL` ให้เพิ่มเองโดยเอา URL จากขั้นตอนที่ 2 มาใส่

---

## ✏️ ขั้นตอนที่ 4 — ใส่ Config ลงในโค้ด

เปิดไฟล์ **`index.html`** หา `const firebaseConfig = {` (อยู่ประมาณบรรทัด 1388)
แล้วแทนที่ค่า `YOUR_...` ทั้งหมดด้วยค่าจริงจากขั้นตอนที่ 3

จากนั้นทำแบบเดียวกันกับไฟล์ **`setup.html`** (ต้องใส่ config ให้เหมือนกันเป๊ะ)

---

## 👤 ขั้นตอนที่ 5 — สร้าง Admin คนแรก

1. เปิดไฟล์ **`setup.html`** ในเบราว์เซอร์ (ดับเบิลคลิกไฟล์ได้เลย)
2. กรอกชื่อผู้ใช้ (เช่น `admin`), ชื่อที่แสดง, รหัสผ่าน, สาขา, และเลือกสิทธิ์ **admin**
3. กด **สร้างบัญชี**
4. ถ้าขึ้น ✅ สำเร็จ แสดงว่าเชื่อม Firebase ได้แล้ว

> 🔒 หลังสร้าง admin เสร็จ **ควรลบไฟล์ `setup.html` ออกจาก GitHub** เพื่อไม่ให้คนอื่นสร้างบัญชีได้
> (ภายหลังสร้างผู้ใช้เพิ่มได้จากในเว็บที่เมนู "ผู้ใช้งาน")

---

## 🌐 ขั้นตอนที่ 6 — อัพขึ้น GitHub + เปิดเว็บด้วย GitHub Pages

### สร้าง repository
1. ไปที่ **https://github.com/new**
2. ตั้งชื่อ repo เช่น `stock-manager` เลือก **Public** แล้วกด Create

### อัพไฟล์
วิธีง่ายที่สุด — กด **Add file → Upload files** แล้วลากไฟล์ทั้งหมด (`index.html`, `setup.html`, `firebase-rules.json`, `README.md`) เข้าไป แล้วกด **Commit changes**

หรือถ้าใช้ Git ในเครื่อง:
```bash
git init
git add .
git commit -m "initial: stock manager"
git branch -M main
git remote add origin https://github.com/<username>/stock-manager.git
git push -u origin main
```

### เปิด GitHub Pages
1. ในหน้า repo ไปที่ **Settings → Pages**
2. ที่ **Source** เลือก **Deploy from a branch**
3. เลือก branch **main** โฟลเดอร์ **/ (root)** แล้วกด Save
4. รอสักครู่ เว็บจะออนไลน์ที่ `https://<username>.github.io/stock-manager/`

เข้าใช้งานที่ URL นั้นได้เลย 🎉

---

## 🔐 เพิ่มโดเมนที่อนุญาต (ถ้าจำเป็น)

ถ้าเจอ error เรื่อง auth/domain ให้ไปที่ Firebase Console → **Authentication → Settings → Authorized domains**
แล้วเพิ่ม `<username>.github.io`
(ปกติระบบนี้ไม่ได้ใช้ Firebase Auth จึงมักไม่ต้องทำขั้นนี้)

---

## 📥 การเริ่มใช้งาน (ไม่มีข้อมูลตั้งต้น)

เนื่องจากเป็นระบบใหม่ ยังไม่มีสต๊อกใด ๆ ให้เริ่มแบบนี้:

1. Login ด้วย admin
2. ไปเมนู **นำเข้าสต๊อก** เพื่อเพิ่มสต๊อกผ้าแต่ละสี/ขนาด (แทนที่การดึงจาก Google Sheet เดิม)
3. เมนู **จัดการสต๊อก** ใช้ปรับจำนวนสต๊อกที่มีอยู่
4. เมนู **ผู้ใช้งาน** ใช้เพิ่มบัญชีพนักงาน (staff) และเซลส์ (sales)

โครงสร้างข้อมูลใน Firebase จะถูกสร้างอัตโนมัติเมื่อมีการบันทึกครั้งแรก
(`users`, `stock`, `transactions`, `imports`, `favorites`, `config`, `colorChart`, `activityLog`, `presence`)

---

## 🧩 สิทธิ์ผู้ใช้ (Roles)

| ฟีเจอร์ | sales | staff | admin |
|---------|:---:|:---:|:---:|
| เบิกสต๊อก | ✅ | ✅ | ✅ |
| Chart สี (ดู) | ✅ | ✅ | ✅ |
| Chart สี (จัดการ) | ❌ | ✅ | ✅ |
| ประวัติของฉัน | ✅ | ✅ | — |
| จับคู่ออเดอร์ | ❌ | ✅ | ✅ |
| จัดการสต๊อก | ❌ | ✅ | ✅ |
| นำเข้าสต๊อก | ❌ | ❌ | ✅ |
| ประวัติการเบิก | ❌ | ✅ | ✅ |
| Dashboard | ✅ | ✅ | ✅ |
| ผู้ใช้งาน | ❌ | ❌ | ✅ |
| Activity Log | ❌ | ❌ | ✅ |

---

## ❓ แก้ปัญหาที่พบบ่อย

- **เปิดเว็บแล้วเด้ง alert "ยังไม่ได้ตั้งค่า Firebase"** → ยังไม่ได้แก้ `firebaseConfig` ในไฟล์ (ขั้นตอนที่ 4)
- **สร้าง admin แล้วขึ้น error write** → Rules ยังเป็น read/write:false ให้ตั้งตามขั้นตอนที่ 2
- **Login แล้วบอกรหัสผ่านไม่ถูก** → ต้องแน่ใจว่า salt ในฟังก์ชัน `hashPassword` ของ `index.html` และ `setup.html` เหมือนกัน (อย่าแก้บรรทัด `_salt_meidea`)
- **ข้อมูลไม่ขึ้น** → เช็ค `databaseURL` ว่าตรงกับ region ที่สร้าง database

---

*ระบบทำงานบน Firebase ล้วน — ไม่ต้องใช้ Google Sheet / Apps Script อีกต่อไป*
