
# 📘 ASP.NET Core – EF Core DbContext, Connection String & Migrations Guide

هذا الملف يشرح خطوة بخطوة كيف تنشئ قاعدة بيانات باستخدام Entity Framework Core في مشروع ASP.NET Core سواء كنت على **Windows** أو **macOS/Linux**.

---

## 🧱 1. إنشاء `ApplicationDbContext` (DbContext Model)

### 📁 المسار المقترح:
```
/YourProject/
│
├── Data/
│   └── ApplicationDbContext.cs
```

### 📄 الكود:

```csharp
using Microsoft.EntityFrameworkCore;
using YourNamespace.Models;

namespace YourNamespace.Data
{
    public class ApplicationDbContext : DbContext
    {
        public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
            : base(options) {}

        public DbSet<Student> Students { get; set; }
        public DbSet<Course> Courses { get; set; }
        // أضف باقي الجداول هنا
    }
}
```

---

## 🔗 2. إضافة Connection String

### 📄 `appsettings.json`:

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=.;Database=MyDb;Trusted_Connection=True;MultipleActiveResultSets=true"
  }
}
```

- `.` = السيرفر المحلي (localhost)
- `Trusted_Connection=True` = Windows Authentication
- للاتصال بواسطة اسم مستخدم وكلمة سر:

```json
"Server=.;Database=MyDb;User Id=sa;Password=yourPassword;"
```

---

### ⚙️ في `Program.cs`:

```csharp
builder.Services.AddDbContext<ApplicationDbContext>(options =>
    options.UseSqlServer(builder.Configuration.GetConnectionString("DefaultConnection")));
```

---

## ⚡ 3. أوامر Migration و Update Database

---

### 🖥️ على **Windows (Visual Studio - PMC)**:

افتح **Package Manager Console** من:

```
Tools > NuGet Package Manager > Package Manager Console
```

ثم نفذ الأوامر:

```powershell
Add-Migration InitialCreate
Update-Database
```

---

### 💻 على **macOS / Linux / VS Code / Terminal (CLI)**:

أولاً، ثبت أداة EF CLI (مرة واحدة فقط):

```bash
dotnet tool install --global dotnet-ef
```

ثم نفذ الأوامر:

```bash
dotnet ef migrations add InitialCreate
dotnet ef database update
```

---

## 🔍 مقارنة الأوامر

| العملية                 | Windows (PMC)         | macOS / Linux / CLI     |
|-------------------------|-----------------------|--------------------------|
| إنشاء Migration         | Add-Migration Name    | dotnet ef migrations add Name |
| تحديث قاعدة البيانات    | Update-Database       | dotnet ef database update     |
| يتطلب Visual Studio     | ✅ نعم                | ❌ لا                        |
| يعمل على كل الأنظمة     | ❌ لا                 | ✅ نعم                      |

---

## 🧠 ملاحظات مهمة

- **لا تضع connection string داخل `ApplicationDbContext` مباشرة**.
- استخدم **Dependency Injection** دائمًا.
- احتفظ بكل الإعدادات في `appsettings.json`.
- يمكن تخصيص Connection Strings حسب بيئة العمل (Development/Production).

---

## 📦 الحزم المطلوبة:

تأكد من تثبيت حزمة EF Core SQL Server:

```bash
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

---

## 📚 مثال على هيكل المشروع:

```
/YourProject/
├── Controllers/
├── Models/
├── Data/
│   └── ApplicationDbContext.cs
├── appsettings.json
├── Program.cs
```

---

## ✅ جاهز لتجربة عملية؟

1. أنشئ `DbContext` كما في الأعلى  
2. أضف Connection String  
3. نفّذ أوامر Migration المناسبة حسب نظامك  
4. استمتع بقاعدة بيانات تعمل بشكل سليم مع EF Core 🚀
