# PulsePay — Next-Generation Global Payment Solution

**PulsePay** is a high-conversion, zero-redirect global payment gateway designed to eliminate user friction, drop-offs, and lag during checkout. Built with a lightning-fast client-side core and a secure Node.js backend, it prioritizes speed, modern design, and robust merchant authentication.

## Features
* **Zero-Redirect Inline Architecture**: Seamlessly process payments asynchronously without forcing users away from your application.
* **Secure Authentication**: Built-in user sign up and login sessions utilizing bcrypt password hashing and express-session middleware.
* **Glassmorphism UI**: A gorgeous, ultra-responsive dark-mode interface built using Tailwind CSS and Google Fonts.
* **Instant Settlement Routing**: Simulated instant transaction token generation and live feedback status states.

## Project Structure
```text
pulsepay/
├── server.js          # Backend server, authentication, and payment routes
├── package.json       # Project dependencies and startup scripts
└── public/
    ├── auth.html      # Merchant Sign Up and Login portal
    └── index.html     # Secure PulsePay checkout dashboard
