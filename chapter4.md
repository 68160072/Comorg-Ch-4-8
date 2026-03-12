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
------------------------------------
Cache Memory (หน่วยความจำแคช)
บทที่ 4
สารบัญ
4.1 ภาพรวมระบบหน่วยความจำคอมพิวเตอร์
คุณลักษณะของระบบหน่วยความจำ
ลำดับชั้นของหน่วยความจำ
4.2 หลักการของหน่วยความจำแคช
4.3 องค์ประกอบของการออกแบบแคช
ที่อยู่แคช
ขนาดแคช
ฟังก์ชันการแมป
อัลกอริทึมการแทนที่
นโยบายการเขียน
ขนาดบรรทัด
จำนวนแคช
4.4 การจัดการแคชของ Pentium 4
4.5 คำสำคัญ คำถามทบทวน และโจทย์ปัญหา
ภาคผนวก 4A คุณลักษณะด้านประสิทธิภาพของหน่วยความจำสองระดับ
Locality
การทำงานของหน่วยความจำสองระดับ
ประสิทธิภาพ
วัตถุประสงค์การเรียนรู้
หลังจากศึกษาบทนี้แล้ว นักศึกษาควรสามารถ:

นำเสนอภาพรวมของคุณลักษณะหลักของระบบหน่วยความจำคอมพิวเตอร์ และการใช้ลำดับชั้นของหน่วยความจำ
อธิบายแนวคิดพื้นฐานและจุดประสงค์ของหน่วยความจำแคช
อภิปรายองค์ประกอบสำคัญของการออกแบบแคช
แยกแยะความแตกต่างระหว่าง Direct Mapping, Associative Mapping และ Set-Associative Mapping
อธิบายเหตุผลของการใช้แคชหลายระดับ
เข้าใจผลกระทบด้านประสิทธิภาพของหน่วยความจำหลายระดับ
4.1 ภาพรวมระบบหน่วยความจำคอมพิวเตอร์
แม้ว่าจะดูเรียบง่ายในแนวคิด แต่หน่วยความจำคอมพิวเตอร์มีความหลากหลายในด้านประเภท เทคโนโลยี การจัดโครงสร้าง ประสิทธิภาพ และราคามากที่สุดในบรรดาคุณลักษณะต่าง ๆ ของระบบคอมพิวเตอร์ ไม่มีเทคโนโลยีเดียวที่ดีที่สุดในการตอบสนองความต้องการหน่วยความจำของระบบคอมพิวเตอร์ ด้วยเหตุนี้ ระบบคอมพิวเตอร์ทั่วไปจึงติดตั้งลำดับชั้นของระบบย่อยหน่วยความจำ บางส่วนอยู่ภายในระบบ (เข้าถึงได้โดยตรงโดยโปรเซสเซอร์) และบางส่วนอยู่ภายนอก (เข้าถึงโดยโปรเซสเซอร์ผ่านโมดูล I/O)

บทนี้และบทถัดไปมุ่งเน้นไปที่องค์ประกอบหน่วยความจำภายใน ในขณะที่บทที่ 6 จะเน้นหน่วยความจำภายนอก ในการเริ่มต้น ส่วนแรกจะพิจารณาคุณลักษณะสำคัญของหน่วยความจำคอมพิวเตอร์ ส่วนที่เหลือของบทนี้จะพิจารณาองค์ประกอบสำคัญของระบบคอมพิวเตอร์สมัยใหม่ทั้งหมด นั่นคือ หน่วยความจำแคช

คุณลักษณะของระบบหน่วยความจำ
เรื่องหน่วยความจำคอมพิวเตอร์ที่ซับซ้อนจะจัดการได้ง่ายขึ้น หากเราจำแนกระบบหน่วยความจำตามคุณลักษณะสำคัญ คุณลักษณะที่สำคัญที่สุดแสดงอยู่ในตารางที่ 4.1

ตาราง 4.1 คุณลักษณะสำคัญของระบบหน่วยความจำคอมพิวเตอร์

Location (ตำแหน่ง)	Performance (ประสิทธิภาพ)
Internal เช่น processor registers, cache, main memory	Access time
External เช่น optical disks, magnetic disks, tapes	Cycle time
Transfer rate
Capacity (ความจุ)	Physical Type (ประเภททางกายภาพ)
Number of words	Semiconductor
Number of bytes	Magnetic
Optical
Unit of Transfer (หน่วยการถ่ายโอน)	Magneto-optical
Word	Physical Characteristics (คุณสมบัติทางกายภาพ)
Block	Volatile/Nonvolatile
Erasable/Nonerasable
Access Method (วิธีการเข้าถึง)	Organization (การจัดโครงสร้าง)
Sequential	Memory modules
Direct	
Random	
Associative	
คำว่า location ในตารางที่ 4.1 หมายถึงหน่วยความจำที่อยู่ภายในหรือภายนอกคอมพิวเตอร์ หน่วยความจำภายในมักถูกเรียกว่า main memory แต่ยังมีหน่วยความจำภายในรูปแบบอื่น โปรเซสเซอร์ต้องการหน่วยความจำเฉพาะในรูปของ register (เช่น ดูรูปที่ 2.3) นอกจากนี้ ส่วนควบคุมของโปรเซสเซอร์อาจต้องการหน่วยความจำภายในของตัวเองด้วย เราจะเลื่อนการอภิปรายหน่วยความจำภายในสองประเภทหลังนี้ไปที่บทต่อ ๆ ไป แคชเป็นหน่วยความจำภายในอีกรูปแบบหนึ่ง หน่วยความจำภายนอกประกอบด้วยอุปกรณ์จัดเก็บข้อมูลแบบ peripheral เช่น disk และ tape ซึ่งโปรเซสเซอร์เข้าถึงได้ผ่าน I/O controller

คุณลักษณะที่ชัดเจนอย่างหนึ่งของหน่วยความจำคือ ความจุ สำหรับหน่วยความจำภายใน มักแสดงเป็น byte (1 byte = 8 bits) หรือ word ความยาว word ที่พบทั่วไปคือ 8, 16 และ 32 บิต ความจุของหน่วยความจำภายนอกมักแสดงเป็น byte

แนวคิดที่เกี่ยวข้องคือ unit of transfer สำหรับหน่วยความจำภายใน unit of transfer เท่ากับจำนวนสายไฟฟ้าที่เข้าและออกจากโมดูลหน่วยความจำ ซึ่งอาจเท่ากับความยาว word แต่มักใหญ่กว่า เช่น 64, 128 หรือ 256 บิต เพื่อให้ชัดเจนขึ้น ลองพิจารณาสามแนวคิดที่เกี่ยวข้องสำหรับหน่วยความจำภายใน:

Word: หน่วยการจัดโครงสร้างของหน่วยความจำที่ "เป็นธรรมชาติ" ขนาด word โดยทั่วไปเท่ากับจำนวนบิตที่ใช้แทนจำนวนเต็มและความยาว instruction น่าเสียดายที่มีข้อยกเว้นมากมาย ตัวอย่างเช่น CRAY C90 (supercomputer รุ่นเก่า) มีความยาว word 64 บิตแต่ใช้การแทนค่าจำนวนเต็ม 46 บิต สถาปัตยกรรม Intel x86 มีความยาว instruction หลากหลายแบบ โดยแสดงเป็นทวีคูณของ byte และมีขนาด word 32 บิต

Addressable units: ในบางระบบ หน่วยที่ระบุที่อยู่ได้คือ word แต่หลายระบบอนุญาตให้ระบุที่อยู่ระดับ byte ในทุกกรณี ความสัมพันธ์ระหว่างความยาว A บิตของที่อยู่และจำนวน N ของหน่วยที่ระบุที่อยู่ได้คือ $2^A = N$

Unit of transfer: สำหรับ main memory คือจำนวนบิตที่อ่านออกหรือเขียนเข้าหน่วยความจำในแต่ละครั้ง unit of transfer ไม่จำเป็นต้องเท่ากับ word หรือ addressable unit สำหรับหน่วยความจำภายนอก ข้อมูลมักถ่ายโอนเป็นหน่วยที่ใหญ่กว่า word มาก เรียกว่า block

ความแตกต่างอีกอย่างระหว่างประเภทหน่วยความจำคือ วิธีการเข้าถึง หน่วยข้อมูล ซึ่งได้แก่:

Sequential access: หน่วยความจำถูกจัดเป็นหน่วยข้อมูลที่เรียกว่า record การเข้าถึงต้องทำตามลำดับ linear ที่กำหนด ข้อมูลที่อยู่ที่จัดเก็บไว้ใช้แยก record และช่วยในกระบวนการค้นหา ใช้กลไกอ่าน-เขียนร่วมกัน และต้องเคลื่อนจากตำแหน่งปัจจุบันไปยังตำแหน่งที่ต้องการ โดยผ่านและข้าม record กลาง ดังนั้น เวลาในการเข้าถึง record ใด ๆ จึงแปรผัน tape unit ที่อภิปรายในบทที่ 6 ใช้ sequential access

Direct access: เช่นเดียวกับ sequential access, direct access มีกลไกอ่าน-เขียนร่วมกัน แต่ละ block หรือ record มีที่อยู่เฉพาะตามตำแหน่งทางกายภาพ การเข้าถึงทำโดยเข้าถึงตรงไปยังบริเวณใกล้เคียงก่อน แล้วค้นหาตามลำดับ นับหรือรอเพื่อให้ถึงตำแหน่งสุดท้าย เวลาการเข้าถึงยังคงแปรผัน Disk unit ที่อภิปรายในบทที่ 6 ใช้ direct access

Random access: แต่ละตำแหน่งที่ระบุที่อยู่ได้ใน memory มีกลไกระบุที่อยู่เฉพาะที่เชื่อมต่อไฟฟ้าแบบ unique เวลาในการเข้าถึงตำแหน่งที่กำหนดเป็นอิสระจากลำดับการเข้าถึงก่อนหน้าและคงที่ ดังนั้นตำแหน่งใดก็ได้สามารถเลือกแบบสุ่มและเข้าถึงโดยตรงได้ Main memory และบางระบบ cache ใช้ random access

Associative: เป็น random access ประเภทหนึ่งที่ช่วยให้สามารถเปรียบเทียบตำแหน่งบิตที่ต้องการใน word สำหรับการจับคู่ที่กำหนด และทำสิ่งนี้กับทุก word พร้อมกัน ดังนั้น word จะถูกค้นหาตามส่วนหนึ่งของเนื้อหา ไม่ใช่ที่อยู่ เช่นเดียวกับ random-access memory ทั่วไป แต่ละตำแหน่งมีกลไกระบุที่อยู่ของตัวเอง และเวลาการค้นหาคงที่ไม่ขึ้นกับตำแหน่งหรือรูปแบบการเข้าถึงก่อนหน้า Cache memory อาจใช้ associative access

จากมุมมองของผู้ใช้ คุณลักษณะที่สำคัญที่สุดสองอย่างของหน่วยความจำคือ ความจุ และ ประสิทธิภาพ พารามิเตอร์ประสิทธิภาพสามตัวถูกใช้:

Access time (latency): สำหรับ random-access memory คือเวลาที่ใช้ในการอ่านหรือเขียน กล่าวคือเวลาตั้งแต่ที่อยู่ถูกส่งไปยังหน่วยความจำจนถึงข้อมูลถูกจัดเก็บหรือพร้อมใช้งาน สำหรับ non-random-access memory, access time คือเวลาที่ใช้ในการวางกลไกอ่าน-เขียนไปยังตำแหน่งที่ต้องการ

Memory cycle time: แนวคิดนี้ใช้กับ random-access memory เป็นหลัก ประกอบด้วย access time บวกกับเวลาเพิ่มเติมที่จำเป็นก่อนที่จะเริ่มการเข้าถึงครั้งที่สองได้ เวลาเพิ่มเติมนี้อาจจำเป็นสำหรับการแกว่งบน signal lines หรือเพื่อฟื้นฟูข้อมูลหากถูกอ่านแบบทำลาย โปรดสังเกตว่า memory cycle time เกี่ยวข้องกับ system bus ไม่ใช่โปรเซสเซอร์

Transfer rate: คืออัตราที่ข้อมูลสามารถถ่ายโอนเข้าหรือออกจากหน่วยความจำ สำหรับ random-access memory เท่ากับ 1/(cycle time) สำหรับ non-random-access memory สูตรดังนี้:

$$T_n = T_A + \frac{n}{R} \quad (4.1)$$

โดยที่:

$T_n$ = เวลาเฉลี่ยในการอ่านหรือเขียน n บิต
$T_A$ = เวลาเข้าถึงเฉลี่ย
$n$ = จำนวนบิต
$R$ = Transfer rate เป็น bits per second (bps)
ประเภททางกายภาพ ของหน่วยความจำที่ใช้มีหลายชนิด ที่พบมากที่สุดในปัจจุบันคือ semiconductor memory, magnetic surface memory (ใช้สำหรับ disk และ tape) และ optical และ magneto-optical

คุณสมบัติทางกายภาพ ของการจัดเก็บข้อมูลที่สำคัญ ได้แก่: ใน volatile memory ข้อมูลจะสูญหายตามธรรมชาติหรือหายไปเมื่อปิดไฟ ใน nonvolatile memory ข้อมูลที่บันทึกแล้วจะคงอยู่โดยไม่เสื่อมสภาพจนกว่าจะตั้งใจเปลี่ยน ไม่จำเป็นต้องมีไฟเลี้ยงเพื่อเก็บข้อมูล Magnetic-surface memory เป็น nonvolatile Semiconductor memory อาจเป็น volatile หรือ nonvolatile Nonerasable memory ไม่สามารถแก้ไขได้ ยกเว้นการทำลายหน่วยเก็บ Semiconductor memory ประเภทนี้เรียกว่า read-only memory (ROM) จำเป็นที่ nonerasable memory ต้องเป็น nonvolatile ด้วย

สำหรับ random-access memory organization เป็นประเด็นสำคัญในการออกแบบ ในบริบทนี้ organization หมายถึงการจัดเรียงบิตทางกายภาพเพื่อสร้าง word การจัดเรียงที่ชัดเจนไม่ใช่สิ่งที่ใช้เสมอไป ดังอธิบายในบทที่ 5

ลำดับชั้นของหน่วยความจำ
ข้อจำกัดในการออกแบบหน่วยความจำคอมพิวเตอร์สรุปได้ด้วยสามคำถาม: มากแค่ไหน? เร็วแค่ไหน? แพงแค่ไหน?

คำถามเรื่องมากแค่ไหนค่อนข้างเปิดกว้าง หากความจุมีเพียงพอ แอปพลิเคชันก็มักจะถูกพัฒนามาใช้งาน คำถามเรื่องเร็วแค่ไหนตอบได้ง่ายกว่าในแง่หนึ่ง เพื่อให้ได้ประสิทธิภาพสูงสุด หน่วยความจำต้องตามทัน processor นั่นคือ ขณะที่ processor กำลังประมวลผล instruction เราไม่ต้องการให้มันหยุดรอ instruction หรือ operand คำถามสุดท้ายต้องพิจารณาด้วย สำหรับระบบที่ใช้งานได้จริง ราคาของหน่วยความจำต้องสมเหตุสมผลเมื่อเทียบกับส่วนประกอบอื่น

ตามที่คาดไว้ มีการแลกเปลี่ยนระหว่างคุณลักษณะสำคัญสามอย่างของหน่วยความจำ ได้แก่ ความจุ เวลาเข้าถึง และราคา เทคโนโลยีหลากหลายถูกนำมาใช้ในการสร้างระบบหน่วยความจำ และในสเปกตรัมของเทคโนโลยีเหล่านี้ ความสัมพันธ์ต่อไปนี้เป็นจริง:

