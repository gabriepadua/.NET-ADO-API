# 📚 ADO.NET + Dapper API Study

This repository contains a study project for **data access and API development with C#**, using **ADO.NET** and **Dapper**.
The main goal is to explore **data access concepts, transactions, stored procedures, and SQL queries** within a .NET application.

---

## 🚀 Technologies Used

- **C#** – Main programming language
- **.NET 8** – Application framework
- **ADO.NET** – Direct database access with SQL Server
- **Dapper** – Micro ORM for query execution and object mapping
- **SQL Server** – Relational database

---

## 🛠️ Features Implemented

The project demonstrates different database operations:

- **Full CRUD** with `Category` (Create, Read, Update, Delete)
- **Stored Procedures execution** (`ExecuteProcedure`, `ExecuteReadProcedure`)
- **Views execution** (`ReadView`)
- **Entity relationships**
  - One-to-One (`OneToOne`)
  - One-to-Many (`OneToMany`)
- **Multiple queries** (`QueryMultiple`)
- **Parameterized queries** (`SelectIn`, `Like`)
- **Transactions** (`Transaction`)

Each method in the code demonstrates a real-world use case for database operations.

---

## 💡 Best Practices Applied

### `using` with `SqlConnection`
Ensures that the database connection is always closed and properly disposed of.

```csharp
using (var connection = new SqlConnection(connectionString))
{
    // Database operations
}
```

### Named parameters with Dapper
Prevents **SQL Injection** and improves readability.

```csharp
connection.QueryFirstOrDefault<Category>(
    "SELECT TOP 1 [Id], [Title] FROM [Category] WHERE [Id]=@id",
    new { id = "af3407aa-11ae-4621-a2ef-2028b85507c4" });
```

### Transaction usage
Ensures data integrity for critical operations. Here it demonstrates `Rollback` for learning purposes.

```csharp
using (var transaction = connection.BeginTransaction())
{
    var rows = connection.Execute(insertSql, category, transaction);
    transaction.Rollback();
}
```

### Separation of concerns
Each method is responsible for a single database operation, improving maintainability and readability.

---

## 🔎 Key Code Highlights

### 🔹 Insert with Dapper

Shows how to simplify `INSERT` queries with parameters.

```csharp
var insertSql = @"INSERT INTO [Category] 
VALUES(@Id, @Title, @Url, @Summary, @Order, @Description, @Featured)";

var rows = connection.Execute(insertSql, new
{
    category.Id,
    category.Title,
    category.Url,
    category.Summary,
    category.Order,
    category.Description,
    category.Featured
});
```

### 🔹 One-to-One Relationship

Mapping related objects with Dapper.

```csharp
var items = connection.Query<CareerItem, Course, CareerItem>(
    sql, (careerItem, course) =>
    {
        careerItem.Course = course;
        return careerItem;
    }, splitOn: "Id"
);
```

### 🔹 Transaction with Rollback

Demonstrates atomic operations and rollback usage.

```csharp
connection.Open();
using (var transaction = connection.BeginTransaction())
{
    var rows = connection.Execute(insertSql, category, transaction);
    transaction.Rollback(); // Undo the operation
}
```

---

## 📂 Project Structure

```
├── Program.cs           # Main class with all database operations
├── Models/
│   ├── Category.cs      # Category entity
│   ├── Career.cs        # Career entity
│   ├── CareerItem.cs    # CareerItem entity
│   └── Course.cs        # Course entity
```

---

## 🎯 Learning Goals

This project aims to:

- Practice **database manipulation with ADO.NET**
- Understand how **Dapper** simplifies queries and mapping
- Apply **best practices** in database access
- Reinforce concepts of **relationships and transactions**
- Explore different approaches to data access in .NET

---

## 📚 Additional Resources

* [ADO.NET Documentation](https://docs.microsoft.com/en-us/dotnet/framework/data/adonet/)
* [SQL Server Best Practices](https://docs.microsoft.com/en-us/sql/sql-server/)

---

## 🤝 Contributing

This is a study project, but feel free to:

- Fork the repository
- Create feature branches for different database scenarios
- Submit pull requests with improvements or additional examples
- Open issues for questions or suggestions

---
