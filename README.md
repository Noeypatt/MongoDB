# MongoDB

[MongoDB | NOSQL](https://www.youtube.com/watch?v=VgyEablNJkk&t=19915s)

Database: ฐานข้อมูล

- กลุ่มข้อมูลที่ถูกรวบรวม
- แบ่งได้เป็น 2 แบบ (Modal)
  - Relational Database
    - ใช้ภาษา SQL
    - มีการจัดเก็บข้อมูลแบบตาราง (Table)
    - โครงสร้างแน่นอน
    - มีความเชื่อมโยงกับตารางอื่น
    - การปรับขนาด => ข้อมูลมีจำนวนมากขึ้น จะปรับขนาดในแนวตั้ง(Vertical-Scaling) โดยติดตั้งอุปกรณ์เพื่อเพิ่มความจุ เพิ่มแรมและหน่วยประมวลผล (ต้องปิดเครื่อง)
    - DBMS: ระบบจัดการฐานข้อมูล (MySQL Oracle ProgressSQL)
  - Non-Relational Database or NoSQL
    - จัดการข้อจำกัดของ Relational Database ในอดีต เพื่อปรับเปลี่ยนโครงสร้างการจัดเก็บข้อมูลให้มีความหยืดหยุ่น (การเพิ่ม feild เปลี่ยนโครงสร้างตารางข้อมูล)
    - จัดการพวกคลังข้อมูล หรือการจัดเก็บข้อมูลที่มีความหลากหลาย
    - ไม่มีการจัดเก็บข้อมูลที่มีความสัมพันธ์กัน
    - โครงสร้าง dynamic
    - การปรับขนาด => ข้อมูลมีจำนวนมากขึ้น จะปรับขนาดในแนวนอน(Horizontal-Scaling) โดยเพิ่ม Server ไปแบ่งภาระของข้อมูล (ติดตั้ง และเปลี่ยนแปลงได้ตลอดเวลา โดยไม่ต้องปิดเครื่อง)
    - การจัดเก็บข้อมูล 4 รูปแบบ (Key-Value, Document Store, Graph, Wid Column)
      - Key-Value: (key:value) => Oracle, Reddit
      - Document Store: สร้างในรูปแบบ object  
        `{ "id":"1", "name":"test" }`
        เก็บข้อมูลเป็น BSON => mongoDB
      - Graph: เก็บเป็นเส้นความสัมพันธ์ => neo4j
      - Wid Column: เก็บในรูปแบบตาราง แถว และคอลัมน์ (แบบ key:value 2 มิติ)โดยในแต่ละคอลัมน์ของแต่ละข้อมูลไม่ต้องเหมือนกันก็ได้ => DynamoDB

---

### รู้จักกับ MongoDB

เป็นฐานข้อมูลเชิงเอกสาร (Document) จะอยู่ในรูปแบบเอกสาร JSON `{}` แล้วเซฟเก็บไว้ในเอกสารรูปแบบไบนารี่ BSON เพื่อให้ MongoDB ทำงานได้เร็วขึ้น

- โครงสร้างการจัดเก็บข้อมูล มีองค์ประกอบ 3 ส่วน (เรียงใหญ่ไปเล็ก)
  - Database
  - Collection ชุดข้อมูล
  - Documents เก็บข้อมูลแบบ Key-Value (Row)
- ความแตกต่างระหว่าง SQL กับ MongoDB

| SQL                  | MongoDB                        |
| -------------------- | ------------------------------ |
| Database             | Database                       |
| Table                | Collection                     |
| Row                  | document or BSON document      |
| Column               | Field                          |
| Index                | Index                          |
| table joins          | embedded documents and linking |
| primary key (unique) | primary key (auto set `_id`)   |
| aggregation (group)  | aggregation pipeline           |

EX. SQL

User Table
| ID | Name |
| ----------- | ----------- |
| 1 | Test |

Address Table
| User ID | City |
| ----------- | ----------- |
| 1 | Bangkok |

Ex. MongoDB

```json
{
  "ID": "1",
  "Name": "Test",
  "Address": {
    "City": "Bangkok"
  }
}
```

---

### JSON และ BSON

- JSON: JavaScrip Object Notation
  - รูปแบบการแลกเปลี่ยนหรือรับส่งข้อมูล (ตอนแรกใช้ในรูปแบบ xml แต่โครงสร้างมันซับซ้อน และมีขนาดใหญ่)
  - อยู่ในรูปแบบของข้อความธรรมดา (xml ต้องใช้ tag xml)
  - ในเริ่มต้นใช้รับส่งข้อมูลให้ server ปัจจุบันใช้เป็นรูปแบบในการแลกเปลี่ยนข้อมูลในเว็บ
  - รองรับชนิดข้อมูลพื้นฐานทั้งหมด
    - Number `1 2 3`
    - String `""`
    - Boolean `true false`
    - Null
    - Array `[val1,val2]`
    - Object `{[key]: val}` => ในส่วนของ key จะเรียกว่า field
- BSON: Binary JSON
  - ในฐานข้อมูล mongoDB จะทำการแปลงเอกสาร JSON โดยเข้ารหัสให้อยู่ในรูปแบบไบนารี่ที่เรียกว่า BSON
  - มีขนาดเบา และรับส่งข้อมูลได้เร็วเหมือน JSON
  - MongoBD จะสามารถเข้าถึงข้อมูลใน BSON เพื่อสร้าง Index (การ Query ข้อมูล)
  - BSON ทำงานซ่อนอยู่เบื้องหลัง ส่วนการแสดงผลข้อมูลจะเป็น JSON

---

[ติดตั้ง MongoDB (mac OS)](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-os-x/)

---

การสร้างและลบฐานข้อมูล

- แสดงฐานข้อมูล `show dbs`
- สร้าง หรือเลือกฐานข้อมูล `use ชื่อฐานข้อมูล`
  - กรณียังไม่เคยมีฐานข้อมูล (สร้าง)
    - `db.createCollection("Collection Name")`
  - กรณีมีฐานข้อมูลอยู่แล้ว (เลือก)
- ลบฐานข้อมูล `db.dropDatabase()`
  - ต้องทำการเข้าไปในฐานข้อมูลก่อน

---

การจัดการ Collection
เราจะจัดการได้ก็ต่อเมื่อเรามีฐานข้อมูลขึ้นมาก่อน

- สร้าง
  `user database-name`
  `db.createCollection("collection-name")`
- เปลี่ยนชื่อ
  `db.old-collection-name.renameCollection("new-collection-name")`
- ลบ
  `db.collection-name.drop()`

---

ชนิดของข้อมูลใน MongoDB
| ชนิดข้อมูล | รายละเอียด |
| ----------- | ----------- |
| String | ชุดข้อความ อักขระ ในรูป `""` |
| Integer | เลขจำนวนเต็ม |
| Double | เลขทศนิยม |
| Boolean | ค่า true/false |
| Null | ไม่มีค่า |
| Object | document ในรูป `{}` |
| Array | ชุดข้อมูลในรูป `[] : brackets` |
| ObjectId | เก็บ id ของ document (มีค่าไม่ซ้ำกัน) |
| Date | เก็บวันที่และเวลา |
| Binary Date | เก็บวันที่และเวลาแบบไบนารี่ |
| Max / Min Key | เปรียบเทียบค่ามากกว่า / น้อยกว่า กับข้อมูลอื่นๆ |

---

การเพิ่ม Document (การบันทึกข้อมูล)

- เพิ่ม 1 Document
  `db.collection-name.insertOne(<Document>)`
- เพิ่มหลาย Document
  `db.collection-name.insertMany([<Document>])`

---

การบันทึก Document แบบ Array

```json
{
  "name": "test",
  "age": 12,
  "email": "test@gmail.com",
  "social": ["facebook", "line", "twitter"]
}
```

`db.collection-name.insertOne({ name:'Test', age:12, email:"test@gmail.com", social:["facebook","line","twitter"] })`

การบันทึก Document แบบ Object

```json
{
   name: "test",
   age: 12,
   email: "test@gmail.com",
   social: ["facebook","line","twitter"]
   address: {
      province: "bangkok",
      district: "Khlong San",
      subDistrict: "Khlong San"
}
```

`db.collection-name.insertOne({ name:'Test', age:12, email:"test@gmail.com", social:["facebook","line","twitter"], address: { province: "bangkok", district: "Khlong San", subDistrict: "Khlong San" } })`

---

สอบถามข้อมูล (findOne,find)

- findOne() : ค้นหา และคืนผลลัพธ์จากการค้นหาแค่ 1 Document
  - เจอมาากว่า 1 ค่า คืนค่าแรกที่เจอ
  - ไม่เจอคืนค่ามาเป็น `Null`
- find() : เลือก Document ทั้งหมดมา
  - ถ้าจะเอาข้อมูลทุกรายการไม่ต้องใส่เงื่อนไข
  - ถ้าต้องการกรอกข้อมูล (Filter) ต้องใส่เงื่อนไขด้วย

ex.

```json
{
   name:"test",
   salary:60000,
   department:"Marketing",
   address:"Bangkok",
   general:{
      weight:50,
      height:170,
      gender:"woman"
   }
   social:["facebook","line"]
}
```

- findOne(): return Document[0]
   - ค้นหาแบบไม่มีเงื่อนไข
   `db.collection-name.findOne()`
    - ค้นหาแบบมีเงื่อนไข
     `db.collection-name.findOne({salary: 15000})`
- find(): return all Document
   - ค้นหาแบบไม่มีเงื่อนไข
   `db.collection-name.find()`
   - ค้นหาแบบมีเงื่อนไข
     `db.collection-name.find({salary: 15000})`
- สอบถามข้อมูลแบบ Object
  `db.collection-name.find/findOne({"general.gender":"woman"})`
- สอบถามข้อมูลแบบ Array[element,element]
  `db.collection-name.find/findOne({"social":["facebook"]})`

---

ตัวดำเนินการเปรียบเทียบ

| ชื่อ | คำอธิบาย | การใช้งาน |
|----|---------|----------|
| $eq | เท่ากับ (equal) | `{field:{$eq:value}} or {field:{field:value}}`
| $gt | มากกว่า (greater) | `{field:{$gt:value}}`
| $gte | มากกว่าหรือเท่ากับ (greater or equal) | `{field:{$gte:value}}`
| $lt | น้อยกว่า (less) | `{field:{$lt:value}}`
| $lte | น้อยกว่าหรือเท่ากับ (less orequal) | `{field:{$lte:value}}`
| $ne | ไม่เท่ากับ (not equal) | `{field:{$ne:value}}`

ex. หาพนักงานที่มีเงินเดือนมากกว่า 15000
`db.collection-name.find/findOne({salary:{$gt:15000}})`

---