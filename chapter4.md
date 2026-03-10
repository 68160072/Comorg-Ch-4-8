# 📘 บทที่ 4 : Cache Memory (หน่วยความจำแคช)

---

# สารบัญของบท

## 4.1 ภาพรวมของระบบหน่วยความจำคอมพิวเตอร์ (Computer Memory System Overview)
- ลักษณะของระบบหน่วยความจำ (Characteristics of Memory Systems)
- ลำดับชั้นของหน่วยความจำ (The Memory Hierarchy)

## 4.2 หลักการของ Cache Memory (Cache Memory Principles)

## 4.3 องค์ประกอบของการออกแบบ Cache (Elements of Cache Design)
- ที่อยู่ของ Cache (Cache Addresses)
- ขนาดของ Cache (Cache Size)
- ฟังก์ชันการแมป (Mapping Function)
- อัลกอริทึมการแทนที่ข้อมูล (Replacement Algorithms)
- นโยบายการเขียนข้อมูล (Write Policy)
- ขนาดของบรรทัดข้อมูล (Line Size)
- จำนวนของ Cache (Number of Caches)

## 4.4 โครงสร้าง Cache ของ Pentium 4 (Pentium 4 Cache Organization)

## 4.5 คำศัพท์สำคัญ คำถามทบทวน และแบบฝึกหัด  
(Key Terms, Review Questions, and Problems)

## ภาคผนวก 4A : ลักษณะประสิทธิภาพของหน่วยความจำสองระดับ  
(Performance Characteristics of Two-Level Memories)

- Locality
- Operation of Two-Level Memory
- Performance

---

# 🎯 วัตถุประสงค์การเรียนรู้ (Learning Objectives)

หลังจากศึกษาบทนี้แล้ว คุณควรจะสามารถ

- อธิบายภาพรวมของลักษณะสำคัญของระบบหน่วยความจำคอมพิวเตอร์และการใช้ลำดับชั้นของหน่วยความจำ
- อธิบายแนวคิดพื้นฐานและวัตถุประสงค์ของหน่วยความจำ Cache
- อธิบายองค์ประกอบสำคัญของการออกแบบ Cache
- แยกความแตกต่างระหว่าง  
  - Direct Mapping  
  - Associative Mapping  
  - Set-Associative Mapping
- อธิบายเหตุผลในการใช้ Cache หลายระดับ
- เข้าใจผลกระทบต่อประสิทธิภาพของระบบจากการใช้หน่วยความจำหลายระดับ

---

# 📖 บทนำ (Introduction)

แม้ว่าแนวคิดของหน่วยความจำคอมพิวเตอร์จะดูเหมือนเรียบง่าย แต่ในความเป็นจริงแล้ว หน่วยความจำเป็นส่วนประกอบของระบบคอมพิวเตอร์ที่มีความหลากหลายมากที่สุด ทั้งในด้าน

- ประเภท
- เทคโนโลยี
- โครงสร้าง
- ประสิทธิภาพ
- ต้นทุน

ไม่มีเทคโนโลยีใดเพียงอย่างเดียวที่สามารถตอบสนองความต้องการของหน่วยความจำของระบบคอมพิวเตอร์ได้อย่างสมบูรณ์

ดังนั้น ระบบคอมพิวเตอร์ทั่วไปจึงถูกออกแบบให้มี **ลำดับชั้นของหน่วยความจำ (Memory Hierarchy)** ซึ่งประกอบด้วยหน่วยความจำหลายระดับ โดยแบ่งเป็น

- หน่วยความจำภายใน (Internal Memory)  
  โปรเซสเซอร์สามารถเข้าถึงได้โดยตรง

- หน่วยความจำภายนอก (External Memory)  
  โปรเซสเซอร์เข้าถึงผ่านโมดูล I/O

บทนี้และบทถัดไปจะเน้นการศึกษาเกี่ยวกับ **หน่วยความจำภายใน** ในขณะที่ **บทที่ 6** จะกล่าวถึง **หน่วยความจำภายนอก**

ในส่วนแรกของบทนี้จะอธิบายลักษณะสำคัญของหน่วยความจำคอมพิวเตอร์ และในส่วนที่เหลือของบทจะกล่าวถึงองค์ประกอบที่สำคัญของระบบคอมพิวเตอร์สมัยใหม่ นั่นคือ **Cache Memory**

---

# 4.1 ภาพรวมของระบบหน่วยความจำคอมพิวเตอร์

## ลักษณะของระบบหน่วยความจำ (Characteristics of Memory Systems)

หัวข้อเกี่ยวกับหน่วยความจำคอมพิวเตอร์มีความซับซ้อน แต่สามารถทำให้เข้าใจได้ง่ายขึ้นโดยการจำแนกระบบหน่วยความจำตาม **ลักษณะสำคัญ (Key Characteristics)** ของมัน

ลักษณะที่สำคัญที่สุดของระบบหน่วยความจำถูกแสดงไว้ใน **ตาราง 4.1**

คำว่า **Location (ตำแหน่ง)** ในตาราง 4.1 หมายถึง หน่วยความจำนั้นอยู่

- ภายในคอมพิวเตอร์ (Internal Memory)
- ภายนอกคอมพิวเตอร์ (External Memory)

โดยทั่วไปแล้ว **Internal Memory** มักถูกมองว่าเป็น **Main Memory** แต่จริง ๆ แล้วหน่วยความจำภายในยังมีประเภทอื่นอีก

โปรเซสเซอร์ต้องมีหน่วยความจำเฉพาะของตัวเองในรูปแบบของ

**Registers**

นอกจากนี้ ส่วนของ **Control Unit** ภายในโปรเซสเซอร์ก็อาจต้องใช้หน่วยความจำภายในของตัวเองด้วย

การกล่าวถึงหน่วยความจำภายในสองประเภทนี้จะถูกอธิบายในบทถัดไป

**Cache Memory** ก็เป็นอีกประเภทหนึ่งของหน่วยความจำภายใน

ส่วน **External Memory** คืออุปกรณ์จัดเก็บข้อมูลภายนอก เช่น

- Disk
- Tape

ซึ่งโปรเซสเซอร์สามารถเข้าถึงได้ผ่าน **I/O Controllers**

---

## ความจุของหน่วยความจำ (Memory Capacity)

ลักษณะที่สำคัญอย่างหนึ่งของหน่วยความจำคือ **ความจุ (Capacity)**

สำหรับ **Internal Memory** ความจุมักแสดงในรูปของ
---

# Table 4.1 ลักษณะสำคัญของระบบหน่วยความจำคอมพิวเตอร์  
(Key Characteristics of Computer Memory Systems)

| Category | Description |
|---|---|
| **Location** | Internal (เช่น processor registers, cache, main memory) <br> External (เช่น optical disks, magnetic disks, tapes) |
| **Capacity** | Number of words <br> Number of bytes |
| **Unit of Transfer** | Word <br> Block |
| **Access Method** | Sequential <br> Direct <br> Random <br> Associative |
| **Performance** | Access time <br> Cycle time <br> Transfer rate |
| **Physical Type** | Semiconductor <br> Magnetic <br> Optical <br> Magneto-optical |
| **Physical Characteristics** | Volatile / Nonvolatile <br> Erasable / Nonerasable |
| **Organization** | Memory modules |

---

# แนวคิดเกี่ยวกับ Unit of Transfer

แนวคิดที่เกี่ยวข้องคือ **หน่วยของการถ่ายโอนข้อมูล (Unit of Transfer)**

สำหรับ **หน่วยความจำภายใน (Internal Memory)**  
หน่วยของการถ่ายโอนข้อมูลจะเท่ากับจำนวน **สายสัญญาณไฟฟ้า (electrical lines)** ที่ใช้รับและส่งข้อมูลเข้าออกจาก **Memory Module**

โดยปกติแล้วค่าดังกล่าวจะเท่ากับ **Word Length**  
แต่ในบางระบบอาจแตกต่างกัน เช่น

ตัวอย่างเช่น **Cray C90 Supercomputer**  
ใช้ความยาว **64-bit word** แต่ใช้ **64-bit integer representation**

ในสถาปัตยกรรม **Intel x86**  
มีความยาวคำสั่ง (instruction) หลายแบบ และ word size เท่ากับ **32 bits**

---

# แนวคิดที่เกี่ยวข้องกับหน่วยความจำภายใน

## Word

คำว่า **Word** หมายถึงหน่วยพื้นฐานของการจัดโครงสร้างหน่วยความจำ

ขนาดของ Word มักจะเท่ากับ

- จำนวนบิตที่ใช้แทนค่า **integer**
- หรือขนาดของ **instruction**

อย่างไรก็ตาม ในหลายระบบอาจมีข้อยกเว้น

ตัวอย่างเช่น

- **Cray C90 Supercomputer**  
  ใช้ integer representation 64 บิต

- **Intel x86 architecture**  
  ใช้ word ขนาด **32 bits**

---

## Addressable Units

ในระบบส่วนใหญ่ **Address** จะระบุหน่วยข้อมูลเป็น **Byte**

ในกรณีนี้ความสัมพันธ์ระหว่าง

- ความยาวของ Address = **A**
- จำนวนหน่วยที่สามารถอ้างถึงได้ = **N**

จะเป็น


- Bytes  
- Words

โดยที่

---

## Unit of Transfer

สำหรับ **Internal Memory**

จำนวนบิตที่อ่านหรือเขียนในแต่ละครั้งจะเท่ากับ

- Word
- หรือ Addressable Unit

สำหรับ **External Memory**

ข้อมูลจะถูกส่งเป็นหน่วยที่ใหญ่กว่า Word  
ซึ่งเรียกว่า

**Blocks**

---

# วิธีการเข้าถึงข้อมูลในหน่วยความจำ  
(Method of Accessing Units of Data)

อีกหนึ่งลักษณะที่สำคัญของหน่วยความจำคือ  
**วิธีการเข้าถึงข้อมูล**

มีหลายรูปแบบ ได้แก่

- Sequential
- Direct
- Random
- Associative

---

## Sequential Access

หน่วยความจำถูกจัดเป็นหน่วยข้อมูลที่เรียกว่า **Records**

การเข้าถึงข้อมูลจะทำตามลำดับเชิงเส้น

เพื่ออ่านหรือเขียน **Record ที่ n**

ระบบต้อง

1. เริ่มจากตำแหน่งปัจจุบัน
2. เลื่อนไปตามลำดับ
3. จนถึง Record ที่ต้องการ

ดังนั้น

**เวลาในการเข้าถึงข้อมูลจึงแปรผันตามตำแหน่งของข้อมูล**

ตัวอย่างอุปกรณ์ที่ใช้ Sequential Access

- **Magnetic Tape**

---

## Direct Access

คล้ายกับ Sequential Access เพราะใช้

**read-write mechanism ร่วมกัน**

แต่แตกต่างตรงที่

แต่ละ **Block หรือ Record**

จะมี **Address เฉพาะ**

ทำให้สามารถเข้าถึงตำแหน่งข้อมูลได้โดยตรงมากขึ้น
แม้ว่าบางครั้งอาจยังต้องมีการเลื่อนตำแหน่งของอุปกรณ์ก่อนอ่านข้อมูล
---

# Random Access

แต่ละตำแหน่งที่สามารถอ้างถึงได้ในหน่วยความจำจะมี **กลไกการอ่าน/เขียน (read-write mechanism)** ที่สามารถเข้าถึงได้โดยตรง

เวลาในการเข้าถึงตำแหน่งใดตำแหน่งหนึ่ง

- ไม่ขึ้นกับลำดับของการเข้าถึงก่อนหน้า
- มีค่า **คงที่ (constant)**

ดังนั้นตำแหน่งใด ๆ ในหน่วยความจำสามารถถูกเลือกแบบสุ่ม และเข้าถึงได้โดยตรง

ตัวอย่างหน่วยความจำประเภทนี้

- **Main Memory**
- **Some Cache Systems**

---

# Associative Access

เป็นรูปแบบการเข้าถึงหน่วยความจำที่หายากกว่า ซึ่งทำให้สามารถ

- เปรียบเทียบ **บิตที่ต้องการค้นหา**
- กับ **ตำแหน่งบิตจำนวนมาก**

ได้ **พร้อมกัน**

คำจะถูกดึงออกจากหน่วยความจำโดย **พิจารณาจากเนื้อหาบางส่วนของข้อมูล** มากกว่าที่อยู่ของข้อมูล

เหมือนกับ **Random Access Memory**

- แต่ละตำแหน่งจะมี **กลไก address ของตัวเอง**
- เวลาในการดึงข้อมูลจะ **คงที่**

**Cache Memory บางระบบใช้ Associative Access**

---

# Characteristics of Memory Performance

จากมุมมองของผู้ใช้  
ลักษณะที่สำคัญที่สุดของหน่วยความจำมีสองอย่างคือ

1. **Capacity (ความจุ)**
2. **Performance (ประสิทธิภาพ)**

---

## Access Time (Latency)

สำหรับ **Random Access Memory**

Access Time คือ

> เวลาที่ใช้ในการทำการอ่านหรือเขียนข้อมูล

เริ่มตั้งแต่

- เวลาที่ **address ถูกส่งไปยังหน่วยความจำ**

จนถึง

- เวลาที่ **ข้อมูลพร้อมใช้งาน**

สำหรับ **Non-Random Access Memory**

Access Time คือ

> เวลาที่ใช้ในการเลื่อนตำแหน่งของกลไกอ่าน/เขียนไปยังตำแหน่งข้อมูลที่ต้องการ

---

## Memory Cycle Time

ค่าคงที่นี้ใช้กับ **Random Access Memory**

ประกอบด้วย

- Access Time
- เวลาที่ต้องใช้ก่อนที่จะสามารถเริ่มการเข้าถึงครั้งถัดไป

เวลาส่วนนี้อาจจำเป็นสำหรับ

- การเปลี่ยนสถานะของสัญญาณ
- การทำให้ bus หรือหน่วยความจำกลับสู่สภาพเดิม