เวลาเข้าถึงเร็วขึ้น ราคาต่อบิตสูงขึ้น
ความจุมากขึ้น ราคาต่อบิตลดลง
ความจุมากขึ้น เวลาเข้าถึงช้าลง
ปัญหาที่นักออกแบบเผชิญชัดเจน นักออกแบบต้องการใช้เทคโนโลยีหน่วยความจำที่มีความจุขนาดใหญ่ ทั้งเพราะต้องการความจุนั้นและเพราะราคาต่อบิตต่ำ อย่างไรก็ตาม เพื่อตอบสนองความต้องการด้านประสิทธิภาพ นักออกแบบต้องใช้หน่วยความจำที่แพงและมีความจุค่อนข้างต่ำที่มีเวลาเข้าถึงสั้น

ทางออกจากปัญหานี้คือไม่พึ่งพาส่วนประกอบหน่วยความจำหรือเทคโนโลยีเดียว แต่ใช้ memory hierarchy ลำดับชั้นทั่วไปแสดงในรูปที่ 4.1 เมื่อลงมาตามลำดับชั้น สิ่งต่อไปนี้เกิดขึ้น:

a. ราคาต่อบิตลดลง b. ความจุเพิ่มขึ้น c. เวลาเข้าถึงช้าลง d. ความถี่ที่โปรเซสเซอร์เข้าถึงหน่วยความจำลดลง

           ┌─────────────┐
           │  Registers  │
           └──────┬──────┘
           ┌──────▼──────┐    ← Inboard memory
           │    Cache    │
           └──────┬──────┘
           ┌──────▼──────┐
           │ Main Memory │
           └──────┬──────┘
           ┌──────▼──────┐    ← Outboard storage
           │ Magnetic    │
           │ disk        │
           │ CD-ROM      │
           │ CD-RW       │
           │ DVD-RW      │
           │ DVD-RAM     │
           │ Blu-Ray     │
           └──────┬──────┘
           ┌──────▼──────┐    ← Off-line storage
           │ Magnetic    │
           │ tape        │
           └─────────────┘
รูปที่ 4.1 The Memory Hierarchy

ดังนั้น หน่วยความจำที่เล็กกว่า แพงกว่า และเร็วกว่าจะถูกเสริมด้วยหน่วยความจำที่ใหญ่กว่า ถูกกว่า และช้ากว่า กุญแจสำคัญสู่ความสำเร็จของการจัดองค์กรนี้คือข้อ (d): ความถี่ในการเข้าถึงที่ลดลง เราจะศึกษาแนวคิดนี้อย่างละเอียดมากขึ้นเมื่ออภิปรายถึงแคชในภายหลังในบทนี้ และ virtual memory ในบทที่ 8 การอธิบายสั้น ๆ มีดังนี้

การใช้หน่วยความจำสองระดับเพื่อลดเวลาเข้าถึงเฉลี่ยทำงานได้ในหลักการ แต่ต่อเมื่อเงื่อนไข (a) ถึง (d) เป็นจริง ด้วยการใช้เทคโนโลยีหลากหลาย จึงมีสเปกตรัมของระบบหน่วยความจำที่ตอบสนองเงื่อนไข (a) ถึง (c) โชคดีที่เงื่อนไข (d) ก็เป็นจริงในทั่วไปด้วย

พื้นฐานของความถูกต้องของเงื่อนไข (d) คือหลักการที่เรียกว่า locality of reference [DENN68] ในระหว่างการทำงานของโปรแกรม การอ้างอิงหน่วยความจำโดยโปรเซสเซอร์ ทั้งสำหรับ instruction และข้อมูล มีแนวโน้มที่จะกระจุกตัว โปรแกรมทั่วไปมี iterative loop และ subroutine จำนวนหนึ่ง เมื่อเข้าสู่ loop หรือ subroutine จะมีการอ้างอิงซ้ำ ๆ ไปยังชุด instruction เล็ก ๆ ในทำนองเดียวกัน การทำงานบน table และ array เกี่ยวข้องกับการเข้าถึงชุด data word ที่อยู่ใกล้กัน ในช่วงเวลายาว กลุ่มที่ใช้งานจะเปลี่ยนแปลง แต่ในช่วงเวลาสั้น โปรเซสเซอร์ส่วนใหญ่ทำงานกับกลุ่มการอ้างอิงหน่วยความจำที่คงที่

ดังนั้นจึงเป็นไปได้ที่จะจัดระเบียบข้อมูลในลำดับชั้นเพื่อให้เปอร์เซ็นต์ของการเข้าถึงในแต่ละระดับที่ต่ำลงน้อยกว่าระดับด้านบนมาก พิจารณาตัวอย่างสองระดับที่นำเสนอไปแล้ว

ตัวอย่าง 4.1
สมมติว่าโปรเซสเซอร์มีหน่วยความจำสองระดับ ระดับ 1 มี 1,000 word และมี access time เท่ากับ 0.01 μs ระดับ 2 มี 100,000 word และมี access time เท่ากับ 0.1 μs สมมติว่าถ้า word ที่จะเข้าถึงอยู่ใน level 1 โปรเซสเซอร์จะเข้าถึงโดยตรง ถ้าอยู่ใน level 2 word นั้นจะถูกถ่ายโอนไปยัง level 1 ก่อนแล้วจึงถูกเข้าถึงโดยโปรเซสเซอร์ เพื่อความเรียบง่าย เราละเว้นเวลาที่โปรเซสเซอร์ใช้ตรวจสอบว่า word อยู่ใน level 1 หรือ level 2 รูปที่ 4.2 แสดงรูปร่างทั่วไปของเส้นโค้งสำหรับสถานการณ์นี้ รูปแสดงเวลาเข้าถึงเฉลี่ยสำหรับหน่วยความจำสองระดับเป็นฟังก์ชันของ hit ratio H โดยที่ H คือเศษส่วนของการเข้าถึงหน่วยความจำทั้งหมดที่พบใน faster memory (เช่น cache), $T_1$ คือ access time ของ level 1 และ $T_2$ คือ access time ของ level 2 ดังที่เห็น สำหรับเปอร์เซ็นต์การเข้าถึง level 1 สูง เวลาเข้าถึงเฉลี่ยรวมจะใกล้เคียงกับ level 1 มากกว่า level 2

ในตัวอย่างของเรา สมมติว่า 95% ของการเข้าถึงหน่วยความจำพบใน level 1 เวลาเฉลี่ยในการเข้าถึง word แสดงได้ดังนี้:

$$(0.95)(0.01\ \mu s) + (0.05)(0.01\ \mu s + 0.1\ \mu s) = 0.0095 + 0.0055 = 0.015\ \mu s$$

เวลาเข้าถึงเฉลี่ยใกล้เคียงกับ 0.01 μs มากกว่า 0.1 μs ตามที่ต้องการ

ดังนั้นจึงเป็นไปได้ที่จะจัดระเบียบข้อมูลในลำดับชั้นเพื่อให้เปอร์เซ็นต์ของการเข้าถึงในแต่ละระดับที่ต่ำลงน้อยกว่าระดับด้านบนมาก สมมติให้ level 2 เก็บ instruction และข้อมูลของโปรแกรมทั้งหมด กลุ่มปัจจุบันสามารถวางไว้ใน level 1 ชั่วคราว เป็นครั้งคราว หนึ่งในกลุ่มใน level 1 จะต้องสลับกลับไปยัง level 2 เพื่อเปิดที่ว่างให้กลุ่มใหม่ที่เข้ามาใน level 1 แต่โดยเฉลี่ยแล้ว การอ้างอิงส่วนใหญ่จะไปยัง instruction และข้อมูลที่อยู่ใน level 1

หลักการนี้สามารถนำไปใช้กับหน่วยความจำมากกว่าสองระดับ ดังแนะในลำดับชั้นที่แสดงในรูปที่ 4.1 หน่วยความจำที่เร็วที่สุด เล็กที่สุด และแพงที่สุดประกอบด้วย register ภายในโปรเซสเซอร์ โดยทั่วไปโปรเซสเซอร์จะมีหลายสิบ register แม้ว่าบางเครื่องจะมีหลายร้อยตัว Main memory เป็นระบบหน่วยความจำภายในหลักของคอมพิวเตอร์ แต่ละตำแหน่งใน main memory มีที่อยู่เฉพาะ Main memory มักถูกขยายด้วย cache ที่มีความเร็วสูงกว่าและเล็กกว่า Cache มักไม่ปรากฏให้โปรแกรมเมอร์เห็น หรือแม้แต่โปรเซสเซอร์ด้วยซ้ำ เป็นอุปกรณ์สำหรับ staging การเคลื่อนย้ายข้อมูลระหว่าง main memory กับ processor register เพื่อปรับปรุงประสิทธิภาพ

หน่วยความจำสามรูปแบบที่กล่าวมา โดยทั่วไปเป็น volatile และใช้เทคโนโลยี semiconductor การใช้สามระดับใช้ประโยชน์จากข้อเท็จจริงที่ว่า semiconductor memory มีหลายประเภทที่แตกต่างกันในด้านความเร็วและราคา ข้อมูลถูกจัดเก็บอย่างถาวรมากขึ้นในอุปกรณ์จัดเก็บข้อมูลมวลภายนอก ซึ่งที่พบบ่อยที่สุดคือ hard disk และสื่อแบบถอดออกได้ เช่น removable magnetic disk, tape และ optical storage หน่วยความจำภายนอก nonvolatile ยังเรียกว่า secondary memory หรือ auxiliary memory ใช้จัดเก็บไฟล์โปรแกรมและข้อมูล และมักปรากฏให้โปรแกรมเมอร์เห็นในรูปของไฟล์และ record เท่านั้น ไม่ใช่ byte หรือ word แต่ละตัว Disk ยังใช้ขยาย main memory ที่เรียกว่า virtual memory ซึ่งอภิปรายในบทที่ 8

หน่วยความจำรูปแบบอื่นอาจรวมอยู่ในลำดับชั้นด้วย ตัวอย่างเช่น IBM mainframe ขนาดใหญ่รวมถึงหน่วยความจำภายในที่เรียกว่า expanded storage ซึ่งใช้เทคโนโลยี semiconductor ที่ช้ากว่าและถูกกว่า main memory อย่างเคร่งครัดแล้ว หน่วยความจำนี้ไม่ได้อยู่ในลำดับชั้นแต่เป็นกิ่งข้าง: ข้อมูลสามารถเคลื่อนย้ายระหว่าง main memory กับ expanded storage แต่ไม่ใช่ระหว่าง expanded storage กับ external memory หน่วยความจำสำรองรูปแบบอื่นรวมถึง optical และ magneto-optical disk สุดท้าย ระดับเพิ่มเติมสามารถเพิ่มในลำดับชั้นด้วย software ได้ ส่วนหนึ่งของ main memory สามารถใช้เป็น buffer เพื่อเก็บข้อมูลชั่วคราวที่จะถ่ายโอนออกไปยัง disk เทคนิคนี้ บางครั้งเรียกว่า disk cache จะปรับปรุงประสิทธิภาพสองวิธี:

การเขียนลง Disk ถูกรวมกลุ่ม แทนที่จะถ่ายโอนข้อมูลจำนวนเล็กน้อยหลายครั้ง เราถ่ายโอนข้อมูลจำนวนมากน้อยครั้ง ซึ่งปรับปรุงประสิทธิภาพ disk และลดการมีส่วนร่วมของโปรเซสเซอร์
ข้อมูลบางส่วนที่กำหนดให้เขียนออกอาจถูกอ้างอิงโดยโปรแกรมก่อนที่จะ dump ลง disk ครั้งถัดไป ในกรณีนั้น ข้อมูลจะถูกเรียกคืนอย่างรวดเร็วจาก software cache แทนที่จะช้าจาก disk
ภาคผนวก 4A ศึกษาผลกระทบด้านประสิทธิภาพของโครงสร้างหน่วยความจำหลายระดับ

4.2 หลักการของหน่วยความจำแคช
หน่วยความจำแคชถูกออกแบบมาเพื่อรวมเวลาการเข้าถึงหน่วยความจำของ หน่วยความจำที่แพงและความเร็วสูงเข้ากับขนาดหน่วยความจำขนาดใหญ่ของหน่วยความจำที่ถูกกว่าและช้ากว่า แนวคิดนี้แสดงในรูปที่ 4.3a มี main memory ขนาดค่อนข้างใหญ่และช้า พร้อมกับ cache memory ขนาดเล็กและเร็ว แคชมีสำเนาบางส่วนของ main memory เมื่อโปรเซสเซอร์พยายามอ่าน word จากหน่วยความจำ จะมีการตรวจสอบว่า word นั้นอยู่ใน cache หรือไม่ ถ้ามี word จะถูกส่งให้โปรเซสเซอร์ ถ้าไม่มี block ของ main memory ที่ประกอบด้วยจำนวน word คงที่จะถูกอ่านเข้า cache แล้ว word จะถูกส่งให้โปรเซสเซอร์ เนื่องจากปรากฏการณ์ locality of reference เมื่อ block ของข้อมูลถูกดึงเข้า cache เพื่อตอบสนองการอ้างอิงหน่วยความจำครั้งเดียว มีความเป็นไปได้สูงที่จะมีการอ้างอิงในอนาคตไปยังตำแหน่งหน่วยความจำเดียวกันหรือ word อื่น ๆ ใน block นั้น

รูปที่ 4.3b แสดงการใช้ cache หลายระดับ L2 cache ช้ากว่าและโดยทั่วไปใหญ่กว่า L1 cache และ L3 cache ช้ากว่าและโดยทั่วไปใหญ่กว่า L2 cache

รูปที่ 4.4 แสดงโครงสร้างของระบบ cache/main memory Main memory มี word ที่สามารถระบุที่อยู่ได้ถึง $2^n$ word โดยแต่ละ word มีที่อยู่ n บิตเฉพาะ สำหรับวัตถุประสงค์การแมป หน่วยความจำนี้ถือว่าประกอบด้วย block ความยาวคงที่จำนวนหนึ่ง โดยแต่ละ block มี K word นั่นคือมี M = 2^n/K block ใน main memory แคชประกอบด้วย m block เรียกว่า line

แต่ละ line มี K word บวก tag สองสามบิต แต่ละ line ยังมี control bits (ไม่แสดง) เช่น bit เพื่อระบุว่า line ถูกแก้ไขตั้งแต่โหลดเข้า cache หรือไม่ ความยาวของ line ไม่รวม tag และ control bits คือ line size line size อาจเล็กถึง 32 บิต โดยแต่ละ "word" เป็น byte เดียว ในกรณีนี้ line size คือ 4 byte จำนวน line น้อยกว่าจำนวน main memory block มาก (m << M) ในช่วงเวลาใดก็ตาม subset ของ block ใน memory จะอยู่ใน line ของ cache ถ้า word ใน block ของ memory ถูกอ่าน block นั้นจะถูกถ่ายโอนไปยังหนึ่งใน line ของ cache เนื่องจากมี block มากกว่า line แต่ละ line จึงไม่สามารถเฉพาะเจาะจงและถาวรสำหรับ block ใดในกลุ่มได้ ดังนั้น แต่ละ line จึงมี tag ที่ระบุว่า block ใดกำลังถูกจัดเก็บอยู่ tag มักเป็นส่วนหนึ่งของที่อยู่ main memory ดังอธิบายในส่วนนี้ในภายหลัง

