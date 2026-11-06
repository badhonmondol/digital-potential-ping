# digital-potential-ping

A GitHub Actions workflow that pings the [Digital Potential](https://digital-potential.onrender.com) server every 14 minutes to prevent it from sleeping on Render's free tier.

## How it works

The workflow runs on a schedule using `cron` and sends an HTTP GET request to the server's health endpoint. This keeps the Render free-tier server awake 24/7.

## Ping URL

`https://digital-potential.onrender.com/`
