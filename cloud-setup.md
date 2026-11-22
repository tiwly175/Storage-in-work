# WorkLife Professional 3.0 — ตั้งค่า Firebase และเผยแพร่ GitHub Pages

เอกสารนี้ทำสำหรับเจ้าของโปรเจกต์เพียงครั้งเดียว ผู้ใช้ทั่วไปไม่ต้องมี Firebase Project ของตัวเอง

## สถาปัตยกรรม
- GitHub Pages: โฮสต์หน้าเว็บแบบ static
- Firebase Authentication: สมัคร/เข้าสู่ระบบด้วยอีเมลและ Google
- Cloud Firestore: เก็บข้อมูลที่ `users/{uid}` โดยผู้ใช้แต่ละคนอ่าน/เขียนได้เฉพาะเอกสาร UID ของตน
- Firebase SDK `onSnapshot`: รับการเปลี่ยนแปลงจากอุปกรณ์อื่นแบบ real-time

## 1. สร้าง Firebase Project
1. ไปที่ https://console.firebase.google.com แล้วสร้างโปรเจกต์
2. เข้า Project settings → General → Your apps → Add app → Web (`</>`)
3. ลงทะเบียน Web App แล้วคัดลอกค่าจาก Firebase config ได้แก่ `apiKey`, `authDomain`, `projectId`, `appId` และ `messagingSenderId`
4. แก้ตัวแปร `CLOUD` ช่วงต้น `index.html` ให้เป็นค่าจริง:

```js
const CLOUD={
  apiKey:'YOUR_API_KEY',
  authDomain:'YOUR_PROJECT_ID.firebaseapp.com',
  projectId:'YOUR_PROJECT_ID',
  appId:'YOUR_APP_ID',
  messagingSenderId:'YOUR_SENDER_ID'
};
```

Firebase Web API key เป็นค่าระบุโปรเจกต์ ไม่ใช่รหัสผ่านหรือ service-account secret แต่ควรจำกัด API key ด้วย HTTP referrers หลังเผยแพร่

## 2. เปิดระบบ Authentication
1. Firebase Console → Build → Authentication → Get started
2. Sign-in method → เปิด Email/Password
3. เปิด Google provider และกำหนด Support email
4. Authentication → Settings → Authorized domains เพิ่มโดเมน GitHub Pages เช่น `YOUR_GITHUB_NAME.github.io`
5. หากใช้โดเมนกำหนดเอง ให้เพิ่มโดเมนนั้นด้วย

## 3. สร้าง Firestore และตั้ง Security Rules
1. Build → Firestore Database → Create database
2. เลือก location ใกล้ไทย เช่น `asia-southeast1` (เลือกแล้วเปลี่ยนภายหลังไม่ได้)
3. เปิดแท็บ Rules แล้ววางไฟล์ `firestore.rules` ในชุดนี้ จากนั้นกด Publish
4. อย่าเปิดสิทธิ์อ่าน/เขียนแบบ public (`if true`) และอย่าใช้ Admin SDK credentials ใน frontend

กฎนี้จำกัดแอปให้ผู้ใช้ที่ล็อกอินอ่าน/เขียนได้เฉพาะ `/users/{uid}` ที่ UID ตรงกับบัญชีของตน

## 4. เผยแพร่ GitHub Pages
อัปโหลดไฟล์เหล่านี้ไว้ใน root ของ repository:
- `index.html`
- `manifest.json`
- `sw.js`
- `icon-192.png`
- `icon-512.png`
- `firestore.rules` (ไฟล์กฎใช้ publish ใน Firebase ไม่ใช่ไฟล์ที่ browser เรียกใช้)

GitHub → Repository → Settings → Pages → Deploy from a branch → เลือก `main` และ `/ (root)` → Save
เมื่อ GitHub แสดง URL แล้ว ให้ตรวจว่า URL อยู่ใน Authorized domains ของ Firebase ด้วย

## 5. ทดสอบก่อนเปิดให้คนทั่วไป
- สมัครบัญชีอีเมลใหม่และล็อกอิน
- ล็อกอินด้วย Google
- เปิด URL เดียวกันในเบราว์เซอร์/มือถืออีกเครื่องด้วยบัญชีเดียวกัน แล้วเพิ่มรายการทดสอบ ตรวจว่าปรากฏอีกเครื่องแบบ real-time
- สมัครบัญชีที่สอง ตรวจว่าไม่เห็นข้อมูลบัญชีแรก
- ลอง sign out แล้ว sign in กลับ ตรวจข้อมูลและการแยก local storage
- ทดสอบลืมรหัสผ่าน และตรวจ Console/Firestore Rules
- ทดสอบ PWA หลังเปลี่ยนเวอร์ชัน Service Worker

## การย้ายข้อมูลเดิม
เมื่อเข้าสู่ระบบครั้งแรก หากพบข้อมูลเดิมในเบราว์เซอร์ แอปจะแสดงตัวเลือกให้นำข้อมูลเดิมเข้าบัญชี หรือเริ่มต้นด้วยข้อมูลว่าง ข้อมูลของแต่ละบัญชีจะถูกเก็บใน local storage key แยกตาม UID

## ความเป็นส่วนตัว — ข้อจำกัดที่ต้องแจ้งผู้ใช้
- ไม่มีหน้า Admin ใน WorkLife สำหรับดูข้อมูลสมาชิก
- Firestore Security Rules ป้องกันผู้ใช้ทั่วไปอ่าน/เขียนข้อมูล UID อื่นผ่านแอป
- อย่างไรก็ตาม เจ้าของ Firebase Project/ผู้มีสิทธิ์ IAM ระดับสูงยังสามารถเข้าถึงข้อมูลใน Firestore ผ่าน Firebase/Google Cloud Console หรือเครื่องมือผู้ดูแลได้ กฎ client-side ไม่สามารถปิดกั้นเจ้าของโปรเจกต์จากการเข้าถึงระดับโครงสร้างพื้นฐานได้
- ข้อมูลใน Firestore เวอร์ชันนี้ยังไม่ได้เข้ารหัสแบบ end-to-end โดยกุญแจอยู่กับผู้ใช้เท่านั้น ดังนั้นห้ามโฆษณาว่าแม้แต่เจ้าของระบบก็อ่านข้อมูลไม่ได้ หากต้องการคุณสมบัตินั้น ต้องออกแบบ client-side encryption และระบบกู้คืนกุญแจแยกต่างหาก
- แจ้งผู้ใช้ให้ทราบว่ามีการจัดเก็บอีเมลและข้อมูลการทำงาน/การเงินบน Firebase ก่อนสมัคร

## ค่าใช้จ่ายและโควตา
Firebase มีแผน Spark ฟรี แต่โควตาและเงื่อนไขเปลี่ยนได้ ควรตรวจหน้า Pricing ปัจจุบันก่อนเปิดใช้งานจริง และตั้ง Budget/Usage alerts ใน Google Cloud หากอัปเกรดเป็นแผนเสียเงิน