---

## Transfer Rate

คือ **อัตราความเร็วในการส่งข้อมูลเข้า/ออกจากหน่วยความจำ**

สำหรับ **Random Access Memory**

สำหรับ **Non-Random Access Memory**

มีความสัมพันธ์ดังนี้

### โดยที่

| Symbol | Meaning |
|------|------|
| Tn | เวลาเฉลี่ยในการอ่านหรือเขียน n บิต |
| TA | เวลาเฉลี่ยในการเข้าถึงข้อมูล |
| n | จำนวนบิต |
| R | อัตราการถ่ายโอนข้อมูล (bits per second) |

---

# Physical Types of Memory

มีเทคโนโลยีหลายประเภทที่ใช้สร้างหน่วยความจำ

ประเภทที่พบมากที่สุดคือ

- **Semiconductor Memory**
- **Magnetic Surface Memory** (ใช้ใน disk และ tape)
- **Optical Memory**
- **Magneto-Optical Memory**

---

# Physical Characteristics of Data Storage

ลักษณะทางกายภาพของการจัดเก็บข้อมูลมีความสำคัญ เช่น

## Volatile Memory

ข้อมูลจะ **หายไปเมื่อไฟฟ้าถูกปิด**

---

## Nonvolatile Memory

ข้อมูลยังคงอยู่แม้ **ไม่มีไฟฟ้า**

ตัวอย่าง

- Magnetic disk
- Magnetic tape

---

## Semiconductor Memory

หน่วยความจำเซมิคอนดักเตอร์อาจเป็น

- **Volatile**
- **Nonvolatile**

---

## Nonerasable Memory

หน่วยความจำประเภทนี้

- ไม่สามารถลบข้อมูลได้
- ยกเว้นการ **ทำลายตัวหน่วยความจำ**

หน่วยความจำเซมิคอนดักเตอร์ประเภทนี้เรียกว่า

**ROM (Read-Only Memory)**

ในทางปฏิบัติจะใช้ **ROM ที่สามารถโปรแกรมได้**

---

# Organization of Memory

สำหรับ **Random Access Memory**

คำว่า **Organization** หมายถึง

> การจัดเรียงบิตทางกายภาพเพื่อสร้างเป็น Word

อย่างไรก็ตามการจัดเรียงที่ชัดเจนนี้ **ไม่ได้ถูกใช้เสมอไป**

รายละเอียดเพิ่มเติมจะกล่าวใน **Chapter 5**

---

# The Memory Hierarchy

ข้อจำกัดของการออกแบบหน่วยความจำของคอมพิวเตอร์สามารถสรุปได้ด้วยคำถามสามข้อ

1. **How much?** (ต้องการความจุเท่าไร)
2. **How fast?** (ต้องการความเร็วเท่าไร)
3. **How expensive?** (ต้นทุนเท่าไร)

---

## Capacity Question

คำถามเรื่องความจุค่อนข้างเปิดกว้าง

ถ้ามีความจุมากพอ  
ก็จะมีการพัฒนาแอปพลิเคชันให้ใช้ความจุนั้น

---

## Speed Question

ความเร็วมีความสำคัญมาก

ถ้า **processor ทำงานเร็วมาก**  
แต่ **memory ช้า**

processor จะต้อง **รอข้อมูล**

---

## Cost Question

ต้นทุนของหน่วยความจำต้องมีความสมเหตุสมผล  
เมื่อเทียบกับองค์ประกอบอื่นของระบบ

---

# Trade-off of Memory Design

มี **trade-off** ระหว่าง

- Access time
- Cost
- Capacity

เทคโนโลยีหน่วยความจำจึงมีลักษณะดังนี้

- Access time เร็ว → cost ต่อบิตสูง
- Capacity สูง → cost ต่อบิตต่ำ
- Capacity สูง → access time ช้า

---

# Problem of Memory Design

นักออกแบบต้องการ

- หน่วยความจำ **ความจุสูง**
- **ราคาถูก**
- **เร็ว**

แต่สิ่งเหล่านี้ **ขัดแย้งกัน**

ดังนั้นจึงใช้แนวคิด

# Memory Hierarchy

แทนที่จะใช้หน่วยความจำชนิดเดียว

---

# แนวคิดของ Memory Hierarchy

เมื่อไล่ลงตามลำดับชั้นของหน่วยความจำ

จะเกิดสิ่งต่อไปนี้

a. **Cost per bit ลดลง**  
b. **Capacity เพิ่มขึ้น**  
c. **Access time เพิ่มขึ้น**  
d. **Frequency of access ลดลง**

---

# Structure of Memory Hierarchy

หน่วยความจำในระบบคอมพิวเตอร์แบ่งเป็นชั้นต่าง ๆ เช่น

### Inboard Memory
- Registers
- Cache
- Main Memory

### Outboard Storage
- Magnetic Disk
- CD-ROM
- CD-RW
- DVD-RW
- DVD-RAM
- Blu-Ray

### Offline Storage
- Magnetic Tape

---

# Figure 4.1 Memory Hierarchy

หน่วยความจำที่

- **เล็กกว่า**
- **แพงกว่า**
- **เร็วกว่า**

จะถูกเสริมด้วยหน่วยความจำที่

- **ใหญ่กว่า**
- **ถูกกว่า**
- **ช้ากว่า**

---

# Locality of Reference

แนวคิดสำคัญที่ทำให้ **Memory Hierarchy ทำงานได้**

เรียกว่า

**Locality of Reference**

ระหว่างการทำงานของโปรแกรม

การอ้างอิงหน่วยความจำมักเกิดขึ้นเป็น

- กลุ่มของ instruction
- กลุ่มของ data

---

## ตัวอย่าง

โปรแกรมมักมี

- loops
- subroutines

ซึ่งทำให้เกิด

- การอ้างถึง instruction เดิมซ้ำ
- การเข้าถึง array บางตำแหน่งซ้ำ

ดังนั้น

ในช่วงเวลาสั้น ๆ

processor มักทำงานกับ **cluster ของข้อมูลเดิม**

---

# Example 4.1

สมมติว่า processor มีหน่วยความจำสองระดับ

## Level 1
- 1000 words
- Access time = **0.01 μs**

## Level 2
- 100,000 words
- Access time = **0.1 μs**

---

เมื่อ processor ต้องการอ่าน word

1. จะตรวจสอบ **Level 1 ก่อน**
2. ถ้าไม่พบ จะไปอ่านที่ **Level 2**

---

ให้ H = hit ratio

หมายถึง

> สัดส่วนของการเข้าถึงที่พบข้อมูลใน Level 1

---

ค่า **Average Access Time**

คำนวณได้จาก Average Time = H × T1 + (1 − H) × (T1 + T2)

---

## Example Calculation

สมมติว่า95% ของการเข้าถึงพบข้อมูลใน Level 1

ดังนั้น
(0.95)(0.01 μs) + (0.05)(0.01 μs + 0.1 μs)

= 0.0095 + 0.0055
= 0.015 μs

---

ค่าเฉลี่ย **ใกล้กับ 0.01 μs มากกว่า 0.1 μs**

ซึ่งเป็นผลที่ต้องการ

---

# Hit และ Miss

**Hit**

คือกรณีที่

> พบข้อมูลในหน่วยความจำระดับที่เร็วกว่า

**Miss**

คือกรณีที่

> ไม่พบข้อมูลในหน่วยความจำระดับที่เร็วกว่า
---

# Memory Hierarchy (continued)

หน่วยความจำที่อยู่ใกล้กับ **processor** มากที่สุดจะมีความเร็วสูงที่สุด  
ตัวอย่างเช่น

**processor registers**

processor ส่วนใหญ่จะมี register หลายตัว  
บางระบบอาจมี **หลายร้อย register**

---

## Main Memory

หน่วยความจำหลัก (Main Memory) เป็นหน่วยความจำหลักของระบบคอมพิวเตอร์

คุณสมบัติสำคัญ

- แต่ละตำแหน่งมี **address เฉพาะ**
- ปกติจะใช้ **semiconductor technology**
- เก็บทั้ง  
  - program instructions  
  - data

Cache memory อยู่ **ระหว่าง processor และ main memory**  
เพื่อเพิ่มประสิทธิภาพการเข้าถึงข้อมูล

---

## External Memory

หน่วยความจำภายนอกเป็น **secondary storage**

ตัวอย่าง

- Disk
- Removable disk
- Magnetic tape
- Optical disk

ข้อมูลใน external memory จะถูกเข้าถึงผ่าน **I/O modules**

---

## Expanded Storage

ระบบบางระบบมีหน่วยความจำเพิ่มที่เรียกว่า **expanded storage**

ตัวอย่าง

**IBM mainframe**

ใช้หน่วยความจำประเภทหนึ่งที่มี

- ความเร็วช้ากว่า main memory
- ราคาถูกกว่า main memory

จึงถูกจัดเป็นระดับหนึ่งของ **memory hierarchy**

---

## External Storage as Backup

ข้อมูลสามารถถูกย้ายระหว่าง

- main memory
- expanded storage

เพื่อเพิ่มประสิทธิภาพของระบบ

ข้อมูลที่ถูกใช้น้อยสามารถย้ายไปเก็บไว้ในระดับที่ช้ากว่า

---

## Disk Cache

ส่วนหนึ่งของ **main memory** สามารถใช้เป็น

disk cache ช่วยเพิ่มประสิทธิภาพได้สองวิธี

### 1. Disk Writes

แทนที่จะเขียนข้อมูลขนาดเล็กหลายครั้ง

ระบบจะ

- รวมข้อมูล
- เขียนเป็น **block ใหญ่**

ทำให้

- disk ทำงานมีประสิทธิภาพมากขึ้น
- ลดจำนวนการเข้าถึง disk

---

### 2. Disk Reads

ข้อมูลบางส่วนที่ถูกอ่านจาก disk

อาจถูกใช้งานอีกครั้งโดยโปรแกรม

ดังนั้นข้อมูลเหล่านั้นจะถูกเก็บไว้ใน **cache**

เพื่อให้

- โปรแกรมสามารถเข้าถึงข้อมูลจาก cache
- แทนการอ่านจาก disk ที่ช้ากว่า

---

# Cache Memory

**Cache memory** ถูกออกแบบมาเพื่อรวมข้อดีของหน่วยความจำสองประเภท

- หน่วยความจำเร็ว ราคาแพง
- หน่วยความจำช้า ราคาถูก

แนวคิดคือ ใช้หน่วยความจำขนาดเล็กแต่เร็ว
ร่วมกับหน่วยความจำขนาดใหญ่แต่ช้า

---

## หลักการทำงานของ Cache

เมื่อ processor ต้องการอ่านข้อมูล

1. processor จะตรวจสอบข้อมูลใน **cache**
2. ถ้าพบข้อมูล → ส่งข้อมูลให้ processor
3. ถ้าไม่พบ → ดึงข้อมูลจาก **main memory**

จากนั้น

- block ของข้อมูลจะถูกคัดลอกเข้า cache
- เพื่อรองรับการเข้าถึงในอนาคต

---

## Locality of Reference

เนื่องจากหลักการ
Locality of Reference

ถ้า block หนึ่งถูกเข้าถึง

มีโอกาสสูงที่

- ตำแหน่งเดิมจะถูกใช้อีก
- หรือข้อมูลใกล้เคียงจะถูกใช้งาน

---

# Multiple Levels of Cache

ระบบคอมพิวเตอร์สมัยใหม่มักมี cache มากกว่า 1 ระดับ

ตัวอย่าง

- **L1 cache**
- **L2 cache**
- **L3 cache**

ลักษณะทั่วไป

| Cache | Speed | Size |
|------|------|------|
| L1 | เร็วที่สุด | เล็กที่สุด |
| L2 | เร็ว | ใหญ่กว่า L1 |
| L3 | ช้ากว่า | ใหญ่กว่า |

---

# Structure of Cache Memory

Cache ประกอบด้วย
2^n addressable words

แต่ละ word มี
n-bit address

---

## Cache Organization

เพื่อให้การจัดการง่ายขึ้น

main memory จะถูกแบ่งเป็น blocks

แต่ละ block มี K words
ดังนั้น


จำนวน blocks ใน main memory

= 2^n / K

---

## Cache Lines

Cache เองก็ถูกแบ่งเป็น cache lines
แต่ละ cache line มี

---

# Figure 4.3 Cache and Main Memory

## (a) Single Cache

โครงสร้างพื้นฐาน
CPU → Cache → Main Memory

ความเร็ว
CPU ↔ Cache = Fast
Cache ↔ Memory = Slow

---

## (b) Three-Level Cache Organization

โครงสร้าง
CPU
↓
L1 Cache (Fastest)
↓
L2 Cache (Fast)
↓
L3 Cache (Less fast)
↓
Main Memory (Slow)


---

# Note about Cache Line

คำว่า
line
ถูกใช้แทนคำว่า block

ใน cache

เหตุผลคือ

1. main memory block อาจมีจำนวน word แตกต่างจาก cache line
2. cache line มีข้อมูลมากกว่า data word เช่น

- tag bits
- control bits
# 📘 Cache Memory Structure และการทำงานของ Cache

เอกสารนี้เป็นการอธิบายโครงสร้างและการทำงานของ **Cache** ที่ใช้ร่วมกับ **Main Memory** ในระบบคอมพิวเตอร์ รวมถึงองค์ประกอบของการออกแบบ **Cache** และแนวคิดเกี่ยวกับ **Virtual Memory**

---

# Figure 4.4 โครงสร้าง Cache และ Main Memory

## โครงสร้างของ Cache

ในระบบ Cache หน่วยความจำ Cache จะประกอบด้วยหน่วยย่อยที่เรียกว่า **Line** โดยมีจำนวนทั้งหมด **C Lines**

แต่ละ **Cache Line** จะประกอบด้วยองค์ประกอบสำคัญดังนี้

* **Tag**
* **Block**
* **Control bits** (ไม่ได้แสดงในรูป)

