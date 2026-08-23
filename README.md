# ⚡ Auto Workers Cloudflare Proxy

<p align="center">

  <img src="https://img.shields.io/badge/Cloudflare-Workers-F38020?style=for-the-badge&logo=cloudflare&logoColor=white" alt="Cloudflare Workers">

  <img src="https://img.shields.io/badge/JavaScript-ES6+-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black" alt="JavaScript">

  <img src="https://img.shields.io/badge/GitHub-Actions-2088FF?style=for-the-badge&logo=github-actions&logoColor=white" alt="GitHub Actions">

  <img src="https://img.shields.io/badge/License-Open%20Source-success?style=for-the-badge" alt="License">

</p>

<p align="center">
  <strong>🚀 Automated Cloudflare Workers Installer & API Proxy</strong>
</p>

<p align="center">
  یک نصب‌کننده مدرن و خودکار برای مدیریت و راه‌اندازی سرویس‌های Cloudflare Workers
</p>

<p align="center">
  <a href="https://github.com/hosseinit1988/Auto-Workers-Cloudflare-Prxoy">
    ⭐ GitHub Repository
  </a>
  •
  <a href="#-features">
    ✨ Features
  </a>
  •
  <a href="#-installation">
    ⚙️ Installation
  </a>
  •
  <a href="#-architecture">
    🏗️ Architecture
  </a>
</p>

---

<div align="center">
  <img src="Screenshot.png" alt="Hit Installer Screenshot" width="800">
</div>
## 🧩 معرفی پروژه

**Auto Workers Cloudflare Proxy** یک پروژه سبک، مدرن و کاربردی بر پایه **Cloudflare Workers** است که یک رابط وب تعاملی برای نصب و مدیریت سرویس‌های Worker فراهم می‌کند.

این پروژه با هدف ساده‌کردن فرآیند راه‌اندازی سرویس‌های Cloudflare طراحی شده است؛ به‌طوری که کاربر می‌تواند از طریق یک رابط گرافیکی، مراحل موردنیاز را طی کرده و عملیات مربوط به Cloudflare API را بدون نیاز به اجرای دستی تعداد زیادی دستور انجام دهد.

هسته پروژه از دو بخش اصلی تشکیل شده است:

```text
┌─────────────────────────────────────┐
│        🌐 Web Installer             │
│        index.html                   │
└──────────────────┬──────────────────┘
                   │
                   ▼
┌─────────────────────────────────────┐
│       ☁️ Installer Proxy Worker     │
│       installer-proxy.js            │
└──────────────────┬──────────────────┘
                   │
                   ▼
┌─────────────────────────────────────┐
│       Cloudflare API                │
│       api.cloudflare.com             │
└─────────────────────────────────────┘
```

Proxy Worker نیز برای جلوگیری از تبدیل‌شدن سرویس به یک **CORS Proxy عمومی**، مسیرهای مجاز API را به‌صورت مشخص محدود می‌کند.

---

# ✨ امکانات

## 🎨 رابط کاربری مدرن

- رابط کاربری Dark Mode
- طراحی Responsive
- پشتیبانی کامل از موبایل و دسکتاپ
- رابط کاربری فارسی
- پشتیبانی از زبان انگلیسی
- پشتیبانی RTL / LTR
- فونت Vazirmatn
- طراحی مرحله‌ای برای فرآیند نصب
- نمایش وضعیت عملیات
- Progress Indicator
- نمایش خطا و موفقیت عملیات
- دکمه‌های Copy برای اطلاعات خروجی

رابط اصلی پروژه در `index.html` پیاده‌سازی شده و از طراحی RTL فارسی و سیستم ترجمه داخلی استفاده می‌کند.

---

## ☁️ Cloudflare Workers

Proxy Worker وظیفه ارتباط بین Installer و Cloudflare API را بر عهده دارد.

این Worker می‌تواند درخواست‌های موردنیاز Installer را به API رسمی Cloudflare منتقل کند.

مسیرهای API مجاز شامل مواردی مانند:

```text
/accounts
/accounts/{account_id}

/accounts/{account_id}/d1/database

/accounts/{account_id}/workers/scripts/

/accounts/{account_id}/workers/scripts/{script}/deployments

/accounts/{account_id}/workers/workers

/accounts/{account_id}/workers/services/

/accounts/{account_id}/workers/subdomain
```

این محدودسازی به‌صورت Regex در Worker انجام می‌شود و درخواست‌هایی که خارج از مسیرهای تعریف‌شده باشند با خطای `403` رد می‌شوند.

---

# 🛡️ Security

یکی از بخش‌های مهم پروژه، محدودکردن Proxy به APIهایی است که Installer واقعاً نیاز دارد.

### 🔒 محدودسازی Cloudflare API

درخواست‌ها باید دارای Header زیر باشند:

```http
Authorization: Bearer YOUR_CLOUDFLARE_API_TOKEN
```

و مقصد API نیز توسط:

```http
X-Proxy-Target
```

مشخص می‌شود.

Proxy ابتدا مسیر را بررسی می‌کند و تنها در صورت مجازبودن، درخواست را به:

