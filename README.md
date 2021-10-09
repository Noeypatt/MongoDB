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
  - ถ้าต้องการกรองข้อมูล (Filter) ต้องใส่เงื่อนไขด้วย

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

| ชื่อ | คำอธิบาย                              | การใช้งาน                                      |
| ---- | ------------------------------------- | ---------------------------------------------- |
| $eq  | เท่ากับ (equal)                       | `{field:{$eq:value}} or {field:{field:value}}` |
| $gt  | มากกว่า (greater)                     | `{field:{$gt:value}}`                          |
| $gte | มากกว่าหรือเท่ากับ (greater or equal) | `{field:{$gte:value}}`                         |
| $lt  | น้อยกว่า (less)                       | `{field:{$lt:value}}`                          |
| $lte | น้อยกว่าหรือเท่ากับ (less orequal)    | `{field:{$lte:value}}`                         |
| $ne  | ไม่เท่ากับ (not equal)                | `{field:{$ne:value}}`                          |

ex. หาพนักงานที่มีเงินเดือนมากกว่า 15000
`db.collection-name.find/findOne({salary:{$gt:15000}})`

---

สอบถามข้อมูลด้วย In และ Not In
คือการกำหนดเงื่อนไขในรูปแบบ Array
in: อยู่ใน
not in: ไม่ได้อยู่ใน

| ชื่อ | คำอธิบาย         | การใช้งาน                           |
| ---- | ---------------- | ----------------------------------- |
| $in  | มีค่าใน Array    | `{field:{$in:[value,value2,...]}}`  |
| $nin | ไม่มีค่าใน Array | `{field:{$nin:[value,value2,...]}}` |

ex. หาพนักงานที่ไม่ได้อาศัยอยู่ในกรุงเทพ
`db.employees.find({address:{$nin:["Bangkok"]}})`
ex. หาพนักงานที่ส่วนสูง 170, 180 และ 190
`db.employees.find({"general.height":{$in:[170,180,190]}})`

---

ตัวดำเนินการทางตรรกศาสตร์
| ชื่อ | คำอธิบาย |
|----|---------|
| $and | ตรงทั้งสองเงื่อนไข |
| $or | ตรงทกับเงื่อนไขใดเงื่อนไขหนึ่ง |
| $nor | ไม่ตรงกับเงื่อนไขใดเลย |
| $not | ตรงกันข้ามกับเงื่อนไขที่กำหมด |

```json
db.collection-name.find({$operator:[{condition1},{condition2}]})
```

ex.หาพนักงานที่เงินเดือนมากกว่า 15000 และน้อยกว่า 45000

```json
db.collection-name.find({$and:[{salary:{$gt:15000}},{salary:{$lt:45000}}]})
```

---

การแก้ไข/อัปเดต Document

- อัปเดตรายการเดียว

```json
db.collection-name.updateOne({1,2,3})
1. Document ที่ต้องการแก้ไข
2. ระบุข้อมูลที่ต้องการแก้ไข
3. ตัวเลือกเพิ่มเติม

ex.
db.collection-name.updateOne({field:"value"},{$set:{ชื่อฟิลด์ที่ต้องการแก้:"ค่าที่ต้องการแก้"}})
```

- อัปเดตหลายรายการ

```json
db.collection-name.updateMany({1,2,3})
1. Document ที่ต้องการแก้ไข
2. ระบุข้อมูลที่ต้องการแก้ไข
3. ตัวเลือกเพิ่มเติม

ex.
db.collection-name.updateMany({field:"value"},{$set:{ชื่อฟิลด์ที่ต้องการแก้:"ค่าที่ต้องการแก้"}})
```

---

การลบ Document

- ลบรายการเดียว

```json
db.collection-name.deleteOne(filter)

ex.
db.collection-name.deleteOne({field:"value"})
```

- ลบหลายรายการ

```json
db.collection-name.deleteMany(filter)

ex.
db.collection-name.deleteMany({field:"value"})
```

---

Aggregation และ Pipeline

- Aggregation: การรวมข้อมูล
- Pipeline: กระบวนการนำข้อมูลมารวมเข้าด้วยกัน และทำการวิเคราะห์/ประมวลผล(stage) หาผลลัพธ์ที่ต้องการออกมา
  - collection => stage => output
  - `aggregate(stage[stage]) `
  - โครงการคำสั่ง
    ```json
    db.collection-name.aggregate([stage])
    ```

การทำงานของ Stage