ภายใน **Line** จะเก็บข้อมูลที่เรียกว่า **Block** ซึ่งมีขนาดเท่ากับ **K words**

ดังนั้น

* Cache จะมีจำนวน **C Lines**
* แต่ละ Line เก็บ **Block ขนาด K words**

นอกจากนี้ในแต่ละ **Line** จะมี **Tag** ซึ่งเป็นข้อมูลที่ใช้ระบุว่า Block ที่เก็บอยู่ใน Cache มาจากตำแหน่งใดของ **Main Memory**

---

## โครงสร้างของ Main Memory

**Main Memory** ถูกแบ่งออกเป็นส่วนย่อยที่เรียกว่า **Block**

จำนวนทั้งหมด **M Blocks**

โดย

* แต่ละ **Block** มีขนาด **K words**
* ดังนั้นขนาดของ Block ใน **Main Memory** จะเท่ากับขนาดของ Block ใน **Cache**

ตำแหน่งของหน่วยความจำใน **Main Memory** จะถูกระบุด้วย **Memory Address**

ช่วงของ Address จะอยู่ในช่วง

0 ถึง 2ⁿ − 1

โดยที่

* n คือจำนวนบิตของ Address
* แต่ละ Address จะชี้ไปยังตำแหน่งของ **Word**

---

## ความสัมพันธ์ระหว่าง Cache และ Main Memory

โดยทั่วไปแล้ว

จำนวน **Blocks ใน Main Memory** จะมีมากกว่าจำนวน **Lines ใน Cache** อย่างมาก

ดังนั้น

```
M >> C
```

ซึ่งหมายความว่า

**Cache ไม่สามารถเก็บ Block จาก Main Memory ได้ทั้งหมดในเวลาเดียวกัน**

ในช่วงเวลาใดเวลาหนึ่ง จะมีเพียง **บางส่วนของ Blocks ใน Main Memory** เท่านั้นที่ถูกโหลดเข้ามาอยู่ใน **Cache**

เมื่อมีการอ่านข้อมูลจาก **Main Memory**

Block ที่มีข้อมูลนั้นจะถูกนำเข้ามาเก็บไว้ใน **Cache Line**

เนื่องจากจำนวน **Block** มีมากกว่าจำนวน **Line**

ดังนั้น **Cache Line หนึ่งไม่สามารถถูกกำหนดให้เก็บ Block เดิมตลอดเวลาได้**

เพราะฉะนั้นจึงต้องมี **Tag** ในแต่ละ **Line**

เพื่อใช้ระบุว่า

**Block ใดของ Main Memory กำลังถูกเก็บอยู่ใน Cache Line นั้น**

โดยทั่วไปแล้ว **Tag** จะเป็น **ส่วนหนึ่งของ Main Memory Address**

รายละเอียดของการแบ่ง Address เพื่อใช้สร้าง Tag จะถูกอธิบายในภายหลัง

---

# Figure 4.5 การทำงานของ Cache ในการอ่านข้อมูล (Cache Read Operation)

รูปนี้อธิบายขั้นตอนของการอ่านข้อมูลจากระบบหน่วยความจำที่มี **Cache**

กระบวนการเริ่มต้นเมื่อ **Processor หรือ CPU** ต้องการอ่านข้อมูลจากหน่วยความจำ

ขั้นตอนการทำงานมีดังนี้

## ขั้นตอนที่ 1

ระบบเริ่มต้นการทำงาน

```
START
```

จากนั้น **CPU จะส่ง Address ที่ต้องการอ่าน**

Address นี้เรียกว่า

**Read Address (RA)**

---

## ขั้นตอนที่ 2

ระบบ **Cache Controller** จะตรวจสอบว่า

**Block ที่มี Address RA อยู่ใน Cache หรือไม่**

กระบวนการนี้เรียกว่า

**Cache Lookup**

---

# กรณีที่ 1 : Cache Hit

ถ้า **Block ที่มี RA อยู่ใน Cache**

เหตุการณ์นี้เรียกว่า

**Cache Hit**

ขั้นตอนการทำงานคือ

1. Cache จะค้นหา **Word ที่ตำแหน่ง RA**
2. อ่านข้อมูลจาก **Cache**
3. ส่งข้อมูลนั้นกลับไปให้ **CPU**

หลังจากนั้นกระบวนการจะสิ้นสุด

```
DONE
```

การเกิด **Cache Hit** จะทำให้ระบบทำงานได้เร็วมาก เพราะไม่ต้องเข้าถึง **Main Memory**

---

# กรณีที่ 2 : Cache Miss

ถ้า **Block ที่มี RA ไม่อยู่ใน Cache**

เหตุการณ์นี้เรียกว่า

**Cache Miss**

ในกรณีนี้ระบบต้องเข้าถึง **Main Memory**

ขั้นตอนมีดังนี้

1. ระบบจะเข้าถึง **Main Memory**
2. อ่าน **Block ที่มี Address RA**
3. จัดสรร **Cache Line** สำหรับเก็บ Block นี้
4. โหลด **Block จาก Main Memory เข้า Cache Line**
5. จากนั้นอ่าน **Word ที่ตำแหน่ง RA**
6. ส่งข้อมูลให้ **CPU**

หลังจากนั้นกระบวนการจะสิ้นสุด

```
DONE
```

---

## การทำงานแบบขนาน (Parallel Operation)

ในบางระบบ

ขั้นตอนต่อไปนี้อาจทำงานพร้อมกัน

* การโหลด Block เข้า Cache
* การส่ง Word ไปให้ CPU

โครงสร้างนี้ถูกออกแบบมาเพื่อเพิ่มประสิทธิภาพของระบบหน่วยความจำ

---

# Figure 4.6 โครงสร้าง Cache ทั่วไป (Typical Cache Organization)

รูปนี้แสดงโครงสร้างการเชื่อมต่อของ **Processor, Cache และ Main Memory**

องค์ประกอบหลักของระบบประกอบด้วย

## Processor

**Processor หรือ CPU** เป็นหน่วยที่ทำหน้าที่ประมวลผลคำสั่งของโปรแกรม

CPU จะส่ง

* Address
* Control signals

ไปยัง **Cache**

และรับ

* Data

กลับจาก **Cache**

---

## Cache

**Cache** เป็นหน่วยความจำความเร็วสูงที่อยู่ระหว่าง **CPU และ Main Memory**

หน้าที่ของ Cache คือ

ลดเวลาที่ CPU ต้องรอการเข้าถึงข้อมูลจาก **Main Memory**

---

## สัญญาณที่ใช้ในการสื่อสาร

ระหว่าง **Processor และ Cache** จะมีสัญญาณดังนี้

* **Address lines**
* **Data lines**
* **Control lines**

---

## Buffer

ก่อนที่ข้อมูลจะถูกส่งผ่าน **System Bus**

จะมีหน่วยกลางที่เรียกว่า

* **Address Buffer**
* **Data Buffer**

Buffer เหล่านี้ช่วยควบคุมการไหลของข้อมูลระหว่างอุปกรณ์ต่าง ๆ

---

## System Bus

**System Bus** เป็นช่องทางการสื่อสารหลักของระบบคอมพิวเตอร์

ทำหน้าที่เชื่อมต่อ

* Cache
* Main Memory
* อุปกรณ์อื่นในระบบ

ผ่าน

* Address bus
* Data bus
* Control bus

---

# 4.3 องค์ประกอบของการออกแบบ Cache (Elements of Cache Design)

หัวข้อนี้อธิบายองค์ประกอบสำคัญที่ใช้ในการออกแบบ **Cache Architecture**

รวมถึงค่าพารามิเตอร์ที่ใช้โดยทั่วไปในการออกแบบ Cache

---

## Cache ใน High Performance Computing

ในระบบ **High Performance Computing (HPC)**

ซึ่งเกี่ยวข้องกับ

* Supercomputers
* Scientific computing
* การประมวลผลข้อมูลจำนวนมาก
* Vector computation
* Matrix computation
* Parallel algorithms

การออกแบบ Cache มีความสำคัญมาก

อย่างไรก็ตามมีการค้นพบว่า

บางครั้งโปรแกรมในระบบ **HPC**

อาจทำงานได้ไม่ดีบนสถาปัตยกรรมที่ใช้ **Cache**

เพราะลักษณะการเข้าถึงข้อมูลของโปรแกรมอาจไม่เหมาะกับการทำงานของ Cache

แต่ในทางกลับกัน

ถ้ามีการออกแบบหรือปรับแต่ง **Software**

ให้สามารถใช้ประโยชน์จาก **Cache**

ประสิทธิภาพของระบบสามารถเพิ่มขึ้นได้อย่างมาก

---

## องค์ประกอบพื้นฐานของ Cache Architecture

แม้ว่าจะมีการออกแบบ Cache หลายรูปแบบ

แต่โดยทั่วไปแล้ว Cache จะมีองค์ประกอบพื้นฐานที่ใช้ในการจำแนกประเภทของ Cache ได้แก่

* **Cache Size**
* **Block Size**
* **Mapping Function**
* **Replacement Algorithm**
* **Write Policy**

องค์ประกอบเหล่านี้จะถูกอธิบายในรายละเอียดในส่วนถัดไปของบทเรียน

---

# Cache Addresses

Processor ส่วนใหญ่ในปัจจุบัน

ทั้งแบบ **Non-Embedded Processor** และ **Embedded Processor**

รองรับระบบที่เรียกว่า

**Virtual Memory**

---

## Virtual Memory

**Virtual Memory** เป็นเทคนิคที่ช่วยให้โปรแกรมสามารถมองเห็นหน่วยความจำในลักษณะ **Logical Address Space**

โดยไม่ต้องคำนึงถึงขนาดของ **Physical Memory** ที่มีอยู่จริง

เมื่อมีการใช้ **Virtual Memory**

Address ที่ปรากฏใน **Machine Instructions**

จะไม่ใช่ **Physical Address**

แต่จะเป็น

**Virtual Address**

ซึ่งจะต้องถูกแปลงเป็น **Physical Address**

ก่อนที่จะใช้เข้าถึง **Main Memory**

กระบวนการแปลง Address นี้จะถูกจัดการโดยฮาร์ดแวร์ของระบบหน่วยความจำ

และมักจะเกี่ยวข้องกับหน่วยที่เรียกว่า

**Memory Management Unit (MMU)**
cache-design-notes
│
├─ README.md
├─ docs
│  ├─ cache-design.md
│  ├─ logical-vs-physical-cache.md
│  ├─ cache-size.md
│  └─ mapping-function.md
│
└─ images
   ├─ logical-cache-diagram.png
   └─ physical-cache-diagram.png
# Computer Architecture – Cache Design Notes

สรุปเนื้อหา **Cache Memory Design** จากบทเรียน Computer Architecture
แปลไทยและจัดรูปแบบสำหรับอ่านบน GitHub

---

# Contents

* Cache Design Elements
* Logical vs Physical Cache
* Cache Size
* Cache Sizes of Processors
* Mapping Function
* Direct Mapping Example

---

# 1. Cache Design Elements

| Element               | Options                                |
| --------------------- | -------------------------------------- |
| Cache Address         | Logical / Physical                     |
| Cache Size            | ขนาดของ Cache                          |
| Mapping Function      | Direct / Associative / Set-Associative |
| Replacement Algorithm | LRU / FIFO / LFU / Random              |
| Write Policy          | Write-through / Write-back             |
| Line Size             | ขนาดของ Cache Line                     |
| Number of Caches      | Single / Two Level / Unified / Split   |

---

# 2. Logical Cache vs Physical Cache

## Logical Cache (Virtual Cache)

Processor เข้าถึง Cache โดยใช้ **Virtual Address**

```
Processor
   │
Logical Address
   │
 Cache
   │
  MMU
   │
Physical Address
   │
Main Memory
```

### ข้อดี

* เร็วกว่า
* ไม่ต้องรอ MMU translate address

### ข้อเสีย

เกิดปัญหา

**Virtual Address Alias**

เพราะแต่ละ process ใช้ virtual address space เดียวกัน

จึงต้อง

* Flush cache ทุก context switch
  หรือ
* ใช้ Address Space Identifier

---

## Physical Cache

Cache เก็บข้อมูลโดยใช้ **Physical Address**

```
Processor
   │
Logical Address
   │
  MMU
   │
Physical Address
   │
 Cache
   │
Main Memory
```

### ข้อดี

ไม่มีปัญหา virtual alias

### ข้อเสีย

ต้องผ่าน MMU ก่อนจึงช้ากว่าเล็กน้อย

---

# 3. Cache Size

เป้าหมายคือ

```
ทำให้ Cache ใหญ่ที่สุดเท่าที่เป็นไปได้
```

เพื่อให้เก็บข้อมูลจาก main memory ได้มาก

ถ้า cache ใหญ่พอ

```
Average Memory Access Time ≈ Cache Access Time
```

แต่ cache ใหญ่เกินไปจะทำให้

* Addressing logic ซับซ้อน
* Access time เพิ่ม
* Circuit ใหญ่ขึ้น

ดังนั้นจึงไม่มี **cache size ที่ดีที่สุดค่าเดียว**

เพราะขึ้นอยู่กับ **workload**

---

# 4. Cache Sizes of Some Processors

| Processor             | Year | L1        | L2        | L3   |
| --------------------- | ---- | --------- | --------- | ---- |
| IBM 360/85            | 1968 | 16–32KB   | –         | –    |
| PDP-11/70             | 1975 | 1KB       | –         | –    |
| VAX 11/780            | 1978 | 16KB      | –         | –    |
| IBM 3033              | 1978 | 64KB      | –         | –    |
| Intel 80386           | 1989 | 8KB       | –         | –    |
| Pentium               | 1993 | 8KB/8KB   | 256–512KB | –    |
| PowerPC G4            | 1999 | 32KB/32KB | 256KB–1MB | 2MB  |
| Pentium 4             | 2000 | 8KB/8KB   | 256KB     | –    |
| Itanium               | 2001 | 16KB/16KB | 96KB      | 4MB  |
| IBM POWER5            | 2003 | 64KB      | 1.9MB     | 36MB |
| IBM POWER6            | 2007 | 64KB/64KB | 4MB       | 32MB |
| Intel Core i7 Extreme | 2011 | 6×32KB    | 1.5MB     | 12MB |

