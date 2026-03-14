# 📋 AI Customer Complaint System

An automated customer complaint system using n8n, Google Gemini AI, Google Sheets, and Gmail.

---

## 🚀 How It Works

```
Customer submits form → Webhook → AI Agent (Gemini) → Google Sheet → Gmail reply
```

1. Customer fills the HTML complaint form
2. n8n receives the data via Webhook
3. Google Gemini AI analyzes and generates a response
4. Complaint is saved to Google Sheets
5. Automated email reply is sent to the customer via Gmail

---

## 🛠️ Tech Stack

| Tool | Purpose |
|------|---------|
| HTML/CSS | Customer-facing complaint form |
| n8n (self-hosted on Render) | Workflow automation |
| Google Gemini AI | AI-powered complaint analysis |
| Google Sheets | Complaint data storage |
| Gmail | Automated email replies |
| Render.com | Free n8n hosting |

---

## 📁 Files

| File | Description |
|------|-------------|
| `complaint_form.html` | Beautiful complaint submission form |
| `README.md` | Project documentation |

---

## ⚙️ Setup

### 1. n8n (Self-hosted on Render)
- URL: `https://n8nio-n8n-0y64.onrender.com`
- Deployed using Docker image: `n8nio/n8n`
- Free tier on Render.com

### 2. HTML Form
- Form submits to:
  ```
  https://n8nio-n8n-0y64.onrender.com/webhook/complaint
  ```
- Fields: Name, Email, Issue Type, Message

### 3. n8n Workflow Nodes

#### Webhook Node
- Method: POST
- Path: `complaint`
- Production URL: `https://n8nio-n8n-0y64.onrender.com/webhook/complaint`

#### AI Agent Node
- Model: Google Gemini
- Prompt mapping:
  ```
  Customer Name: {{ $('Webhook').item.json.body.name }}
  Email: {{ $('Webhook').item.json.body.email }}
  Issue Type: {{ $('Webhook').item.json.body.issue }}
  Message: {{ $('Webhook').item.json.body.message }}
  ```

#### Google Sheets Node
- Operation: Append row
- Column mapping:
  ```
  name    → {{ $('Webhook').item.json.body.name }}
  Email   → {{ $('Webhook').item.json.body.email }}
  Issue   → {{ $('Webhook').item.json.body.issue }}
  Message → {{ $('Webhook').item.json.body.message }}
  ```

#### Gmail Node
- To: `{{ $('Webhook').item.json.body.email }}`
- Subject: `Your complaint has been received`
- Body includes AI-generated response:
  ```
  {{ $('AI Agent').item.json.output }}
  ```

---

## 🔑 Credentials Required

| Service | Type |
|---------|------|
| Google Gemini | API Key |
| Google Sheets | OAuth2 (Client ID + Secret) |
| Gmail | OAuth2 (Client ID + Secret) |

> Google OAuth credentials from: [console.cloud.google.com](https://console.cloud.google.com)

---

## ⚠️ Important Notes

- Render free tier sleeps after **15 min of inactivity**
- Use [UptimeRobot](https://uptimerobot.com) to keep it awake (free)
- Ping URL every 5 minutes: `https://n8nio-n8n-0y64.onrender.com`

---

## 📊 Google Sheet Structure

| Column A | Column B | Column C | Column D |
|----------|----------|----------|----------|
| name | Email | Issue | Message |

---