| ชื่อ Stage | คำอธิบาย                                        |
| ---------- | ----------------------------------------------- |
| $match     | กรองเอาเฉพาะ Document ที่ตรงตามเงื่อนไขที่กำหนด |
| $group     | จัดกลุ่ม Document และคำนวณค่าเก็บใน Output      |
| $project   | แสดงข้อมูล Document เอาเฉาะ field ที่กำหนด      |
| $sort      | จัดเรียง Document                               |
| $skip      | ข้าม Document ตามจำนวนที่ระบุ                   |
| $limit     | จำกัดการแสดงจำนวน Document                      |
| $unwind    | แยกสมาชิด field Array ออกเป็น Document          |
| $count     | นับจำนวน Document                               |
| $lookup    | ดูข้อมูล Document ที่มี Collection ต่างกัน      |

เปรียบเทียบ SQL กับ Stage

| SQL      | Stage    |
| -------- | -------- |
| WHERE    | $match   |
| GROUP BY | $group   |
| SELECT   | $project |
| ORDER BY | $sort    |
| LIMIT    | $limit   |
| JOIN     | $lookup  |

การใช้งาน Stage

1. Projection (Select) `$project`
: สอบถามข้อมูล และให้แสดงผลเฉพาะฟิลด์ที่ต้องการออกไป
- ต้องการให้แสดงผลให้กำหนดเป็นหมายเลข 1
- ซ่อนการแสดงผลให้กำหนดเป็นหมายเลข 0

```json
{$project:{field-name:number}}

ex.
db.collection-name.aggregate({$project:{id:1,name:1,}})
```

2. Match (WHERE) `$match`
: เป็นรูปแบบการสอบถามข้อมูล โดยเลือกเอาเฉพาะ Document ที่ผ่านเงื่อนไขที่ระบุแล้วส่งออกไป

```json
{$match:{field-name:condition}}

ex. แสดงชื่อ และแผนก ของพนักงานที่มีเงินเดือน 15000
db.collection-name.find({salary:{$eq:15000}})
db.collection-name.aggregate([{$match:{salary:{$eq:15000}}},{$project:{name:1,department:1}}])
```

3. Count (WHERE) `$count`
: เป็นการนับจำนวน Document ใน Collection โดยส่งออกเป็นจำนวนที่นับได้

```json
{$count:"ชื่อฟิลด์ที่ต้องการให้เก็บค่าจำนวนที่นับได้ เช่น total"}

ex. จำนวนของพนักงานที่มีเงินเดือน 15000
db.collection-name.aggregate([{$match:{salary:{$eq:15000}}},{$count:"จำนวนพนักงาน"}])
```

4. Sort (OREDER BY) `$sort`
: เป็นการเรียงลำดับข้อมูลใหม่
- หมายเลข 1 เรียงจากน้อยไปมาก พยัญชนะ => สระ
- หมายเลข -1 เรียงจากมากไปน้อย สระ => พยัญชนะ


```json
{$sort:{field:number,field2:number}}

ex. เรียงลำดับเงินเดือนของพนักงานจากน้อยไปมาก
db.collection-name.aggregate([{$sort:{salary:1}}])
```

5. Limit (LIMIT) `$limit`
: เป็นการจำกัดข้อมูล ตามจำนวนที่กำหนด
เนื่องจากหากฐานข้อมูลของเรามีจำนวนข้อมูลมาก ตอนที่ดึก Document มาทั้งหมดอาจไม่สะดวกในการอ่านรายละเอียกข้อมูล และอาจใช้เวลานานในการนำมาแสดงผล จึงต้องมีการจำกัดการแสดงจำนวน Document


```json
{$limit:number}

ex. แสดงข้อมูลพนักงาน 2 คนแรก
db.collection-name.aggregate([{$limit:2}])
```

6. Skip `$skip`
: เป็นการข้ามไปยัง Document ที่เราต้องการ

```json
{$skip:number}

ex. แสดงข้อมูลพนักงาน 5 คนสุดท้าย จากพนักงานทั้งหมด 15 คน
db.collection-name.aggregate([{$skip:10}])
```

7. Group (GROUP BY) `$group`
: เป็นการจัดกลุ่มข้อมูลของ Document ไว้เป็นกลุ่มเดียวกัน
โดยมี _id เก็บรายการข้อมูลของแต่ละกลุ่ม และยังสามารถเพิ่มฟิลด์ในการคำนวณหาผลลัพธ์ภายในกลุ่มข้อมูลที่สร้างขึ้นได้ (ใช้งานร่วมกับตัวดำเนินการทางสถิติ)

