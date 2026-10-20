# Customer Order Automation with n8n

An automated customer order management workflow built with **n8n**, **Google Sheets**, **Gmail**, and a custom HTML form.

The project demonstrates how businesses can streamline order processing by automatically collecting customer information, validating incoming data, storing orders, and sending email notifications.

## Project Overview

Manual order processing can be time-consuming and prone to human error. This project provides a simple automation solution that eliminates repetitive data entry and improves the efficiency of handling customer orders.

When a customer submits an order through the web form, the information is sent to an n8n webhook, validated, saved to Google Sheets, and forwarded to the business owner via email.

## Key Features

- **Customer Order Form** — Responsive HTML form for submitting customer orders.
- **Webhook Integration** — Receives order information in real time.
- **Data Validation** — Validates email addresses, phone numbers, product names, and order quantities.
- **Google Sheets Integration** — Automatically stores valid orders in a spreadsheet.
- **Gmail Notifications** — Sends email notifications containing order details.
- **Error Handling** — Handles invalid submissions and integration failures.
- **HTTP Responses** — Returns appropriate status codes for successful and unsuccessful requests.
- **International Phone Support** — Provides country selection, international dialing codes, and phone number validation.

## Workflow Architecture

```text
Customer Order Form
        |
        v
    n8n Webhook
        |
        v
    Edit Fields
        |
        v
   IF (Validation)
        |
        +---- TRUE
        |       |
        |       v
        |   Google Sheets
        |       |
        |       +---- Success
        |       |       |
        |       |       v
        |       |     Gmail
        |       |       |
        |       |       +---- Success --> HTTP 200
        |       |       |
        |       |       +---- Error ----> HTTP 500
        |       |
        |       +---- Error ------------> HTTP 500
        |
        +---- FALSE
                |
                v
           Invalid Order
                |
                v
             HTTP 400
```

## Technologies Used

| Technology | Purpose |
|---|---|
| n8n | Workflow automation |
| HTML5 | Customer order form |
| CSS3 | Responsive user interface |
| JavaScript | Form validation and webhook requests |
| intl-tel-input | International phone number input and validation |
| Google Sheets API | Order storage |
| Gmail API | Email notifications |
| Google Cloud OAuth 2.0 | Secure Google service authentication |
| Docker | Local n8n deployment |
| ngrok | Public webhook access during development |
| GitHub Pages | Static website hosting |

## Order Data Structure

Each submitted order contains the following information:

```json
{
  "email": "customer@example.com",
  "country": "Ukraine",
  "country_code": "ua",
  "phone": "+380671234567",
  "product": "Office Desk",
  "quantity": 2,
  "order_date": "2026-10-20T12:00:00.000Z"
}
```

## Data Validation

The workflow validates incoming order information before processing it.

Validation rules include:

- Email must have a valid format.
- Phone number must follow the supported international format.
- Product name cannot be empty.
- Quantity must be a positive integer.

Only valid orders proceed to Google Sheets and Gmail.

## Error Handling

The workflow uses HTTP status codes to indicate the result of each request.

| HTTP Status | Description |
|---|---|
| 200 OK | Order successfully processed |
| 400 Bad Request | Invalid order information |
| 500 Internal Server Error | Google Sheets or email notification failure |

Separate error branches prevent invalid orders from being stored and help identify integration failures.

## How It Works

1. A customer opens the order form hosted on GitHub Pages.
2. The customer enters their email, phone number, product, and quantity.
3. JavaScript sends the order data to the n8n webhook using an HTTP POST request.
4. The Edit Fields node structures the incoming information.
5. The IF node validates the order.
6. Valid orders are added to Google Sheets.
7. Gmail sends an automated notification to the business owner.
8. The webhook returns an appropriate HTTP response.
9. Invalid orders and integration failures are handled through dedicated error branches.

## Live Demo

**Customer Order Form:**

https://mansorri.github.io/form-google_table-email/

The live form requires an active n8n instance and webhook tunnel to process submissions.

## Local Setup

### Prerequisites

- Docker Desktop
- n8n
- ngrok
- Google Cloud project
- Google Sheets and Gmail API access
- OAuth 2.0 credentials

### Setup Instructions

1. Run n8n using Docker.
2. Open the n8n editor at `http://localhost:5678`.
3. Create or import the order processing workflow.
4. Configure the Webhook node to accept POST requests.
5. Connect Google Sheets and Gmail using OAuth 2.0 credentials.
6. Configure the Google Sheets document and destination sheet.
7. Start an ngrok tunnel for port `5678`.
8. Update the webhook URL in `index.html`.
9. Activate the workflow.
10. Submit a test order through the form.

**Security note:** Never commit OAuth client secrets, access tokens, API keys, or other sensitive credentials to a public repository.

For production use, deploy n8n to a secure, continuously available environment rather than relying on a temporary local tunnel.

## Testing

The automation is designed to support the following test scenarios:

| Test Case | Expected Result |
|---|---|
| Valid customer order | Saved to Google Sheets and notification sent |
| Invalid email | Order rejected |
| Invalid phone number | Order rejected |
| Empty product name | Order rejected |
| Quantity less than 1 | Order rejected |
| Google Sheets failure | HTTP 500 response |
| Gmail notification failure | HTTP 500 response |

## Business Benefits

- Reduces manual order processing.
- Eliminates repetitive data entry.
- Centralizes customer order information.
- Provides automatic notifications.
- Improves data accuracy through validation.
- Creates a foundation for further business process automation.

## Future Improvements

- Unique order IDs and duplicate prevention.
- Automatic order confirmation emails to customers.
- Order status tracking.
- Additional notification channels.
- Customer relationship management (CRM) integration.
- A dashboard for monitoring incoming orders.

## Project Status

**Functional demonstration / portfolio project**

The core order processing workflow has been implemented and tested with sample submissions. Additional production safeguards, monitoring, and deployment configuration may be required before use in a live business environment.

---

**Built with n8n, JavaScript, Google Sheets, and Gmail.**
