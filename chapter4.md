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