ตัวดำเนินการทางสถิติ
| ชื่อ | ความหมาย |
|----|----------|
| $sum | หาผลรวม |
| $avg | หาค่าเฉลี่ย |
| $min | หาค่าต่ำสุด |
| $max | หาค่าสูงสุด |
| $first | หาค่า Document ลำดับแรก |
| $last | หาค่า Document ลำดับสุดท้าย  |
| $push | แทรกค่าไปยังผลลัพธ์ในกลุ่ม |
| $addToSet | แทรกค่าไปยังผลลัพธ์ในกลุ่มโดยไม่ซ้ำกัน |


```json
{$group:{_id:<1>,ชื่อฟิลด์ที่ต้องการให้เก็บค่าผลลัพธ์ที่ได้:"ชื่อฟิลด์ที่สนใจ"}}
1.  null => ไม่จัด group เอาทุกค่าที่มี
    $ชื่อฟิลด์ => group ฟิลด์นั้น (ต้องเป็นฟิลด์ที่มีค่าซ้ำกัน เช่น จังหวัดท ตำแหน่ง เงินเดือน)

ex. จำนวนของพนักงานในแต่ละฝ่าย
db.collection-name.aggregate([{$group:{_id:"$department",count:{$count:{}}}}])

ex. หาผลรวมของเงินเดือนทั้งหมด
db.collection-name.aggregate([{$group:{_id:null,total:{$sum:"$salary"}}}])

ex. หาผลรวมของเงินเดือนทั้งหมด โดยแบ่งตามแผนก
db.collection-name.aggregate([{$group:{_id:"$department",total:{$sum:"$salary"}}}])

ex. หาผลรวมของเงินเดือนทั้งหมด ขอลฝ่ายขายอย่างเดียว
db.collection-name.aggregate([{$match:{department:{$eq:"ฝ่ายขาย"}}},{$group:{_id:"$department",total:{$sum:"$salary"}}}])

ex. หาเงินเดือนค่าเฉลี่ยพนักงานแต่ละแผนก
db.collection-name.aggregate([{$group:{_id:"$department",total_avg:{$avg:"$salary"}}}])
```

7.1 Push & addToset `$pust & $addToSet`
: เป็นการเพิ่ม/แทรกลุ่มข้อมูล
- pust => object
- addToset => array

```json
$pust
ex. หาผลรวมเงินเดือนของพนักงานโดยจำแนกเป็นแต่ละแผนก และให้ระบุรายละเอียดของพนักงานพร้อมเงินเดือนแต่ละคน
db.collection-name.aggregate([{$group:{_id:"$department","ยอดรวม":{$sum:"$salary"},"รายละเอียด":{$push:{name:"$name",salary:"$salary"}}}}])

$addToSet
ex. หาผลรวมเงินเดือนของพนักงานโดยจำแนกเป็นแต่ละแผนก และให้ระบุกลุ่มชื่อพนักงาน (มีมากว่า  1 คน)
db.employees.aggregate([{$group:{_id:"$department","ยอดรวม":{$sum:"$salary"},"ข้อมูลพนักงาน":{$addToSet:"$name"}}}])

```


8. Lookup (JOIN) `$lookup`
: เป็นการเชื่อม Collection
โดยกำหนดให้ Collection หลัก เชื่อมกับ Collection ด้านนอก ซึ่งอยู่ในฐานข้อมูลเดียวกัน

Collection employees
| name | department_id |
|------|---------------|
| สมชาย | DP001 |
| ก้อง | DP002 |
| โจโจ้ | DP003 |
| แจ๋ม | DP001 |

Collection departments
| id | name |
|------|---------------|
| DP001 | โปรแกรมเมอร์ |
| DP002 | ฝ่ายการตลาด |
| DP003 | ฝ่ายขาย |

**department_id: localField**

**id: foreignField**


```json
{$lookup:
  {
    from:"ชื่อ Collection ด้านนอกที่เชื่อมโยง",
    localField:"Field จาก Collection หลัก",
    foreignField:"Field จาก Collection ด้านนอกที่เชื่อมโยง",
    as:"ชื่อ Field ที่เก็บผลลัพธ์"
  }
}

ex.
db.employees.aggregate([{$lookup:{from:"depatments",localField:"department_id",foreignField:"id",as:"ข้อมูลแผนก"}}])

```

---

คำสั่ง docker พื้นฐาน

```json
docker ps (check container is running)
docker-compose up -d (pass => is connect mongo compass)
docker exec -it container_name bash (inside mongo)

docker stop container_name
docker start container_name

docker-compose down
```

คำสั่ง mongo พื้นฐาน (ใช้คำสั่งได้เหมือน mongo shell)

```json
mongo
mongo -host localhost -port 27017
cls
show dbs
exit
```