หมายเหตุ

```
8KB/8KB = Instruction Cache / Data Cache
```

---

# 5. Mapping Function

เพราะว่า

```
จำนวน Cache Lines < จำนวน Memory Blocks
```

จึงต้องมี algorithm สำหรับ

```
Mapping Main Memory Block → Cache Line
```

มี 3 เทคนิคหลัก

1. Direct Mapping
2. Associative Mapping
3. Set-Associative Mapping

---

# 6. Example

กำหนดว่า

* Cache = **64KB**
* Block size = **4 bytes**
* Cache line = **4 bytes**
* Main memory = **16MB**
* Address size = **24 bits**

เพื่อความง่ายในการอธิบาย

ใช้ main memory ขนาด

```
4MB
```

แบ่งเป็น block ขนาด

```
4 bytes
```

---

# 7. Direct Mapping

เทคนิคที่ง่ายที่สุด

```
Main Memory Block
→ Map ไปได้ Cache Line เดียว
```

สูตร

```
i = j mod m
```

โดย

| Symbol | Meaning               |
| ------ | --------------------- |
| i      | Cache Line Number     |
| j      | Memory Block Number   |
| m      | Number of Cache Lines |

ตัวอย่าง

```
Block 0 → Line 0
Block 1 → Line 1
Block m → Line 0
Block m+1 → Line 1
```

---

# Author

Computer Architecture Study Notes
แปลและจัดรูปแบบสำหรับการเรียน
# Cache Mapping (Direct & Associative)

> Computer Architecture Notes  
> สรุปการทำงานของ Cache Memory Mapping จากภาพ Figure 4.8 – 4.10

---

# Figure 4.8 – Mapping from Main Memory to Cache

## (a) Direct Mapping

First **m blocks of main memory**  
(equal to size of cache)

Cache memory

- `b` = length of block in bits  
- `t` = length of tag in bits  

### คำแปล

บล็อก **m บล็อกแรกของหน่วยความจำหลัก**  
(มีขนาดรวมเท่ากับ cache)

หน่วยความจำ Cache

- `b` = ขนาดของ block (หน่วยเป็นบิต)  
- `t` = ขนาดของ tag (หน่วยเป็นบิต)

---

## (b) Associative Mapping

One block of main memory  
สามารถถูกวางไว้ที่ **cache line ใดก็ได้**

### คำแปล

หนึ่ง block จาก main memory  
สามารถถูกเก็บไว้ใน **cache line ใดก็ได้ใน cache**

---

# Direct Mapping Concept

Mapping ของ main memory ไปยัง cache ทำดังนี้

```
Bm → Lq
```

โดย

```
q = m mod number_of_cache_lines
```

ตัวอย่าง

```
B11 → L3
```

---

# Address Structure

Main Memory Address ถูกแบ่งเป็น

```
+------+-------+------+
| Tag  | Line  | Word |
+------+-------+------+
```

คำอธิบาย

| Field | Meaning |
|------|------|
| Tag | ใช้ตรวจสอบว่า block ที่อยู่ใน cache เป็น block ที่ต้องการหรือไม่ |
| Line | ระบุ cache line |
| Word | ระบุ word หรือ byte ภายใน block |

---

# Address and Cache Parameters

ให้

```
s = number of bits for main memory block
r = number of bits for cache line
w = number of bits for word
```

---

## Address length

```
Address length = (s + w) bits
```

---

## Number of addressable units

```
2^(s+w) words หรือ bytes
```

---

## Block size

```
Block size = line size = 2^w words หรือ bytes
```

---

## Number of blocks in main memory

```
2^(s+w) / 2^w = 2^s
```

---

## Number of lines in cache

```
m = 2^r
```

---

## Cache size

```
Cache size = 2^(r+w) words หรือ bytes
```

---

## Tag size

```
Tag size = (s − r) bits
```

---

# Figure 4.9 – Direct-Mapping Cache Organization

Memory Address

```
+------+-------+------+
| Tag  | Line  | Word |
+------+-------+------+
```

การทำงาน

1. CPU ส่ง Memory Address
2. ส่วน **Line** ใช้เลือก cache line
3. ส่วน **Tag** ใช้เปรียบเทียบกับ tag ที่เก็บใน cache
4. ถ้าเท่ากัน → **Cache Hit**
5. ถ้าไม่เท่ากัน → **Cache Miss**

---

# Cache Result

```
0 → Hit in cache
1 → Miss in cache
```

---

# Example 4.2a – Direct Mapping Example

ระบบตัวอย่างใช้ **Direct Mapping**

```
m = 16K = 2^14
```

Mapping function

```
j = i mod 2^14
```

---

# Cache Line Mapping

| Cache Line | Starting Memory Address |
|------------|------------------------|
| 0 | 000000, 010000, ..., FF000 |
| 1 | 000004, 010004, ..., FF004 |
| ... | ... |
| 2^14 − 1 | 00FFFC, 01FFFC, ..., FFFFFC |

---

# Example Address Format

Main memory

```
16 MB
```

Address size

```
24-bit address
```

Address format

```
+--------+-------------+------+
| Tag    | Line        | Word |
+--------+-------------+------+
| 8 bits | 14 bits     | 2 bits |
```

---

# Example Operation

สมมุติ

```
Tag = 16
Line = 0CE7
```

Cache จะตรวจสอบว่า

```
Tag ใน line 0CE7
ตรงกับ Tag ของ address หรือไม่
```

ถ้าตรง

```
Cache Hit
```

ถ้าไม่ตรง

```
Cache Miss
```

เมื่อเกิด miss

ระบบจะใช้

```
Tag + Line
```

ต่อกับ

```
00
```

เพื่อดึงข้อมูล **4 bytes** จาก main memory

---

# Figure 4.10 – Direct Mapping Example

การกำหนด block ของ main memory ไปยัง cache

| Cache line | Main memory blocks assigned |
|-------------|----------------------------|
| 0 | 0, m, 2m, ..., 2^s − m |
| 1 | 1, m+1, 2m+1, ..., 2^s − m + 1 |
| ... | ... |
| m−1 | m−1, 2m−1, 3m−1, ..., 2^s − 1 |

---

# Direct Mapping Summary

Direct Mapping

```
Main Memory Block
        ↓
Cache Line เดียวที่กำหนดไว้
```

ข้อดี

```
✔ Hardware simple
✔ Fast implementation
```

ข้อเสีย

```
✘ Conflict Miss เกิดง่าย
✘ หลาย block อาจ map มาที่ line เดียวกัน
```

---

# Associative Mapping Summary

Associative Mapping

```
Main Memory Block
        ↓
Any Cache Line
```

CPU ต้อง

```
Compare Tag กับทุก cache line
```

---

# Notes

ค่าของ memory

```
Binary → ใช้กับ address
Hexadecimal → ใช้แสดงค่า memory
```
# Cache Mapping (Associative & Set-Associative)

> Computer Architecture Notes  
> เนื้อหาจาก Figure 4.11 – 4.12

---

# Direct Mapping Limitation

Direct Mapping มีข้อจำกัดคือ

```
Fixed cache location for each block
```

หมายความว่า

Main Memory Block แต่ละตัว  
จะถูก map ไปยัง **Cache Line เดียวเท่านั้น**

### ปัญหาที่เกิดขึ้น

ถ้าโปรแกรมอ้างอิง block หลายตัวที่ map ไป line เดียวกัน

```
Cache blocks จะถูกสลับเข้าออกตลอดเวลา
```

ผลที่เกิดขึ้น

```
Hit ratio ต่ำ
```

ปรากฏการณ์นี้เรียกว่า

```
Thrashing
```

---

# Victim Cache

วิธีลดปัญหา miss

```
Victim Cache
```

แนวคิด

Block ที่ถูกแทนที่ (discarded block)

```
เก็บไว้ใน cache ขนาดเล็กอีกชั้น
```

ถ้า block นั้นถูกใช้ซ้ำ

```
สามารถนำกลับมาใช้ได้ทันที
```

---

# Associative Mapping

Associative Mapping แก้ปัญหาของ Direct Mapping

```
Main Memory Block
        ↓
Any Cache Line
```

กล่าวคือ

```
Block ใด ๆ สามารถถูกเก็บใน cache line ใดก็ได้
```

---

# Address Structure (Associative Mapping)

Memory Address

```
+--------+------+
|  Tag   | Word |
+--------+------+
```

ไม่มี field สำหรับ **line**

เพราะ block สามารถไปอยู่ **line ใดก็ได้**

---

# Cache Search

เมื่อ CPU ต้องการข้อมูล

Cache จะทำ

```
Compare Tag with every cache line
```

ถ้า

```
Tag match → Cache Hit
```

ถ้า

```
Tag mismatch → Cache Miss
```

---

# Figure 4.11 – Fully Associative Cache Organization

การทำงาน

```
Memory Address
      │
      ▼
+-----------+
| Tag |Word |
+-----------+
      │
      ▼
Compare with ALL cache tags
      │
 ┌────┴────┐
 │         │
Hit       Miss
```

---

# Example 4.2b – Associative Mapping Example

Figure 4.12 แสดงตัวอย่าง associative mapping

Main memory address ประกอบด้วย

```
22-bit tag
2-bit byte number
```

ดังนั้น

```
Address = 24 bits
```

---

# Example Address

ตัวอย่าง address

```
16339C (hex)
```

Binary

```
0001 0110 0011 0011 1001 1100
```

Tag (22 bits)

```
0001 0110 0011 0011 1001 11
```

Word

```
00
```

---

# Memory Address Structure

```
+----------------------+------+
|        Tag           | Word |
+----------------------+------+
|       22 bits        | 2 bits |
```

---

# Example Cache Data

ตัวอย่างข้อมูลใน cache

| Tag | Data | Line |
|----|------|------|
| 3FFFED | 33333333 | 3FFD |
| 000000 | 13579246 | 0000 |
| 3FFFFF | 24682468 | 3FFF |

---

# Associative Mapping Parameters

Note:

```
Address ไม่มี field ที่ระบุ line number
```

ดังนั้น

```
จำนวน line ใน cache
ไม่ได้ถูกกำหนดโดย address
```

---

# Important Parameters

## Address length

```
(s + w) bits
```

---

## Number of addressable units

```
2^(s+w) words or bytes
```

---

## Block size

```
2^w words or bytes
```

---

## Number of blocks in main memory

```
2^(s+w) / 2^w = 2^s
```

---

## Number of lines in cache

```
Undetermined
```

---

## Tag size

```
Tag = s bits
```

---

# Associative Mapping Advantage

ข้อดี

```
✔ Flexible block placement
✔ ลด conflict miss
✔ เพิ่ม hit ratio
```

---

# Associative Mapping Disadvantage

ข้อเสีย

```
✘ Hardware ซับซ้อน
✘ ต้อง compare ทุก cache line
```

---

# Cache Time Analysis

การตรวจสอบ tag ของทุก line พร้อมกัน

```
Parallel comparison
```

ทำให้ต้องใช้

```
Complex circuitry
```

---

# Set-Associative Mapping

Set-Associative Mapping เป็น **การผสมระหว่าง**

```
Direct Mapping
Associative Mapping
```

---

# Cache Structure

Cache แบ่งออกเป็น

```
Sets
```

แต่ละ set มีหลาย line

---

# Relationship

```
m = v × k
```

โดย

| Symbol | Meaning |
|------|------|
| m | number of cache lines |
| v | number of sets |
| k | lines per set |

---

# Mapping Function

```
i = j mod v
```

โดย

| Symbol | Meaning |
|------|------|
| i | cache set number |
| j | main memory block number |

---

# Mapping Behavior

Main Memory Block

```
Block j
```

จะ map ไปยัง

```
Set i
```

แต่สามารถอยู่ได้ใน

```
Any line within that set
```

---

# Example

ถ้า

```
4-way set associative cache
```

แต่ละ set จะมี

```
4 lines
```

Block ที่ map ไป set เดียวกัน

```
สามารถอยู่ line ใดก็ได้ใน set
```

---

# Set-Associative Summary

Direct Mapping

```
Block → 1 line
```

Associative Mapping

```
Block → Any line
```

Set Associative

```
Block → Any line in a specific set
```

---

# Comparison

| Mapping Type | Flexibility | Hardware Complexity |
|--------------|-------------|--------------------|
| Direct | Low | Low |
| Associative | High | High |
| Set Associative | Medium | Medium |

---   
# Set-Associative Cache Mapping

> Computer Architecture Notes  
> Figure 4.13 – 4.15

---

# Mapping from Main Memory to Cache: k-Way Set Associative

Set-Associative Mapping เป็นการผสมระหว่าง

```
Direct Mapping
Associative Mapping
```

แนวคิดคือ

```
Cache ถูกแบ่งเป็นหลาย sets
แต่ละ set มีหลาย lines
```

---

# Figure 4.13 – Mapping from Main Memory to Cache

## (a) v Associative-Mapped Caches

Main memory blocks

```
B0
B1
...
Bj
```

สามารถ map ไปยัง

```
Cache memory set 0
Cache memory set 1
...
Cache memory set v−1
```

---

## (b) k Direct-Mapped Caches

Cache แบ่งเป็นหลาย **ways**

```
Cache way 1
Cache way 2
...
Cache way k
```

แต่ละ way ทำงานเหมือน

```
Direct-mapped cache
```

---

# Associativity Concept

ระดับของ associativity

```
k-way set associative cache
```

หมายถึง

```
แต่ละ set มี k lines
```

ตัวอย่าง

```
2-way set associative
4-way set associative
8-way set associative
```

---

# Address Structure

Memory address แบ่งเป็น

```
+------+-------+------+
| Tag  | Set   | Word |
+------+-------+------+
```

---

