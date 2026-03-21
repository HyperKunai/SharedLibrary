⚙️ SharedLibrary — Lightweight HTTP Framework

A minimal, dependency-free HTTP framework built in C# using HttpListener.
This library provides the foundational components required to build a web API from scratch, including routing, middleware, configuration, and request/response handling.

🚀 Features

🌐 Custom HTTP server (HttpListener)

🔗 Middleware pipeline (chainable & async)

🧭 Router with:

Global middleware

Per-route middleware

Parametrized routing (/users/:id)

Nested routers (sub-routing)

⚙️ Configuration system (file + environment variables)

📦 JSON utilities for consistent API responses

🪵 Structured logging middleware

⚠️ Centralized error handling

📄 Static file serving

🌍 CORS support

🔍 URL, query string, and body parsing

🏗️ Architecture Overview

This framework mimics how modern backend frameworks (like Express.js or ASP.NET) work internally.

Request flow:
HTTP Request
   ↓
HttpServer
   ↓
HttpRouter
   ↓
Middleware Pipeline
   ↓
Controller
   ↓
Response
📁 Project Structure
SharedLibrary/
│
├── Config/        # Configuration system
├── Http/          # Core HTTP framework
│   ├── HttpRouter.cs
│   ├── HttpServer.cs
│   ├── HttpUtils.cs
│   ├── JsonUtils.cs
│   ├── Result.cs
│   └── PagedResult.cs
⚙️ Configuration System

The framework includes a lightweight configuration loader.

Supported files:
appsettings.cfg
appsettings.{environment}.cfg
Example:
HOST=http://localhost
PORT=5000
Usage:
string host = Configuration.Get<string>("HOST", "http://127.0.0.1");
int port = Configuration.Get<int>("PORT", 5000);
🔗 Middleware System

Middleware follows this signature:

public delegate Task HttpMiddleware(
    HttpListenerRequest req,
    HttpListenerResponse res,
    Hashtable props,
    Func<Task> next
);

Each middleware can:

Read/modify request & response

Share data via props

Call next() to continue the pipeline

🧭 Routing
Basic routing
router.MapGet("/", controller.Home);
router.MapPost("/users", controller.CreateUser);
Parametrized routes
router.MapGet("/users/:id", controller.GetUser);
Sub-routing
router.UseRouter("/api/v1", apiRouter);
⚙️ Built-in Middleware
🪵 Structured Logging

Logs request/response as JSON.

⚠️ Centralized Error Handling

Catches exceptions and returns safe responses.

🌍 CORS Headers

Handles cross-origin requests.

📄 Static Files

Serves files directly from disk.

🔍 Parsing Middleware

URL parsing

Query string parsing

Body parsing:

JSON

Form data

Text

XML

Binary (blob)

📡 Response Helpers
Send OK
await HttpUtils.SendOkResponse(req, res, props, "Hello");
Send Error
await HttpUtils.SendResponse(req, res, props, 500, "Error");
Send Result (type-safe)
await JsonUtils.SendResultResponse(req, res, props, result);
📄 JSON Utilities

Provides standardized API responses and pagination support.

Paginated response format:
{
  "data": [...],
  "meta": {
    "totalCount": 100,
    "page": 1,
    "size": 10,
    "totalPages": 10
  },
  "links": {
    "self": "...",
    "next": "...",
    "prev": "..."
  }
}
📦 Result Pattern

Encapsulates success and error responses:

new Result<T>(payload);         // Success
new Result<T>(exception, 400);  // Error
▶️ Running a Server

Example:

public class App : HttpServer
{
    public override void Init()
    {
        router.Use(HttpUtils.StructuredLogging);
        router.Use(HttpUtils.CentralizedErrorHandling);
        router.UseParametrizedRouteMatching();

        router.MapGet("/", async (req, res, props, next) =>
        {
            await HttpUtils.SendOkResponse(req, res, props, "Hello World");
        });
    }
}
await new App().Start();
🧠 Design Goals

Understand how web frameworks work internally

Avoid external dependencies

Promote clean architecture (separation of concerns)

Provide a flexible and extensible foundation

📌 Notes

This is a learning-oriented framework, not production-ready

Lacks advanced features like:

Dependency injection container

Thread safety guarantees

Advanced routing optimizations

Easily extendable for real-world use

👨‍💻 Author

Edward Navarreto