```text
https://api.cloudflare.com/client/v4
```

ارسال می‌کند.

---

## 🌐 GitHub Content Proxy

پروژه یک Endpoint اختصاصی برای دریافت محتوای GitHub نیز دارد:

```text
/github
```

آدرس مقصد از طریق Header زیر ارسال می‌شود:

```http
X-GitHub-Url
```

و تنها URLهای مشخصی پذیرفته می‌شوند:

```text
https://raw.githubusercontent.com/
https://cdn.jsdelivr.net/
https://api.github.com/
```

در نتیجه Proxy اجازه دریافت URLهای دلخواه و ناشناخته را نمی‌دهد.

---

# ❤️ Health Check

برای بررسی وضعیت Worker یک Endpoint ساده وجود دارد:

```text
/health
```

نمونه پاسخ:

```json
{
  "ok": true,
  "ts": 1750000000000
}
```

این Endpoint برای بررسی سریع در دسترس بودن Worker بسیار مناسب است.

---

# 📁 ساختار پروژه

```text
Auto-Workers-Cloudflare-Prxoy/
│
├── .github/
│   └── workflows/
│       └── deploy-proxy.yml
│
├── proxy/
│   ├── src/
│   │   └── index.js
│   │
│   └── wrangler.toml
│
├── index.html
├── installer-proxy.js
└── README.md
```

### 📄 `index.html`

رابط گرافیکی اصلی Installer.

شامل:

- UI
- فرم‌ها
- مراحل نصب
- سیستم ترجمه
- مدیریت وضعیت
- نمایش نتایج
- ارتباط با Proxy

---

### 📄 `installer-proxy.js`

نسخه Worker مربوط به Installer Proxy.

وظیفه اصلی:

```text
Browser
   ↓
Installer Proxy
   ↓
Cloudflare API
```

و همچنین دریافت منابع مجاز GitHub را مدیریت می‌کند.

---

### 📁 `proxy/`

نسخه مستقل Proxy Worker.

ساختار:

```text
proxy/
├── src/
│   └── index.js
└── wrangler.toml
```

تنظیمات Wrangler پروژه شامل نام Worker، فایل Entry Point و `nodejs_compat` است.

---

# ⚙️ نصب Proxy Worker

## پیش‌نیازها

برای Deploy کردن Worker به موارد زیر نیاز دارید:

- حساب Cloudflare
- Cloudflare Account ID
- Cloudflare API Token
- Git
- Node.js
- Wrangler

---

## 1️⃣ Clone Repository

```bash
git clone https://github.com/hosseinit1988/Auto-Workers-Cloudflare-Prxoy.git

cd Auto-Workers-Cloudflare-Prxoy
```

---

## 2️⃣ ورود به پوشه Proxy

```bash
cd proxy
```

---

## 3️⃣ نصب Wrangler

در صورت نیاز:

```bash
npm install -g wrangler
```

بررسی نصب:

```bash
wrangler --version
```

---

## 4️⃣ ورود به Cloudflare

```bash
wrangler login
```

پس از بازشدن مرورگر، حساب Cloudflare خود را انتخاب کنید.

---

## 5️⃣ Deploy

```bash
wrangler deploy
```

پس از Deploy، Cloudflare آدرس Worker را در خروجی نمایش خواهد داد.

---

# 🤖 GitHub Actions

پروژه دارای Workflow اختصاصی برای Deploy خودکار Proxy Worker است.

Workflow در:

```text
.github/workflows/deploy-proxy.yml
```

قرار دارد.

با Push شدن تغییرات به Branch:

```text
main
```

در صورتی که تغییر مربوط به:

```text
proxy/**
```

یا Workflow باشد، GitHub Actions فرآیند Deploy را اجرا می‌کند.

---

## 🔐 GitHub Secrets

در Repository خود این Secrets را ایجاد کنید:

```text
CLOUDFLARE_API_TOKEN
CLOUDFLARE_ACCOUNT_ID
```

Workflow از این دو Secret برای Deploy کردن Worker استفاده می‌کند.

---

# 🔄 معماری پروژه

```text
                    ┌─────────────────┐
                    │      User       │
                    │    Browser 🌐   │
                    └────────┬────────┘
                             │
                             ▼
                ┌────────────────────────┐
                │    Web Installer UI    │
                │       index.html       │
                └───────────┬────────────┘
                            │
                            ▼
                ┌────────────────────────┐
                │   Installer Proxy      │
                │   Cloudflare Worker    │
                └───────────┬────────────┘
                            │
                 ┌──────────┴──────────┐
                 │                     │
                 ▼                     ▼
       ┌─────────────────┐   ┌──────────────────┐
       │ Cloudflare API  │   │ GitHub Raw/API   │
       │      ☁️         │   │       🐙         │
       └─────────────────┘   └──────────────────┘
```

---

# 🧠 نحوه کار

فرآیند کلی پروژه را می‌توان به شکل زیر خلاصه کرد:

