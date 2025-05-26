# PostgreSQL Assignment

### 1. **What PostgreSQL?**

**PostgreSQL** একটি ওপেন-সোর্স, অবজেক্ট-রিলেশনাল ডেটাবেস ম্যানেজমেন্ট সিস্টেম (ORDBMS)। এটি ডেটা সংরক্ষণ, পরিচালনা এবং অনুসন্ধানের জন্য ব্যবহৃত হয়। এটি শক্তিশালী, নিরাপদ, এবং ACID-compliant হওয়ায় বড় ও জটিল অ্যাপ্লিকেশনেও ব্যবহৃত হয়।

---

### 2. **What is the purpose of a database schema in PostgreSQL?**

**স্কিমা** হলো একটি ডেটাবেসের ভিতরে সংগঠিত একটি যুক্তিসঙ্গত ইউনিট যা টেবিল, ভিউ, ফাংশন ইত্যাদি গ্রুপ করে। এটি ডেটার কাঠামোকে সংগঠিত করতে এবং **নাম কনফ্লিক্ট** এড়াতে সাহায্য করে। একাধিক স্কিমা একই ডেটাবেসে থাকতে পারে।

---

### 3. **Explain the Primary Key and Foreign Key concepts in PostgreSQL.**

- **Primary Key**: একটি টেবিলের প্রতিটি রেকর্ডকে **অন্যদের থেকে আলাদা (Unique)** এবং **Null নয়** এমন একটি কলাম বা কলামগুলোর সমষ্টি।
- **Foreign Key**: অন্য একটি টেবিলের Primary Key কে **রেফারেন্স** করে এমন একটি কলাম। এটি দুই টেবিলের মধ্যে **সম্পর্ক (relationship)** তৈরি করে এবং ডেটার অখণ্ডতা বজায় রাখে।

---

### 4. **What is the difference between the `VARCHAR` and `CHAR` data types?**

- **CHAR(n)**: একটি ফিক্সড দৈর্ঘ্যের স্ট্রিং; যদি ডেটা ছোট হয়, তখনও অতিরিক্ত জায়গা নেয় (padding দেয়)।
- **VARCHAR(n)**: একটি পরিবর্তনশীল দৈর্ঘ্যের স্ট্রিং; শুধুমাত্র যতটুকু জায়গা দরকার, ততটুকুই নেয়।

সাধারণত `VARCHAR` বেশি ব্যবহার হয় কারণ এটি কম জায়গা নেয় এবং বেশি নমনীয়।

---

### 5. **Explain the purpose of the `WHERE` clause in a `SELECT` statement.**

`WHERE` ক্লজ ব্যবহার করে নির্দিষ্ট শর্ত অনুযায়ী ডেটা **ফিল্টার** করা হয়।

#### উদাহরণ:

```sql
SELECT * FROM species WHERE conservation_status = 'Endangered';
