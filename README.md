# 🚀 Buy & Sell Website - Keep-Alive Service

এই repository টি **Buy & Sell Website** server কে Render.com এ sleep থেকে জাগিয়ে রাখে।

## ⚡ Features

- ✅ প্রতি 7 মিনিটে automatic server ping
- ✅ Health endpoint check (`/api/health`)
- ✅ Keep-alive endpoint check (`/api/keep-alive`)
- ✅ 3 retry attempts with 10 second delay
- ✅ সম্পূর্ণ Free (GitHub Actions - Public Repo)

## 📊 Status

**Server:** `https://digital-potential.onrender.com`

**Endpoints:**
- `/api/health` - Server health check with database connection
- `/api/keep-alive` - Keep-alive ping endpoint

## 🔄 Schedule

**Strategy:** Hourly cron (reliable) + 55-minute in-job ping loop

```
GitHub Actions hourly cron (top of every hour)
    ↓
Single job runs for 55 minutes
    ↓
Pings /api/health every 5 minutes (11 pings)
    ↓
Next hour's run takes over seamlessly
```

> ⚠️ Note: High-frequency crons (every 5-7 min) are **unreliable** on GitHub Actions —
> the scheduler delays or drops runs during peak load. That's why we use an hourly
> cron with an in-job loop instead.

## 🎯 How It Works

```
GitHub Actions (Every 7 min)
    ↓ Ping
Render Server (/api/health)
    ↓ Verify
MySQL Database Connection
    ↓ Response
GitHub Logs (Success/Failure)
```

## 📝 Setup Instructions

### Step 1: Update Server URL

Edit `.github/workflows/keep-alive.yml` and replace:
```yaml
SERVER="https://digital-potential.onrender.com"
```
with your actual Render server URL (already configured!).

### Step 2: Push to GitHub

```bash
git add .
git commit -m "Add keep-alive system"
git push
```

### Step 3: Create Public Repository

1. Go to **https://github.com/new**
2. Repository name: `buy-sell-keep-alive` (or any name)
3. Visibility: **Public** ✅ (Important for free GitHub Actions!)
4. Push this `ping` folder content to that repository

### Step 4: Test Workflow

1. Go to repository **"Actions"** tab
2. Click **"Keep Buy & Sell Server Alive"** workflow
3. Click **"Run workflow"** → **"Run workflow"**
4. Wait ~30 seconds for results

**Expected output:**
```
⏰ Ping started at 2026-11-22 10:00:00 UTC
  Health check attempt 1 → Status: 200
✅ Server is alive and healthy!
  Keep-alive endpoint → Status: 200
✅ Ping completed at 10:00:15 UTC
```

## 🔧 Customization

### Change Ping Frequency

Edit `cron` schedule and the loop count in `keep-alive.yml`:

```yaml
# Hourly start, ping every 5 min (default)
- cron: '0 * * * *'      # in-job loop: 11 pings, sleep 265s

# Ping every 3 minutes (more aggressive)
# change: sleep 175 in the loop, and increase iterations to 18
```

### Ping Multiple Servers

Add more servers in the workflow:

```yaml
SERVERS=(
  "https://app1.onrender.com"
  "https://app2.onrender.com"
  "https://app3.onrender.com"
)

for SERVER in "${SERVERS[@]}"; do
  echo "Pinging $SERVER..."
  curl -s "$SERVER/api/health"
done
```

## 📈 Benefits

### Before Keep-Alive System:
- 😴 Server sleeps after 15+ minutes of inactivity
- ⏱️ First request takes 30-60 seconds to wake up
- 😡 Poor user experience

### After Keep-Alive System:
- ✅ Server always active and responsive
- ⚡ Instant response for all requests
- 😊 Smooth and fast user experience

## 💰 Cost

**সম্পূর্ণ Free! 💯**

- GitHub Actions: Free (public repository)
- Render: Free tier
- Database: Free tier
- Monitoring: Free

## 🚨 Troubleshooting

### ❌ Workflow not running?

**Check:**
- Repository is **Public** (not private)
- File path is exactly `.github/workflows/keep-alive.yml`
- YAML syntax is correct (no tabs, only spaces)

### ❌ Ping failing (Status: 000 or 500)?

**Check:**
- Render dashboard - server is running?
- SERVER URL is correct (starts with `https://`)
- Endpoints are accessible in browser
- Database connection is working

### ❌ Scheduled runs delayed?

✅ **Normal!** GitHub Actions cron scheduler can have 10-30 minute delays during high load. Manual runs work instantly.

## 📝 Last Updated

November 22, 2026

---

**Main Repository:** [buy_and_sell_website](https://github.com/badhonmondol/website-account-sell)