# Address Fields

| Field | Description |
|------|-------------|
| Tag | ใช้ตรวจสอบ block ที่อยู่ใน cache |
| Set | ใช้เลือก set |
| Word | เลือก word ภายใน block |

---

# Important Parameters

ให้

```
s = number of bits for main memory block
d = number of bits for set number
w = number of bits for word
```

---

## Address length

```
(s + w) bits
```

---

## Number of addressable units

```
2^(s+w) words or bytes
```

---

## Block size

```
2^w words or bytes
```

---

## Number of blocks in main memory

```
2^(s+w) / 2^w = 2^s
```

---

## Number of lines in set

```
k
```

---

## Number of sets

```
v = 2^d
```

---

## Number of lines in cache

```
m = kv = k × 2^d
```

---

## Cache size

```
Cache size = k × 2^(d+w)
```

---

## Tag size

```
Tag = (s − d) bits
```

---

# Figure 4.14 – k-Way Set Associative Cache Organization

โครงสร้างการทำงาน

```
Memory Address
      │
      ▼
+------+-------+------+
| Tag  | Set   | Word |
+------+-------+------+
      │
      ▼
Select Cache Set
      │
      ▼
Compare Tag with all lines in set
      │
 ┌────┴────┐
 │         │
Hit       Miss
```

---

# Cache Operation

1. CPU ส่ง **Memory Address**
2. ส่วน **Set** ใช้เลือก cache set
3. Cache จะตรวจสอบ **Tag ทุก line ใน set**
4. ถ้าพบ tag ตรงกัน

```
Cache Hit
```

5. ถ้าไม่พบ

```
Cache Miss
```

---

# Example 4.2c – Two-Way Set Associative Mapping

ระบบตัวอย่างใช้

```
2-way set associative cache
```

Cache มี

```
16K lines
```

แบ่งเป็น

```
8192 sets
```

เพราะ

```
2 lines per set
```

---

# Address Format Example

Memory address

```
24 bits
```

แบ่งเป็น

```
+------+-----------+------+
| Tag  | Set       | Word |
+------+-----------+------+
| 9 bits | 13 bits | 2 bits |
```

---

# Mapping Function

Block number

```
j
```

Set number

```
i = j mod 2^13
```

ดังนั้น

Main memory blocks

```
000000
...
FF8000
```

จะ map ไปยัง

```
Set 0
```

---

# Example Cache Content

| Tag | Data | Set |
|----|------|-----|
| 000 | 13579246 | 0000 |
| 02C | 77777777 | 02C |
| 02C | FEDCBA98 | 02C |
| 1FF | 11223344 | 1FF |

---

# Example Operation

เมื่อ CPU ต้องการข้อมูล

1. อ่าน **Set number**
2. ไปยัง **Set นั้นใน cache**
3. ตรวจสอบ **Tag ของทุก line ใน set**

ถ้า

```
Tag match
```

จะเกิด

```
Cache Hit
```

ถ้า

```
Tag mismatch
```

จะเกิด

```
Cache Miss
```

---

# Comparison of Cache Mapping

| Mapping Type | Block Placement | Hardware Complexity |
|--------------|----------------|---------------------|
| Direct | 1 Line เท่านั้น | Low |
| Associative | Any Line | High |
| Set Associative | Any line in a set | Medium |

---

# Summary

Direct Mapping

```
Block → 1 specific line
```

Associative Mapping

```
Block → Any cache line
```

Set-Associative Mapping

```
Block → Any line within a specific set
```

ข้อดี

```
✔ ลด conflict miss
✔ Hardware complexity ปานกลาง
✔ Performance ดี
```

---
# Cache Performance, Replacement Algorithms, and Write Policies

> Computer Architecture Notes  
> Figure 4.16 – Replacement Algorithms – Write Policy – Cache Coherency

---

# Figure 4.16 – Varying Associativity over Cache Size

กราฟแสดงความสัมพันธ์ระหว่าง

```
Cache Size
Associativity
Hit Ratio
```

### แนวโน้มที่พบ

- เมื่อ **Cache Size เพิ่มขึ้น → Hit Ratio เพิ่มขึ้น**
- เมื่อ **Associativity เพิ่มขึ้น → Hit Ratio ดีขึ้น**

ประเภทที่เปรียบเทียบ

```
Direct mapping
Two-way set associative
Four-way set associative
Eight-way set associative
Sixteen-way set associative
```

---

## Observations

### Extreme Cases

```
k = 1
```

จะเท่ากับ

```
Direct Mapping
```

---

```
k = m
```

จะเท่ากับ

```
Fully Associative Mapping
```

---

### Most Common Organization

รูปแบบที่นิยมมากที่สุดคือ

```
Two-way set associative
```

เพราะ

```
balance ระหว่าง performance และ hardware complexity
```

---

### Performance Insight

- **Four-way set associative** ให้ improvement เพิ่มขึ้นเล็กน้อย
- การเพิ่ม associativity มากเกินไปให้ประโยชน์น้อย

ตัวอย่าง

```
Difference between 2-way and 4-way
< difference between 4 KB and 8 KB cache size
```

---

### Practical Observation

เพิ่ม cache size ถึงประมาณ

```
32 KB
```

หลังจากนั้น

```
Performance improvement ลดลง
```

---

# Replacement Algorithms

เมื่อ cache เต็มและมี block ใหม่ต้องเข้ามา

```
Block หนึ่งต้องถูกแทนที่
```

---

## Direct Mapping

Direct mapping

```
ไม่มีตัวเลือก
```

ต้อง replace block ที่ถูกกำหนดไว้แล้ว

---

## Associative / Set Associative

ต้องใช้

```
Replacement Algorithm
```

เพื่อเลือก block ที่จะถูกแทนที่

---

# Most Common Replacement Algorithms

## 1. Least Recently Used (LRU)

แนวคิด

```
Replace block that has not been used for the longest time
```

ขั้นตอน

- บันทึกการใช้งาน block
- block ที่ถูกใช้ล่าสุดจะถูกเลื่อนขึ้น
- block ที่ใช้น้อยสุดจะถูกลบ

ข้อดี

```
Hit ratio ดี
```

ข้อเสีย

```
Hardware ซับซ้อน
```

---

## 2. First-In First-Out (FIFO)

แนวคิด

```
Replace the block that has been in cache the longest
```

ลักษณะ

```
Queue based
```

ข้อดี

```
ง่ายต่อการ implement
```

---

## 3. Least Frequently Used (LFU)

แนวคิด

```
Replace block that has the smallest reference count
```

ข้อเสีย

```
ต้องเก็บ counter สำหรับทุก block
```

---

## 4. Random Replacement

แนวคิด

```
Randomly select a block to replace
```

ข้อดี

```
Hardware ง่าย
```

---

# Replacement Algorithm Comparison

| Algorithm | Idea | Complexity |
|----------|------|-----------|
| LRU | Replace least recently used | High |
| FIFO | Replace oldest block | Low |
| LFU | Replace least frequently used | Medium |
| Random | Random selection | Very Low |

---

# Write Policy

เมื่อ CPU ต้องการ **write data ลง cache**

มี 2 แนวทางหลัก

```
Write-through
Write-back
```

---

# Write-Through

แนวคิด

```
Write data to cache AND main memory simultaneously
```

ข้อดี

```
Main memory always up-to-date
```

ข้อเสีย

```
Memory traffic สูง
```

---

# Write-Back

แนวคิด

```
Write only to cache
```

Main memory จะถูก update

```
เมื่อ block ถูก replace
```

ใช้

```
Dirty bit
```

เพื่อบอกว่า block ถูกแก้ไขแล้วหรือไม่

---

# Dirty Bit

```
1 = block modified
0 = block clean
```

ถ้า dirty bit = 1

```
ต้อง write block กลับ main memory ก่อน replace
```

---

# Write Policy Comparison

| Policy | Memory Traffic | Complexity |
|------|---------------|-----------|
| Write-through | High | Low |
| Write-back | Low | Higher |

---

# Example 4.3

กำหนด

```
Cache line size = 32 bytes
```

Main memory

```
30 ns per word write
```

สมมุติว่า block ถูกเขียน

```
8 ครั้ง
```

---

### Write-Through

ต้องเขียน

```
8 writes
```

ใช้เวลา

```
8 × 30 ns = 240 ns
```

---

### Write-Back

เขียน main memory

```
ครั้งเดียว
```

ใช้เวลา

```
30 ns
```

---

### Conclusion

```
Write-back efficient เมื่อ block ถูกเขียนหลายครั้ง
```

---

# Cache Coherency Problem

ในระบบ

```
Multiprocessor
Shared Memory
```

CPU หลายตัวมี cache ของตัวเอง

ปัญหาที่เกิดขึ้น

```
Cache copies อาจไม่เหมือนกัน
```

---

# Cache Coherency Solutions

## 1. Bus Watching

Cache controllers

```
monitor address lines
```

ถ้า processor อื่น update memory

```
invalidate cache entry
```

---

## 2. Hardware Transparency

hardware จะ

```
update all caches automatically
```

---

## 3. Noncacheable Memory

กำหนดบาง memory region

```
noncacheable
```

เพื่อป้องกัน coherency problem

---

# Summary

Cache design มีหลายองค์ประกอบ

```
Mapping
Replacement algorithm
Write policy
Cache coherency
```

ทั้งหมดมีผลต่อ

```
Performance
Hardware complexity
System cost
```

---
# Cache Design Issues: Line Size, Number of Caches, and Cache Organization

> Computer Architecture Notes  
> Topics: Line Size, Multilevel Cache, Unified vs Split Cache

---

# Cache Coherency

Cache coherency เป็นหัวข้อวิจัยที่สำคัญในระบบ multiprocessor  
รายละเอียดเพิ่มเติมมักศึกษาในหัวข้อ **advanced computer architecture**

---

# Line Size

อีกหนึ่งองค์ประกอบสำคัญของ cache design คือ

```
Line Size
```

เมื่อ block ของข้อมูลถูกนำเข้ามาใน cache

```
ข้อมูลที่อยู่ติดกัน (adjacent words)
```

มักจะถูกนำเข้ามาด้วย

---

## Principle of Locality

เหตุผลคือ

```
Programs exhibit locality
```

หมายความว่า

```
ถ้ามีการอ้างอิง word หนึ่ง
มีแนวโน้มที่จะใช้ word ใกล้เคียงในอนาคต
```

---

## Effect of Increasing Line Size

เมื่อ **Line Size เพิ่มขึ้น**

ข้อดี

```
นำ useful data เข้ามาใน cache มากขึ้น
```

แต่ก็มีข้อเสีย

```
Hit ratio อาจลดลง
```

---

## Two Important Effects

### 1. Cache Pollution

block ที่มีขนาดใหญ่

```
อาจทำให้ cache เต็มเร็ว
```

และข้อมูลที่มีประโยชน์

```
ถูก overwrite เร็วขึ้น
```

---

### 2. Reduced Spatial Locality Benefit

เมื่อ block ใหญ่ขึ้น

```
แต่ละ word จะอยู่ห่างจาก requested word มากขึ้น
```

ทำให้

```
โอกาสที่จะถูกใช้งานลดลง
```

---

## Typical Line Size

จากการศึกษา

```
8 – 64 bytes
```

มักให้ performance ที่ดี

ในระบบ HPC

```
64 – 128 bytes
```

---

# Number of Caches

ในอดีต

```
computer systems มี cache เพียงตัวเดียว
```

ปัจจุบัน

```
ใช้ multiple levels of cache
```

เช่น

```
L1
L2
L3
```

---

# Multilevel Cache

Cache hierarchy

```
CPU
 │
 ▼
L1 Cache (fastest)
 │
 ▼
L2 Cache
 │
 ▼
L3 Cache
 │
 ▼
Main Memory
```

---

## On-Chip Cache

Cache รุ่นใหม่มักอยู่

```
on-chip (inside processor)
```

ข้อดี

```
ลด memory latency
```

---

## External Cache

บางระบบมี

```
off-chip cache
```

ซึ่งมักถูกใช้เป็น

```
L2 cache
```

---

# Why L2 Cache is Used

ถ้าไม่มี L2 cache

เมื่อ L1 miss

```
processor ต้องเข้าถึง DRAM โดยตรง
```

ซึ่ง

```
ช้ามาก
```

L2 cache ช่วย

```
reduce memory access time
```

---

# Advantages of L2 Cache

1️⃣ ลดจำนวนการเข้าถึง main memory

2️⃣ เพิ่ม performance ของระบบ

3️⃣ ลด bus traffic

---

# Figure 4.17 – Total Hit Ratio

กราฟแสดง

```
Total Hit Ratio (L1 + L2)
```

สำหรับ

```
8 KB และ 16 KB L1 cache
```

แกน X

```
L2 Cache Size
```

แกน Y

```
Hit Ratio
```

---

## Observations

### Small L2 Cache

```
L2 < 8 KB
```

มีผลต่อ performance

```
น้อย
```

---

### Larger L2 Cache

เมื่อ

```
L2 ≥ 64 KB
```

performance

```
เพิ่มขึ้นอย่างเห็นได้ชัด
```

---

# Unified vs Split Cache

เมื่อมี on-chip cache

มี 2 แนวทาง

```
Unified Cache
Split Cache
```

---

# Unified Cache

Unified cache

```
ใช้ cache เดียว
```

สำหรับ

```
Instructions
Data
```

---

## Advantages

1️⃣ สามารถปรับ allocation ได้อัตโนมัติ

เช่น

```
program ใช้ instructions มากกว่า data
```

cache จะใช้พื้นที่

```
เก็บ instructions มากขึ้น
```

---

2️⃣ Hardware ง่าย

```
ออกแบบ cache เพียงชุดเดียว
```

---

# Split Cache

Split cache

```
Instruction Cache (I-cache)
Data Cache (D-cache)
```

---

## Advantages

ลด

```
contention
```

ระหว่าง

```
instruction fetch
data access
```

---

## Why Split Cache is Important

ใน pipeline processor

