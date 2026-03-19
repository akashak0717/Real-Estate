# 🏠 Real Estate App — DevOps CI/CD Pipeline on AWS

A modern and responsive Real Estate web application built with
React.js, Vite, and Tailwind CSS — fully containerised with Docker
and deployed on AWS EC2 using an automated Jenkins CI/CD pipeline.

## ✨ App Features
- 🏘️ Property Listings — Browse available properties
- 🏡 Property Detail Page — Full info and pricing
- 🦸 Hero / Landing Section — Clean homepage with CTA
- 🔐 Login / Signup — User authentication flow
- 📬 Contact Form — Reach out to agents
- 📱 Fully Responsive — Mobile, tablet and desktop

## 🛠️ Tech Stack
### Frontend
- React.js, Vite, Tailwind CSS, Framer Motion

### DevOps & Cloud
- Docker + Nginx (containerisation)
- Jenkins (CI/CD pipeline with GitHub Webhook + SCM)
- AWS EC2 (hosting)
- Ansible (server configuration & deployment)

## 🏗️ CI/CD Pipeline Flow
Push code → GitHub Webhook triggers Jenkins →
Build Docker image → Push to Docker Hub →
Ansible deploys to AWS EC2 → App is live ✅

## 🚀 Run Locally
npm install
npm run dev
