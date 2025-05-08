# PiklHub Deployment Guide

## Solution Overview
PiklHub is a modern web application with a Blazor WebAssembly frontend and an ASP.NET Core Web API backend. It is designed for affordable, scalable deployment on Azure.

## Azure Hosting Plan

### Backend: PiklHub.API
- **Host:** Azure App Service (B1 tier for cost savings)
- **Resource Group:** `picklhub`
- **Production Database:** Azure SQL Database (Serverless tier for cost efficiency)
- **Local Development Database:** SQLite (file-based, no cost)

### Frontend: PiklHub.Web
- **Host:** Azure Static Web Apps (Free Tier)
- **Resource Group:** `picklhub`

## Database Strategy
- **Development:** Uses SQLite for zero-cost, simple local development.
- **Production:** Uses Azure SQL Database (Serverless) for scalable, pay-per-usage costs.

## Environment-Specific Configuration
- **Connection Strings:**
  - `appsettings.Development.json` uses SQLite:
    ```json
    "ConnectionStrings": {
      "DefaultConnection": "Data Source=piklhub-dev.db"
    }
    ```
  - `appsettings.json` (production) uses Azure SQL. The connection string should be set via Azure App Service configuration or environment variables for security:
    ```json
    "ConnectionStrings": {
      "DefaultConnection": "<Azure SQL connection string>"
    }
    ```
- **DbContext** is configured in `Program.cs` to use SQLite in development and SQL Server in production.

## Deployment Notes
- **Resource Group:** All resources should be deployed to the `picklhub` resource group for easy management.
- **App Service Plan:** Use the B1 (Basic) tier for the API to minimize costs.
- **Static Web App:** Use the Free Tier for the Blazor frontend.
- **Azure SQL:** Use the Serverless compute tier for cost-effective, auto-pausing database.
- **Configuration:**
  - Set the production connection string in the Azure Portal under App Service > Configuration.
  - No code changes are needed to switch between dev/prod databases; the environment and connection string determine the provider.

## Quick Start
1. Clone the repo and open the solution.
2. For local dev, run both projects. The API will use SQLite automatically.
3. For Azure deployment, publish the API to App Service (B1) and the frontend to Static Web Apps (Free).
4. Set the Azure SQL connection string in App Service configuration.

---

**This setup ensures you only pay for what you use in production, and incur no costs for local development.** 