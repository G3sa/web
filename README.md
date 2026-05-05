# Course Management Portal (Next.js)

โปรเจกต์นี้เป็นแอปพลิเคชัน `Next.js` สำหรับระบบจัดการรายวิชา ที่ออกแบบมาสำหรับนักเรียนและอาจารย์
โดยใช้ `localStorage` เก็บข้อมูลเนื้อหาและ Q&A ในฝั่งผู้ใช้ และใช้ MySQL สำหรับเก็บรหัสวิชาพื้นฐาน

## โครงสร้างหลัก

- `web/` - โฟลเดอร์โปรเจกต์ Next.js
- `web/app/` - หน้าและ API routes ของ Next.js
- `web/lib/db.js` - การตั้งค่าการเชื่อมต่อ MySQL
- `web/package.json` - Dependencies และสคริปต์
- `web/README.md` - README เริ่มต้นของ create-next-app
- `web/script.js/` - โฟลเดอร์ว่าง (ไม่มีไฟล์)

## ฟีเจอร์สำคัญ

- หน้า `Home` พร้อมลิงก์สำหรับนักเรียนและอาจารย์
- หน้า `course` เป็นจุดเข้าใช้งานหลัก
  - นักเรียนกรอกรหัสวิชาเพื่อเช็คและเข้าหน้าเนื้อหา
  - อาจารย์กรอกชื่อ, วิชา และรหัสวิชา เพื่อเข้าสู่ระบบหรือสร้างวิชาใหม่
- หน้า `student` view: `/course/[courseId]`
  - แสดงเนื้อหา ทีมงาน และ Q&A
  - อ่านข้อมูลจาก `localStorage` ของเบราว์เซอร์
- หน้า `teacher` editor: `/teacher/[subjectCode]`
  - สร้าง/แก้ไขเนื้อหา
  - อัปโหลดไฟล์หรือเพิ่มลิงก์
  - บันทึกเนื้อหาไปยัง `localStorage`
- API
  - `GET /api/courses/check?code=...` - ตรวจสอบว่ารหัสวิชามีอยู่ในฐานข้อมูลไหม
  - `POST /api/courses` - สร้างรายการวิชาใหม่ในตาราง `classt`

## Route สำคัญ

- `/` - หน้าโฮม
- `/course` - หน้าล็อกอิน/เข้าสู่ระบบสำหรับนักเรียนและอาจารย์
- `/course/[courseId]` - หน้าเนื้อหาสำหรับนักเรียน
- `/teacher/[subjectCode]` - หน้าแก้ไขคอร์สสำหรับอาจารย์
- `/api/courses/check` - ตรวจรหัสวิชาในฐานข้อมูล
- `/api/courses` - สร้างวิชาใหม่ในฐานข้อมูล

## การติดตั้งและรัน

```bash
cd web
npm install
npm run dev
```

เปิดเบราว์เซอร์ที่ `http://localhost:3000`

## สคริปต์ที่ใช้

- `npm run dev` - รัน development server ด้วย Turbopack
- `npm run build` - สร้าง production build
- `npm run start` - รันแอปในโหมด production
- `npm run lint` - ตรวจสอบโค้ดด้วย ESLint

## การตั้งค่าฐานข้อมูล

ไฟล์เชื่อมต่อฐานข้อมูล: `web/lib/db.js`

ตัวอย่างการตั้งค่า:

```js
import mysql from "mysql2/promise";

export const db = mysql.createPool({
  host: "localhost",
  user: "root",
  password: "",
  database: "lmsme",
  waitForConnections: true,
  connectionLimit: 10,
  queueLimit: 0,
});
```

ตารางที่ต้องมีในฐานข้อมูล `lmsme`:

```sql
CREATE TABLE classt (
  id INT AUTO_INCREMENT PRIMARY KEY,
  teacher_name VARCHAR(255) NOT NULL,
  subject_name VARCHAR(255) NOT NULL,
  subject_code VARCHAR(50) NOT NULL,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

## หมายเหตุสำคัญ

- ข้อมูลคอร์สและคอมเมนต์สำหรับนักเรียน/อาจารย์ถูกเก็บใน `localStorage` ของเบราว์เซอร์ ไม่ได้อยู่ในเซิร์ฟเวอร์
- ถ้าใช้งานในเครื่องอื่นหรือคนละเบราว์เซอร์ ข้อมูลจะไม่ซิงค์กัน
- Dependency `firebase` ถูกติดตั้งใน `package.json` แต่ในโค้ดที่สแกนยังไม่พบการใช้งาน

## ข้อมูลเพิ่มเติม

- Next.js: `15.5.0`
- React: `19.1.0`
- MySQL client: `mysql2`

---

README นี้สร้างจากการสแกนโครงสร้างไฟล์ปัจจุบันของโปรเจกต์ในโฟลเดอร์ `web/`.