```
instruction fetch
execution
```

เกิดพร้อมกัน

ถ้าใช้ unified cache

```
requests อาจชนกัน
```

ทำให้

```
pipeline stall
```

---

# Split Cache Solution

Split cache แก้ปัญหา

```
instruction pipeline interference
```

โดย

```
แยก cache สำหรับ instructions และ data
```

---

# Modern Trend

แนวโน้มของระบบสมัยใหม่

```
L1 → Split cache
L2 / L3 → Unified cache
```

---

# Example: Modern Processors

ตัวอย่างเช่น

```
IBM Enterprise servers
```

มี

```
3 levels of on-chip cache
```

และ

```
shared L3 cache
```

---

# Next Topic

```
Pentium 4 Cache Organization
```

ซึ่งเป็นตัวอย่างจริงของ

```
modern cache architecture
```

---
# Intel Cache Evolution and Pentium 4 Cache Organization

> Computer Architecture Notes  
> Topics: Intel Cache Evolution, Pentium 4 Architecture, Cache Operating Modes

---

# Intel Cache Evolution

ตารางแสดงพัฒนาการของ cache ในโปรเซสเซอร์ Intel

| Problem | Solution | Processor on Which Feature First Appears |
|-------|---------|--------------------------------|
| External memory slower than the system bus | Add external cache using faster memory technology | 386 |
| Increased processor speed results in external bus becoming a bottleneck for cache access | Move external cache on-chip, freeing the same speed as the processor | 486 |
| Internal cache is rather small due to limited space on chip | Add external L2 cache using faster technology than main memory | 486 |
| Contention occurs when instruction prefetch and execution units access cache simultaneously | Create separate data and instruction caches | Pentium |
| Increased processor speed results in external bus becoming a bottleneck for L2 cache access | Create separate back-side bus dedicated to L2 cache | Pentium Pro |
| Move L2 cache onto processor chip | Pentium II |
| Applications require massive datasets and rapid access | Add external L3 cache | Pentium III |
| Move L3 cache on-chip | Pentium 4 |

---

# Pentium Cache Organization

Pentium processors use

```
Set-associative cache organization
```

ทุก Pentium processor มี

```
Two on-chip L1 caches
```

- Instruction cache
- Data cache

---

## Pentium 4 L1 Cache

L1 Data Cache

```
Size = 16 KB
Line size = 64 bytes
Organization = 4-way set associative
```

---

## Pentium 4 L2 Cache

Pentium 4 ยังมี

```
L2 cache feeding both L1 caches
```

ลักษณะ

```
8-way set associative
Size = 512 KB
Line size = 128 bytes
```

---

## L3 Cache

เพิ่มใน

```
Pentium III
```

และ

```
High-end Pentium 4
```

---

# Pentium 4 Processor Components

Pentium 4 processor มีองค์ประกอบหลัก

```
Fetch / Decode Unit
Out-of-order Execution Logic
Execution Units
Memory Subsystem
```

---

# Fetch / Decode Unit

หน้าที่

```
Fetch instructions
Decode into micro-operations
Store results in L1 cache
```

---

# Out-of-Order Execution Logic

ทำหน้าที่

```
Schedule execution of micro-operations
```

คุณสมบัติ

- จัดลำดับ execution ใหม่ได้
- รองรับ speculative execution

---

# Pentium 4 Block Diagram

องค์ประกอบสำคัญ

```
L1 Instruction Cache
Instruction Fetch/Decode Unit
Integer Register File
Floating Point Register File
Execution Units
L1 Data Cache
L2 Cache
L3 Cache
System Bus
```

---

## Execution Units

ประกอบด้วย

```
Simple Integer ALU
Complex Integer ALU
Load Address Unit
Store Address Unit
Floating Point / MMX Unit
FP Move Unit
```

---

# Cache Operating Modes

Pentium 4 cache สามารถทำงานได้หลาย mode

| CD | NW | Cache Fills | Write Throughs | Invalidates |
|----|----|-------------|---------------|-------------|
| 0 | 0 | Enabled | Enabled | Enabled |
| 0 | 1 | Disabled | Enabled | Enabled |
| 1 | 0 | Disabled | Disabled | Disabled |

Note

```
CD = 0 and NW = 1 is invalid
```

---

# Execution Units

Execution units ทำงานโดย

```
Fetch micro-operations
```

จาก

```
L1 Data Cache
```

ผลลัพธ์จะถูกเก็บใน

```
Registers
```

---

# Memory Subsystem

ประกอบด้วย

```
L2 cache
L3 cache
System bus
```

ใช้สำหรับ

```
Access main memory
Access I/O resources
```

---

# Pentium 4 Instruction Decoding

Pentium 4 แปล

```
Complex x86 instructions
```

เป็น

```
Micro-operations
```

ซึ่ง

```
ง่ายต่อการ execute ใน pipeline
```

---

# Micro-Operations Advantages

ข้อดี

```
Simplifies pipeline design
Improves scheduling
Improves performance
```

---

# Cache Write Policy

Pentium 4 ใช้

```
Write-back cache policy
```

แต่สามารถกำหนดให้

```
Write-through
```

ได้ด้วย

```
Control bits
```

---

# Cache Control Bits

Control bits

```
CD (Cache Disable)
NW (Not Write-through)
```

ใช้ควบคุม

```
Cache behavior
```

---

# Cache Control Instructions

Pentium 4 มี instruction สำหรับจัดการ cache เช่น

```
INVD
WBINVD
```

---

## INVD

```
Invalidate internal cache
```

---

## WBINVD

```
Write back and invalidate internal cache
```

---

# Cache Organization

ทั้ง

```
L2 cache
L3 cache
```

ใช้

```
8-way set associative organization
```

และ

```
Line size = 128 bytes
```

---

# Key Terms

คำศัพท์สำคัญ

| Term | Meaning |
|------|--------|
| Access time | เวลาในการเข้าถึงข้อมูลใน memory |
| Cache line | หน่วยของข้อมูลที่เก็บใน cache |
| Cache memory | หน่วยความจำความเร็วสูงระหว่าง CPU และ RAM |
| Cache hit | ข้อมูลที่ต้องการอยู่ใน cache |
| Cache miss | ข้อมูลไม่อยู่ใน cache |
| Associative mapping | mapping ที่ block อยู่ได้หลายตำแหน่ง |
| Cache set | กลุ่มของ cache lines |
| Data cache | cache สำหรับข้อมูล |
| Direct access | การเข้าถึง memory โดยตรง |

---

# Summary

Pentium 4 cache architecture ประกอบด้วย

```
L1 cache
L2 cache
L3 cache
```

ใช้

```
Set-associative organization
Write-back policy
Micro-operation execution
```

เพื่อ

```
Increase performance
Reduce memory latency
Improve pipeline efficiency
```

---
# Cache Memory – Key Terms, Review Questions, and Problems

---

# Key Terms

# Cache Memory – คำศัพท์สำคัญ คำถามทบทวน และโจทย์

---

# คำศัพท์สำคัญ (Key Terms)

| คำศัพท์ | ความหมาย |
|-----|--------|
| Direct mapping | วิธีการแมปของแคชที่แต่ละบล็อกของหน่วยความจำหลักจะถูกแมปไปยังแคชไลน์เพียงตำแหน่งเดียว |
| High-performance computing (HPC) | การประมวลผลประสิทธิภาพสูงที่มุ่งเน้นความเร็วและพลังการคำนวณ |
| Hit | การเข้าถึงข้อมูลที่พบข้อมูลที่ต้องการอยู่ในแคช |
| Hit ratio | อัตราส่วนของการเข้าถึงหน่วยความจำที่พบข้อมูลในแคช |
| Instruction cache | แคชที่ใช้เก็บคำสั่งของโปรแกรม |
| L1 cache | แคชระดับที่ 1 ซึ่งอยู่ใกล้ CPU มากที่สุด |
| L2 cache | แคชระดับที่ 2 |
| L3 cache | แคชระดับที่ 3 |
| Line size | ขนาดของ cache line |
| Locality | แนวโน้มที่โปรแกรมจะเข้าถึงข้อมูลเดิมหรือข้อมูลที่อยู่ใกล้กัน |
| Logical cache | แคชที่อ้างอิงโดยใช้ logical address |
| Memory hierarchy | โครงสร้างลำดับชั้นของหน่วยความจำตามความเร็วและขนาด |
| Miss | การเข้าถึงข้อมูลที่ไม่พบในแคช |
| Multilevel cache | การใช้แคชหลายระดับ |
| Physical address | ที่อยู่จริงของหน่วยความจำที่ฮาร์ดแวร์ใช้ในการเข้าถึง |
| Random access | การเข้าถึงตำแหน่งใด ๆ ของหน่วยความจำโดยใช้เวลาเท่ากัน |
| Replacement algorithm | อัลกอริทึมที่ใช้เลือกบล็อกในแคชที่จะถูกแทนที่ |
| Secondary memory | หน่วยความจำสำรอง เช่น ฮาร์ดดิสก์ |
| Sequential access | การเข้าถึงข้อมูลตามลำดับ |
| Set-associative mapping | วิธีแมประหว่าง direct mapping และ associative mapping |
| Spatial locality | การเข้าถึงตำแหน่งข้อมูลที่อยู่ใกล้กัน |
| Split cache | การแยกแคชสำหรับคำสั่งและข้อมูล |
| Tag | ตัวระบุที่ใช้ตรวจสอบว่าบล็อกของหน่วยความจำอยู่ในแคชหรือไม่ |
| Temporal locality | การเข้าถึงข้อมูลเดิมซ้ำในช่วงเวลาสั้น ๆ |
| Unified cache | แคชเดียวที่ใช้เก็บทั้งคำสั่งและข้อมูล |
| Virtual address | ที่อยู่หน่วยความจำที่ CPU สร้างขึ้นก่อนถูกแปลงเป็น physical address |
| Virtual cache | แคชที่ทำงานโดยใช้ virtual address |
| Write-back | การเขียนข้อมูลลงแคชก่อน แล้วค่อยอัปเดตหน่วยความจำหลักภายหลัง |
| Write-through | การเขียนข้อมูลลงแคชและหน่วยความจำหลักพร้อมกัน |

---

# คำถามทบทวน (Review Questions)

### 4.1
อธิบายความแตกต่างระหว่าง

- Sequential access (การเข้าถึงตามลำดับ)  
- Direct access (การเข้าถึงโดยตรง)  
- Random access (การเข้าถึงแบบสุ่ม)

---

### 4.2
ความสัมพันธ์โดยทั่วไประหว่าง

- เวลาในการเข้าถึงหน่วยความจำ (Memory access time)
- ต้นทุนหน่วยความจำ (Memory cost)
- ความจุหน่วยความจำ (Memory capacity)

คืออะไร

---

### 4.3
หลักการ **Locality** เกี่ยวข้องกับการใช้ **หน่วยความจำหลายระดับ (Multiple memory levels)** อย่างไร

---

### 4.4
อธิบายความแตกต่างระหว่าง

- Direct mapping
- Associative mapping
- Set-associative mapping

---

### 4.5
สำหรับ **Direct-mapped cache**

ที่อยู่ของหน่วยความจำหลัก (Main memory address) ประกอบด้วย **3 ส่วน**

จงระบุและอธิบายส่วนต่าง ๆ เหล่านั้น

---

### 4.6
สำหรับ **Associative cache**

ที่อยู่ของหน่วยความจำหลักประกอบด้วย **2 ส่วน**

จงระบุและอธิบาย

---

### 4.7
สำหรับ **Set-associative cache**

ที่อยู่ของหน่วยความจำหลักประกอบด้วย **3 ส่วน**

จงระบุและอธิบาย

---

### 4.8
ความแตกต่างระหว่าง

- Spatial locality
- Temporal locality

คืออะไร

---

### 4.9
โดยทั่วไปมีวิธีการใดบ้างในการใช้ประโยชน์จาก

- Spatial locality
- Temporal locality

---

# โจทย์ (Problems)

---

## 4.1

แคชแบบ **set-associative** มี

```
64 lines
แบ่งเป็นชุดละ 4 lines
```

หน่วยความจำหลักมี

```
4K blocks
ขนาด block = 128 words
```

จงแสดง **รูปแบบของ address ของ main memory**

---

## 4.2

แคชแบบ **two-way set-associative** มี

```
ขนาดบล็อก = 16 bytes
ขนาดรวมของ cache = 8 KB
หน่วยความจำหลักเป็นแบบ byte addressable
```

จงแสดง **รูปแบบของ main memory address**

---

## 4.3

กำหนด address ของหน่วยความจำ

```
111111
666666
BBBBBB
```

(ในรูปแบบ hexadecimal)

จงแสดง

### a) รูปแบบ Direct-mapped cache

ส่วนประกอบ

```
Tag
Line
Word
```

---

### b) รูปแบบ Associative cache

ส่วนประกอบ

```
Tag
Word
```

---

### c) รูปแบบ Two-way set-associative

ส่วนประกอบ

```
Tag
Set
Word
```

---

## 4.4

จงระบุค่าต่อไปนี้

### a) สำหรับตัวอย่าง Direct cache

- ความยาวของ address  
- จำนวนหน่วยที่สามารถอ้างอิงได้  
- ขนาด block  
- จำนวน block ใน main memory  
- จำนวน lines ใน cache  
- ขนาดของ tag  

---

### b) สำหรับตัวอย่าง Associative cache

- ความยาวของ address  
- จำนวนหน่วยที่สามารถอ้างอิงได้  
- ขนาด block  
- จำนวน block ใน main memory  
- จำนวน lines ใน cache  
- ขนาดของ tag  

---

### c) สำหรับ Two-way set associative cache

- ความยาวของ address  
- จำนวนหน่วยที่สามารถอ้างอิงได้  
- ขนาด block  
- จำนวน block ใน main memory  
- จำนวน lines ใน cache  
- ขนาดของ tag  

---

## 4.5

ไมโครโปรเซสเซอร์ **32-bit** มี