รูปที่ 4.5 แสดงการทำงานของ read operation โปรเซสเซอร์สร้าง read address (RA) ของ word ที่จะอ่าน ถ้า word อยู่ใน cache จะถูกส่งให้โปรเซสเซอร์ มิฉะนั้น block ที่มี word นั้นจะถูกโหลดเข้า cache และ word จะถูกส่งให้โปรเซสเซอร์ รูปที่ 4.5 แสดงสองการทำงานสุดท้ายที่เกิดขึ้นพร้อมกัน และสะท้อนองค์กรที่แสดงในรูปที่ 4.6 ซึ่งเป็นตัวแทนของ cache organization ร่วมสมัย ในองค์กรนี้ cache เชื่อมต่อกับโปรเซสเซอร์ผ่าน data, control และ address lines data และ address lines ยังเชื่อมต่อกับ data และ address buffer ซึ่งเชื่อมต่อกับ system bus ที่ใช้ถึง main memory

เมื่อเกิด cache hit, data และ address buffer จะถูก disable และการสื่อสารจะอยู่ระหว่างโปรเซสเซอร์กับ cache เท่านั้น โดยไม่มี system bus traffic เมื่อเกิด cache miss ที่อยู่ที่ต้องการจะถูกโหลดบน system bus และข้อมูลจะถูกส่งกลับผ่าน data buffer ไปยังทั้ง cache และโปรเซสเซอร์ ในองค์กรอื่น cache จะถูกวางแบบ physically ระหว่างโปรเซสเซอร์กับ main memory สำหรับ data, address และ control lines ทั้งหมด ในกรณีหลังสำหรับ cache miss word ที่ต้องการจะถูกอ่านเข้า cache ก่อนแล้วจึงถ่ายโอนจาก cache ไปยังโปรเซสเซอร์

การอภิปรายเกี่ยวกับพารามิเตอร์ประสิทธิภาพที่เกี่ยวข้องกับการใช้งาน cache อยู่ในภาคผนวก 4A

4.3 องค์ประกอบของการออกแบบแคช
ส่วนนี้ให้ภาพรวมของพารามิเตอร์การออกแบบแคชและรายงานผลลัพธ์ทั่วไปบางส่วน เราอ้างถึงการใช้ cache ใน high-performance computing (HPC) เป็นครั้งคราว HPC เกี่ยวข้องกับ supercomputer และซอฟต์แวร์ของมัน โดยเฉพาะสำหรับแอปพลิเคชัน scientific ที่เกี่ยวข้องกับข้อมูลจำนวนมาก การคำนวณแบบ vector และ matrix และการใช้ parallel algorithm การออกแบบ cache สำหรับ HPC แตกต่างอย่างมากจากแพลตฟอร์มฮาร์ดแวร์และแอปพลิเคชันอื่น ๆ แท้จริงแล้ว นักวิจัยหลายคนพบว่า HPC application ทำงานได้ไม่ดีบนสถาปัตยกรรมคอมพิวเตอร์ที่ใช้ cache [BAIL93] นักวิจัยคนอื่นได้แสดงให้เห็นตั้งแต่นั้นว่า cache hierarchy สามารถมีประโยชน์ในการปรับปรุงประสิทธิภาพได้ หาก application software ได้รับการปรับให้ใช้ประโยชน์จาก cache [WANG99, PRES01]

แม้ว่าจะมี cache implementation จำนวนมาก แต่มี design element พื้นฐานไม่กี่อย่างที่ใช้ในการจำแนกและแยกแยะ cache architecture ตารางที่ 4.2 แสดง key element

ตาราง 4.2 องค์ประกอบของการออกแบบแคช

Cache Addresses	Write Policy
Logical	Write through
Physical	Write back
Cache Size	Line Size
Mapping Function	Number of Caches
Direct	Single or two level
Associative	Unified or split
Set associative	
Replacement Algorithm	
Least recently used (LRU)	
First in first out (FIFO)	
Least frequently used (LFU)	
Random	
ที่อยู่แคช
โปรเซสเซอร์ที่ไม่ใช่ embedded เกือบทั้งหมด และ embedded processor จำนวนมาก รองรับ virtual memory ซึ่งเป็นแนวคิดที่อภิปรายในบทที่ 8 โดยพื้นฐานแล้ว virtual memory คือ facility ที่อนุญาตให้โปรแกรมระบุที่อยู่หน่วยความจำจากมุมมองเชิง logic โดยไม่คำนึงถึงปริมาณ main memory ที่มีจริง เมื่อใช้ virtual memory field ที่อยู่ใน machine instruction จะมี virtual address สำหรับการอ่านและเขียนจาก main memory hardware memory management unit (MMU) จะแปลที่อยู่ virtual แต่ละตัวเป็น physical address ใน main memory

เมื่อใช้ virtual address นักออกแบบระบบอาจเลือกวาง cache ระหว่างโปรเซสเซอร์กับ MMU หรือระหว่าง MMU กับ main memory (รูปที่ 4.7) Logical cache หรือที่เรียกว่า virtual cache จัดเก็บข้อมูลโดยใช้ virtual address โปรเซสเซอร์เข้าถึง cache โดยตรงโดยไม่ผ่าน MMU Physical cache จัดเก็บข้อมูลโดยใช้ physical address ของ main memory

ข้อได้เปรียบที่ชัดเจนของ logical cache คือ cache access speed เร็วกว่า physical cache เนื่องจาก cache สามารถตอบสนองก่อนที่ MMU จะทำการแปลที่อยู่ ข้อเสียเกี่ยวข้องกับข้อเท็จจริงที่ว่า virtual memory system ส่วนใหญ่จัดหา virtual memory address space เดียวกันให้กับแต่ละแอปพลิเคชัน กล่าวคือ แต่ละแอปพลิเคชันเห็น virtual memory ที่เริ่มต้นที่ที่อยู่ 0 ดังนั้น virtual address เดียวกันในแอปพลิเคชันสองตัวที่แตกต่างกันจึงอ้างถึง physical address สองตัวที่แตกต่างกัน ดังนั้น cache memory จะต้อง flush อย่างสมบูรณ์กับการสลับ application context แต่ละครั้ง หรือต้องเพิ่ม bit พิเศษให้กับแต่ละ line ของ cache เพื่อระบุว่า virtual address space ใดที่ที่อยู่นี้อ้างถึง

เรื่องของ logical versus physical cache เป็นเรื่องซับซ้อน และเกินขอบเขตของหนังสือเล่มนี้ สำหรับการอภิปรายเชิงลึกมากขึ้น ดู [CEKL97] และ [JACO08]

ขนาดแคช
item ที่สองในตารางที่ 4.2 ขนาด cache ได้อภิปรายไปแล้ว เราต้องการให้ขนาดของ cache เล็กพอที่ราคาต่อบิตเฉลี่ยรวมใกล้เคียงกับ main memory เพียงอย่างเดียว และใหญ่พอที่เวลาเข้าถึงเฉลี่ยรวมใกล้เคียงกับ cache เพียงอย่างเดียว มีแรงจูงใจอีกหลายอย่างในการลดขนาด cache ยิ่ง cache ใหญ่ขึ้น ยิ่งมี gate จำนวนมากขึ้นในการระบุที่อยู่ cache ผลลัพธ์คือ cache ขนาดใหญ่มีแนวโน้มช้ากว่า cache ขนาดเล็กเล็กน้อย แม้แต่เมื่อสร้างด้วยเทคโนโลยี integrated circuit เดียวกันและวางไว้ในที่เดียวกันบนชิปและ circuit board พื้นที่ชิปและ board ที่มีอยู่ยังจำกัดขนาด cache ด้วย เนื่องจากประสิทธิภาพของ cache ขึ้นอยู่กับ workload มาก จึงเป็นไปไม่ได้ที่จะหาขนาด cache ที่ "เหมาะสมที่สุด" เพียงขนาดเดียว ตารางที่ 4.3 แสดงขนาด cache ของโปรเซสเซอร์บางตัวในปัจจุบันและอดีต

ตาราง 4.3 ขนาดแคชของโปรเซสเซอร์บางตัว

Processor	Type	Year of Introduction	L1 Cache	L2 Cache	L3 Cache
IBM 360/85	Mainframe	1968	16–32 kB	—	—
PDP-11/70	Minicomputer	1975	1 kB	—	—
VAX 11/780	Minicomputer	1978	16 kB	—	—
IBM 3033	Mainframe	1978	64 kB	—	—
IBM 3090	Mainframe	1985	128–256 kB	—	—
Intel 80486	PC	1989	8 kB	—	—
Pentium	PC	1993	8 kB/8 kB	256–512 kB	—
PowerPC 601	PC	1993	32 kB	—	—
PowerPC 620	PC	1996	32 kB/32 kB	—	—
PowerPC G4	PC/server	1999	32 kB/32 kB	256 kB to 1 MB	2 MB
IBM S/390 G6	Mainframe	1999	256 kB	8 MB	—
Pentium 4	PC/server	2000	8 kB/8 kB	256 kB	—
IBM SP	High-end server/supercomputer	2000	64 kB/32 kB	8 MB	—
CRAY MTA	Supercomputer	2000	8 kB	2 MB	—
Itanium	PC/server	2001	16 kB/16 kB	96 kB	4 MB
Itanium 2	PC/server	2002	32 kB	256 kB	6 MB
IBM POWER5	High-end server	2003	64 kB	1.9 MB	36 MB
CRAY XD-1	Supercomputer	2004	64 kB/64 kB	1 MB	—
IBM POWER6	PC/server	2007	64 kB/64 kB	4 MB	32 MB
IBM z10	Mainframe	2008	64 kB/128 kB	3 MB	24–48 MB
Intel Core i7 EE 990	Workstation/server	2011	6×32 kB/32 kB	1.5 MB	12 MB
IBM zEnterprise 196	Mainframe/server	2011	24×64 kB/128 kB	24×1.5 MB	24 MB L3, 192 MB L4
หมายเหตุ: ค่าสองตัวที่คั่นด้วย slash หมายถึง instruction cache และ data cache

ฟังก์ชันการแมป
เนื่องจากมี cache line น้อยกว่า main memory block จำเป็นต้องมีอัลกอริทึมในการแมป main memory block เข้า cache line นอกจากนี้ยังต้องการวิธีในการระบุว่า main memory block ใดกำลังครอง cache line ในปัจจุบัน ทางเลือกของ mapping function จะกำหนดวิธีการจัดระเบียบ cache สามเทคนิคที่สามารถใช้ได้: direct, associative และ set-associative เราจะตรวจสอบแต่ละเทคนิค ในแต่ละกรณีเราจะดูโครงสร้างทั่วไปและตัวอย่างเฉพาะ

ตัวอย่าง 4.2
สำหรับทั้งสามกรณี ตัวอย่างประกอบด้วย element ต่อไปนี้:

Cache สามารถเก็บข้อมูลได้ 64 kB
ข้อมูลถูกถ่ายโอนระหว่าง main memory กับ cache เป็น block ขนาด 4 byte ซึ่งหมายความว่า cache ถูกจัดเป็น 16K = 2¹⁴ line ของ 4 byte แต่ละ line
Main memory มีขนาด 16 MB โดยแต่ละ byte ระบุที่อยู่ได้โดยตรงด้วย 24-bit address (2²⁴ = 16M) ดังนั้น สำหรับวัตถุประสงค์การแมป เราสามารถถือว่า main memory ประกอบด้วย 4M block ของ 4 byte แต่ละ block
Direct Mapping
เทคนิคที่ง่ายที่สุด เรียกว่า direct mapping จะแมปแต่ละ block ของ main memory ไปยัง cache line ที่เป็นไปได้เพียงบรรทัดเดียว การแมปแสดงได้ดังนี้:

$$i = j \bmod m$$

โดยที่:

$i$ = หมายเลข cache line
$j$ = หมายเลข main memory block
$m$ = จำนวน line ใน cache
รูปที่ 4.8a แสดงการแมปสำหรับ block แรก m ตัวของ main memory แต่ละ block ของ main memory แมปไปยัง cache line เดียว m block ถัดไปของ main memory แมปเข้า cache ในรูปแบบเดียวกัน นั่นคือ block Bm ของ main memory แมปไปยัง line L₀ ของ cache, block Bm+1 แมปไปยัง line L₁ เป็นต้น

mapping function นำไปใช้ได้ง่ายโดยใช้ที่อยู่ main memory รูปที่ 4.9 แสดงกลไกทั่วไป สำหรับวัตถุประสงค์ cache access ที่อยู่ main memory แต่ละตัวสามารถมองได้ว่าประกอบด้วยสามฟิลด์ w bits ที่มีนัยสำคัญน้อยที่สุดระบุ word หรือ byte เฉพาะภายใน block ของ main memory ในคอมพิวเตอร์ร่วมสมัยส่วนใหญ่ ที่อยู่อยู่ในระดับ byte s bits ที่เหลือระบุหนึ่งใน 2^s block ของ main memory logic ของ cache ตีความ s bits เหล่านี้เป็น tag ของ s - r bits (ส่วนที่มีนัยสำคัญมากที่สุด) และ line field ของ r bits ฟิลด์หลังนี้ระบุหนึ่งใน m = 2^r line ของ cache สรุปดังนี้:

ความยาวที่อยู่ = (s + w) bits
จำนวน addressable unit = $2^{s+w}$ word หรือ byte
Block size = line size = $2^w$ word หรือ byte
จำนวน block ใน main memory = $\frac{2^{s+w}}{2^w} = 2^s$
จำนวน line ใน cache = m = $2^r$
ขนาด cache = $2^{r+w}$ word หรือ byte
ขนาด tag = (s - r) bits
ตัวอย่าง 4.2a
รูปที่ 4.10 แสดงตัวอย่างระบบของเราโดยใช้ direct mapping ในตัวอย่าง m = 16K = 2¹⁴ และ i = j mod 2¹⁴ การแมปกลายเป็น:

Cache Line	Starting Memory Address of Block
0	000000, 010000, …, FF0000
1	000004, 010004, …, FF0004
⋮	⋮
2¹⁴ - 1	00FFFC, 01FFFC, …, FFFFFC
โปรดสังเกตว่าไม่มี block สอง block ใดที่แมปไปยังหมายเลข line เดียวกันที่มี tag number เดียวกัน ดังนั้น block ที่มีที่อยู่เริ่มต้น 000000, 010000, …, FF0000 จึงมี tag number 00, 01, …, FF ตามลำดับ

อ้างอิงกลับไปที่รูปที่ 4.5 read operation ทำงานดังนี้: ระบบ cache จะได้รับ 24-bit address 14-bit line number ใช้เป็น index เข้าสู่ cache เพื่อเข้าถึง line เฉพาะ ถ้า 8-bit tag number ตรงกับ tag number ที่จัดเก็บอยู่ใน line นั้นในปัจจุบัน 2-bit word number จะถูกใช้เพื่อเลือกหนึ่งใน 4 byte ใน line นั้น มิฉะนั้น 22-bit tag-plus-line field จะถูกใช้เพื่อดึง block จาก main memory ที่อยู่จริงที่ใช้สำหรับการดึงคือ 22-bit tag-plus-line ต่อกับ 0 bits สองตัว เพื่อให้ดึง 4 byte โดยเริ่มที่ block boundary

ผลของการแมปนี้คือ block ของ main memory ถูกกำหนดให้กับ line ของ cache ดังนี้:

Cache line	Main memory blocks assigned
0	0, m, 2m, …, 2^s - m
1	1, m+1, 2m+1, …, 2^s - m+1
⋮	⋮
m-1	m-1, 2m-1, 3m-1, …, 2^s - 1
ดังนั้น การใช้ส่วนหนึ่งของที่อยู่เป็น line number จึงให้การแมปเฉพาะของแต่ละ block ใน main memory เข้าสู่ cache เมื่อ block ถูกอ่านเข้า line ที่กำหนดจริง ๆ จำเป็นต้อง tag ข้อมูลเพื่อแยกแยะจาก block อื่นที่สามารถพอดีกับ line นั้น s - r bits ที่มีนัยสำคัญมากที่สุดทำหน้าที่นี้

เทคนิค direct mapping ง่ายและราคาถูกในการนำไปใช้ ข้อเสียหลักคือมี cache location คงที่สำหรับ block ที่กำหนดใด ๆ ดังนั้น ถ้าโปรแกรมอ้างอิง word ซ้ำ ๆ จาก block ต่างสองตัวที่แมปไปยัง line เดียวกัน block เหล่านั้นจะถูกสลับใน cache ตลอดเวลา และ hit ratio จะต่ำ (ปรากฏการณ์ที่เรียกว่า thrashing)

Selective Victim Cache Simulator

แนวทางหนึ่งในการลด miss penalty คือการจำไว้ว่าถูกละทิ้งในกรณีที่จำเป็นอีกครั้ง เนื่องจากข้อมูลที่ละทิ้งได้รับการดึงมาแล้ว จึงสามารถใช้ซ้ำได้ในราคาน้อย การ recycle ดังกล่าวเป็นไปได้โดยใช้ victim cache Victim cache ถูกเสนอขึ้นในตอนแรกเพื่อลด conflict miss ของ direct mapped cache โดยไม่ส่งผลต่อเวลาเข้าถึงที่เร็ว Victim cache เป็น fully associative cache ที่มีขนาดโดยทั่วไป 4 ถึง 16 cache line อยู่ระหว่าง direct mapped L1 cache กับระดับหน่วยความจำถัดไป แนวคิดนี้ศึกษาในภาคผนวก F

Associative Mapping
Associative mapping เอาชนะข้อเสียของ direct mapping โดยอนุญาตให้แต่ละ main memory block โหลดเข้า line ใดก็ได้ของ cache (รูปที่ 4.8b) ในกรณีนี้ cache control logic ตีความที่อยู่ memory เพียงแค่เป็น Tag field และ Word field Tag field ระบุ block ของ main memory อย่างเฉพาะเจาะจง เพื่อพิจารณาว่า block อยู่ใน cache หรือไม่ cache control logic ต้องตรวจสอบ tag ของทุก line พร้อมกัน รูปที่ 4.11 แสดง logic

โปรดสังเกตว่าไม่มีฟิลด์ในที่อยู่ที่ตรงกับ line number ดังนั้น จำนวน line ใน cache จึงไม่ถูกกำหนดโดยรูปแบบที่อยู่ สรุปดังนี้:

ความยาวที่อยู่ = (s + w) bits
จำนวน addressable unit = $2^{s+w}$ word หรือ byte
Block size = line size = $2^w$ word หรือ byte
จำนวน block ใน main memory = $2^s$
จำนวน line ใน cache = ไม่แน่นอน
ขนาด tag = s bits
ด้วย associative mapping จะมีความยืดหยุ่นในการเลือก block ที่จะแทนที่เมื่อ block ใหม่ถูกอ่านเข้า cache อัลกอริทึมการแทนที่ ซึ่งอภิปรายในส่วนนี้ในภายหลัง ถูกออกแบบมาเพื่อเพิ่ม hit ratio ให้สูงสุด ข้อเสียหลักของ associative mapping คือวงจรซับซ้อนที่จำเป็นในการตรวจสอบ tag ของ cache line ทั้งหมดพร้อมกัน

ตัวอย่าง 4.2b
รูปที่ 4.12 แสดงตัวอย่างของเราโดยใช้ associative mapping ที่อยู่ main memory ประกอบด้วย 22-bit tag และ 2-bit byte number 22-bit tag ต้องถูกจัดเก็บพร้อมกับ 32-bit block ของข้อมูลสำหรับแต่ละ line ใน cache โปรดสังเกตว่าเป็น 22 bits ทางซ้าย (มีนัยสำคัญมากที่สุด) ของที่อยู่ที่สร้าง tag ดังนั้น 24-bit hexadecimal address 16339C จึงมี 22-bit tag 058CE7 ซึ่งเห็นได้ง่ายใน binary notation:

Memory address: 0001 0110 0011 0011 1001 1100 (binary) = 1 6 3 3 9 C (hex)
Tag (22 bits ซ้าย): 00 0101 1000 1100 1110 0111 (binary) = 0 5 8 C E 7 (hex)
Set-Associative Mapping
Set-associative mapping เป็นการประนีประนอมที่แสดงจุดแข็งของทั้ง direct และ associative approach ในขณะที่ลดข้อเสียของทั้งสอง

ในกรณีนี้ cache ประกอบด้วย set จำนวนหนึ่ง แต่ละ set ประกอบด้วย line จำนวนหนึ่ง ความสัมพันธ์คือ:

$$m = v \times k$$ $$i = j \bmod v$$

โดยที่:

$i$ = หมายเลข cache set
$j$ = หมายเลข main memory block
$m$ = จำนวน line ใน cache
$v$ = จำนวน set
$k$ = จำนวน line ในแต่ละ set
นี่เรียกว่า k-way set-associative mapping ด้วย set-associative mapping block Bj สามารถแมปเข้า line ใดก็ได้ของ set j รูปที่ 4.13a แสดงการแมปนี้สำหรับ block แรก v ตัวของ main memory เช่นเดียวกับ associative mapping แต่ละ word แมปไปยัง cache line หลายตัว สำหรับ set-associative mapping แต่ละ word แมปไปยัง cache line ทั้งหมดในชุด set เฉพาะ ดังนั้น main memory block B₀ แมปเข้า set 0 เป็นต้น ดังนั้น set-associative cache จึงสามารถนำไปใช้ทางกายภาพเป็น v associative cache ยังสามารถนำไปใช้ set-associative cache เป็น k direct mapping cache ดังแสดงในรูปที่ 4.13b แต่ละ direct-mapped cache เรียกว่า way ประกอบด้วย v line v line แรกของ main memory ถูกแมปโดยตรงเข้าสู่ v line ของแต่ละ way กลุ่ม v line ถัดไปของ main memory ก็ถูกแมปในรูปแบบเดียวกัน เป็นต้น การนำไปใช้แบบ direct-mapped มักใช้สำหรับระดับ associativity ต่ำ (ค่า k ต่ำ) ในขณะที่การนำไปใช้แบบ associative-mapped มักใช้สำหรับระดับ associativity สูงกว่า [JACO08]

สำหรับ set-associative mapping cache control logic ตีความที่อยู่ memory เป็นสามฟิลด์: Tag, Set และ Word d set bits ระบุหนึ่งใน v = 2^d set s bits ของฟิลด์ Tag และ Set ระบุหนึ่งใน 2^s block ของ main memory รูปที่ 4.14 แสดง cache control logic ด้วย fully associative mapping tag ในที่อยู่ memory มีขนาดค่อนข้างใหญ่และต้องเปรียบเทียบกับ tag ของทุก line ใน cache ด้วย k-way set-associative mapping tag ในที่อยู่ memory มีขนาดเล็กกว่ามากและเปรียบเทียบกับ k tag ภายใน set เดียวเท่านั้น สรุปดังนี้:

ความยาวที่อยู่ = (s + w) bits
จำนวน addressable unit = $2^{s+w}$ word หรือ byte
Block size = line size = $2^w$ word หรือ byte
จำนวน block ใน main memory = $2^s$
จำนวน line ใน set = k
จำนวน set = v = $2^d$
จำนวน line ใน cache = m = $k \times 2^d$
ขนาด cache = $k \times 2^{d+w}$ word หรือ byte
ขนาด tag = (s - d) bits
ตัวอย่าง 4.2c
รูปที่ 4.15 แสดงตัวอย่างของเราโดยใช้ set-associative mapping ที่มีสอง line ในแต่ละ set เรียกว่า two-way set-associative 13-bit set number ระบุ set เฉพาะของสอง line ภายใน cache ยังให้หมายเลขของ block ใน main memory, modulo 2¹³ ซึ่งกำหนดการแมปของ block เข้า line ดังนั้น block 000000, 008000, …, FF8000 ของ main memory แมปเข้า cache set 0 block ใดก็ได้เหล่านั้นสามารถโหลดเข้า line ใดของสอง line ใน set ก็ได้ โปรดสังเกตว่าไม่มี block สอง block ใดที่แมปเข้า cache set เดียวกันที่มี tag number เดียวกัน สำหรับ read operation 13-bit set number ถูกใช้เพื่อระบุว่าจะตรวจสอบ set ใดของสอง line ทั้งสอง line ใน set ถูกตรวจสอบสำหรับการจับคู่กับ tag number ของที่อยู่ที่จะเข้าถึง

ในกรณีสุดขีดของ v = m, k = 1 เทคนิค set-associative ลดเป็น direct mapping และสำหรับ v = 1, k = m จะลดเป็น associative mapping การใช้สอง line ต่อ set (v = m/2, k = 2) เป็น set-associative organization ที่พบบ่อยที่สุด มันปรับปรุง hit ratio เหนือ direct mapping อย่างมีนัยสำคัญ Four-way set associative (v = m/4, k = 4) ทำการปรับปรุงเพิ่มเติมเล็กน้อยด้วยค่าใช้จ่ายเพิ่มเติมที่ค่อนข้างน้อย [MAYB84, HILL89] การเพิ่มจำนวน line ต่อ set ในระดับที่สูงกว่ามีผลน้อยมาก

รูปที่ 4.16 แสดงผลการศึกษาจำลองหนึ่งของ set-associative cache performance ในฐานะฟังก์ชันของขนาด cache [GENU04] ความแตกต่างในประสิทธิภาพระหว่าง direct และ two-way set associative มีนัยสำคัญขึ้นไปถึงขนาด cache อย่างน้อย 64 kB โปรดสังเกตด้วยว่าความแตกต่างระหว่าง two-way และ four-way ที่ 4 kB น้อยกว่าความแตกต่างในการเพิ่มขนาด cache จาก 4 kB เป็น 8 kB มาก ความซับซ้อนของ cache เพิ่มขึ้นตาม associativity และในกรณีนี้ไม่สมเหตุสมผลเมื่อเทียบกับการเพิ่มขนาด cache เป็น 8 หรือแม้แต่ 16 kB จุดสุดท้ายที่ควรสังเกตคือ เกิน 32 kB การเพิ่มขนาด cache ไม่ได้นำมาซึ่งการปรับปรุงประสิทธิภาพที่มีนัยสำคัญ

ผลลัพธ์ของรูปที่ 4.16 อิงจากการจำลองการทำงานของ GCC compiler แอปพลิเคชันต่างกันอาจให้ผลลัพธ์ต่างกัน ตัวอย่างเช่น [CANT01] รายงานผลสำหรับ cache performance โดยใช้ CPU2000 SPEC benchmark หลายตัว ผลลัพธ์ของ [CANT01] ในการเปรียบเทียบ hit ratio กับขนาด cache ตามรูปแบบเดียวกับรูปที่ 4.16 แต่ค่าเฉพาะแตกต่างกันบ้าง

อัลกอริทึมการแทนที่
เมื่อ cache ถูกเติมเต็มแล้ว เมื่อ block ใหม่ถูกนำเข้า cache หนึ่งใน block ที่มีอยู่ต้องถูกแทนที่ สำหรับ direct mapping มีเพียง line เดียวที่เป็นไปได้สำหรับ block เฉพาะใด ๆ และไม่มีทางเลือก สำหรับเทคนิค associative และ set-associative จำเป็นต้องมี replacement algorithm เพื่อให้ได้ความเร็วสูง อัลกอริทึมดังกล่าวต้องนำไปใช้ใน hardware มีอัลกอริทึมหลายตัวที่ได้รับการทดลอง เราจะกล่าวถึงสี่ตัวที่พบบ่อยที่สุด

อาจมีประสิทธิภาพมากที่สุดคือ least recently used (LRU): แทนที่ block ใน set ที่อยู่ใน cache นานที่สุดโดยไม่มีการอ้างอิง สำหรับ two-way set associative นี้นำไปใช้ได้ง่าย แต่ละ line มี USE bit เมื่อ line ถูกอ้างอิง USE bit ของมันถูกตั้งเป็น 1 และ USE bit ของ line อื่นใน set นั้นถูกตั้งเป็น 0 เมื่อ block จะถูกอ่านเข้า set จะใช้ line ที่มี USE bit เป็น 0 เนื่องจากเราสมมติว่าตำแหน่งหน่วยความจำที่ใช้ล่าสุดมีแนวโน้มที่จะถูกอ้างอิงมากกว่า LRU ควรให้ hit ratio ที่ดีที่สุด LRU ยังค่อนข้างง่ายในการนำไปใช้สำหรับ fully associative cache กลไก cache เก็บรายการแยกต่างหากของ index ของ line ทั้งหมดใน cache เมื่อ line ถูกอ้างอิง มันเคลื่อนไปด้านหน้าของรายการ สำหรับการแทนที่ใช้ line ที่ด้านหลังของรายการ เนื่องจากความเรียบง่ายในการนำไปใช้ LRU จึงเป็น replacement algorithm ที่นิยมที่สุด

อีกทางเลือกหนึ่งคือ first-in-first-out (FIFO): แทนที่ block ใน set ที่อยู่ใน cache นานที่สุด FIFO นำไปใช้ได้ง่ายด้วยเทคนิค round-robin หรือ circular buffer ยังมีความเป็นไปได้อีกอย่างคือ least frequently used (LFU): แทนที่ block ใน set ที่มีการอ้างอิงน้อยที่สุด LFU สามารถนำไปใช้ได้โดยการเชื่อม counter กับแต่ละ line เทคนิคที่ไม่อิงการใช้งาน (ไม่ใช่ LRU, LFU, FIFO หรือตัวแปรบางอย่าง) คือการเลือก line แบบสุ่มจาก candidate line การศึกษาจำลองแสดงให้เห็นว่า random replacement ให้ประสิทธิภาพด้อยกว่าอัลกอริทึมที่อิงการใช้งานเพียงเล็กน้อย [SMIT82]

นโยบายการเขียน
เมื่อ block ที่อยู่ใน cache จะถูกแทนที่ มีสองกรณีที่ต้องพิจารณา ถ้า block เดิมใน cache ไม่ได้ถูกแก้ไข ก็อาจเขียนทับด้วย block ใหม่โดยไม่ต้องเขียน block เดิมออกก่อน ถ้ามีการเขียนอย่างน้อยหนึ่งครั้งบน word ใน line ของ cache นั้น จะต้องอัปเดต main memory โดยเขียน line ของ cache ออกไปยัง block ของ memory ก่อนนำ block ใหม่เข้ามา มีนโยบายการเขียนหลากหลาย พร้อมการแลกเปลี่ยนด้านประสิทธิภาพและเศรษฐกิจ มีปัญหาสองอย่างที่ต้องรับมือ

