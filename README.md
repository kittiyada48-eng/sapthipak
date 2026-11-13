# ทรัพย์พิทักษ์ | ระบบจัดการงานเกษตร

เว็บแอปไฟล์เดียว (`index.html`) สำหรับบันทึกทะเบียนชาวไร่ งานรถไถ รถเกี่ยว ปุ๋ย และค่าแรง
เก็บข้อมูลบน Firebase Firestore ซิงก์หลายอุปกรณ์แบบเรียลไทม์

## ไฟล์ในโปรเจกต์

| ไฟล์ | หน้าที่ |
|---|---|
| `index.html` | ตัวแอปทั้งหมด (HTML + CSS + JS) |
| `firestore.rules` | กฎความปลอดภัยของ Firestore (วางในหน้า Firebase Console) |
| `README.md` | ไฟล์นี้ |

## 1. เผยแพร่บน GitHub Pages

1. สร้าง repository ใหม่บน GitHub
2. อัปโหลด `index.html`, `firestore.rules`, `README.md` ไว้ที่ราก (root) ของ repo
3. ไปที่ **Settings → Pages**
4. ใต้ **Build and deployment** เลือก **Source: Deploy from a branch**
5. เลือก Branch `main` และโฟลเดอร์ `/ (root)` แล้วกด **Save**
6. รอ 1–2 นาที จะได้ลิงก์ `https://<ชื่อผู้ใช้>.github.io/<ชื่อ-repo>/`

## 2. ตั้งค่า Firebase (ต้องทำ ไม่เช่นนั้นเว็บจะเชื่อมต่อไม่ได้)

เปิด [Firebase Console](https://console.firebase.google.com) → โปรเจกต์ `sapthipak`

1. **Authentication → Sign-in method** → เปิดใช้ **Anonymous**
2. **Authentication → Settings → Authorized domains** → กด **Add domain** แล้วเพิ่ม `<ชื่อผู้ใช้>.github.io`
3. **Firestore Database** → สร้างฐานข้อมูล (ถ้ายังไม่มี)
4. **Firestore Database → Rules** → วางเนื้อหาจาก `firestore.rules` แล้วกด **Publish**
   - ทดสอบด้วยแท็บ **Rules Playground** ก่อนใช้งานจริง

## 3. หมายเหตุด้านความปลอดภัย

- `firebaseConfig` (รวมถึง `apiKey`) ในไฟล์เป็นค่าสาธารณะของแอปเว็บ Firebase การมีอยู่ใน repo สาธารณะเป็นเรื่องปกติ
  สิ่งที่ปกป้องข้อมูลจริงคือ **Firestore Rules** และ **Authorized domains** จึงต้องตั้งสองส่วนนี้ให้เรียบร้อย
- เพิ่มความปลอดภัยได้อีกขั้นโดยจำกัด API key ให้ใช้ได้เฉพาะโดเมนของคุณ:
  Google Cloud Console → APIs & Services → Credentials → เลือก Browser key → **Website restrictions**
  แล้วเพิ่ม `https://<ชื่อผู้ใช้>.github.io/*`
- หน้าเว็บตั้ง `noindex` ไว้เพื่อไม่ให้ Google แสดงในผลการค้นหา แต่ไม่ได้ทำให้เป็นส่วนตัว
  ใครมีลิงก์ก็เปิดหน้าแรกได้ ส่วนข้อมูลจะเข้าถึงได้เฉพาะผู้ที่มีรหัสกิจการและรหัสเชิญ
- อย่าแชร์รหัสกิจการและรหัสเชิญในที่สาธารณะ

## 4. ข้อจำกัดที่ควรรู้

- ปุ่ม **นำเข้าข้อมูล JSON** ในหน้ารายงานจะนำเข้าเฉพาะในหน้าจอเครื่องนั้น ไม่ได้เขียนกลับไปยัง Firestore
  ข้อมูลจึงอาจถูกแทนที่เมื่อระบบซิงก์ครั้งถัดไป ใช้ **ดาวน์โหลดข้อมูลสำรอง** สำหรับสำรองข้อมูลได้ตามปกติ
- ถ้าล้างข้อมูลเบราว์เซอร์หรือเปลี่ยนเครื่อง ตัวตนแบบ Anonymous จะเปลี่ยน ต้องขอรหัสเชิญจากเจ้าของใหม่