```
On-chip cache = 16 KB
4-way set associative
Cache line size = 32 bytes
```

จงวาด **โครงสร้างของ address field** ที่ใช้ในการตรวจสอบ

```
Cache hit
Cache miss
```

---

## 4.6

External cache memory มี

```
Four-way set associative
Line size = 16 words
ขนาดรวม = 4K คำ (32-bit words)
```

Main memory ใช้

```
24-bit address
```

จงออกแบบโครงสร้าง cache และแสดงการตีความ address ของโปรเซสเซอร์

---

## 4.7

Intel **80486** มี on-chip unified cache

```
ขนาด 8 KB
4-way set associative
Line size = 32 bytes
```

Cache line ประกอบด้วย

```
Valid bit
Tag
LRU bits
```

จงวาด **แผนภาพ cache อย่างง่าย**

---

## 4.8

เครื่องคอมพิวเตอร์มี

```
Main memory = 2^16 bytes
Direct mapped cache
32 lines
```

### a)

address ขนาด **16 bits** ถูกแบ่งเป็น

```
Tag
Line number
Byte number
```

อย่างไร

---

### b)

ไบต์ของ address ต่อไปนี้จะถูกเก็บไว้ที่ใดใน cache

```
0001 0001 0001 1011
1100 0011 0011 0100
1101 0000 0001 1101
1010 0110 1010 1010
```

---

### c)

สมมติว่า byte

```
0010 1000 1001 0101
```

ถูกเก็บใน cache

ไบต์อื่นที่อยู่ใน block เดียวกันจะถูกเก็บที่ใด

---

### d)

Cache สามารถเก็บข้อมูลจากหน่วยความจำหลักได้ทั้งหมดกี่ไบต์

---

### e)

เหตุใดจึงต้องเก็บ **tag** ไว้ใน cache

---

## 4.9

อัลกอริทึม **cache replacement** ของ Intel 80486

Cache มี

```
128 sets
แต่ละ set มี 4 blocks
```

LRU bits

```
B0
B1
B2
```

### a)

แสดงขั้นตอนการอัปเดต B0 B1 B2

---

### b)

แสดงว่าอัลกอริทึมนี้ประมาณค่า **LRU replacement** ได้อย่างไร

---

### c)

พิสูจน์ว่าเป็น **LRU algorithm ที่แท้จริง**

---

## 4.10

ออกแบบ cache ที่มี

```
Block size = 16-bit words
Set size = 2
Cache size = 4096 words
Memory size = 64K × 32 bits
```

แสดง

```
โครงสร้าง cache
การตีความ address
```

---

## 4.11

ระบบหน่วยความจำมี

```
32-bit address
Cache line = 64 bytes
Tag field = 20 bits
```

จงหา

```
รูปแบบ address
จำนวนหน่วยที่อ้างอิงได้
จำนวน blocks ใน main memory
จำนวน lines ใน cache
ขนาด tag
```

---

## 4.12

คอมพิวเตอร์มี

```
Main memory = 1 MB
1 byte ต่อ address
Cache size = 16 KB
Block size = 64 bytes
```

### a)

จงระบุ

```
Tag
Line
Offset
```

สำหรับ **direct-mapped cache**

---

### b)

ยกตัวอย่าง **สอง address ที่แมปไปยังตำแหน่ง cache เดียวกัน**

---

### c)

สำหรับ address

```
F001
CABB
```

จงหา

```
Tag
Line
Offset
```

---

### d)

ทำซ้ำสำหรับ **two-way set associative cache**

---

## 4.13

เขียน **pseudo code**

สำหรับอัลกอริทึม

```
LRU replacement
```

ใน **four-way set-associative cache**

---

## 4.14

พิจารณา Example 4.3

คำตอบจะเปลี่ยนอย่างไรถ้า

```
Block transfer time = 30 ns
Access time = 5 ns
```

---

## 4.15

พิจารณาโค้ด

```c
for (i = 0; i < 20; i++)
    for (j = 0; j < 10; j++)
        a[i] = a[i] * j;
```

### a)

ยกตัวอย่าง **Spatial locality**

### b)

ยกตัวอย่าง **Temporal locality**

---

## 4.16

จงขยายสมการ

```
(4.2)
(4.3)
Appendix A
```

ให้รองรับ **N-level memory hierarchy**

---

## 4.17

ระบบมี

```
Main memory = 32K words (16-bit)
Cache = 4K words
Block size = 4 words
```

Cache เริ่มต้นว่าง

โปรเซสเซอร์อ่านข้อมูลจาก

```
0,1,2,...,4351
```

จงวิเคราะห์พฤติกรรมของ cache
---
# Cache Memory – คำศัพท์สำคัญ คำถามทบทวน และโจทย์

---

# คำศัพท์สำคัญ (Key Terms)

| คำศัพท์ | ความหมาย |
|-------|---------|
| Direct mapping | วิธีแมปที่บล็อกของหน่วยความจำหลักถูกกำหนดให้ไปยังตำแหน่ง cache เพียงตำแหน่งเดียว |
| High-performance computing (HPC) | การประมวลผลที่เน้นประสิทธิภาพสูง |
| Hit | ข้อมูลที่ต้องการพบอยู่ใน cache |
| Hit ratio | อัตราส่วนของการเข้าถึงหน่วยความจำที่พบข้อมูลใน cache |
| Instruction cache | cache ที่เก็บคำสั่งของโปรแกรม |
| L1 cache | cache ระดับที่ 1 ใกล้ CPU มากที่สุด |
| L2 cache | cache ระดับที่ 2 |
| L3 cache | cache ระดับที่ 3 |
| Line size | ขนาดของ cache line |
| Locality | แนวโน้มที่โปรแกรมจะเข้าถึงข้อมูลเดิมหรือข้อมูลใกล้เคียง |
| Logical cache | cache ที่อ้างอิงโดยใช้ logical address |
| Memory hierarchy | โครงสร้างลำดับชั้นของหน่วยความจำ |
| Miss | ไม่พบข้อมูลที่ต้องการใน cache |
| Multilevel cache | การใช้ cache หลายระดับ |
| Physical address | address จริงที่ใช้เข้าถึงหน่วยความจำ |
| Random access | การเข้าถึงตำแหน่งใดก็ได้ในเวลาเท่ากัน |
| Replacement algorithm | อัลกอริทึมที่ใช้เลือกบล็อกที่จะถูกแทนที่ใน cache |
| Secondary memory | หน่วยความจำสำรอง เช่น ฮาร์ดดิสก์ |
| Sequential access | การเข้าถึงข้อมูลตามลำดับ |
| Set-associative mapping | วิธีแมปแบบผสมระหว่าง direct และ associative |
| Spatial locality | การเข้าถึงข้อมูลที่อยู่ใกล้กัน |
| Split cache | การแยก cache สำหรับคำสั่งและข้อมูล |
| Tag | ตัวระบุที่ใช้ตรวจสอบว่าข้อมูลอยู่ใน cache หรือไม่ |
| Temporal locality | การเข้าถึงข้อมูลเดิมซ้ำในช่วงเวลาสั้น ๆ |
| Unified cache | cache เดียวใช้ทั้งคำสั่งและข้อมูล |
| Virtual address | address ที่ CPU สร้างก่อนแปลงเป็น physical address |
| Virtual cache | cache ที่ใช้ virtual address |
| Write-back | เขียนข้อมูลลง cache ก่อน แล้วค่อยอัปเดตหน่วยความจำหลัก |
| Write-through | เขียนข้อมูลลง cache และหน่วยความจำหลักพร้อมกัน |

---

# คำถามทบทวน (Review Questions)

### 4.1
ความแตกต่างระหว่าง

- Sequential access  
- Direct access  
- Random access  

คืออะไร

---

### 4.2
ความสัมพันธ์ระหว่าง

- เวลาเข้าถึงหน่วยความจำ (access time)
- ต้นทุนหน่วยความจำ
- ความจุหน่วยความจำ

เป็นอย่างไร

---

### 4.3
หลักการ **locality** เกี่ยวข้องกับการใช้ **memory หลายระดับ** อย่างไร

---

### 4.4
อธิบายความแตกต่างระหว่าง

- Direct mapping
- Associative mapping
- Set-associative mapping

---

### 4.5
สำหรับ **direct-mapped cache** address ของ main memory ประกอบด้วย 3 ส่วน

จงบอกและอธิบายส่วนเหล่านั้น

---

### 4.6
สำหรับ **associative cache** address ของ main memory ประกอบด้วย 2 ส่วน

จงบอกและอธิบาย

---

### 4.7
สำหรับ **set-associative cache** address ของ main memory ประกอบด้วย 3 ส่วน

จงบอกและอธิบาย

---

### 4.8
ความแตกต่างระหว่าง

- Spatial locality
- Temporal locality

คืออะไร

---

### 4.9
โดยทั่วไปมีวิธีใดบ้างในการใช้ประโยชน์จาก spatial และ temporal locality

---

# โจทย์ (Problems)

---

## 4.18

พิจารณา cache ที่มี

- 4 lines
- แต่ละ line ขนาด 16 bytes

Main memory ถูกแบ่งเป็น block ขนาด 16 bytes

ให้พิจารณาลำดับ address ต่อไปนี้

```
0,2,4,6,8,10,12,14
```

ลำดับนี้ถูกทำซ้ำ 10 ครั้ง

### a

ถ้า cache ใช้ **direct mapping**

คำนวณ **hit ratio**

### b

ถ้า cache ใช้ **two-way set associative**

คำนวณ **hit ratio**

---

## 4.19

กำหนดระบบหน่วยความจำ

```
Tc = 100 ns
Tm = 1200 ns
Cm = 10^-5 $/bit
```

จงหา

a) ราคาของ main memory ขนาด 1 MB  
b) ราคาของ 1 MB เมื่อใช้เทคโนโลยี cache  
c) เวลาเข้าถึงเฉลี่ยเมื่อ hit ratio = 0.95

---

## 4.20

ให้ L1 cache มี

```
Access time = 1 ns
Hit ratio = 0.95
```

a) ต้องมีค่า hit ratio เท่าไรเพื่อให้ performance ดีขึ้น 10%  

b) อธิบายเหตุผลว่าทำไมผลลัพธ์นี้จึงสมเหตุสมผล

---

## 4.21

พิจารณา cache ที่มี

```
Access time = 2.5 ns
Block transfer = 64 bytes
Hit ratio = 0.95
```

คำนวณ **average access time**

---

## 4.22

ระบบคอมพิวเตอร์มี

```
Cache access = 20 ns
Memory access = 60 ns
```

จงหา **เวลาเฉลี่ยในการเข้าถึงข้อมูล**

---

## 4.23

Cache line ขนาด

```
64 bytes
```

มี **dirty lines = 30%**

a) เมื่อ miss rate = 3%  
คำนวณ traffic ระหว่าง cache และ memory  

b) ทำซ้ำเมื่อใช้ **write-through policy**

c) ทำซ้ำเมื่อ **miss rate = 5%**

d) สรุปผลลัพธ์

---

## 4.24

Motorola **68040 cache**

ข้อมูลจาก memory ใช้เวลา

```
3 clock cycles
```

Clock rate

```
16.67 MHz
```

คำนวณ **memory cycle length**

---

## 4.25

Processor มี

```
Memory cycle time = 300 ns
Instruction rate = 1 MIPS
```

จงคำนวณ

- การใช้งาน bus
- ผลกระทบเมื่อมี cache และ miss ratio = 5%

---

## 4.26

สมการ performance ของ cache

```
Ta = Tc + (1 − H)Tm
```

โดย

```
Ta = average access time
Tc = cache access time
Tm = memory access time
H = hit ratio
```

จงปรับสมการเมื่อใช้

- write-through policy
- write-back policy

---

## 4.27

กำหนด

```
Tc = second-level cache access time
Tm = memory access time
H1 = hit ratio L1
H2 = combined hit ratio
```

จงหาสมการ performance ของระบบ

---

## 4.28

Cache read miss penalty = 1 clock cycle

จงคำนวณ miss penalty เมื่อ

a) line size = 1 word  
b) line size = 4 words (burst transfer)  
c) line size = 4 words (single transfer)

---

## 4.29

เมื่อเพิ่ม line size

miss ratio ลดจาก

```
3.2% → 1.1%
```

จงคำนวณ **average miss penalty**

---

# Appendix 4A  
# ประสิทธิภาพของหน่วยความจำสองระดับ

---

## Two-Level Memory

ระบบหน่วยความจำสองระดับประกอบด้วย

```
Processor
Cache
Main Memory
```

จุดประสงค์คือ

```
ลดเวลาในการเข้าถึงหน่วยความจำ
```

โดยใช้หลักการ **locality**

---

## หลักการ Locality

โปรแกรมมักเข้าถึงข้อมูลเป็นกลุ่ม

2 รูปแบบ

```
Temporal locality
Spatial locality
```

---

## เหตุผลที่โปรแกรมมี locality

1. โปรแกรมส่วนใหญ่ทำงานแบบ **ลำดับคำสั่ง**

2. โปรแกรมใช้ **procedure เดิมซ้ำ**

3. มี **loop** ที่ทำงานซ้ำหลายครั้ง

4. ข้อมูลมักอยู่ใน **array หรือ structure**

---

# ตารางลักษณะของ Two-Level Memory

| คุณสมบัติ | Cache + Main Memory | Virtual Memory | Disk Cache |
|----------|--------------------|---------------|-----------|
| เวลาเข้าถึง | 5:1 | 10:1 | 10:1 |
| การจัดการหน่วยความจำ | Hardware | Hardware + OS | Software |
| ขนาด block/page | 4–128 bytes | 64–4096 bytes | 64–4096 bytes |
| การเข้าถึงของ CPU | Direct | Indirect | Indirect |

---

# สรุป

ระบบหน่วยความจำหลายระดับช่วย

- ลดเวลาเข้าถึงหน่วยความจำ
- ใช้ประโยชน์จาก locality
- เพิ่มประสิทธิภาพของ CPU

