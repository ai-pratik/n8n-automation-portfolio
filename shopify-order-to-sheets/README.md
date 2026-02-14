# 🛒 Shopify Order Automation using n8n

---

## 📌 Project Overview

This project implements a real-time event-driven automation workflow using **n8n** to capture Shopify order events via webhook and log structured order data into Google Sheets automatically.

The workflow eliminates manual order tracking by ingesting incoming Shopify order payloads and transforming the data into a clean format suitable for reporting and analytics.

---

## 🧠 Problem Statement

Manual order tracking from Shopify into spreadsheets or CRMs is time-consuming and error-prone.
Businesses often require automated ingestion of order data for:

* Inventory tracking
* Revenue monitoring
* CRM integration
* Reporting dashboards
* Downstream logistics automation

This workflow automates the entire process in real time using webhook-based event listening.

---

## ⚙️ Architecture Overview

```
Shopify Order Event
        ↓
Webhook Trigger (n8n)
        ↓
Payload Parsing
        ↓
Data Transformation (Set Node)
        ↓
Google Sheets API (OAuth2)
        ↓
Append Order Data
```

---

## 🔧 Tech Stack

| Tool              | Purpose                    |
| ----------------- | -------------------------- |
| n8n               | Workflow Automation Engine |
| Shopify Webhooks  | Event Trigger              |
| Google Sheets API | Data Storage               |
| OAuth 2.0         | Secure Authentication      |
| REST API          | Data Communication         |

---

## 📥 Incoming Payload (Shopify)

When an order is placed, Shopify sends an HTTP POST request:

```json
{
  "id": "ORD888",
  "email": "demo@gmail.com",
  "total_price": "4999"
}
```

n8n wraps this inside:

```json
{
  "body": {
    "id": "ORD888",
    "email": "demo@gmail.com",
    "total_price": "4999"
  }
}
```

---

## 🔄 Data Transformation

The workflow extracts required fields using a Set Node:

```javascript
order_id = {{$json["body"]["id"]}}
email = {{$json["body"]["email"]}}
price = {{$json["body"]["total_price"]}}
```

Transforms payload into:

```json
{
  "order_id": "ORD888",
  "email": "demo@gmail.com",
  "price": "4999"
}
```

---

## 📤 Output (Google Sheet)

| order_id | email                                   | price |
| -------- | --------------------------------------- | ----- |
| ORD888   | [demo@gmail.com](mailto:demo@gmail.com) | 4999  |

---

## 🔐 Authentication Setup

Google Sheets API integration is configured using:

* OAuth 2.0 Client ID
* Google Drive API
* Google Sheets API

Authorized Redirect URI:

```
http://localhost:5678/rest/oauth2-credential/callback
```

---

## 🚀 Setup Instructions

### 1. Start n8n

```bash
npx n8n
```

---

### 2. Import Workflow

Inside n8n:

```
Workflows → Import from File
```

Upload:

```
workflow.json
```

---

### 3. Configure Google OAuth2 Credential

* Create OAuth Client in Google Cloud Console
* Enable:

  * Google Drive API
  * Google Sheets API
* Add Redirect URI:

```
http://localhost:5678/rest/oauth2-credential/callback
```

---

### 4. Prepare Google Sheet

Create columns:

```
order_id | email | price
```

---

### 5. Test Webhook

Click:

```
Execute Workflow
```

Send POST Request:

```
POST http://localhost:5678/webhook-test/shopify-order
```

Body:

```json
{
  "id": "ORD9001",
  "email": "demo@gmail.com",
  "total_price": "3500"
}
```

---

## 📈 Use Cases

* Automated Order Logging
* Revenue Tracking
* CRM Sync
* Inventory Monitoring
* Financial Reporting
* Business Intelligence Pipelines

---

## 🧩 Future Enhancements

* Slack Order Notifications
* ERP Integration
* AI-based Order Classification
* Retry Logic for API Failures
* Order Fraud Detection

---

## 📂 Workflow File

```
workflow.json
```

Import inside n8n to run automation.

---

## 👨‍💻 Author

**Pratik Gade**
AI Automation Engineer