อย่างแรก อุปกรณ์มากกว่าหนึ่งตัวอาจเข้าถึง main memory ได้ ตัวอย่างเช่น I/O module อาจสามารถอ่าน-เขียนโดยตรงไปยัง memory ถ้า word ถูกแก้ไขเฉพาะใน cache word ใน memory ที่สอดคล้องกันก็จะ invalid ยิ่งไปกว่านั้น ถ้า I/O device ได้แก้ไข main memory แล้ว cache word ก็จะ invalid ปัญหาที่ซับซ้อนกว่าเกิดขึ้นเมื่อหลายโปรเซสเซอร์เชื่อมต่อกับ bus เดียวกันและแต่ละโปรเซสเซอร์มี local cache ของตัวเอง ดังนั้นถ้า word ถูกแก้ไขใน cache หนึ่ง ก็อาจ invalidate word ใน cache อื่น

เทคนิคที่ง่ายที่สุดเรียกว่า write through โดยใช้เทคนิคนี้ การเขียนทั้งหมดจะถูกส่งไปยัง main memory และ cache พร้อมกัน เพื่อให้แน่ใจว่า main memory ถูกต้องเสมอ processor-cache module อื่นใดก็ตามสามารถตรวจสอบ traffic ไปยัง main memory เพื่อรักษาความสอดคล้องภายใน cache ของตัวเอง ข้อเสียหลัก

ของเทคนิคนี้คือสร้าง memory traffic จำนวนมากและอาจสร้าง bottleneck อีกเทคนิคหนึ่งที่เรียกว่า write back ลดการเขียนหน่วยความจำให้น้อยที่สุด ด้วย write back การอัปเดตจะทำเฉพาะใน cache เมื่อเกิดการอัปเดต dirty bit หรือ use bit ที่เชื่อมกับ line จะถูกตั้งค่า จากนั้น เมื่อ block ถูกแทนที่ จะถูกเขียนกลับ main memory ถ้าและต่อเมื่อ dirty bit ถูกตั้งค่า ปัญหาของ write back คือส่วนของ main memory อาจ invalid ดังนั้น การเข้าถึงโดย I/O module จึงสามารถอนุญาตได้เฉพาะผ่าน cache เท่านั้น ทำให้วงจรซับซ้อนและอาจเกิด bottleneck ประสบการณ์แสดงให้เห็นว่าเปอร์เซ็นต์ของการอ้างอิงหน่วยความจำที่เป็นการเขียนอยู่ที่ประมาณ 15% [SMIT82] อย่างไรก็ตาม สำหรับ HPC application ตัวเลขนี้อาจเข้าใกล้ 33% (vector-vector multiplication) และอาจสูงถึง 50% (matrix transposition)

ตัวอย่าง 4.3
พิจารณา cache ที่มี line size 32 bytes และ main memory ที่ต้องใช้ 30 ns ในการถ่ายโอน 4-byte word สำหรับ line ใดก็ตามที่ถูกเขียนอย่างน้อยหนึ่งครั้งก่อนถูก swap ออกจาก cache จำนวนครั้งเฉลี่ยที่ line ต้องถูกเขียนก่อนถูก swap ออกเพื่อให้ write-back cache มีประสิทธิภาพมากกว่า write-through cache คือเท่าไหร่?

สำหรับกรณี write-back แต่ละ dirty line จะถูกเขียนกลับครั้งเดียวเมื่อ swap-out time ใช้เวลา 8 × 30 = 240 ns สำหรับกรณี write-through แต่ละการอัปเดต line ต้องเขียน word ออกไปยัง main memory ใช้เวลา 30 ns ดังนั้น ถ้า line เฉลี่ยที่ถูกเขียนอย่างน้อยหนึ่งครั้งถูกเขียนมากกว่า 8 ครั้งก่อน swap out แล้ว write back จะมีประสิทธิภาพมากกว่า

ในองค์กรแบบ bus ที่มีอุปกรณ์มากกว่าหนึ่งตัว (โดยทั่วไปคือโปรเซสเซอร์) มี cache และ main memory ร่วมกัน จะมีปัญหาใหม่เกิดขึ้น ถ้าข้อมูลใน cache ตัวหนึ่งถูกแก้ไข นี่จะ invalidate ไม่เพียงแค่ word ที่สอดคล้องกันใน main memory แต่ยัง word เดียวกันใน cache อื่น ๆ ด้วย (ถ้า cache อื่นมี word เดียวกัน) แม้ว่าจะใช้ write-through policy cache อื่นก็อาจมีข้อมูลที่ invalid ระบบที่ป้องกันปัญหานี้เรียกว่ารักษา cache coherency แนวทางที่เป็นไปได้สำหรับ cache coherency ได้แก่:

Bus watching with write through: cache controller แต่ละตัวตรวจสอบ address line เพื่อตรวจจับ write operation ไปยัง memory โดย bus master อื่น ถ้า master อื่นเขียนไปยังตำแหน่งใน shared memory ที่อยู่ใน cache memory ด้วย cache controller จะ invalidate cache entry นั้น กลยุทธ์นี้ขึ้นอยู่กับการใช้ write-through policy โดย cache controller ทั้งหมด

Hardware transparency: ใช้ hardware เพิ่มเติมเพื่อให้แน่ใจว่าการอัปเดตทั้งหมดไปยัง main memory ผ่าน cache สะท้อนใน cache ทั้งหมด ดังนั้น ถ้าโปรเซสเซอร์หนึ่งแก้ไข word ใน cache ของมัน การอัปเดตนี้จะถูกเขียนไปยัง main memory นอกจากนี้ word ที่ตรงกันใน cache อื่นก็จะถูกอัปเดตในทำนองเดียวกัน

Noncacheable memory: เฉพาะส่วนหนึ่งของ main memory ที่ใช้ร่วมกันโดยโปรเซสเซอร์มากกว่าหนึ่งตัว และถูกกำหนดให้เป็น noncacheable ในระบบดังกล่าว การเข้าถึง shared memory ทั้งหมดเป็น cache miss เพราะ shared memory ไม่เคยถูก copy เข้า cache noncacheable memory สามารถระบุโดยใช้ chip-select logic หรือ high-address bit

Cache coherency เป็นสาขาการวิจัยที่ active หัวข้อนี้จะศึกษาต่อในส่วนที่ห้า

ขนาดบรรทัด
design element อีกอย่างคือ line size เมื่อ block ของข้อมูลถูกดึงมาและวางใน cache ไม่เพียงแต่ word ที่ต้องการเท่านั้น แต่ยัง word ที่อยู่ติดกันจำนวนหนึ่งด้วยที่ถูกดึงมา เมื่อขนาด block เพิ่มขึ้นจากขนาดเล็กมากไปยังขนาดใหญ่ขึ้น hit ratio จะเพิ่มขึ้นในตอนแรกเนื่องจากหลักการ locality ซึ่งระบุว่าข้อมูลในบริเวณใกล้เคียงกับ word ที่ถูกอ้างอิงมีแนวโน้มที่จะถูกอ้างอิงในอนาคตอันใกล้ เมื่อขนาด block เพิ่มขึ้น ข้อมูลที่มีประโยชน์มากขึ้นจะถูกนำเข้า cache hit ratio จะเริ่มลดลง อย่างไรก็ตาม เมื่อ block ใหญ่ขึ้นและความน่าจะเป็นของการใช้ข้อมูลที่ดึงมาใหม่น้อยกว่าความน่าจะเป็นของการนำข้อมูลที่ต้องแทนที่กลับมาใช้ใหม่ ผลสองอย่างเฉพาะเข้ามามีบทบาท:

Block ที่ใหญ่กว่าลดจำนวน block ที่พอดีใน cache เนื่องจาก block fetch แต่ละครั้งเขียนทับเนื้อหา cache เดิม จำนวน block น้อยส่งผลให้ข้อมูลถูกเขียนทับไม่นานหลังจากถูกดึงมา
เมื่อ block ใหญ่ขึ้น แต่ละ word เพิ่มเติมอยู่ห่างจาก word ที่ต้องการมากขึ้นและมีแนวโน้มน้อยลงที่จะถูกต้องการในอนาคตอันใกล้
ความสัมพันธ์ระหว่างขนาด block และ hit ratio ซับซ้อน ขึ้นอยู่กับคุณลักษณะ locality ของโปรแกรมเฉพาะ และไม่มีค่าที่เหมาะสมที่สุดที่ชัดเจน ขนาดตั้งแต่ 8 ถึง 64 byte ดูเหมือนจะใกล้เคียงกับจุดที่เหมาะสม [SMIT87, PRZY88, PRZY90, HAND98] สำหรับระบบ HPC ขนาด cache line 64 และ 128 byte ถูกใช้บ่อยที่สุด

จำนวนแคช
เมื่อ cache ถูกนำมาใช้ครั้งแรก ระบบทั่วไปมี cache เพียงตัวเดียว ล่าสุดมากขึ้น การใช้ cache หลายตัวกลายเป็นเรื่องปกติ สองด้านของปัญหาการออกแบบนี้เกี่ยวข้องกับจำนวนระดับของ cache และการใช้ unified versus split cache

Multilevel Caches
เมื่อความหนาแน่น logic เพิ่มขึ้น จึงเป็นไปได้ที่จะมี cache อยู่บนชิปเดียวกับโปรเซสเซอร์ ซึ่งเรียกว่า on-chip cache เมื่อเทียบกับ cache ที่เข้าถึงผ่าน external bus on-chip cache จะลด external bus activity ของโปรเซสเซอร์ ดังนั้นจึงเร่งเวลาประมวลผลและเพิ่มประสิทธิภาพรวมของระบบ เมื่อ instruction หรือข้อมูลที่ต้องการพบใน on-chip cache การเข้าถึง bus จะถูกขจัดออก เนื่องจาก data path ภายในโปรเซสเซอร์สั้นกว่าความยาว bus มาก การเข้าถึง on-chip cache จะเสร็จสิ้นเร็วกว่าอย่างเห็นได้ชัดเมื่อเทียบกับ bus cycle แบบ zero-wait state ยิ่งไปกว่านั้น ในระหว่างช่วงเวลานี้ bus ก็ว่างเพื่อรองรับการถ่ายโอนอื่น ๆ

การรวม on-chip cache เปิดคำถามว่ายังต้องการ off-chip หรือ external cache หรือไม่ โดยทั่วไป คำตอบคือใช่ และการออกแบบร่วมสมัยส่วนใหญ่รวมทั้ง on-chip และ external cache องค์กรที่ง่ายที่สุดเรียกว่า two-level cache โดยมี level 1 (L1) ภายใน และ external cache ที่กำหนดเป็น level 2 (L2) เหตุผลในการรวม L2 cache มีดังนี้: ถ้าไม่มี L2 cache และโปรเซสเซอร์ส่ง access request ไปยังตำแหน่งหน่วยความจำที่ไม่อยู่ใน L1 cache โปรเซสเซอร์จะต้องเข้าถึง DRAM หรือ ROM memory ผ่าน bus เนื่องจาก bus speed ที่ช้าโดยทั่วไปและเวลาเข้าถึง memory ที่ช้า ส่งผลให้ประสิทธิภาพต่ำ ในทางกลับกัน ถ้าใช้ L2 SRAM (static RAM) cache ข้อมูลที่ขาดหายมักสามารถดึงมาได้อย่างรวดเร็ว ถ้า SRAM เร็วพอที่จะตรงกับ bus speed ข้อมูลสามารถเข้าถึงได้โดยใช้ zero-wait state transaction ซึ่งเป็นประเภทการถ่ายโอน bus ที่เร็วที่สุด

คุณลักษณะสองอย่างของการออกแบบ cache ร่วมสมัยสำหรับ multilevel cache ควรสังเกต อย่างแรก สำหรับ off-chip L2 cache การออกแบบหลายอย่างไม่ใช้ system bus เป็นเส้นทางสำหรับการถ่ายโอนระหว่าง L2 cache กับโปรเซสเซอร์ แต่ใช้ data path แยกต่างหาก เพื่อลดภาระบน system bus อย่างที่สอง ด้วยการหดตัวอย่างต่อเนื่องของส่วนประกอบโปรเซสเซอร์ โปรเซสเซอร์จำนวนมากในปัจจุบันรวม L2 cache ไว้บน processor chip ปรับปรุงประสิทธิภาพ

การประหยัดที่เป็นไปได้จากการใช้ L2 cache ขึ้นอยู่กับ hit ratio ทั้งใน L1 และ L2 cache การศึกษาหลายชิ้นแสดงให้เห็นว่าโดยทั่วไป การใช้ second-level cache จะปรับปรุงประสิทธิภาพ อย่างไรก็ตาม การใช้ multilevel cache ทำให้ปัญหาการออกแบบทั้งหมดที่เกี่ยวข้องกับ cache ซับซ้อนขึ้น รวมถึงขนาด, replacement algorithm และ write policy

รูปที่ 4.17 แสดงผลการศึกษาจำลองหนึ่งของ two-level cache performance ในฐานะฟังก์ชันของขนาด cache [GENU04] รูปสมมติว่าทั้งสอง cache มี line size เดียวกัน และแสดง total hit ratio นั่นคือ hit ถูกนับถ้าข้อมูลที่ต้องการปรากฏใน L1 หรือ L2 cache รูปแสดงผลกระทบของ L2 ต่อ total hit เทียบกับขนาด L1 L2 มีผลน้อยต่อ total hit จนกว่าจะมีขนาดอย่างน้อยสองเท่าของขนาด L1 cache โปรดสังเกตว่าส่วนที่ชันที่สุดของ slope สำหรับ L1 cache ขนาด 8 kB คือสำหรับ L2 cache ขนาด 16 kB และสำหรับ L1 cache ขนาด 16 kB ส่วนที่ชันที่สุดของเส้นโค้งคือสำหรับ L2 cache ขนาด 32 kB ก่อนถึงจุดนั้น L2 cache มีผลน้อยหรือไม่มีผลต่อ total cache performance ความจำเป็นที่ L2 cache จะต้องใหญ่กว่า L1 cache เพื่อส่งผลต่อประสิทธิภาพสมเหตุสมผล ถ้า L2 cache มี line size และความจุเดียวกับ L1 cache เนื้อหาของมันจะสะท้อน L1 cache ไม่มากก็น้อย

ด้วยพื้นที่ on-chip ที่มีเพิ่มขึ้นสำหรับ cache microprocessor ร่วมสมัยส่วนใหญ่ได้ย้าย L2 cache เข้าสู่ processor chip และเพิ่ม L3 cache เดิมที L3 cache เข้าถึงผ่าน external bus ล่าสุดมากขึ้น microprocessor ส่วนใหญ่รวม on-chip L3 cache ในทั้งสองกรณีดูเหมือนว่าจะมีข้อได้เปรียบด้านประสิทธิภาพจากการเพิ่มระดับที่สาม นอกจากนี้ ระบบขนาดใหญ่ เช่น IBM mainframe zEnterprise systems ปัจจุบันรวม 3 ระดับ cache บนชิปและระดับที่สี่ของ cache ที่ใช้ร่วมกันในหลายชิป [CURR11]

Unified versus Split Caches
เมื่อ on-chip cache ปรากฏตัวครั้งแรก การออกแบบหลายอย่างประกอบด้วย cache เดียวที่ใช้จัดเก็บการอ้างอิงทั้ง data และ instruction ล่าสุดมากขึ้น การแยก cache เป็นสองตัวกลายเป็นเรื่องปกติ: ตัวหนึ่งเฉพาะสำหรับ instruction และตัวหนึ่งเฉพาะสำหรับ data cache ทั้งสองนี้อยู่ในระดับเดียวกัน โดยทั่วไปเป็น L1 cache สองตัว เมื่อโปรเซสเซอร์พยายามดึง instruction จาก main memory จะค้นหา instruction L1 cache ก่อน และเมื่อโปรเซสเซอร์พยายามดึงข้อมูลจาก main memory จะค้นหา data L1 cache ก่อน

