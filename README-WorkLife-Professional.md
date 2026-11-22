# WorkLife Professional 3.2.1 — My Workspace (Local Only)

เว็บแอปบันทึกเวลาทำงาน ค่าแรง/OT และรายรับรายจ่าย พร้อมบัญชี Firebase Authentication และ Cloud Firestore สำหรับข้อมูลแอป

## ความสามารถ
- สมัคร/เข้าสู่ระบบด้วยอีเมลและรหัสผ่าน หรือ Google
- Cloud Firestore แยกข้อมูลตาม Firebase UID และซิงก์ข้อมูลแอปแบบ real-time
- หน้า Login แยกจากหน้าตั้งค่า
- My Workspace: เลือกรูปจากอุปกรณ์เป็นพื้นหลังทั้งหน้า Login และหน้าใช้งาน
- ปรับความเข้มและความเบลอของภาพได้
- รูปพื้นหลังจัดเก็บใน IndexedDB บนอุปกรณ์เท่านั้น ไม่อัปโหลดไป Firebase และไม่ต้องเปิด Firebase Storage
- GitHub Pages และ PWA

## การจัดเก็บรูป My Workspace
- รองรับ JPG, PNG และ WebP; ระบบย่อภาพให้ด้านยาวไม่เกิน 1800px และบันทึกเป็น JPEG ก่อนจัดเก็บ
- ภาพและการตั้งค่าพื้นหลังอยู่ในพื้นที่จัดเก็บของเบราว์เซอร์บนอุปกรณ์นั้น
- รูปจะไม่ซิงก์ไปอุปกรณ์อื่น แม้ผู้ใช้ล็อกอินบัญชี Firebase เดียวกัน
- หากล้างข้อมูลเว็บไซต์/เบราว์เซอร์ ถอนข้อมูลเว็บไซต์ หรือใช้โหมดส่วนตัว รูปอาจถูกลบ ควรเก็บไฟล์ต้นฉบับไว้ในอุปกรณ์ด้วย
- IndexedDB เป็นพื้นที่จัดเก็บฝั่งอุปกรณ์ โควตาและการคงอยู่ขึ้นกับเบราว์เซอร์และการตั้งค่าของผู้ใช้ ดูรายละเอียด: https://developer.mozilla.org/en-US/docs/Web/API/Storage_API/Storage_quotas_and_eviction_criteria

## Firebase
Firebase Authentication และ Cloud Firestore ยังคงใช้ตามการตั้งค่าเดิม ดู `cloud-setup.md` และเผยแพร่ `firestore.rules` ตามคู่มือ ไม่ต้องสร้าง Firebase Storage Bucket และไม่ต้องเผยแพร่ `storage.rules` สำหรับเวอร์ชันนี้

## เผยแพร่ GitHub Pages
อัปโหลดไฟล์ `index.html`, `manifest.json`, `sw.js`, `icon-192.png`, `icon-512.png`, `cloud-setup.md`, `firestore.rules` ไปที่ root ของ repository จากนั้นรอ GitHub Pages deploy เสร็จ

## ความเป็นส่วนตัว
รูปพื้นหลังที่เลือกจะถูกเก็บในเบราว์เซอร์ของอุปกรณ์ผู้ใช้ ไม่ถูกส่งไป Firebase Storage หรือ Firestore โดยฟีเจอร์ My Workspace ส่วนข้อมูลการทำงานและการเงินยังคงซิงก์ผ่าน Firestore ตามระบบบัญชีเดิม
