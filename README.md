# 💳 Payment Processing System

A full-stack payment processing platform built with ASP.NET Core 8, React 18, and Azure DevOps CI/CD, demonstrating secure transactions, refunds, reporting, and background processing using Clean Architecture.

---

## 🏗️ Tech Stack

- **Backend:** ASP.NET Core 8 Web API
- **Frontend:** React JS 18
- **Database:** SQL Server (Code-First approach)
- **Architecture:** Clean Architecture (Layered: API, Services, Repositories, Models)
- **CI Pipeline:** Azure DevOps YAML (no Docker or Kubernetes)
- **Background Worker:** .NET Hosted Service for auto-confirmation

---

## 🚀 Features Overview

- ✅ **Card Validation** API  
  Validates card number, CVV, holder name, expiry, and status.

- 💰 **Payment Processing**  
  Simulates card balance deduction and returns a transaction ID and refund code.

- 🔁 **Refund API**  
  Processes refund if requested before 12:00 AM using a valid refund code.

- 🕓 **Auto-confirmation (Background Worker)**  
  Every 10 minutes, unrefunded payments are automatically confirmed after 12:00 AM.

- 📊 **Reporting Dashboard**  
  Displays payment and card balance reports with pagination and filters.

- 🔒 **Security Considerations**  
  - Card data is validated strictly
  - Refund codes are valid only for a limited time
  - Data access is layered via repository pattern

---

## 🧱 Project Architecture – Clean Architecture

This project follows **Clean Architecture** principles to enforce separation of concerns and scalable structure.

### 💡 Why Clean Architecture?

- ✅ Loose coupling between layers (UI, business logic, data)
- ✅ Easy to maintain, test, and extend
- ✅ Promotes single responsibility and better separation of concerns
- ✅ Core domain logic is isolated and reusable

### 🔄 Layers in This Project

/backend/PaymentApp.API/
│
├── Controllers/ → Handle HTTP Requests (API Layer)
├── Services/ → Business Logic Layer (Use Cases)
├── Repositories/ → Data Access Layer (SQL Server)
├── DTOs/ → Data Transfer Objects
├── Models/ → EF Core Entities (Domain Models)
├── Data/ → DbContext, Seed Data, Migrations
├── BackgroundWorker.cs → Hosted Service for background jobs

This architecture ensures that:
- Controllers never directly access the database
- Services handle core logic
- Repositories abstract the database

---

## 🚀 Features

- **Card Validation API**
- **Payment Processing API**
- **Refund API with time-bound logic**
- **Background Worker** for automatic transaction confirmation
- **Paginated Reports** for payments and card balances

---

## 📦 Database Tables

| Table           | Purpose                                     |
|------------------|---------------------------------------------|
| `Cards`          | Stores card details (Card Number, CVV, Expiry, Balance) |
| `Transactions`   | Central payment table: holds, confirms, and refunds |
| `RefundRequests` | Logs refund activity (optional in use)     |

> 🧾 Note: `Transactions` table acts as the **Payment table** in this design.

---

## 🌱 Seeded Card Data

The application uses **seeded dummy card records** for demo/testing purposes.

| Card Number        | Holder Name | CVV  | Expiry Date | Balance |
|--------------------|-------------|------|-------------|---------|
| 1234567812345678   | John Doe    | 123  | +1 year     | 1000    |
| 2345678923456789   | Jane Smith  | 456  | +2 years    | 1500    |
| 3456789034567890   | Alice Brown | 789  | +1 year     | 2000    |
| 4567890145678901   | Bob White   | 234  | +3 years    | 500     |
| 5678901256789012   | Eva Green   | 567  | +1 year     | 3000    |

These records are seeded during application startup in the `AppDbContext`.

---

## 🧠 Background Worker: Auto-confirmation Logic

A `.NET Hosted Service` runs in the background **every 10 minutes**.

### Logic:
- Checks for payments that are **held but not refunded**
- After **12:00 AM**, if no refund is requested → payment is auto-confirmed

This is implemented using:
PeriodicTimer + Dependency Injection in IHostedService

---

## 🧪 Running the Application (Local Setup)

### ✅ Prerequisites

- [.NET 8 SDK](https://dotnet.microsoft.com/en-us/download)
- [Node.js 18+](https://nodejs.org/)
- [SQL Server](https://www.microsoft.com/en-us/sql-server)
- [Visual Studio 2022+] or VS Code

---

### ⚙️ Backend Setup

cd backend/PaymentApp.API/PaymentApp.API
dotnet restore
dotnet ef database update     # Apply code-first migrations
dotnet run
This project uses Code-First Approach with EF Core

Update Database using NuGet Package Manager Console
To apply existing migrations and update the database using Visual Studio:
Open Tools → NuGet Package Manager → Package Manager Console
Run the following command:
Update-Database

Make sure your appsettings.json has a valid SQL Server connection string:
"ConnectionStrings": {
  "DefaultConnection": "Server=.;Database=YourDbName;Trusted_Connection=True;"
}

---

🌐 Frontend Setup

cd frontend/payment-dashboard
npm install
npm run dev
Visit: http://localhost:5173

---

🛠️ Azure DevOps Pipeline
Located in azure-pipelines.yml at the root.

Features:
Builds and publishes ASP.NET Core API

Builds and publishes React frontend

Publishes build artifacts for both (no Docker/Kubernetes required)

---

Dashboard:

- The frontend is a React JS 18 application that:

- Displays Payment Reports with filters & pagination

- Displays Card Balance Reports with grouping & pagination

- Includes actions like Make Payment and Refund Payment

---

🔒 Validation Rules
All payment and refund processes are subject to strict validation:

✅ Card must exist and be active

✅ Expiry date must not be in the past

✅ Cardholder name and CVV must match

✅ Refunds only allowed before 12:00 AM with valid code

---

📁 Project Structure
bash
Copy
Edit
/payment-app-test/
│
├── backend/
│   └── PaymentApp.API/
│       ├── PaymentApp.API/         → ASP.NET Core Web API Project
│       └── PaymentApp.API.sln
│
├── frontend/
│   └── payment-dashboard/          → React JS 18 App
│
└── azure-pipelines.yml             → Azure DevOps CI Pipeline

---

🙋‍♀️ Author & Credits
Project developed by Shazma Afzal as part of a Technical Assessment for a Senior .NET Developer role.

📬 Contact
For any queries, please contact:
📧 shazmaafzal@gmail.com
