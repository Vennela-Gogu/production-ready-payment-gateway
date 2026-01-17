Payment Gateway – Async Processing & Webhooks
This project is a production-ready payment gateway built as an extension of an earlier payment system.
It enhances the basic payment flow with asynchronous processing, idempotency, secure webhooks, retries, and refunds, similar to real-world fintech platforms.
Key Features
API key–based authentication
Payment creation with safe retries (Idempotency-Key)
Asynchronous payment processing using Redis and Celery
Webhook notifications for payment status updates
HMAC signature verification for webhook security
Automatic retries with exponential backoff
Refund processing support
Dockerized setup for easy execution
Payment Flow (High Level)
Client creates a payment via API
Payment is stored with status pending
Background worker processes the payment asynchronously
Final status is updated (success / failed)
Merchant is notified via a signed webhook
Authentication
All APIs require API credentials sent as headers:
Copy code

X-API-Key
X-API-Secret
Requests with invalid credentials are rejected.
Create Payment API
Endpoint
Copy code

POST /api/v1/payments
Optional Header
Copy code

Idempotency-Key
The idempotency key ensures duplicate requests return the same response and prevents double charging.
Asynchronous Processing
Payment processing is handled in the background using a queue.
This keeps APIs fast, scalable, and resilient to failures.
Webhooks & Security
Webhooks are triggered on payment status changes
Each webhook payload is signed using HMAC SHA-256
Merchants verify authenticity using the shared secret
Failed deliveries are retried with exponential backoff
Refunds
Refunds can be initiated for completed payments using a dedicated API endpoint.
Refund status is tracked independently from the payment.
Running the Project
Copy code
Bash
docker-compose up --build
This starts the API server, database, Redis, and background worker.