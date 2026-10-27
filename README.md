# JP REPAIR POS — OOP Professional Edition

This version is organized around the four pillars of OOP:

1. Encapsulation — Repair owns billing calculations and payment validation is contained in processor classes.
2. Abstraction — PaymentProcessor defines a common payment interface.
3. Inheritance — Cash/Wallet/Card/BankTransfer processors inherit PaymentProcessor.
4. Polymorphism — PaymentProcessorFactory returns different processors while PaymentService calls the same interface.

## Stack
- Python
- Flask
- MySQL
- PyMySQL
- Tailwind CSS CDN
- OOP services/repositories/models

## Features
- Admin, cashier, technician and staff users
- Repair/customer/device records
- Parts/items with quantity and price
- Labor and discount
- Automatic total, paid and balance
- Repair Action and Billing Status are separate: Repair Action tracks workflow; Billing Status is UNPAID, PARTIALLY PAID, PAID, or REFUNDED. Billing status is automatically set from recorded payments.
- Cash payment with change calculation
- Online Wallet choice: GCash, Maya, GoTyme, QRPh
- Card and bank transfer recording
- Payment transaction history
- Release blocked until fully paid
- CSV history download
- Admin history deletion
- MySQL-only architecture
- No SQLite/PyMySQL mixing

## Install
Create the database first:

CREATE DATABASE jp_repair CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;

Then:

python -m venv .venv
.venv\Scripts\activate
python -m pip install -r requirements.txt
copy .env.example .env

Set your MySQL password and database settings, then:

python run.py

Open:
http://127.0.0.1:5000

Initial login:
admin / admin123

Change this immediately.

## Important payment note
The Online Wallet options are accounting/payment-method choices. A live dynamic QR and automatic payment confirmation require an approved merchant/payment-provider API and credentials. This project intentionally does not invent or bypass banking APIs.


### Automatic Record of Sale for Completed Repairs
When a repair is fully paid and the cashier releases it, JP Repair automatically creates one Sales / Record of Sale entry linked to that repair. It records the customer, device/repair service, total transaction amount, payment method/reference, cashier, and repair number. The linked repair ID prevents duplicate automatic sales. Automatic repair sales are shown in Sales history but are excluded from the separate retail-sales income subtotal so Total Income is not double-counted.

## Password security
- Passwords are stored as Werkzeug hashes.
- Logged-in users can change their password.
- Forgot Password uses a per-user recovery code configured by an admin.
- Recovery codes are hashed and never displayed after saving.
- POST requests require a CSRF token; basic login rate limiting and security headers are enabled.
- For production, run behind HTTPS and set SESSION_COOKIE_SECURE=True.

## Password storage choice
This build intentionally stores the login password in the `users.password` column as plain text because the owner requested database-visible passwords. This is less secure than password hashing. The application still uses parameterized SQL, role checks, session protections, login rate limiting, CSRF/security controls where implemented, and server-side validation.

Password features:
- Change Password while logged in
- Forgot Password using the user's recovery code
- Admin password reset through user management
- Recovery code is also stored directly for database visibility in `users.recovery_code`
"# jprepair" 
