# SQL__DATACAMP

### مقدمة إلى SQL

- **SELECT**: نستخدمها لتحديد العمود الذي نريد إظهاره.
- **FROM**: نستخدمها لتحديد الجدول الذي نريد عرض البيانات منه.
- **AS**: تستخدم لإنشاء اسم مستعار للجدول أو العمود.
- **DISTINCT**: تستخدم للحصول على القيم الفريدة في العمود دون تكرار.

#### ملاحظات إضافية:
- **SELECT** و **FROM** يتم استخدامهم فقط لعرض البيانات، ولكن لا يتم تخزين النتائج في مكان محدد. إذا كنت ترغب في تخزين النتائج في جدول جديد، نستخدم:
  ```sql
  CREATE VIEW name_table AS
  SELECT any_column
  FROM table;
  ```

### مفاهيم SQL المتقدمة

- **COUNT()**: تستخدم لحساب عدد الصفوف أو السجلات التي تحتوي على قيم.
  ```sql
  SELECT COUNT(fieldName) AS aliasName
  FROM tableName;
  ```
- **COUNT WITH DISTINCT**: لحساب العدد بدون تكرار.
  ```sql
  COUNT(DISTINCT fieldName);
  ```

- **LIMIT**: لتحديد عدد الصفوف التي تظهر من العمود المحدد.
  ```sql
  SELECT fieldName
  FROM tableName
  LIMIT 10;
  ```

- **WHERE condition**: تستخدم لعرض البيانات بشرط محدد، أي تقوم بتصفية البيانات بناءً على الشرط.

#### ملاحظة هامة:
ترتيب تنفيذ SQL ليس تسلسليًا:
```plaintext
FROM -> WHERE -> SELECT -> LIMIT
```

- **AND, OR, BETWEEN**: تستخدم لإضافة أكثر من شرط.

### الفلترة

- **LIKE**: نستخدمها مع النصوص لتصفية النصوص التي تحتوي على حروف معينة.
- **NOT LIKE**: لتصفية النصوص التي لا تحتوي على حرف معين.
- **IN**: لتصفية البيانات بناءً على قيم متعددة.

### NULL
تعني القيم الفارغة. نستخدمها أحيانًا في الفلترة بالشكل التالي:
```sql
WHERE fieldName IS NULL;
-- أو
WHERE fieldName IS NOT NULL;
```

### دوال التجميع (Aggregate Functions)

- **AVG(), SUM()**: للأعداد فقط.
- **MIN(), MAX(), COUNT()**: للأعداد أو النصوص.

- **ROUND()**: تستخدم لتحديد عدد الأرقام العشرية.
  ```sql
  ROUND(123.56655, 2);  -- النتيجة: 123.56
  ```

### ترتيب البيانات (Sorting Data)

- **ORDER BY fieldName ASC**: ترتيب تصاعدي.
- **ORDER BY fieldName DESC**: ترتيب تنازلي.

### تجميع البيانات (Grouping Data)

- **GROUP BY**: نستخدمها لتجميع البيانات بناءً على قيمة معينة.
  ```sql
  SELECT certificat, COUNT(name) AS numOfName
  FROM table
  GROUP BY certificat;
  ```

### تصفية البيانات المجمعة (HAVING)

- تستخدم لتصفية البيانات المجمعة. على سبيل المثال، إذا أردت تصفية الشهادات التي حصل عليها أكثر من شخص:
  ```sql
  SELECT certificat, COUNT(name) AS numOfName
  FROM table
  GROUP BY certificat
  HAVING COUNT(name) > 1;
  ```
