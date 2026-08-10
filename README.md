# Smart Education System SaaS

A production-oriented **Multi-Tenant Smart Education Management SaaS** built with **ASP.NET Core**, **Entity Framework Core**, and **SQL Server**.

The system is designed to provide educational organizations with a centralized platform for managing students, teachers, classes, attendance, courses, examinations, assignments, subscriptions, and other educational operations.

---

## 🏗️ Architecture

The project follows a hybrid architecture combining:

* Clean Architecture principles
* Vertical Slice Architecture
* CQRS-ready design
* Domain-Driven Design principles
* Multi-Tenancy
* Entity Framework Core
* SQL Server

### High-Level Structure

```text
SmartEducation
│
├── API
│   ├── Features
│   │   ├── Students
│   │   ├── Teachers
│   │   ├── Attendance
│   │   ├── Classes
│   │   ├── Courses
│   │   ├── Exams
│   │   ├── Assignments
│   │   ├── Enrollments
│   │   ├── Notifications
│   │   ├── Payments
│   │   ├── Subscriptions
│   │   ├── Tenants
│   │   └── Authentication
│   │
│   └── Middleware
│
├── Application
│   ├── Abstractions
│   ├── Behaviors
│   └── Common
│
├── Domain
│   ├── Entities
│   ├── ValueObjects
│   ├── Enums
│   └── DomainEvents
│
└── Infrastructure
    ├── Persistence
    ├── Identity
    ├── Payments
    └── ExternalServices
```

---

# 🎯 Project Goals

The main goals of the platform are:

* Provide a scalable education management system.
* Support multiple independent tenants.
* Isolate tenant data securely.
* Manage students and teachers.
* Manage academic years and classes.
* Manage student enrollments.
* Track attendance.
* Manage courses and subjects.
* Manage exams and assessments.
* Manage assignments and submissions.
* Support SaaS subscriptions and payments.
* Provide a foundation for notifications and external integrations.
* Maintain high database performance and scalability.

---

# 🏢 Multi-Tenancy

Multi-Tenancy is a first-class architectural concern.

Each educational organization operates as an independent tenant.

For example:

```text
Tenant A
│
├── Students
├── Teachers
├── Classes
├── Attendance
├── Exams
└── Assignments

Tenant B
│
├── Students
├── Teachers
├── Classes
├── Attendance
├── Exams
└── Assignments
```

Tenant-owned data must never be accessible across tenant boundaries.

The architecture is designed around a `CurrentTenant` abstraction so tenant resolution can later be connected to mechanisms such as:

* JWT Claims
* Subdomains
* Headers
* Routes

---

# 🧩 Vertical Slice Architecture

Application functionality is organized by business feature rather than technical type.

Instead of:

```text
Controllers/
Services/
Repositories/
DTOs/
Validators/
```

features are organized as:

```text
Features
│
├── Students
│   ├── CreateStudent
│   ├── UpdateStudent
│   ├── GetStudent
│   └── DeleteStudent
│
├── Attendance
│   ├── MarkAttendance
│   └── GetStudentAttendance
│
└── Exams
    ├── CreateExam
    └── GetExamResult
```

This makes individual business capabilities easier to understand, maintain, test, and evolve.

---

# 🧠 Domain Model

The domain is designed around educational business concepts such as:

* Tenant
* Student
* Teacher
* Parent / Guardian
* Academic Year
* Class
* Course
* Enrollment
* Attendance
* Exam
* Assignment
* Notification
* Subscription
* Payment

The final domain model may evolve as business requirements become clearer.

---

# 🗄️ Database

The system uses:

```text
SQL Server
      +
Entity Framework Core
```

Database design focuses on:

* Referential integrity
* Proper foreign keys
* Tenant isolation
* Composite indexes
* Unique constraints
* Query performance
* High-volume tables
* Appropriate delete behaviors
* Explicit decimal precision
* Appropriate string lengths

Every entity has its own EF Core configuration.

Example:

```text
Infrastructure
└── Persistence
    └── Configurations
        ├── TenantConfiguration.cs
        ├── StudentConfiguration.cs
        ├── TeacherConfiguration.cs
        ├── AttendanceConfiguration.cs
        └── ...
```

