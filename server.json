const express = require('express');
const cors = require('cors');
const bcrypt = require('bcryptjs');
const session = require('express-session');
require('dotenv').config();

const app = express();
const PORT = process.env.PORT || 3000;

// In-memory user database simulation (use a real DB for production scaling)
const users = [];

app.use(cors());
app.use(express.json());
app.use(express.urlencoded({ extended: true }));

// Configure secure session handling
app.use(session({
    secret: process.env.SESSION_SECRET || 'pulsepay_secure_secret_key',
    resave: false,
    saveUninitialized: false,
    cookie: { secure: false, maxAge: 24 * 60 * 60 * 1000 } // 1 day session
}));

app.use(express.static('public'));

// --- AUTHENTICATION ROUTES ---

// Sign Up Endpoint
app.post('/api/v1/auth/signup', async (req, res) => {
    try {
        const { email, password, businessName } = req.body;
        
        if (!email || !password) {
            return res.status(400).json({ success: false, message: 'Email and password are required.' });
        }

        const existingUser = users.find(u => u.email === email);
        if (existingUser) {
            return res.status(400).json({ success: false, message: 'An account with this email already exists.' });
        }

        const hashedPassword = await bcrypt.hash(password, 10);
        const newUser = { id: `usr_${Date.now()}`, email, password: hashedPassword, businessName: businessName || 'My Business' };
        
        users.push(newUser);

        // Auto login session after signup
        req.session.userId = newUser.id;
        req.session.email = newUser.email;

        res.status(201).json({ success: true, message: 'Account created successfully.', redirect: '/index.html' });
    } catch (error) {
        res.status(500).json({ success: false, message: 'Server error during registration.' });
    }
});

// Login Endpoint
app.post('/api/v1/auth/login', async (req, res) => {
    try {
        const { email, password } = req.body;

        const user = users.find(u => u.email === email);
        if (!user) {
            return res.status(400).json({ success: false, message: 'Invalid email or password.' });
        }

        const isMatch = await bcrypt.compare(password, user.password);
        if (!isMatch) {
            return res.status(400).json({ success: false, message: 'Invalid email or password.' });
        }

        req.session.userId = user.id;
        req.session.email = user.email;

        res.status(200).json({ success: true, message: 'Login successful.', redirect: '/index.html' });
    } catch (error) {
        res.status(500).json({ success: false, message: 'Server error during login.' });
    }
});

// Check Session Status
app.get('/api/v1/auth/session', (req, res) => {
    if (req.session.userId) {
        res.json({ isAuthenticated: true, email: req.session.email });
    } else {
        res.json({ isAuthenticated: false });
    }
});

// Logout Endpoint
app.post('/api/v1/auth/logout', (req, res) => {
    req.session.destroy(() => {
        res.json({ success: true, redirect: '/auth.html' });
    });
});

// --- SECURE PAYMENT ROUTE ---
app.post('/api/v1/create-payment-intent', async (req, res) => {
    if (!req.session.userId) {
        return res.status(401).json({ success: false, message: 'Unauthorized. Please log in first.' });
    }

    try {
        const { amount, currency, paymentMethod, customerEmail } = req.body;

        if (!amount || !currency) {
            return res.status(400).json({ success: false, message: 'Invalid payment parameters.' });
        }

        // Simulate ultra-fast secure gateway processing / ledger settlement token generation
        const transactionId = `txn_${Math.random().toString(36).substring(2, 15)}_${Date.now()}`;
        
        // In a live integration, you would interface directly with banking partner APIs / Stripe / Paystack / Flutterwave here.
        setTimeout(() => {
            res.status(200).json({
                success: true,
                transactionId,
                status: 'SUCCESS',
                message: 'Payment processed instantly via PulsePay smart-route.',
                amount,
                currency: currency.toUpperCase(),
                timestamp: new Date().toISOString()
            });
        }, 800); // Simulated sub-second latency advantage

    } catch (error) {
        console.error('Payment processing error:', error);
        res.status(500).json({ success: false, message: 'Internal gateway timeout or error.' });
    }
});

// Keep local development listener
if (process.env.NODE_ENV !== 'production') {
    app.listen(PORT, () => {
        console.log(`PulsePay server running at http://localhost:${PORT}`);
    });
}

// Export for Vercel Serverless functions
module.exports = app;
