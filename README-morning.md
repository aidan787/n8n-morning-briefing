# n8n Morning Briefing

An n8n workflow that emails you a good morning message every day with an inspirational quote and the current weather for your city.

## How it works

```
Schedule Trigger -> Get_Quote -> Get_Weather -> Send a message
(7:30 AM daily)    (ZenQuotes)  (OpenWeatherMap)  (Gmail)
```

1. **Schedule Trigger** starts the workflow every day at 7:30 AM.
2. **Get_Quote** fetches a random quote from the ZenQuotes API.
3. **Get_Weather** fetches the current temperature (in Fahrenheit) and conditions for your city from the OpenWeatherMap API.
4. **Send a message** emails you a "Good Morning!" message that combines the quote and the weather.

## What you need

- [n8n](https://docs.n8n.io/hosting/) running (this project uses a self-hosted n8n in Docker)
- A free [OpenWeatherMap API key](https://openweathermap.org/api)
- A Google account (for Gmail)

## Setup

### 1. Import the workflow

1. Open n8n (usually `http://localhost:5678`) and create a new workflow.
2. Click the three dots menu, then **Import from File**.
3. Choose `workflows/morning-briefing-workflow.json`.

The workflow file has no keys, logins, or personal info, so you add your own next.

### 2. Add your own details

- **Get_Weather:** in the URL, replace `YOUR_OPENWEATHERMAP_API_KEY` with your key and `YOUR_CITY` with your city (for example `q=Boston,US`). New OpenWeatherMap keys can take a short while to activate.
- **Send a message:** connect your own Gmail credential (Google OAuth) and replace `YOUR_EMAIL_HERE` with the address that should get the email. Also replace `YOUR_CITY` in the message text.

n8n's docs walk through creating Google credentials: [Google credentials](https://docs.n8n.io/integrations/builtin/credentials/google/).

### 3. Run it

1. Click **Execute workflow** to test. You should get the email within a few seconds.
2. Change the time in the **Schedule Trigger** if you don't want 7:30 AM, then publish the workflow so it runs daily.

## Troubleshooting

- **401 error on Get_Weather:** the API key is wrong or hasn't activated yet. Wait a bit and try again.
- **City not found:** use the format `City,US` (or the right country code) in the URL.
- **Gmail node shows a warning:** connect your Google credential (step 2).
- **No email at 7:30:** make sure the workflow is published, and that your n8n container is running at that time. If Docker is stopped, nothing runs.

## Notes

- Quotes come from [ZenQuotes.io](https://zenquotes.io). The free API asks for attribution, so credit them if you display the quotes publicly.
- Never commit your real API key. Keep it only inside your own n8n.