Configurations use:

```csharp
IEntityTypeConfiguration<TEntity>
```

instead of placing all database configuration inside `OnModelCreating`.

---

# ⚡ Performance

Database performance is considered from the beginning.

Special attention is given to high-volume entities such as:

* Attendance
* Enrollment
* Exam Attempts
* Assignment Submissions
* Notifications
* Payments

Indexes are created based on expected query patterns rather than indiscriminately indexing every column.

Tenant-aware composite indexes are considered where appropriate.

---

# 🔐 Security

The architecture is designed to support:

* ASP.NET Core Identity
* JWT Authentication
* Role-Based Authorization
* Tenant Isolation
* Secure password management
* Refresh Tokens
* Permission-based authorization

Security-related implementation will be introduced incrementally as the project evolves.

---

# 💳 SaaS Model

The platform is designed as a SaaS product.

The conceptual relationship is:

```text
Tenant
   │
   ▼
Subscription
   │
   ▼
Plan
   │
   ▼
Payment
```

The initial architecture provides boundaries for future payment provider integrations without tightly coupling the Domain to a specific provider.

---

# 📁 Documentation

Important architectural decisions are documented in:

```text
ARCHITECTURE.md
DATABASE-DESIGN.md
```

These documents explain:

* Architectural decisions
* Domain boundaries
* Tenant ownership
* Entity relationships
* Aggregate boundaries
* Database indexes
* Constraints
* Delete behaviors
* Performance considerations

---

# 🚧 Current Development Phase

The project is currently being developed incrementally.

### Phase 1 — Architecture & Database Foundation

* [x] Solution architecture
* [x] Domain model foundation
* [x] EF Core DbContext
* [x] Entity configurations
* [x] Database relationships
* [x] Initial indexing strategy
* [x] Multi-Tenancy abstractions

### Phase 2 — Identity & Authentication

* [ ] ASP.NET Core Identity
* [ ] JWT Authentication
* [ ] Refresh Tokens
* [ ] Roles
* [ ] Permissions
* [ ] Authentication Features

### Phase 3 — Tenant Management

* [ ] Tenant registration
* [ ] Tenant configuration
* [ ] Tenant resolution
* [ ] Tenant administration

### Phase 4 — Core Education Features

* [ ] Students
* [ ] Teachers
* [ ] Parents / Guardians
* [ ] Academic Years
* [ ] Classes
* [ ] Courses
* [ ] Enrollments

### Phase 5 — Academic Operations

* [ ] Attendance
* [ ] Exams
* [ ] Assignments
* [ ] Grades
* [ ] Results

### Phase 6 — SaaS

* [ ] Subscription plans
* [ ] Tenant subscriptions
* [ ] Payments
* [ ] Payment webhooks
* [ ] Subscription lifecycle

### Phase 7 — Platform Services

* [ ] Notifications
* [ ] Email
* [ ] SMS
* [ ] Push Notifications
* [ ] Background Jobs
* [ ] External Integrations

---

# 🛠️ Technologies

| Technology                  | Purpose                   |
| --------------------------- | ------------------------- |
| C#                          | Main programming language |
| ASP.NET Core                | Backend/API               |
| Entity Framework Core       | ORM                       |
| SQL Server                  | Database                  |
| ASP.NET Core Identity       | Authentication & Identity |
| JWT                         | Authentication            |
| FluentValidation            | Request validation        |
| Clean Architecture          | Architectural boundaries  |
| Vertical Slice Architecture | Feature organization      |
| CQRS                        | Command/Query separation  |

---

# 📌 Architecture Principles

The project prioritizes:

1. **Maintainability**
2. **Scalability**
3. **Security**
4. **Tenant Isolation**
5. **Database Performance**
6. **Separation of Concerns**
7. **Explicit Domain Modeling**
8. **Simple abstractions**
9. **Testability**
10. **Long-term extensibility**

The architecture intentionally avoids unnecessary abstractions and patterns that do not provide real value.

---

# 📄 License

This project is currently under development.
