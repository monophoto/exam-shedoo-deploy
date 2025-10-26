# ExamShedoo

**ExamShedoo** คือแอปพลิเคชันสำหรับช่วยจัดการวันสอบวิชาเลือกเฉพาะของภาควิชาวิศวกรรมคอมพิวเตอร์ มหาวิทยาลัยเชียงใหม่  
ออกแบบมาเพื่อให้อาจารย์และนักศึกษามองเห็นภาพรวมวันสอบของทุกคลาส เพื่อลดการชนกันของตารางสอบ และช่วยเลือกวันที่เหมาะสมที่สุด

ระบบนี้มาพร้อมตัวดึงข้อมูล (Scraper) ที่ทำการนำเข้าข้อมูลรายวิชาและตารางสอบจากเว็บไซต์สำนักทะเบียนอัตโนมัติ

- Frontend repository: https://github.com/Maimeetang/exam-shedoo-frontend.git
- Backend repository: https://github.com/NokiaTh131/exam-shedoo-backend.git

---

## คุณสมบัติของระบบ (Key Features)

- ดูข้อมูลรายวิชา / นักศึกษา / ตารางสอบ ได้จากหน้าเว็บเดียว
- ระบบดึงข้อมูลอัตโนมัติจากเว็บสำนักทะเบียน (Scraper)
- แสดงตารางสอบรวมเพื่อวิเคราะห์วันว่างของวันสอบ

---

## สมาชิกผู้พัฒนา

| ชื่อ                  | รหัสนักศึกษา |
| --------------------- | ------------ |
| นายจักพงษ์ จิณะเสน    | 650610751    |
| นายสิรวิชญ์ กองคำ     | 650610814    |
| นายเจษฎาวุฒิ ปะเพระตา | 650610753    |

---

## Technology Stack

### Frontend

| Tech                  | Description                                 |
| --------------------- | ------------------------------------------- |
| Next.js               | Framework สำหรับ Web Application ฝั่งผู้ใช้ |
| TanStack Query        | จัดการ State ของข้อมูล และ Data Fetching    |
| Form Handling Library | ใช้จัดการฟอร์มและตรวจสอบข้อมูล              |
| Tailwind CSS          | ระบบตกแต่ง UI                               |

### Backend

| Tech        | Description                         |
| ----------- | ----------------------------------- |
| Go (Golang) | ภาษาหลักประมวลผลฝั่ง Server         |
| Fiber       | Web Framework ที่มีประสิทธิภาพสูง   |
| GORM        | ORM ใช้เชื่อมต่อและ query ฐานข้อมูล |

### Other Services

| Tool           | Purpose                          |
| -------------- | -------------------------------- |
| PostgreSQL     | ฐานข้อมูลหลัก                    |
| Redis          | Session storage / caching        |
| Python Scraper | ดึงข้อมูลจากเว็บไซต์สำนักทะเบียน |

---

## การเตรียมข้อมูลเริ่มต้น (Seed Data)

มีข้อมูลตัวอย่างอยู่ในโฟลเดอร์ /datamock ซึ่งสามารถใช้สำหรับทดสอบระบบหรือสาธิตการทำงานได้ โดยสามารถอัปโหลดข้อมูลผ่านหน้า Admin
ในการอัปโหลดข้อมูลการลงทะเบียนนักเรียน (student-registration-168) จำเป็นต้องมีข้อมูลรายวิชานั้นอยู่ในระบบก่อน ดังนั้นให้ทำการดึงข้อมูลคอร์สเข้าระบบก่อนเสมอ ไม่เช่นนั้นไฟล์การลงทะเบียนจะไม่สามารถอัปโหลดได้

### ขั้นตอนการนำไปพัฒนาต่อ

หลังจากที่โคลนโปรเจคไปแล้วสามารถพัฒนาต่อได้โดย step ดังนี้:
ให้ set environemt ของแต่ละ repo สำหรับการพัฒนาก่อนตาม env.example
1. การพัฒนาส่วน **backend** มีสองรูปแบบได้แก่:
   - 1.1. หากต้องการพัฒนาเฉพาะส่วน backend server ทำได้โดยการรันเซ็ตอัพในที่ *exam-shedoo-backend* ไฟล์ดังกล่าวจะสร้าง service ในส่วนของ database และ scraper ให้พร้อมใช้งานได้เลย 
     ```bash
     docker compose -f docker-compose.dev2.yml up -d
     ```
   - 1.2. หากต้องการพัฒนาในส่วน scraper ไปด้วยทำได้โดยการรันเซ็ตอัพในที่ *exam-shedoo-backend* ไฟล์ดังกล่าวจะสร้าง service ในส่วนของ database เท่านั้น (โดยในส่วนการ start backend server นักพัฒนาต้องลงมือทำด้วยตนเอง [รายละเอียด](https://github.com/NokiaTh131/exam-shedoo-backend)) แล้วทำการติดตั้ง library ที่จำเป็นสำหรับทำ scraper service
     ```bash
     pip install -r web-scraper/requirements.txt
     pip install -r web-scraper/exam-scraper/requirements.txt
     ```
     จากนั้นให้ run command ข้างล่าง เพื่อ set up database service.
     ```bash
     docker compose -f docker-compose.dev1.yml up -d
     make watch
     ```
2. การพัฒนาส่วน **frontend** สามารถ set up ระบบดังนี้
   
   ให้ run ในส่วนของ backend ขึ้นมาทั้งหมดก่อนโดยการ run ที่ *exam-shedoo-backend*
     ```bash
     docker compose -f docker-compose.prod.yml up -d
     ```
   ตรง *exam-shedoo-frontend* ให้รัน
     ```bash
     npm install
     npm run dev
     ```
