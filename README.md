# Task-stock-intraday-calculation-API-code-
This project is a simple and powerful Intraday PnL (Profit &amp; Loss) Calculation API built using Flask, Pandas, and YFinance. It downloads 1-minute stock data from Yahoo Finance and calculates the PnL between selected entry and exit times for every trading day in the selected date range.

# Features

Fetch 1-minute intraday data using yfinance
Automatically detect nearest entry and exit candles
Calculate daily PnL and total PnL
Clean JSON output
Easy to integrate with:
  Trading dashboard
  Algorithms
  Backtesting tools
  Mobile or Web apps

# How Your Code Works (Simple Explanation)

Let's break your code in simple English.
🔹 1. Import Required Libraries
import yfinance as yf
import pandas as pd
from flask import Flask, request, jsonify
yfinance → downloads stock data
pandas → handles dataframe
flask → API framework

🔹 2. Create Flask App
app = Flask(__name__)
This starts your API server.

🔹 3. Create API Route
@app.route('/intraday_pnl', methods=['POST'])
This means:
✔ API link = /intraday_pnl
✔ Method = POST

🔹 4. Read JSON Input
data = request.get_json()
You get values from API body:
stock, start_date, end_date, entry_time, exit_time

🔹 5. Download 1-Minute Data
df = yf.download(stock, start=start_date, end=end_date, interval="1m")
This downloads 1-minute candles from Yahoo Finance.
Columns received:
Datetime | Open | High | Low | Close | Volume

🔹 6. Add Date + Time Columns
df["Date"] = df["Datetime"].dt.date
df["Time"] = df["Datetime"].dt.strftime("%H:%M")
This makes it easy to filter each minute.

🔹 7. Loop Through Each Day
unique_days = df["Date"].unique()
For each day:
✔ Find entry candle
entry_time >= selected time
✔ Find exit candle
exit_time <= selected time

🔹 8. Calculate PNL
pnl = exit_price - entry_price
Daily PNL stored in list.

🔹 9. Calculate Total PNL
total_pnl = sum([x["PnL"] for x in daily_pnl])

🔹 10. Send Response
return jsonify({
    "Daily_PnL": daily_pnl,
    "Total_PnL": total_pnl
})
API returns JSON output.

# How to Run This Flask API (Step-by-Step)
Follow these steps exactly.

🟩 STEP 1 — Install Python Libraries
Open Terminal / CMD:
pip install flask yfinance pandas

🟩 STEP 2 — Save Your File
Create a file:
app.py
Paste your code inside.

🟩 STEP 3 — Run the API
Open terminal inside your project folder:
python app.py
If it runs successfully, you will see:
 * Running on http://127.0.0.1:5000
This means your API is live.

🟩 STEP 4 — Test API in Postman (or VS Code API Client)
⚡ Request Type
POST

⚡ URL
http://127.0.0.1:5000/intraday_pnl

⚡ Body → JSON
{
  "stock": "SBIN.NS",
  "start_date": "2025-11-14",
  "end_date": "2025-11-19",
  "entry_time": "09:30",
  "exit_time": "15:00"
}

🟩 STEP 5 — You Will Get Output Like
{
  "Daily_PnL": [
    {
      "Date": "2025-11-14",
      "Entry Price": 742.5,
      "Exit Price": 751.2,
      "PnL": 8.7
    }
  ],
  "Total_PnL": 8.7
}
