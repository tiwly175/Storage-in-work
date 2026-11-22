# WorkLife Professional 3.0

เว็บแอปบันทึกเวลาทำงาน ค่าแรง/OT และรายรับรายจ่าย พัฒนาต่อจากโค้ด WorkLife เดิม โดยคง data schema หลักและเพิ่มระบบบัญชีแยกผู้ใช้

## ความสามารถในรุ่นนี้
- สมัคร/เข้าสู่ระบบด้วยอีเมลและรหัสผ่าน
- เข้าสู่ระบบด้วย Google ผ่าน Firebase Authentication
- Cloud Firestore แยกเอกสารตาม Firebase UID
- รับข้อมูลข้ามอุปกรณ์ด้วย Firestore `onSnapshot` (real-time)
- แยก Local Storage ตาม UID เพื่อไม่ให้บัญชีอื่นบนอุปกรณ์เดียวกันเห็นข้อมูลที่แคชไว้
- ย้ายข้อมูลเดิม `worklife.v1` เข้าบัญชีโดยให้ผู้ใช้ยืนยันก่อน
- GitHub Pages และ PWA

## ไฟล์ที่ต้องเผยแพร่
`index.html`, `manifest.json`, `sw.js`, `icon-192.png`, `icon-512.png`

## ก่อนเผยแพร่
1. ทำตาม `cloud-setup.md` เพื่อสร้าง Firebase Project
2. เปิด Email/Password และ Google provider
3. เพิ่ม GitHub Pages domain ใน Authorized domains
4. ใส่ Firebase Web App config ใน `const CLOUD` ใน `index.html`
5. Publish `firestore.rules` ใน Firestore Rules
6. ทดสอบบัญชีอีเมล, Google, การแยกข้อมูล และ real-time หลายอุปกรณ์

## ข้อจำกัดความเป็นส่วนตัว
ไม่มีหน้า Admin ในแอป และ Firestore Rules จำกัดผู้ใช้ทั่วไปให้เข้าถึงข้อมูล UID ของตนเท่านั้น แต่เจ้าของ Firebase Project/ผู้มีสิทธิ์ IAM ยังอาจเข้าถึงข้อมูลผ่านเครื่องมือระดับโครงสร้างพื้นฐานได้ รุ่นนี้ยังไม่ใช่ end-to-end encryption