มีข้อได้เปรียบที่เป็นไปได้สองอย่างของ unified cache:

สำหรับขนาด cache ที่กำหนด unified cache มี hit rate สูงกว่า split cache เนื่องจากสมดุล load ระหว่าง instruction fetch กับ data fetch โดยอัตโนมัติ กล่าวคือ ถ้า execution pattern เกี่ยวข้องกับ instruction fetch มากกว่า data fetch จาก cache จะมีแนวโน้มเต็มด้วย instruction และถ้า execution pattern เกี่ยวข้องกับ data fetch มากกว่าสิ่งตรงกันข้ามก็จะเกิดขึ้น
ต้องออกแบบและนำไปใช้เพียง cache เดียว
แนวโน้มเป็นไปในทิศทางของ split cache ที่ L1 และ unified cache สำหรับระดับสูงกว่า โดยเฉพาะสำหรับ superscalar machine ที่เน้นการประมวลผล instruction แบบ parallel และการ prefetch instruction ในอนาคตที่คาดการณ์ไว้ ข้อได้เปรียบสำคัญของการออกแบบ split cache คือขจัด contention สำหรับ cache ระหว่าง instruction fetch/decode unit กับ execution unit ซึ่งสำคัญในการออกแบบที่พึ่งพาการ pipeline ของ instruction โดยทั่วไปโปรเซสเซอร์จะดึง instruction ล่วงหน้าและเติม buffer หรือ pipeline ด้วย instruction ที่จะประมวลผล สมมติว่าตอนนี้เรามี unified instruction/data cache เมื่อ execution unit เข้าถึงหน่วยความจำเพื่อ load และ store data request จะถูกส่งไปยัง unified cache ถ้าในเวลาเดียวกัน instruction prefetcher ออก read request ไปยัง cache สำหรับ instruction request นั้นจะถูกบล็อกชั่วคราวเพื่อให้ cache บริการ execution unit ก่อน ทำให้มันทำ instruction ที่กำลังประมวลผลอยู่เสร็จ cache contention นี้อาจลดประสิทธิภาพโดยรบกวนการใช้งาน instruction pipeline อย่างมีประสิทธิภาพ โครงสร้าง split cache เอาชนะปัญหานี้ได้

4.4 การจัดการแคชของ Pentium 4
วิวัฒนาการของ cache organization เห็นได้ชัดในวิวัฒนาการของ Intel microprocessor (ตารางที่ 4.4) 80386 ไม่มี on-chip cache 80486 มี on-chip cache เดียวขนาด 8 kB ใช้ line size 16 bytes และ four-way set-associative organization โปรเซสเซอร์ Pentium ทั้งหมดมี on-chip L1 cache สองตัว ตัวหนึ่งสำหรับ data และตัวหนึ่งสำหรับ instruction สำหรับ Pentium 4 L1 data cache มีขนาด 16 kB ใช้ line size 64 bytes และ four-way set-associative organization Pentium 4 instruction cache จะอธิบายในภายหลัง Pentium II ยังมี L2 cache ที่ feed L1 cache ทั้งสอง L2 cache เป็น eight-way set associative มีขนาด 512 kB และ line size 128 bytes L3 cache ถูกเพิ่มสำหรับ Pentium III และกลายเป็น on-chip กับ high-end version ของ Pentium 4

รูปที่ 4.18 ให้มุมมองที่เรียบง่ายของ Pentium 4 organization โดยเน้นตำแหน่งของ cache สามตัว processor core ประกอบด้วยส่วนประกอบหลักสี่อย่าง:

Fetch/decode unit: ดึง program instruction ตามลำดับจาก L2 cache แปลงเป็นชุด micro-operation และจัดเก็บผลลัพธ์ใน L1 instruction cache
Out-of-order execution logic: กำหนดตารางการประมวลผล micro-operation ตาม data dependency และความพร้อมของ resource ดังนั้น micro-operation อาจถูกกำหนดตารางสำหรับการประมวลผลในลำดับที่แตกต่างจากที่ดึงมาจาก instruction stream เมื่อมีเวลาว่าง unit นี้จะกำหนดตารางการประมวลผลแบบ speculative ของ micro-operation ที่อาจต้องการในอนาคต
Execution units: unit เหล่านี้ประมวลผล micro-operation โดยดึงข้อมูลที่ต้องการจาก L1 data cache และจัดเก็บผลลัพธ์ชั่วคราวใน register
Memory subsystem: unit นี้รวมถึง L2 และ L3 cache และ system bus ซึ่งใช้เข้าถึง main memory เมื่อ L1 และ L2 cache เกิด cache miss และเข้าถึง system I/O resource
ต่างจากองค์กรที่ใช้ใน Pentium model ก่อนหน้าทั้งหมด และโปรเซสเซอร์อื่นส่วนใหญ่ Pentium 4 instruction cache วางอยู่ระหว่าง instruction decode logic กับ execution core เหตุผลเบื้องหลังการตัดสินใจออกแบบนี้มีดังนี้: ดังที่อภิปรายอย่างเต็มที่มากขึ้นในบทที่ 16 Pentium process แปล Pentium machine instruction เป็น RISC-like instruction ที่เรียบง่ายที่เรียกว่า micro-operation การใช้ micro-operation ที่เรียบง่ายและมีความยาวคงที่ช่วยให้ใช้เทคนิค superscalar pipelining และ scheduling ที่ปรับปรุงประสิทธิภาพ อย่างไรก็ตาม Pentium machine instruction ยุ่งยากในการ decode มีจำนวน byte ที่แปรผันและตัวเลือกมากมาย ปรากฏว่าประสิทธิภาพจะดีขึ้นถ้าการ decode นี้ทำโดยอิสระจาก scheduling และ pipelining logic เราจะกลับมาที่หัวข้อนี้ในบทที่ 16

data cache ใช้ write-back policy: ข้อมูลถูกเขียนไปยัง main memory เฉพาะเมื่อถูกลบออกจาก cache และมีการอัปเดต Pentium 4 processor สามารถกำหนดค่าแบบ dynamic เพื่อรองรับ write-through caching

L1 data cache ถูกควบคุมโดยสองบิตใน control register หนึ่งตัว ซึ่งกำหนดเป็น CD (cache disable) และ NW (not write-through) bits (ตารางที่ 4.5) นอกจากนี้ยังมี Pentium 4 instruction สองตัวที่สามารถใช้ควบคุม data cache: INVD invalidates (flushes) internal cache memory และส่งสัญญาณให้ external cache (ถ้ามี) invalidate WBINVD เขียนกลับและ invalidate internal cache แล้วเขียนกลับและ invalidate external cache

ทั้ง L2 และ L3 cache เป็น eight-way set-associative มี line size 128 bytes

ตาราง 4.4 วิวัฒนาการของ Intel Cache

ปัญหา	แนวทางแก้ไข	โปรเซสเซอร์แรกที่ใช้
External memory ช้ากว่า system bus	เพิ่ม external cache โดยใช้เทคโนโลยีหน่วยความจำที่เร็วกว่า	386
ความเร็วโปรเซสเซอร์ที่เพิ่มขึ้นทำให้ external bus กลายเป็น bottleneck สำหรับการเข้าถึง cache	ย้าย external cache เข้าชิปทำงานที่ความเร็วเดียวกับโปรเซสเซอร์	486
Internal cache เล็กเกินไป เนื่องจากพื้นที่บนชิปจำกัด	เพิ่ม external L2 cache โดยใช้เทคโนโลยีที่เร็วกว่า main memory	486
Contention เกิดขึ้นเมื่อทั้ง Instruction Prefetcher และ Execution Unit ต้องการเข้าถึง cache พร้อมกัน	สร้าง data และ instruction cache แยกกัน	Pentium
ความเร็วโปรเซสเซอร์ที่เพิ่มขึ้นทำให้ external bus กลายเป็น bottleneck สำหรับการเข้าถึง L2 cache	สร้าง back-side bus แยกต่างหากที่ทำงานเร็วกว่า main (front-side) external bus โดย BSB เฉพาะสำหรับ L2 cache	Pentium Pro
ย้าย L2 cache เข้า processor chip	Pentium II
แอปพลิเคชันบางตัวจัดการ database ขนาดใหญ่และต้องการเข้าถึงข้อมูลจำนวนมากอย่างรวดเร็ว on-chip cache เล็กเกินไป	เพิ่ม external L3 cache	Pentium III
ย้าย L3 cache เข้าชิป	Pentium 4
ตาราง 4.5 โหมดการทำงานของ Pentium 4 Cache

Control Bits		Operating Mode		
CD	NW	Cache Fills	Write Throughs	Invalidates
0	0	Enabled	Enabled	Enabled
1	0	Disabled	Enabled	Enabled
1	1	Disabled	Disabled	Disabled
หมายเหตุ: CD = 0; NW = 1 เป็น combination ที่ไม่ถูกต้อง

4.5 คำสำคัญ คำถามทบทวน และโจทย์ปัญหา
คำสำคัญ
access time, associative mapping, cache hit, cache line, cache memory, cache miss, cache set, data cache, direct access, direct mapping, high-performance computing (HPC), hit, hit ratio, instruction cache, L1 cache, L2 cache, L3 cache, line, locality, logical cache, memory hierarchy, miss, multilevel cache, physical address, physical cache, random access, replacement algorithm, secondary memory, sequential access, set-associative mapping, spatial locality, split cache, tag, temporal locality, unified cache, virtual address, virtual cache, write back, write through

คำถามทบทวน
4.1 ความแตกต่างระหว่าง sequential access, direct access และ random access คืออะไร?

4.2 ความสัมพันธ์ทั่วไประหว่าง access time, ราคาหน่วยความจำ และความจุคืออะไร?

4.3 หลักการ locality เกี่ยวข้องกับการใช้หน่วยความจำหลายระดับอย่างไร?

4.4 ความแตกต่างระหว่าง direct mapping, associative mapping และ set-associative mapping คืออะไร?

4.5 สำหรับ direct-mapped cache ที่อยู่ main memory ถูกมองว่าประกอบด้วยสามฟิลด์ แสดงรายการและนิยามสามฟิลด์นั้น

4.6 สำหรับ associative cache ที่อยู่ main memory ถูกมองว่าประกอบด้วยสองฟิลด์ แสดงรายการและนิยามสองฟิลด์นั้น

4.7 สำหรับ set-associative cache ที่อยู่ main memory ถูกมองว่าประกอบด้วยสามฟิลด์ แสดงรายการและนิยามสามฟิลด์นั้น

4.8 ความแตกต่างระหว่าง spatial locality และ temporal locality คืออะไร?

4.9 โดยทั่วไป กลยุทธ์สำหรับการใช้ประโยชน์จาก spatial locality และ temporal locality คืออะไร?

โจทย์ปัญหา
4.1 set-associative cache ประกอบด้วย 64 line หรือ slot แบ่งเป็น four-line set Main memory มี 4K block ของ 128 word แต่ละ block แสดงรูปแบบที่อยู่ main memory

4.2 two-way set-associative cache มี line ขนาด 16 byte และขนาดรวม 8 kB Main memory ขนาด 64 MB สามารถระบุที่อยู่ได้ระดับ byte แสดงรูปแบบที่อยู่ main memory

4.3 สำหรับที่อยู่ main memory แบบ hexadecimal 111111, 666666, BBBBBB แสดงข้อมูลต่อไปนี้ในรูปแบบ hexadecimal:

a. ค่า Tag, Line และ Word สำหรับ direct-mapped cache โดยใช้รูปแบบของรูปที่ 4.10
b. ค่า Tag และ Word สำหรับ associative cache โดยใช้รูปแบบของรูปที่ 4.12
c. ค่า Tag, Set และ Word สำหรับ two-way set-associative cache โดยใช้รูปแบบของรูปที่ 4.15
4.4 แสดงค่าต่อไปนี้:

a. สำหรับตัวอย่าง direct cache ในรูปที่ 4.10: ความยาวที่อยู่, จำนวน addressable unit, ขนาด block, จำนวน block ใน main memory, จำนวน line ใน cache, ขนาด tag
b. สำหรับตัวอย่าง associative cache ในรูปที่ 4.12: ความยาวที่อยู่, จำนวน addressable unit, ขนาด block, จำนวน block ใน main memory, จำนวน line ใน cache, ขนาด tag
c. สำหรับตัวอย่าง two-way set-associative cache ในรูปที่ 4.15: ความยาวที่อยู่, จำนวน addressable unit, ขนาด block, จำนวน block ใน main memory, จำนวน line ใน set, จำนวน set, จำนวน line ใน cache, ขนาด tag
4.5 พิจารณา 32-bit microprocessor ที่มี on-chip 16-kB four-way set-associative cache สมมติว่า cache มี line size สี่ 32-bit word วาด block diagram ของ cache นี้ที่แสดงองค์กรและวิธีที่ address field ต่าง ๆ ใช้ระบุ cache hit/miss word จากตำแหน่ง memory ABCDE8F8 แมปไปยังที่ใดใน cache?

4.6 กำหนด specification ต่อไปนี้สำหรับ external cache memory: four-way set associative; line size สอง 16-bit word; สามารถรองรับ 4K 32-bit word รวมจาก main memory; ใช้กับ 16-bit processor ที่ออก 24-bit address ออกแบบโครงสร้าง cache พร้อมข้อมูลที่เกี่ยวข้องทั้งหมดและแสดงวิธีที่ cache ตีความที่อยู่ของโปรเซสเซอร์

4.7 Intel 80486 มี on-chip, unified cache มีขนาด 8 kB และมี four-way set-associative organization และ block length สี่ 32-bit word cache ถูกจัดเป็น 128 set มี "line valid bit" เดียวและสามบิต B0, B1 และ B2 ("LRU" bits) ต่อ line เมื่อ cache miss 80486 อ่าน 16-byte line จาก main memory ใน bus memory read burst วาด diagram ที่เรียบง่ายของ cache และแสดงวิธีที่ address field ต่าง ๆ ถูกตีความ

4.8 พิจารณาเครื่องที่มี byte addressable main memory ขนาด 2¹⁶ byte และ block size 8 byte สมมติว่าใช้ direct mapped cache ประกอบด้วย 32 line กับเครื่องนี้

a. 16-bit memory address ถูกแบ่งอย่างไรเป็น tag, line number และ byte number?
b. byte ที่มีที่อยู่แต่ละตัวต่อไปนี้จะถูกจัดเก็บใน line ใด?
0001 0001 0001 1011
1100 0011 0011 0100
1101 0000 0001 1101
1010 1010 1010 1010
c. สมมติว่า byte ที่มีที่อยู่ 0001 1010 0001 1010 ถูกจัดเก็บใน cache ที่อยู่ของ byte อื่นที่จัดเก็บพร้อมกับมันคืออะไร?
d. จำนวน byte ทั้งหมดของ memory ที่สามารถจัดเก็บใน cache คือเท่าไหร่?
e. ทำไม tag ถึงถูกจัดเก็บใน cache ด้วย?
4.9 สำหรับ on-chip cache Intel 80486 ใช้ replacement algorithm ที่เรียกว่า pseudo least recently used เชื่อมกับแต่ละ 128 set ของสี่ line (ระบุว่า L0, L1, L2, L3) มีสามบิต B0, B1 และ B2 replacement algorithm ทำงานดังนี้: เมื่อ line ต้องถูกแทนที่ cache จะระบุก่อนว่าการใช้ล่าสุดมาจาก L0 และ L1 หรือ L2 และ L3 แล้ว cache จะระบุว่า block คู่ใดถูกใช้น้อยกว่าล่าสุดและระบุให้แทนที่