```text
1. User opens Installer
             ↓
2. Installer collects required information
             ↓
3. Request is sent to Proxy Worker
             ↓
4. Proxy validates requested path
             ↓
5. Valid request → Cloudflare API
             ↓
6. Cloudflare response
             ↓
7. Proxy returns response
             ↓
8. Installer updates UI
```

برای درخواست‌های GitHub نیز مسیر جداگانه‌ای وجود دارد:

```text
Installer
   │
   ▼
/github
   │
   ▼
X-GitHub-Url
   │
   ▼
URL Validation
   │
   ├── ❌ Not Allowed
   │
   └── ✅ Allowed
          │
          ▼
    GitHub / jsDelivr
```

---

# 🧪 تست Health Check

پس از Deploy شدن Worker، آدرس زیر را باز کنید:

```text
https://YOUR-WORKER-DOMAIN/health
```

مثلاً:

```text
https://your-worker.workers.dev/health
```

اگر Worker فعال باشد، باید پاسخی مشابه زیر دریافت کنید:

```json
{
  "ok": true,
  "ts": 1750000000000
}
```

---

# 🧰 تکنولوژی‌های استفاده‌شده

| Technology | Usage |
|---|---|
| ☁️ Cloudflare Workers | Proxy / Serverless Runtime |
| 🟨 JavaScript | Worker Logic |
| 🌐 HTML5 | Installer UI |
| 🎨 CSS3 | UI / Responsive Design |
| ⚡ JavaScript | Frontend Logic |
| 🐙 GitHub API | Repository / Raw Content |
| 🔧 Wrangler | Worker Deployment |
| 🤖 GitHub Actions | CI/CD |
| 🔤 Vazirmatn | Persian Typography |

---

# 📱 Responsive Design

Installer برای نمایشگرهای مختلف طراحی شده است:

```text
┌─────────────────────────────┐
│         Desktop             │
│                             │
│      ┌───────────────┐      │
│      │   Installer   │      │
│      │               │      │
│      │    Form       │      │
│      │               │      │
│      └───────────────┘      │
└─────────────────────────────┘


┌─────────────────┐
│     Mobile      │
│                 │
│  ┌───────────┐  │
│  │ Installer │  │
│  │           │  │
│  │   Form    │  │
│  │           │  │
│  └───────────┘  │
└─────────────────┘
```

در CSS پروژه breakpoint مخصوص نمایشگرهای کوچک نیز در نظر گرفته شده است.

---

# 🌍 چندزبانه

رابط Installer از دو زبان پشتیبانی می‌کند:

```text
🇮🇷 فارسی
🇬🇧 English
```

و کاربر می‌تواند از طریق گزینه:

```text
🌐 EN / فا
```

زبان رابط را تغییر دهد.

---

# ⚠️ نکات امنیتی مهم

این پروژه یک Proxy عمومی برای استفاده آزاد در اینترنت نیست.

Proxy مسیرهای Cloudflare API را محدود می‌کند، اما همچنان توصیه می‌شود:

- API Token را در کد قرار ندهید.
- Token را در GitHub Commit نکنید.
- از Token با کمترین سطح دسترسی موردنیاز استفاده کنید.
- Worker را بدون نیاز در اختیار عموم قرار ندهید.
- لاگ‌های Worker را بررسی کنید.
- در صورت مشاهده رفتار غیرعادی، Token را Rotate کنید.

> **Never commit your Cloudflare API Token to GitHub.**

---

# 🚀 Deploy سریع

اگر محیط Wrangler شما آماده است:

```bash
git clone https://github.com/hosseinit1988/Auto-Workers-Cloudflare-Prxoy.git
cd Auto-Workers-Cloudflare-Prxoy/proxy
wrangler deploy
```

---

# 🔗 Repository

<p align="center">

### ⭐ GitHub

**https://github.com/hosseinit1988/Auto-Workers-Cloudflare-Prxoy**

</p>

---

# 👨‍💻 Developer

<p align="center">

### Hossein Shourgashti

**hosseinit1988**

Web Designer • Linux Developer • Network Security • Software Developer

🐙 GitHub:

**https://github.com/hosseinit1988**

</p>

---

# 🤝 Contributing

Pull Request و پیشنهادهای فنی برای بهبود پروژه استقبال می‌شود.

برای مشارکت:

```bash
git clone https://github.com/hosseinit1988/Auto-Workers-Cloudflare-Prxoy.git

cd Auto-Workers-Cloudflare-Prxoy

git checkout -b feature/my-feature
```

تغییرات خود را اعمال کرده و سپس:

```bash
git add .
git commit -m "feat: add new feature"
git push origin feature/my-feature
```

سپس یک Pull Request در GitHub ایجاد کنید.

---

# ⭐ Support

اگر این پروژه برای شما مفید بود، با یک ⭐ در GitHub از پروژه حمایت کنید.

<p align="center">

**⭐ Star the repository if you find it useful!**

</p>

---

# 📜 License

این پروژه به‌صورت Open Source منتشر شده است.

لطفاً قبل از استفاده تجاری یا بازنشر، فایل License موجود در Repository را بررسی کنید.

---

<p align="center">

### ⚡ Built with Cloudflare Workers
### ❤️ Made by Hossein Shourgashti

</p>
