# Product Management App

This repository contains an ASP.NET Core MVC application for browsing product information and a separate ASP.NET Core Web API that provides the product data. The solution also includes shared caching and product-expiration business logic.

### Solution projects

| Project | Purpose |
| --- | --- |
| `ProductManagementApp` | MVC web interface for the product list, details, and edit pages. |
| `ProductAPI` | API for retrieving, updating, and deleting products. Its initial sample catalog is in `ProductAPI/TestData/productstestdata.json`. |
| `Product.Business` | Product-expiration information and related telemetry. |
| `CachingService` | Shared in-memory and Redis cache implementations. |

The projects target **.NET 8**.

## Requirements

- .NET 8 SDK
- Optional: a Redis instance when running outside the `Development` environment

## Build

From the repository root, restore dependencies and build the solution:

```shell
dotnet restore ProductManagementApp.sln
dotnet build ProductManagementApp.sln
```

## Run locally

Run the API and web application in separate terminals. The API launch profile listens on `https://localhost:5003` and `http://localhost:5002`; the web application listens on `http://localhost:5000`.

**Terminal 1 - API**

```shell
dotnet run --project ProductAPI --launch-profile ProductAPI
```

The API's Swagger UI is available at [https://localhost:5003/swagger](https://localhost:5003/swagger).

**Terminal 2 - web application**

The web app's default `APIURI` points to a hosted API. To use the API running locally, override that setting before starting the web app.

PowerShell:

```powershell
$env:APIURI = "https://localhost:5003/products"
dotnet run --project ProductManagementApp --launch-profile ProductManagementApp
```

Bash:

```bash
APIURI="https://localhost:5003/products" dotnet run --project ProductManagementApp --launch-profile ProductManagementApp
```

Open [http://localhost:5000](http://localhost:5000) to browse the product list. If the local HTTPS development certificate is not trusted, trust it with `dotnet dev-certs https --trust` or configure the web app to use the API's HTTP URL.

## API endpoints

The API uses the `/products` route:

| Method | Route | Description |
| --- | --- | --- |
| `GET` | `/products` | Retrieve the product list. |
| `GET` | `/products/{productId}` | Retrieve a product and its expiration details. |
| `PUT` | `/products/{productId}` | Update a product using a JSON request body. |
| `DELETE` | `/products/{productId}` | Delete a product. |

## Configuration and data

- `ProductAPI/TestData/productstestdata.json` provides the API's initial sample product catalog.
- The MVC application's `APIURI` setting determines which API it calls. Use an environment variable to override it locally without changing committed settings.
- Both applications use in-memory caching in `Development`. Other environments use Redis; configure `RedisCacheConnection` through a secure environment-specific configuration source before running outside Development.
- Application Insights is enabled by both applications. Configure its connection settings for the environment where telemetry is required.

Do not commit Redis credentials, telemetry secrets, or other environment-specific credentials to the repository.