a. ระบุวิธีที่บิต B0, B1 และ B2 ถูกตั้งค่า แล้วอธิบายด้วยคำพูดว่าใช้อย่างไรใน replacement algorithm
b. แสดงว่า algorithm ของ 80486 เป็น approximation ของ LRU algorithm จริง ๆ
c. แสดงให้เห็นว่า LRU algorithm จริงต้องการ 6 bit ต่อ set
4.10 set-associative cache มี block size สี่ 16-bit word และ set size 2 cache สามารถรองรับรวม 4096 word main memory ขนาดที่สามารถ cache ได้คือ 64K × 32 bit ออกแบบโครงสร้าง cache และแสดงวิธีที่ที่อยู่ของโปรเซสเซอร์ถูกตีความ

4.11 พิจารณาระบบหน่วยความจำที่ใช้ 32-bit address ระบุที่อยู่ระดับ byte บวกกับ cache ที่ใช้ line size 64 byte

a. สมมติว่า direct mapped cache มี tag field ในที่อยู่ 20 bit แสดงรูปแบบที่อยู่และระบุ parameter ต่อไปนี้: จำนวน addressable unit, จำนวน block ใน main memory, จำนวน line ใน cache, ขนาด tag
b. สมมติว่า associative cache แสดงรูปแบบที่อยู่และระบุ parameter: จำนวน addressable unit, จำนวน block ใน main memory, จำนวน line ใน cache, ขนาด tag
c. สมมติว่า four-way set-associative cache มี tag field ในที่อยู่ 9 bit แสดงรูปแบบที่อยู่และระบุ parameter: จำนวน addressable unit, จำนวน block ใน main memory, จำนวน line ใน set, จำนวน set ใน cache, จำนวน line ใน cache, ขนาด tag
4.12 พิจารณาคอมพิวเตอร์ที่มีคุณลักษณะต่อไปนี้: main memory รวม 1 MB; word size 1 byte; block size 16 byte; และ cache size 64 kB

a. สำหรับที่อยู่ main memory F0010, 01234 และ CABBE ให้ tag ที่สอดคล้องกัน, cache line address และ word offset สำหรับ direct-mapped cache
b. ให้สองที่อยู่ main memory ใด ๆ ที่มี tag ต่างกันซึ่งแมปไปยัง cache slot เดียวกันสำหรับ direct-mapped cache
c. สำหรับที่อยู่ main memory F0010 และ CABBE ให้ค่า tag และ offset ที่สอดคล้องกันสำหรับ fully-associative cache
d. สำหรับที่อยู่ main memory F0010 และ CABBE ให้ค่า tag, cache set และ offset ที่สอดคล้องกันสำหรับ two-way set-associative cache
4.13 อธิบายเทคนิคง่าย ๆ สำหรับการนำ LRU replacement algorithm ไปใช้ใน four-way set-associative cache

4.14 พิจารณาตัวอย่าง 4.3 อีกครั้ง คำตอบเปลี่ยนไปอย่างไรถ้า main memory ใช้ block transfer capability ที่มี first-word access time 30 ns และ access time 5 ns สำหรับแต่ละ word ต่อไป?

4.15 พิจารณา code ต่อไปนี้:

for (i = 0; i < 20; i++)
  for (j = 0; j < 10; j++)
    a[i] = a[i] * j
a. ให้ตัวอย่างหนึ่งของ spatial locality ใน code
b. ให้ตัวอย่างหนึ่งของ temporal locality ใน code
4.16 Generalize สมการ (4.2) และ (4.3) ในภาคผนวก 4A ให้กับ N-level memory hierarchy

4.17 ระบบคอมพิวเตอร์มี main memory 32K 16-bit word และ 4K word cache แบ่งเป็น four-line set ที่มี 64 word ต่อ line สมมติว่า cache ว่างในตอนแรก โปรเซสเซอร์ดึง word จากตำแหน่ง 0, 1, 2, …, 4351 ตามลำดับ แล้วทำซ้ำลำดับการดึงนี้อีกเก้าครั้ง cache เร็วกว่า main memory 10 เท่า ประมาณการปรับปรุงที่เกิดจากการใช้ cache สมมติว่าใช้ LRU policy สำหรับการแทนที่ block

4.18 พิจารณา cache ที่มี 4 line ของ 16 byte แต่ละตัว Main memory แบ่งเป็น block ขนาด 16 byte แต่ละ block นั่นคือ block 0 มี byte ที่มีที่อยู่ 0 ถึง 15 เป็นต้น พิจารณาโปรแกรมที่เข้าถึงหน่วยความจำในลำดับที่อยู่ต่อไปนี้: ครั้งเดียว: 63 ถึง 70; วนซ้ำสิบครั้ง: 15 ถึง 32; 80 ถึง 95

a. สมมติว่า cache ถูกจัดเป็น direct mapped Memory block 0, 4 และต่อไปถูกกำหนดให้ line 1; block 1, 5 และต่อไปให้ line 2; เป็นต้น คำนวณ hit ratio
b. สมมติว่า cache ถูกจัดเป็น two-way set associative มีสอง set ของสอง line แต่ละตัว block คู่ถูกกำหนดให้ set 0 และ block คี่ถูกกำหนดให้ set 1 คำนวณ hit ratio สำหรับ two-way set-associative cache โดยใช้ least recently used replacement scheme
4.19 พิจารณาระบบหน่วยความจำที่มี parameter ต่อไปนี้: $$T_c = 100\ \text{ns} \quad C_c = 10^{-4}\ \text{$/bit}$$ $$T_m = 1200\ \text{ns} \quad C_m = 10^{-5}\ \text{$/bit}$$

a. ราคาของ main memory 1 MB คือเท่าไหร่?
b. ราคาของ main memory 1 MB โดยใช้เทคโนโลยี cache memory คือเท่าไหร่?
c. ถ้า effective access time มากกว่า cache access time 10% hit ratio H คือเท่าไหร่?
4.20

a. พิจารณา L1 cache ที่มี access time 1 ns และ hit ratio H = 0.95 สมมติว่าเราสามารถเปลี่ยนการออกแบบ cache (ขนาด cache, cache organization) เพื่อเพิ่ม H เป็น 0.97 แต่เพิ่ม access time เป็น 1.5 ns เงื่อนไขใดบ้างที่ต้องเป็นจริงเพื่อให้การเปลี่ยนแปลงนี้ส่งผลให้ประสิทธิภาพดีขึ้น?
d. อธิบายว่าทำไมผลลัพธ์นี้จึงสมเหตุสมผลในเชิงสัญชาตญาณ
4.21 พิจารณา single-level cache ที่มี access time 2.5 ns, line size 64 byte และ hit ratio H = 0.95 Main memory ใช้ block transfer capability ที่มี first-word (4 byte) access time 50 ns และ access time 5 ns สำหรับแต่ละ word ต่อไป

a. access time เมื่อเกิด cache miss คือเท่าไหร่? สมมติว่า cache รอจนกว่า line จะถูกดึงจาก main memory แล้วประมวลผลซ้ำสำหรับ hit
b. สมมติว่าการเพิ่ม line size เป็น 128 byte ทำให้ H เพิ่มเป็น 0.97 สิ่งนี้ลด average memory access time หรือไม่?
4.22 คอมพิวเตอร์มี cache, main memory และ disk ที่ใช้สำหรับ virtual memory ถ้า word ที่อ้างอิงอยู่ใน cache จะใช้เวลา 20 ns ในการเข้าถึง ถ้าอยู่ใน main memory แต่ไม่ใน cache จะต้องใช้ 60 ns เพื่อโหลดเข้า cache แล้วเริ่มอ้างอิงใหม่ ถ้า word ไม่อยู่ใน main memory จะต้องใช้ 12 ms เพื่อดึง word จาก disk ตามด้วย 60 ns เพื่อ copy ไปยัง cache แล้วเริ่มอ้างอิงใหม่ cache hit ratio คือ 0.9 และ main memory hit ratio คือ 0.6 เวลาเฉลี่ยเป็นนาโนวินาทีที่ต้องการในการเข้าถึง word ที่อ้างอิงบนระบบนี้คือเท่าไหร่?

4.23 พิจารณา cache ที่มี line size 64 byte สมมติว่าโดยเฉลี่ย 30% ของ line ใน cache เป็น dirty word ประกอบด้วย 8 byte

a. สมมติว่ามี miss rate 3% (0.97 hit ratio) คำนวณปริมาณ main memory traffic เป็น byte ต่อ instruction สำหรับทั้ง write-through และ write-back policy Memory ถูกอ่านเข้า cache ทีละ line แต่สำหรับ write-back word เดียวสามารถเขียนจาก cache ไปยัง main memory
b. ทำซ้ำส่วน a สำหรับ rate 5%
c. ทำซ้ำส่วน a สำหรับ rate 7%
d. ข้อสรุปใดที่คุณสามารถดึงออกมาจากผลลัพธ์เหล่านี้?
4.24 บน Motorola 68020 microprocessor การเข้าถึง cache ใช้เวลา clock cycle สองรอบ การเข้าถึง data จาก main memory ผ่าน bus ไปยังโปรเซสเซอร์ใช้เวลา clock cycle สามรอบในกรณีที่ไม่มีการแทรก wait state; ข้อมูลถูกส่งให้โปรเซสเซอร์พร้อมกันกับการส่งไปยัง cache

a. คำนวณความยาวที่มีประสิทธิภาพของ memory cycle โดยกำหนด hit ratio 0.9 และ clocking rate 16.67 MHz
b. ทำซ้ำการคำนวณโดยสมมติว่ามีการแทรก wait state สองตัวของ one cycle แต่ละตัวต่อ memory cycle คุณสรุปอะไรได้จากผลลัพธ์?
4.25 สมมติว่าโปรเซสเซอร์มี memory cycle time 300 ns และ instruction processing rate 1 MIPS โดยเฉลี่ยแต่ละ instruction ต้องการ bus memory cycle หนึ่งรอบสำหรับ instruction fetch และหนึ่งรอบสำหรับ operand ที่เกี่ยวข้อง

a. คำนวณ utilization ของ bus โดยโปรเซสเซอร์
b. สมมติว่าโปรเซสเซอร์ติดตั้ง instruction cache และ hit ratio ที่เกี่ยวข้องคือ 0.5 ระบุผลกระทบต่อ bus utilization
4.26 ประสิทธิภาพของระบบ single-level cache สำหรับ read operation สามารถระบุลักษณะได้ด้วยสมการต่อไปนี้: $$T_a = T_c + (1-H)T_m$$ โดยที่ $T_a$ คือ average access time, $T_c$ คือ cache access time, $T_m$ คือ memory access time (memory to processor register) และ H คือ hit ratio สำหรับความเรียบง่าย สมมติว่า word ที่เป็นคำถามถูกโหลดเข้า cache พร้อมกันกับการโหลดเข้า processor register นี่เป็นรูปแบบเดียวกับสมการ (4.2)

a. นิยาม $T_b$ = เวลาในการถ่ายโอน line ระหว่าง cache และ main memory และ W = เศษส่วนของ write reference ปรับสมการก่อนหน้าเพื่อรองรับทั้ง write และ read โดยใช้ write-through policy
b. นิยาม $W_b$ เป็นความน่าจะเป็นที่ line ใน cache ถูกแก้ไข ให้สมการสำหรับ $T_a$ สำหรับ write-back policy
4.27 สำหรับระบบที่มี cache สองระดับ นิยาม $T_{c_1}$ = first-level cache access time; $T_{c_2}$ = second-level cache access time; $T_m$ = memory access time; $H_1$ = first-level cache hit ratio; $H_2$ = combined first/second level cache hit ratio ให้สมการสำหรับ $T_a$ สำหรับ read operation

4.28 สมมติว่ามีคุณลักษณะประสิทธิภาพต่อไปนี้บน cache read miss: หนึ่ง clock cycle เพื่อส่งที่อยู่ไปยัง main memory และสี่ clock cycle เพื่อเข้าถึง 32-bit word จาก main memory และถ่ายโอนไปยังโปรเซสเซอร์และ cache

a. ถ้า cache line size เป็น one word miss penalty (เวลาเพิ่มเติมที่ต้องการสำหรับ read เมื่อเกิด read miss) คือเท่าไหร่?
b. miss penalty คือเท่าไหรถ้า cache line size เป็นสี่ word และมีการดำเนินการ multiple, nonburst transfer?
c. miss penalty คือเท่าไหรถ้า cache line size เป็นสี่ word และดำเนินการ transfer โดยมีหนึ่ง clock cycle ต่อ word transfer?
4.29 สำหรับการออกแบบ cache ของปัญหาก่อนหน้า สมมติว่าการเพิ่ม line size จาก one word เป็นสี่ word ส่งผลให้ read miss rate ลดลงจาก 3.2% เป็น 1.1% สำหรับทั้ง nonburst transfer และ burst transfer case average miss penalty เฉลี่ยในการ read ทั้งหมดสำหรับ line size ทั้งสองแบบคือเท่าไหร่?

ภาคผนวก 4A คุณลักษณะด้านประสิทธิภาพของหน่วยความจำสองระดับ
ในบทนี้ มีการอ้างถึง cache ที่ทำหน้าที่เป็น buffer ระหว่าง main memory กับโปรเซสเซอร์ สร้างหน่วยความจำภายในสองระดับ สถาปัตยกรรมสองระดับนี้ใช้ประโยชน์จากคุณสมบัติที่เรียกว่า locality เพื่อให้ประสิทธิภาพที่ดีกว่าหน่วยความจำระดับเดียวที่เทียบเคียงได้

กลไก main memory cache เป็นส่วนหนึ่งของสถาปัตยกรรมคอมพิวเตอร์ นำไปใช้ใน hardware และโดยทั่วไปมองไม่เห็นสำหรับ operating system มีอีกสองกรณีของ two-level memory approach ที่ใช้ประโยชน์จาก locality และที่อย่างน้อยบางส่วนนำไปใช้ใน operating system ได้แก่ virtual memory และ disk cache (ตารางที่ 4.6) Virtual memory ศึกษาในบทที่ 8; disk cache อยู่นอกขอบเขตของหนังสือเล่มนี้แต่ศึกษาใน [STAL15] ในภาคผนวกนี้ เราจะดูคุณลักษณะด้านประสิทธิภาพบางส่วนของหน่วยความจำสองระดับที่เหมือนกันในสามแนวทาง

ตาราง 4.6 คุณลักษณะของหน่วยความจำสองระดับ

Main Memory Cache	Virtual Memory (paging)	Disk Cache
Typical access time ratios	5:1 (main memory vs. cache)	10⁶:1 (main memory vs. disk)	10⁶:1 (main memory vs. disk)
Memory management system	Implemented by special hardware	Combination of hardware and system software	System software
Typical block or page size	4 to 128 bytes (cache block)	64 to 4096 bytes (virtual memory page)	64 to 4096 bytes (disk block or pages)
Access of processor to second level	Direct access	Indirect access	Indirect access
Locality
พื้นฐานของข้อได้เปรียบด้านประสิทธิภาพของหน่วยความจำสองระดับคือหลักการที่เรียกว่า locality of reference [DENN68] หลักการนี้ระบุว่าการอ้างอิงหน่วยความจำมีแนวโน้มที่จะกระจุกตัว ในช่วงเวลายาว กลุ่มที่ใช้งานจะเปลี่ยนแปลง แต่ในช่วงเวลาสั้น โปรเซสเซอร์ส่วนใหญ่ทำงานกับกลุ่มการอ้างอิงหน่วยความจำที่คงที่

