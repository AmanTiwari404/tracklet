<div align="center">
  <img src="./screenshots/logo.png" alt="Tracklet Logo" width="100" />
</div>

# 📉 Tracklet — Smart Product Price Tracker  

Track product prices across e-commerce platforms and get instant alerts when prices drop.  
Built with Next.js, Firecrawl, Supabase, and Resend for reliable and automated price tracking.

---

## 🎯 Features

🔍 Track Any Product – Works across multiple e-commerce platforms  
📊 Price History Charts – Interactive charts showing price trends  
🔐 Google Authentication – Secure login via Supabase OAuth  
🔄 Automated Daily Price Checks – Cron-based automation  
📧 Email Alerts – Instant notifications via Resend  
🧠 Secure & Scalable – Row Level Security (RLS) enabled  

---

## 🖼️ Screenshots

### 🏠 Landing Page
![Landing Page](./screenshots/landing-page.png)

### 📦 Dashboard
![Dashboard](./screenshots/dashboard.png)

### 📊 Price History Chart
![Price History](./screenshots/price-history.png)

### 📧 Price Drop Alert Email
![Email Alert](./screenshots/email-alert.png)

---

## 🛠️ Tech Stack

- Next.js 16 (App Router)
- React
- Tailwind CSS
- shadcn/ui
- Recharts
- Supabase (PostgreSQL, Auth, RLS, pg_cron)
- Firecrawl (Web scraping & extraction)
- Resend (Transactional emails)

---

## 📋 Prerequisites

- Node.js 18+
- Supabase account
- Firecrawl account
- Resend account
- Google OAuth credentials

---

## 🚀 Setup Instructions

### 1. Clone & Install

git clone https://github.com/your-username/tracklet.git  
cd tracklet  
npm install  

---

## 2. Supabase Setup

### Create Project

Go to https://supabase.com  
Create a new project  
Wait for initialization  

---

### Database Schema (SQL Editor)

create extension if not exists "uuid-ossp";

create table products (
  id uuid primary key default uuid_generate_v4(),
  user_id uuid references auth.users(id) on delete cascade not null,
  url text not null,
  name text not null,
  current_price numeric(10,2) not null,
  currency text default 'USD',
  image_url text,
  created_at timestamptz default now(),
  updated_at timestamptz default now()
);

create table price_history (
  id uuid primary key default uuid_generate_v4(),
  product_id uuid references products(id) on delete cascade not null,
  price numeric(10,2) not null,
  currency text not null,
  checked_at timestamptz default now()
);

alter table products enable row level security;
alter table price_history enable row level security;

---

## 3. Enable Google Authentication

Supabase Dashboard → Authentication → Providers  
Enable Google  

Redirect URI:  
https://<your-project>.supabase.co/auth/v1/callback  

---

## 4. Firecrawl Setup

Sign up at https://firecrawl.dev  
Copy API key from dashboard  

---

## 5. Resend Setup

Sign up at https://resend.com  
Get API key  
(Optional) Verify custom domain  

---

## 6. Environment Variables

Create .env.local in the root directory:

# Supabase
NEXT_PUBLIC_SUPABASE_URL=your_supabase_url  
NEXT_PUBLIC_SUPABASE_ANON_KEY=your_anon_key  
SUPABASE_SERVICE_ROLE_KEY=your_service_role_key  

# Firecrawl
FIRECRAWL_API_KEY=your_firecrawl_api_key  

# Resend
RESEND_API_KEY=your_resend_api_key  
RESEND_FROM_EMAIL=onboarding@resend.dev  

# App
NEXT_PUBLIC_APP_URL=http://localhost:3000  
CRON_SECRET=your_generated_secret  

Generate CRON secret:

node -e "console.log(require('crypto').randomBytes(32).toString('hex'))"

---

## 7. Run Development Server

npm run dev  

Open 👉 http://localhost:3000  

---

## 🔍 How It Works

### User Flow

User logs in using Google  
Adds a product URL  
Firecrawl extracts product data  
Data stored securely in Supabase  
User views price history chart  
Email alert sent on price drop  

---

### Automated Price Tracking

pg_cron runs daily  
Calls secure API endpoint  
Firecrawl re-scrapes prices  
Database updates price history  
Resend sends alerts if prices drop  

---

## 📁 Project Structure

tracklet/
├── app/
│   ├── page.js
│   ├── api/
│   │   └── cron/check-prices/route.js
│   └── auth/callback/route.js
├── components/
│   ├── AddProductForm.js
│   ├── ProductCard.js
│   ├── PriceChart.js
│   └── AuthModal.js
├── lib/
│   ├── firecrawl.js
│   ├── email.js
│   └── utils.js
├── supabase/
│   └── migrations/
├── screenshots/
└── .env.local

---

## ⚠️ Limitations

Scraping depends on website structure  
Some sites may block scraping  
Price updates are scheduled, not real-time  

---

## 🚀 Future Enhancements

Browser extension  
Mobile app  
Custom alert thresholds  
Price prediction using ML  
Multi-currency support  

---

## 🤝 Contributing

Fork the repository  
Create a feature branch  
Commit your changes  
Push to GitHub  
Open a Pull Request  

---

## 👨‍💻 Author

Aman Kumar  
Final Year Student | Backend Developer  

---

## 📄 License

This project is licensed under the MIT License.

⭐ If you like this project, don’t forget to give it a star!