ตัวอย่าง

```
Cache memory
Virtual memory
Disk cache
```

---
# Cache Memory – Dynamic Frequency, Locality และ Two-Level Memory

---

# Table 4.7 ความถี่สัมพัทธ์ของคำสั่งในภาษาโปรแกรมระดับสูง

| คำสั่ง | HUCK83 Pascal (Scientific) | KNUT71 FORTRAN (Student) | PAT182 Pascal (System) | PAT182 C (System) | TANEN78 SAL (System) |
|------|------|------|------|------|------|
| Assign | 74 | 67 | 45 | 38 | 42 |
| Loop | 4 | 3 | 5 | 3 | 4 |
| Call | 1 | 3 | 15 | 12 | 12 |
| IF | 20 | 11 | 29 | 43 | 36 |
| GOTO | 2 | 9 | – | 3 | – |
| Other | – | 7 | 6 | 1 | 6 |

---

# การศึกษาพฤติกรรมของโปรแกรม

แนวคิดนี้ได้รับการยืนยันจากการศึกษาหลายงานวิจัย  
โดยมีการวิเคราะห์พฤติกรรมของโปรแกรมภาษาโปรแกรมระดับสูง

การศึกษาต่าง ๆ เช่น

- **Knuth (1971)** วิเคราะห์โปรแกรม FORTRAN จากแบบฝึกหัดนักศึกษา  
- **Tanenbaum (1978)** วิเคราะห์โปรแกรมที่เขียนด้วยภาษา SAL  
- **Patterson และ Sequin (1982)** วิเคราะห์คอมไพเลอร์และโปรแกรม CAD  
- **Huck (1983)** วิเคราะห์โปรแกรมที่ใช้ในงานวิศวกรรม

ผลการศึกษาพบว่า

- คำสั่ง **branch และ call** มีสัดส่วนไม่มาก
- โปรแกรมจำนวนมากใช้ **รูปแบบการทำงานที่คล้ายกัน**

---

# พฤติกรรม Call-Return ของโปรแกรม

จากการศึกษา พบว่า

- โปรแกรมมี **call และ return ซ้ำ ๆ**
- การทำงานมักเกิดอยู่ใน **ช่วงของโค้ดเล็ก ๆ**

ตัวอย่างแสดงใน **Figure 4.20**

- แกนเวลาแสดงลำดับการเรียกฟังก์ชัน
- ค่า **w = 5** คือ window ของการสังเกต
- ค่า **d = 33** คือระยะเวลาของ call-return

ลักษณะสำคัญคือ

โปรแกรมมักทำงานอยู่ใน **ช่วงของ stack call ที่ใกล้เคียงกันเป็นเวลานาน**

---

# Locality (หลักการของ Locality)

Locality แบ่งเป็นสองประเภท

---

## Spatial Locality

หมายถึง

โปรแกรมมีแนวโน้มจะเข้าถึง **ตำแหน่งหน่วยความจำที่อยู่ใกล้กัน**

ตัวอย่าง

- การอ่าน array
- การอ่านคำสั่งโปรแกรมต่อเนื่อง

สาเหตุคือ

โปรแกรมมักเก็บข้อมูลที่เกี่ยวข้องไว้ในตำแหน่งใกล้กัน

---

## Temporal Locality

หมายถึง

โปรแกรมมีแนวโน้มจะเข้าถึง **ข้อมูลเดิมซ้ำในช่วงเวลาสั้น ๆ**

ตัวอย่าง

- ตัวแปรใน loop
- คำสั่งใน loop

เช่น

```c
for(i=0;i<100;i++)
```

ตัวแปร `i` จะถูกใช้งานซ้ำหลายครั้ง

---

# การใช้ประโยชน์จาก Locality

โดยทั่วไป

### Temporal locality

ใช้โดย

- เก็บข้อมูลที่ใช้ล่าสุดไว้ใน **cache**

---

### Spatial locality

ใช้โดย

- โหลด **block ของข้อมูลหลายคำพร้อมกัน**

หรือ

- ใช้ **prefetching**

ซึ่งคือการดึงข้อมูลที่คาดว่าจะถูกใช้ในอนาคตเข้ามาก่อน

---

# Operation of Two-Level Memory

หลักการ locality ทำให้เกิดแนวคิดของ **หน่วยความจำสองระดับ**

ประกอบด้วย

| ระดับ | คุณสมบัติ |
|------|------|
| M1 | ขนาดเล็ก เร็ว ราคาแพง (เช่น cache) |
| M2 | ขนาดใหญ่ ช้า ราคาถูก (เช่น main memory) |

---

## ขั้นตอนการทำงาน

1. CPU ตรวจสอบข้อมูลใน **M1 (cache)** ก่อน
2. ถ้าพบข้อมูล → **Hit**
3. ถ้าไม่พบ → **Miss**
4. ข้อมูลจะถูกโหลดจาก **M2 → M1**

เนื่องจาก locality

เมื่อโหลด block มาแล้ว  
มักจะมีการใช้งานข้อมูลใน block นั้นหลายครั้ง

---

# สมการ Average Access Time

เวลาเฉลี่ยในการเข้าถึงหน่วยความจำ

\[
T_a = H \times T_1 + (1-H) \times (T_1 + T_2)
\]

สามารถย่อเป็น

\[
T_a = T_1 + (1-H)T_2
\]

---

## ความหมายของตัวแปร

| ตัวแปร | ความหมาย |
|------|------|
| Ta | เวลาเฉลี่ยในการเข้าถึง |
| T1 | เวลาเข้าถึงหน่วยความจำระดับที่ 1 (cache) |
| T2 | เวลาเข้าถึงหน่วยความจำระดับที่ 2 (main memory / disk) |
| H | Hit ratio |

---

# ความสัมพันธ์ระหว่าง Hit Ratio และ Access Time

จาก **Figure 4.2**

ถ้า

```
Hit ratio สูง
```

เวลาเฉลี่ยจะใกล้เคียงกับ

```
T1 (cache access time)
```

มากกว่าหน่วยความจำระดับล่าง

---

# Performance ของ Two-Level Memory

นอกจากเวลาเข้าถึง  
ต้องพิจารณา **ต้นทุนของหน่วยความจำ**

---

## Average Cost per Bit

สมการต้นทุนเฉลี่ย

\[
C_a =
\frac{C_1 S_1 + C_2 S_2}
{S_1 + S_2}
\]

---

## ความหมายของตัวแปร

| ตัวแปร | ความหมาย |
|------|------|
| Ca | ต้นทุนเฉลี่ยต่อบิต |
| C1 | ต้นทุนต่อบิตของหน่วยความจำระดับบน |
| C2 | ต้นทุนต่อบิตของหน่วยความจำระดับล่าง |
| S1 | ขนาดของหน่วยความจำระดับบน |
| S2 | ขนาดของหน่วยความจำระดับล่าง |

---

โดยทั่วไป

```
C1 >> C2
```

ดังนั้น

```
S1 < S2
```

เพื่อให้ต้นทุนรวมต่ำ

---

# Figure 4.21

แสดงความสัมพันธ์ระหว่าง

```
Relative Memory Cost
กับ
Relative Memory Size
```

ผลลัพธ์คือ

เมื่อเพิ่มขนาดของหน่วยความจำระดับล่าง  
ต้นทุนเฉลี่ยของระบบจะลดลง

---

# สรุปแนวคิดสำคัญ

1️⃣ โปรแกรมมีพฤติกรรม **Locality**

2️⃣ Locality ทำให้ **Cache ทำงานได้ดี**

3️⃣ Two-Level Memory ช่วยให้

```
ได้ความเร็วใกล้ cache
แต่ขนาดใกล้ main memory
```

4️⃣ การออกแบบต้องพิจารณา

- Hit ratio
- Access time
- Memory cost

---

# Keywords

```
Locality
Spatial Locality
Temporal Locality
Two-Level Memory
Cache Memory
Average Access Time
Hit Ratio
Memory Hierarchy
```
# Cache Memory – Access Efficiency และ Hit Ratio

---

# การพิจารณาเวลาในการเข้าถึง (Access Time)

เพื่อให้ **หน่วยความจำสองระดับ (Two-Level Memory)** ช่วยเพิ่มประสิทธิภาพได้อย่างชัดเจน  
จำเป็นต้องมีเงื่อนไขดังนี้

```
T1 << T2
```

โดยที่

- **T1** = เวลาเข้าถึงหน่วยความจำระดับบน (เช่น Cache)
- **T2** = เวลาเข้าถึงหน่วยความจำระดับล่าง (เช่น Main Memory)

เนื่องจาก **T1 มีค่าน้อยกว่า T2 มาก**  
จึงต้องมี **Hit Ratio สูง**

---

## ข้อกำหนดสำคัญ

หน่วยความจำระดับบน **M1 ต้องมีขนาดเล็ก**

เพราะ

- ต้องการลด **ต้นทุน**
- แต่ยังต้องรักษา **Hit Ratio สูง**

ดังนั้นต้องตอบคำถามสำคัญสองข้อ

- Hit ratio ต้องเท่าไรจึงจะทำให้ `Ta ≈ T1`
- ขนาดของ M1 ต้องเท่าไรจึงจะได้ Hit ratio ที่ต้องการ

---

# Access Efficiency

กำหนดค่า

```
T1 / Ta
```

ซึ่งเรียกว่า **Access Efficiency**

เป็นตัววัดว่า

```
เวลาเฉลี่ยในการเข้าถึง (Ta)
ใกล้เคียงกับ
เวลาเข้าถึง cache (T1)
มากแค่ไหน
```

---

## สมการ Access Efficiency

จากสมการ (4.2)

```
T1 / Ta = 1 / ( 1 + (1 - H) (T2 / T1) )
```

โดยที่

| ตัวแปร | ความหมาย |
|------|------|
| Ta | เวลาเฉลี่ยในการเข้าถึง |
| T1 | เวลาเข้าถึง Cache |
| T2 | เวลาเข้าถึง Main Memory |
| H | Hit Ratio |

---

# Figure 4.22 – Access Efficiency vs Hit Ratio

กราฟแสดงความสัมพันธ์ระหว่าง

```
Access Efficiency
และ
Hit Ratio
```

โดยมีพารามิเตอร์

```
r = T2 / T1
```

---

## ค่าโดยทั่วไปของ r

### On-chip cache

เร็วกว่า main memory ประมาณ

```
25 – 50 เท่า
```

ดังนั้น

```
T2 / T1 ≈ 25 – 50
```

---

### Off-chip cache

เร็วกว่า main memory ประมาณ

```
5 – 15 เท่า
```

ดังนั้น

```
T2 / T1 ≈ 5 – 15
```

---

### Disk cache

เร็วกว่า disk ประมาณ

```
1000 เท่า
```

ดังนั้น

```
T2 / T1 ≈ 1000
```

---

### สรุป

เพื่อให้ระบบมีประสิทธิภาพ  
มักต้องการ

```
Hit ratio ≈ 0.9 หรือมากกว่า
```

---

# ผลของ Locality ต่อ Hit Ratio

คำถามต่อไปคือ

```
M1 ต้องใหญ่แค่ไหนจึงจะได้ Hit ratio สูง
```

คำตอบขึ้นอยู่กับ

- โปรแกรม
- การออกแบบ cache
- ระดับของ **locality**

---

# Figure 4.23 – Hit Ratio vs Memory Size

กราฟแสดงความสัมพันธ์ระหว่าง

```
Hit Ratio
กับ
Relative Memory Size (S1/S2)
```

---

## กรณีต่าง ๆ

### 1️⃣ No Locality

ถ้าไม่มี locality

```
การเข้าถึงหน่วยความจำเป็นแบบสุ่ม
```

ดังนั้น

```
Hit Ratio ≈ S1 / S2
```

---

### 2️⃣ Moderate Locality

มี locality ปานกลาง

Hit ratio จะสูงขึ้น  
แม้ cache จะมีขนาดเล็ก

---

### 3️⃣ Strong Locality

ถ้าโปรแกรมมี locality สูง

สามารถได้

```
Hit Ratio > 0.75
```

แม้ cache จะมีขนาดเล็กมาก

---

# ขนาด Cache ที่เหมาะสม

การศึกษาหลายงานวิจัยพบว่า

```
Cache ขนาด 1K – 128K words
```

มักเพียงพอสำหรับระบบส่วนใหญ่

ในขณะที่

```
Main Memory
```

อาจมีขนาดหลาย **Gigabytes**

---

# Virtual Memory และ Disk Cache

เมื่อพิจารณา

- Virtual Memory
- Disk Cache

จะพบว่า

```
M1 มีขนาดเล็กกว่า M2 มาก
```

แต่ยังสามารถให้ประสิทธิภาพดีได้  
เนื่องจาก **Locality**

---

# สรุปแนวคิดสำคัญ

## 1️⃣ เงื่อนไขสำคัญของ Two-Level Memory

```
T1 << T2
```

---

## 2️⃣ ต้องมี Hit Ratio สูง

```
H ≈ 0.9 หรือมากกว่า
```

---

## 3️⃣ Locality เป็นเหตุผลที่ทำให้ Cache ทำงานได้ดี

มีสองประเภท

```
Spatial Locality
Temporal Locality
```

---

## 4️⃣ Cache ไม่จำเป็นต้องใหญ่

แม้

```
Cache เล็ก
Main Memory ใหญ่
```

ก็ยังได้ประสิทธิภาพสูง

เพราะ

```
Program Locality
```

---

# Keywords

```
Cache Memory
Hit Ratio
Access Efficiency
Two-Level Memory
Locality
Temporal Locality
Spatial Locality
Memory Hierarchy
```

---

# หมายเหตุ

ถ้ามี

```
L2 Cache
L3 Cache
```

การวิเคราะห์จะซับซ้อนมากขึ้น  
เพราะระบบจะกลายเป็น

```
Multi-Level Cache
```

ซึ่งต้องใช้โมเดลวิเคราะห์ที่ละเอียดกว่า