โดยสัญชาตญาณ หลักการของ locality สมเหตุสมผล พิจารณา reasoning ต่อไปนี้:

ยกเว้น branch และ call instruction ซึ่งคิดเป็นเพียงเศษส่วนเล็กน้อยของ program instruction ทั้งหมด การทำงานของโปรแกรมเป็น sequential ดังนั้นในกรณีส่วนใหญ่ instruction ถัดไปที่จะดึงจะตามหลัง instruction สุดท้ายที่ดึงมาทันที

การมีลำดับยาวของ procedure call ที่ไม่ถูกขัดจังหวะตามด้วยลำดับ return ที่สอดคล้องกันเป็นเรื่องที่เกิดขึ้นน้อย โดยทั่วไปโปรแกรมจะถูกจำกัดอยู่ในช่วงความลึกของการเรียก procedure ที่แคบ ดังนั้น ในช่วงเวลาสั้นการอ้างอิงไปยัง instruction มีแนวโน้มที่จะอยู่ใน procedure จำนวนน้อย

iterative construct ส่วนใหญ่ประกอบด้วย instruction จำนวนน้อยที่ทำซ้ำหลายครั้ง ตลอดระยะเวลาของการ iteration การคำนวณจึงถูกจำกัดอยู่ในส่วนเล็ก ๆ ที่ต่อเนื่องกันของโปรแกรม

ในโปรแกรมหลายตัว การคำนวณส่วนใหญ่เกี่ยวข้องกับการประมวลผล data structure เช่น array หรือลำดับ record ในหลายกรณี การอ้างอิงต่อเนื่องไปยัง data structure เหล่านี้จะเป็น data item ที่อยู่ใกล้เคียงกัน

reasoning นี้ได้รับการยืนยันในการศึกษาหลายชิ้น อ้างอิงถึงข้อ 1 การศึกษาหลายชิ้นวิเคราะห์พฤติกรรมของ high-level language program ตารางที่ 4.7 รวมผลลัพธ์สำคัญ ซึ่งวัดการปรากฏของ statement ประเภทต่าง ๆ ในระหว่างการทำงาน จากการศึกษาต่อไปนี้ การศึกษาแรกสุดของพฤติกรรม programming language โดย Knuth [KNUT71] ตรวจสอบ FORTRAN program ที่ใช้เป็น student exercise Tanenbaum [TANE78] เผยแพร่การวัดที่รวบรวมจาก procedure มากกว่า 300 ตัวที่ใช้ใน operating-system program Patterson and Sequein [PATT82a] วิเคราะห์ชุดการวัดที่นำมาจาก compiler และ program สำหรับ typesetting, computer-aided design (CAD), sorting และ file comparison Huck [HUCK83] วิเคราะห์สี่โปรแกรมที่ตั้งใจจะเป็นตัวแทนของ general-purpose scientific computing มีข้อตกลงที่ดีในผลลัพธ์ของ language และ application ที่หลากหลายนี้ว่า branch และ call instruction คิดเป็นเพียงเศษส่วนของ statement ที่ทำงานในระหว่างอายุการใช้งานของโปรแกรม ดังนั้น การศึกษาเหล่านี้ยืนยัน assertion 1

ตาราง 4.7 ความถี่ Dynamic สัมพัทธ์ของการดำเนินการภาษาระดับสูง

Study	[HUCK83] Pascal Scientific	[KNUT71] FORTRAN Student	[PATT82a] Pascal System	[PATT82a] C System	[TANE78] SAL System
Assign	74	67	45	38	42
Loop	4	3	5	3	4
Call	1	3	15	12	12
IF	20	11	29	43	36
GOTO	2	9	—	3	—
Other	—	7	6	1	6
เกี่ยวกับ assertion 2 การศึกษาที่รายงานใน [PATT85a] ให้การยืนยัน รูปที่ 4.20 แสดงพฤติกรรม call-return แต่ละ call แสดงด้วยเส้นที่เคลื่อนลงและไปทางขวา และแต่ละ return โดยเส้นที่เคลื่อนขึ้นและไปทางขวา ในรูป window ที่มีความลึกเท่ากับ 5 ถูกนิยาม เฉพาะลำดับของ call และ return ที่มีการเคลื่อนสุทธิ 6 ในทิศทางใดทิศทางหนึ่งทำให้ window เคลื่อน ดังที่เห็น โปรแกรมที่ทำงานอยู่สามารถอยู่ใน window ที่อยู่กับที่เป็นระยะเวลายาว การศึกษาโดยนักวิเคราะห์คนเดียวกันของ C และ Pascal program แสดงว่า window ที่มีความลึก 8 จะต้องเคลื่อนเพียงน้อยกว่า 1% ของ call หรือ return [TAMI83]

ในวรรณกรรมมีการแยกแยะระหว่าง spatial locality และ temporal locality Spatial locality หมายถึงแนวโน้มของการทำงานที่เกี่ยวข้องกับตำแหน่งหน่วยความจำจำนวนหนึ่งที่กระจุกตัว ซึ่งสะท้อนแนวโน้มของโปรเซสเซอร์ในการเข้าถึง instruction ตามลำดับ Spatial locality ยังสะท้อนแนวโน้มของโปรแกรมในการเข้าถึงตำแหน่งข้อมูลตามลำดับ เช่น เมื่อประมวลผล table ของข้อมูล Temporal locality หมายถึงแนวโน้มของโปรเซสเซอร์ในการเข้าถึงตำแหน่งหน่วยความจำที่ใช้ล่าสุด ตัวอย่างเช่น เมื่อ iteration loop ถูกทำงาน โปรเซสเซอร์จะทำชุด instruction เดิมซ้ำ ๆ

โดยเดิมทีแล้ว temporal locality ถูกใช้ประโยชน์โดยการเก็บค่า instruction และ data ที่ใช้ล่าสุดไว้ใน cache memory และโดยการใช้ประโยชน์จาก cache hierarchy Spatial locality โดยทั่วไปถูกใช้ประโยชน์โดยใช้ cache block ขนาดใหญ่กว่าและโดยการรวม prefetching mechanism (การดึง item ที่คาดว่าจะใช้) เข้าไปใน cache control logic ล่าสุดมีการวิจัยอย่างมากในการปรับปรุงเทคนิคเหล่านี้เพื่อให้ได้ประสิทธิภาพที่มากขึ้น แต่กลยุทธ์พื้นฐานยังคงเดิม

การทำงานของหน่วยความจำสองระดับ
คุณสมบัติ locality สามารถใช้ประโยชน์ในการสร้างหน่วยความจำสองระดับ upper-level memory (M1) เล็กกว่า เร็วกว่า และแพงกว่า (ต่อบิต) กว่า lower-level memory (M2) M1 ถูกใช้เป็น temporary store สำหรับส่วนหนึ่งของเนื้อหาของ M2 ที่ใหญ่กว่า เมื่อมีการอ้างอิงหน่วยความจำ จะพยายามเข้าถึง item ใน M1 ถ้าสำเร็จก็เข้าถึงได้อย่างรวดเร็ว ถ้าไม่สำเร็จ block ของตำแหน่งหน่วยความจำจะถูก copy จาก M2 ไปยัง M1 แล้วการเข้าถึงจะเกิดขึ้นผ่าน M1 เนื่องจาก locality เมื่อ block ถูกนำเข้า M1 ควรมีการเข้าถึงตำแหน่งใน block นั้นจำนวนหนึ่ง ส่งผลให้บริการรวมเร็ว

เพื่อแสดงเวลาเฉลี่ยในการเข้าถึง item เราต้องพิจารณาไม่เพียงความเร็วของหน่วยความจำสองระดับ แต่ยังความน่าจะเป็นที่การอ้างอิงที่กำหนดสามารถพบได้ใน M1 เรามี:

$$T_s = H \times T_1 + (1-H) \times (T_1 + T_2) = T_1 + (1-H) \times T_2 \quad (4.2)$$

โดยที่:

$T_s$ = เวลาการเข้าถึงเฉลี่ย (system)
$T_1$ = access time ของ M1 (เช่น cache, disk cache)
$T_2$ = access time ของ M2 (เช่น main memory, disk)
$H$ = hit ratio (เศษส่วนของเวลาที่พบการอ้างอิงใน M1)
รูปที่ 4.2 แสดง average access time ในฐานะฟังก์ชันของ hit ratio ดังที่เห็น สำหรับเปอร์เซ็นต์ hit สูง เวลาเข้าถึงรวมเฉลี่ยใกล้เคียงกับ M1 มากกว่า M2

ประสิทธิภาพ
มาดู parameter บางอย่างที่เกี่ยวข้องกับการประเมินกลไกหน่วยความจำสองระดับ ก่อนอื่นพิจารณาค่าใช้จ่าย เรามี:

$$C_s = \frac{C_1 S_1 + C_2 S_2}{S_1 + S_2} \quad (4.3)$$

โดยที่:

$C_s$ = ราคาต่อบิตเฉลี่ยสำหรับหน่วยความจำสองระดับรวม
$C_1$ = ราคาต่อบิตเฉลี่ยของ upper-level memory M1
$C_2$ = ราคาต่อบิตเฉลี่ยของ lower-level memory M2
$S_1$ = ขนาดของ M1
$S_2$ = ขนาดของ M2
เราต้องการ $C_s \approx C_2$ เมื่อกำหนดว่า $C_1 \gg C_2$ ต้องการ $S_1 < S_2$

ต่อมาพิจารณา access time สำหรับหน่วยความจำสองระดับเพื่อให้ได้การปรับปรุงประสิทธิภาพที่มีนัยสำคัญ เราต้องการ $T_s \approx T_1$ เมื่อกำหนดว่า $T_1 \ll T_2$ จำเป็นต้องมี hit ratio ใกล้ 1

ดังนั้นเราต้องการให้ M1 เล็กเพื่อลดค่าใช้จ่าย และใหญ่เพื่อปรับปรุง hit ratio และประสิทธิภาพ มีขนาดของ M1 ที่ตอบสนองความต้องการทั้งสองในระดับที่สมเหตุสมผลหรือไม่? เราสามารถตอบคำถามนี้ด้วยชุดคำถามย่อย:

Hit ratio ที่ต้องการเพื่อให้ $T_s \approx T_1$ คือเท่าไหร่?
ขนาดของ M1 ที่ต้องการเพื่อรับประกัน hit ratio ที่ต้องการคือเท่าไหร่?
ขนาดนี้ตอบสนองความต้องการด้านค่าใช้จ่ายหรือไม่?
เพื่อหาคำตอบ พิจารณา $T_1/T_s$ ซึ่งเรียกว่า access efficiency เป็นการวัดว่า average access time ($T_s$) ใกล้เคียงกับ M1 access time ($T_1$) มากแค่ไหน จากสมการ (4.2):

$$\frac{T_1}{T_s} = \frac{1}{1 + (1-H)\frac{T_2}{T_1}} \quad (4.4)$$

รูปที่ 4.22 วาดกราฟ $T_1/T_s$ ในฐานะฟังก์ชันของ hit ratio H โดยมีปริมาณ $T_2/T_1$ เป็น parameter โดยทั่วไป on-chip cache access time เร็วกว่า main memory access time ประมาณ 25 ถึง 50 เท่า (กล่าวคือ $T_2/T_1$ คือ 25 ถึง 50) off-chip cache access time เร็วกว่า main memory access time ประมาณ 5 ถึง 15 เท่า (กล่าวคือ $T_2/T_1$ คือ 5 ถึง 15) และ main memory access time เร็วกว่า disk access time ประมาณ 1000 เท่า ($T_2/T_1 = 1000$) ดังนั้น hit ratio ในช่วงใกล้ 0.9 ดูเหมือนจะต้องการเพื่อตอบสนองความต้องการด้านประสิทธิภาพ

ตอนนี้เราสามารถกำหนดคำถามเกี่ยวกับขนาดหน่วยความจำสัมพัทธ์ได้อย่างแม่นยำขึ้น Hit ratio ที่ 0.8 หรือดีกว่านั้นสมเหตุสมผลสำหรับ $S_1 \ll S_2$ หรือไม่? ซึ่งจะขึ้นอยู่กับปัจจัยหลายอย่าง รวมถึงลักษณะของซอฟต์แวร์ที่ทำงานอยู่และรายละเอียดของการออกแบบหน่วยความจำสองระดับ ตัวกำหนดหลักคือแน่นอนว่าระดับของ locality รูปที่ 4.23 แสดงให้เห็นผลที่ locality มีต่อ hit ratio ชัดเจน ถ้า M1 มีขนาดเดียวกับ M2 แล้ว hit ratio จะเป็น 1.0: item ทั้งหมดใน M2 ก็จะถูกจัดเก็บใน M1 เสมอ สมมติว่าไม่มี locality กล่าวคือ การอ้างอิงเป็นแบบสุ่มสมบูรณ์ ในกรณีนั้น hit ratio ควรเป็นฟังก์ชัน linear อย่างเคร่งครัดของขนาดหน่วยความจำสัมพัทธ์ ตัวอย่างเช่น ถ้า M1 มีขนาดครึ่งหนึ่งของ M2 ดังนั้นในเวลาใดก็ตาม ครึ่งหนึ่งของ item จาก M2 ก็อยู่ใน M1 และ hit ratio จะเป็น 0.5 ในทางปฏิบัติ อย่างไรก็ตาม มีระดับของ locality บางอย่างในการอ้างอิง ผลของ moderate และ strong locality แสดงในรูป โปรดสังเกตว่ารูปที่ 4.23 ไม่ได้ดึงมาจากข้อมูลหรือโมเดลเฉพาะ รูปแนะนำประเภทประสิทธิภาพที่เห็นกับระดับต่าง ๆ ของ locality

ดังนั้นถ้ามี strong locality จะเป็นไปได้ที่จะได้ hit ratio สูงแม้กระทั่งกับขนาด upper-level memory ที่ค่อนข้างเล็ก ตัวอย่างเช่น การศึกษาหลายชิ้นแสดงให้เห็นว่า cache ขนาดค่อนข้างเล็กจะให้ hit ratio เกิน 0.75 โดยไม่ขึ้นกับขนาดของ main memory (เช่น [AGAR89], [PRZY88], [STRE83] และ [SMIT82]) cache ในช่วง 1K ถึง 128K word โดยทั่วไปเพียงพอ ในขณะที่ main memory ในปัจจุบันโดยทั่วไปอยู่ในช่วง gigabyte เมื่อเราพิจารณา virtual memory และ disk cache เราจะอ้างถึงการศึกษาอื่น ๆ ที่ยืนยันปรากฏการณ์เดียวกัน กล่าวคือ M1 ขนาดเล็กสัมพัทธ์ให้ hit ratio สูงเนื่องจาก locality

ทำให้เรามาถึงคำถามสุดท้ายที่ระบุไว้ก่อนหน้า: ขนาดสัมพัทธ์ของหน่วยความจำสองตัวตอบสนองความต้องการด้านค่าใช้จ่ายหรือไม่? คำตอบชัดเจนว่าใช่ ถ้าเราต้องการเพียง upper-level memory ขนาดค่อนข้างเล็กเพื่อให้ได้ประสิทธิภาพที่ดี ราคาต่อบิตเฉลี่ยของหน่วยความจำสองระดับจะเข้าใกล้ค่าของ lower-level memory ที่ถูกกว่า

โปรดสังเกตว่าด้วย L2 cache หรือแม้กระทั่ง L2 และ L3 cache ที่เกี่ยวข้อง การวิเคราะห์ซับซ้อนกว่ามาก ดู [PEIR99] และ [HAND98] สำหรับการอภิปราย
