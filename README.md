# Automated Trading Bot on Raspberry Pi 

> [!IMPORTANT]
> Scroll down to find Full explanation of change logs made. (2023,2024,2025)

>[!NOTE]
>For intermediate Python programmers. Users must read this whole Document.
------------------------------------------------------------------------------------------

**ABOUT THIS PROJECT -**

This bot is developed and deployed based on a algorithmic trading bot that runs 24/7 on a Raspberry Pi. The bot implements a simple moving average crossover strategy to trade stocks and cryptocurrencies. The project involved data fetching, strategy implementation, backtesting, and automation. Once you are able to understand this you can expand on this by adding more advanced strategies, risk management techniques, and real-time data analysis.

<img src="/images/header.jpg" alt="Picture of USB 2.0 card reader" width="640">

------------------------------------------------------------------------------------------

**ABOUT RASPBERRY PI -**

Unlike any Arduino models, you dont have to connect raspberry pie to a computer/laptop, because technically raspberry pie is a computer in itself. It has multiple pins You can connect your mouse, keyboard and monitor to the raspberry pi directly, it uses a quad kit micro processor, it has upto 8 gb of ram, built in wifi and bluetooth, 4 usb ports, Micro SD card and much more. We can also run our trading bot on a cloud server, where you buy a cloud service and upload and run your code on a cloud server for 24/7, Cloud servers on one hand can be costly becuase you'll have to purchase them every month while Raspberry pie could be a one time investment. Think of it as a very small computer that can easily run our python program 24/7.

![raspberry-pi-4](/images/raspberry-pi.png)

------------------------------------------------------------------------------------------

>[!WARNING]
>I highly recommend that you test your bot on paper trading(Fake Money).

------------------------------------------------------------------------------------------
**KEY FEATURES -**

- Automated Trading: The bot can execute trades based on predefined strategies without manual intervention. 
- Backtesting: Historical data is used to test and optimize the trading strategy. 
- Real-time Data: Integration with APIs to fetch real-time market data. 
- 24/7 Operation: Hosted on a Raspberry Pi to ensure continuous operation.

------------------------------------------------------------------------------------------
> [!IMPORTANT]
>TOOLS AND TECHNOLGIES USED -:

- Programming Language: Python.
- Python Libraries: Pandas, NumPy, Matplotlib, yfinance, ta, ccxt, alpaca-trade-api.
- Hardware: Raspberry Pi 4, Mini sd card 16gb + (suggested), USB 2.0 card reader.

  ![Picture of Raspberry Pi 4](/images/raspberry-pi.png)

<img src="/images/raspberry-pi-cardreader-1.jpg" alt="Picture of USB 2.0 card reader" width="300" height="200">

<img src="/images/sandisk-32-gb-card.jpg" alt="Picture of Mini sd card" width="300" height="200">

- Software: Raspbian OS, SSH, cron jobs. 
- APIs: Yahoo Finance (yfinance) for stock data, CCXT for cryptocurrency data or (If you want to trade in forex, ill suggest ForexConnect API- Provided by OANDA), Alpaca API for stock trading.

  [yfinance Documentation](https://pypi.org/project/yfinance/)

  [CCXT Documentation](https://docs.ccxt.com/#/)

------------------------------------------------------------------------------------------

**DETAILED EXPLANATION**

1. Setup and Configuration: Installed Raspbian OS on a Raspberry Pi and set up the necessary environment. Enabled SSH for remote access and installed required Python libraries.

- This is how Raspberry Pi OS looks like

![Image os Raspberry Pi OS](/images/raspberry-pi-site.png){ width=600pt }

- [Quick tutorial on Raspbian Setup](https://www.youtube.com/watch?v=CQtliTJ41ZE)
   
- [Detailed Documentation on Raspbian Setup by Raspberry pie](https://www.raspberrypi.com/documentation/computers/getting-started.html)


1. Data Fetching: Utilized yfinance to download historical stock data. Used ccxt to fetch real-time cryptocurrency data from various exchanges.
2. Strategy Implementation: Implemented a simple moving average crossover strategy. Calculated short-term (50-day) and long-term (200-day) moving averages. Generated buy/sell signals based on the crossover points.
3. Backtesting: Developed a backtesting framework to evaluate the strategy against historical data. Simulated trades and calculated final balance to assess performance. Automation:
4. Created a shell script to start the bot and configured it to run at system boot using cron jobs. Ensured 24/7 operation by hosting the bot on the Raspberry Pi. Logging and Monitoring:
5. Implemented logging to record all trading actions and decisions. Set up log monitoring to track the bot’s performance and actions in real-time.

------------------------------------------------------------------------------------------
**SETUP BELOW**
------------------------------------------------------------------------------------------

**Step 1 (Get the Hardware Ready)**

Step 1.1 Setting up Raspberry Pi (Get the Hardware Ready)
- Raspberry Pi (preferably model 4)
- MicroSD card (at least 16GB)
- Power supply
- Monitor, keyboard, and mouse (for initial setup)
- Ethernet cable (Preffered) or Wi-Fi connection

Step 1.2. Install Raspbian OS
- Download the Raspbian OS image from the

- [official Raspberry Pi website](https://www.raspberrypi.com/software/operating-systems/)
  
- [Detailed Tutorial on Raspbian Setup](https://www.youtube.com/watch?v=wvxCNQ5AYPg)
  
- Use a tool like [Etcher](https://etcher.download/) to write the image to your microSD card.
- Insert the microSD card into the Raspberry Pi and connect the power supply, monitor, keyboard, and mouse.
- Follow the on-screen instructions to complete the setup.

Step 1.3. Update Your System - Open a terminal on your Raspberry Pi (you can find it in the main menu) and run this command
```
sudo apt-get update
sudo apt-get upgrade
```

Step 1.4. Enable SSH for Remote Access (run the following command in the terminal)
```
sudo raspi-config
```

Step 1.5. Find Your Raspberry Pi’s IP Address (in terminal paste this code and press enter, once you get the ip adress note it somehwere)
```
ifconfig
```
------------------------------------------------------------------------------------------

**## Step 2 (Setting Up Your Development Environment)**

Step 2.1 SSH into Your Raspberry Pi from Your Main Computer
- Open a terminal (Mac/Linux) or cmd (w admin permissions)(Windows).
- Connect to your Raspberry Pi using its IP address
``` 
ssh pi@<Raspberry_Pi_IP_Address>
```
You may be asked a password, bydeafult is 'raspberry'.

Step 2.2 Install Python and its Libraries
- In the SSH terminal paste these commands
```
sudo apt-get install python3 python3-pip
pip3 install pandas numpy matplotlib yfinance ta ccxt alpaca-trade-api
```
------------------------------------------------------------------------------------------

**Step 3 Coding our Trading Bot**

Step 3.1 Create a Directory(Folder) for Your Trading Bot
- In terminal run this command to make a new directory and open it  
```
mkdir trading-bot
cd trading-bot
```

Step 3.2 Create a Python File for Your Bot
- In termianl run
```
nano bot.py # replace 'bot' with any name you want, Nano is not a name, Nano is a text editor.
```
Then

1. Import Libraries
```python
import yfinance as yf
import pandas as pd
import numpy as np
import logging

#Update (Change logs - 2023) If you want (recommeded) to trade of paper trading(fake money) to test out this bot, I recommend using Alpaca Trading brokers, Its easy to setup a paper trading account there.
#For alpaca you'll need to import following libraries, If you wish to use any another provider make sure to research on how their apis can be implemented in the code

import alpaca_trade_api as tradeapi
from datetime import datetime

```
Explanation - 
yfinance: A Python library to download historical market data from Yahoo Finance. 
pandas: Used for data manipulation and analysis.
numpy: A library for numerical operations. It is used here to perform element-wise conditions.
logging: Used to log messages (like trades made during the whole day) to a file for future tracking.

------------------------------------------------------------------------------------------ 


2. Fetch Historical Data
```python
data = yf.download("AAPL", start="2022-01-01", end="2022-12-31")
```
Explanation - This line fetches historical stock price data for Apple Inc. (AAPL) from January 1, 2022, to December 31, 2022.


>Just for fun if you want to see how yfinance work and stores data for a given time, refer to the image below. (Use the given code to fetch data of any stocks,crypto you like)


![image](/images/historical_data.png)


------------------------------------------------------------------------------------------  

3. Defining the Trading Strategy

```python
def moving_average_strategy(data):
    data['SMA_50'] = data['Close'].rolling(window=50).mean()
    data['SMA_200'] = data['Close'].rolling(window=200).mean()
    data['Signal'] = 0
    data['Signal'][50:] = np.where(data['SMA_50'][50:] > data['SMA_200'][50:], 1, 0)
    data['Position'] = data['Signal'].diff()
    return data

```
Explanation - 
- Simple Moving Averages - The code calculates two moving averages from the closing prices—SMA over 50 days and 200 days. 

- [(Read Documentation on SMA by geeksforgeeks)](https://www.geeksforgeeks.org/how-to-calculate-moving-averages-in-python/) 

- [(Read Documentation on SMA by Gregorycernera Medium)](https://gregorycernera.medium.com/computing-simple-moving-average-sma-with-python-pandas-yfinance-0458bb0b5d3b)

- Signal: This is a binary indicator (1 or 0). A '1' signal is generated when the 50-day SMA crosses above the 200-day SMA.
- Position: Indicates a change in the signal (buy or sell). If the signal changes from 0 to 1, it suggests buying. If from 1 to 0, it suggests selling.

>[!NOTE]
> You must be connected to [Alpaca trader broker](https://alpaca.markets/) ( For paper trading) Please add your API key and Security key in the code I have provided, Otherwise oders wont be placed and a error will come like this- 

![image](/images/trading_bot_log.png)


------------------------------------------------------------------------------------------  

4. Backtesting the Strategy

```python
def backtest(data, initial_balance=10000):
    balance = initial_balance
    position = 0
    for i in range(len(data)):
        if data['Position'][i] == 1:
            position = balance / data['Close'][i]
            balance = 0
        elif data['Position'][i] == -1:
            balance = position * data['Close'][i]
            position = 0
    return balance
```
Explanation - This function simulates trading based on the signals. It starts with an initial balance (e.g., 10,000 INR ). It buys stocks when the signal is to buy (position=1) and sells all when the signal is to sell (position=-1). The final balance after processing all signals is returned.

------------------------------------------------------------------------------------------  

5. Calculate Final Balance
```python
final_balance = backtest(data)
print(f"Final Balance: ${final_balance:.2f}")
```
Explanation - Executes the backtest and prints out the final balance after following the trading strategy. (How much did we gain, or loose will be adjusted in our final balance which will be rounded of to two 2 float values (upto 2 decimals)

>Visual representation of what Output comes when this process executes-

![image](/images/final_balance.png)


------------------------------------------------------------------------------------------  

6. Setup Logging
```python
logging.basicConfig(filename='/home/pi/trading-bot/trading_bot.log', level=logging.INFO)
```
Explanation - Configures the logging system to write informational messages into a file named trading_bot.log. (Example of how logs get saved every time the process executes)

![image](/images/trading_bot_log-1.png)


------------------------------------------------------------------------------------------ 

- Remember to press enter to save this file.
- You may exit Nano editor now.

------------------------------------------------------------------------------------------ 

**Step 4 Automating and Running our Bot 24/7**

Step 4.1 Create a Shell Script to Start Your Bot (in terminal run)
```
nano start_bot.sh
```
- Copy and paste the following content into start_bot.sh
```
#!/bin/bash
cd /home/pi/trading-bot
python3 bot.py
```
- save and exit.
- Now run this command to make our script executable

```
chmod +x start_bot.sh
```

Step 4.2 Set Up a Cron Job to Run our Bot at Boot
- In the SSH terminal, run
```
crontab -e
```
- Add the following line at the end of the file to run the script at reboot
```
@reboot /home/pi/trading-bot/start_bot.sh
```
- save and exit

Step 4.3 Reboot Your Raspberry Pi
- In the SSH terminal, run
```
sudo reboot
```
------------------------------------------------------------------------------------------  

## Step 5 Monitor the bot's activity
- After rebooting, SSH back into your Raspberry Pi
- Check the logs to ensure your bot is running
```
tail -f /home/pi/trading-bot/trading_bot.log (Edit the command!! Make sure to add file name of your bot)
```

------------------------------------------------------------------------------------------


# 2023-2025 CHANGE LOGS BELOW


------------------------------------------------------------------------------------------  

> [!IMPORTANT]
> CHANGELOGS - Added some New features in my bot to make it more strategically strong and added more functions in it such as

Usefull Documents I highly suggest you to read

>[A small yfinance Documentation, Credits: Medium](https://medium.com/@kasperjuunge/yfinance-10-ways-to-get-stock-data-with-python-6677f49e8282)

>[Detailed Documentation on RSI, Crredits: Medium](https://rbdundas.medium.com/calculate-relative-strength-index-rsi-and-chart-with-candles-using-python-pandas-and-matplotlib-f58d926249ac)

>[If you want to recap RSI, while learning more, Credits: EODHD API's](https://eodhd.com/financial-academy/backtesting-strategies-examples/algorithmic-trading-with-relative-strength-index-in-python)

------------------------------------------------------------------------------------------


**Additions (2023 version)**
- EMA Calculation [(Read Documentation on EMA Calculation)](https://www.geeksforgeeks.org/how-to-calculate-an-exponential-moving-average-in-python/) which is more responsive than SMA, it reacts faster to price changes.
- RSI Calculation [(Read Documentation on RSI Calculation)](https://stackoverflow.com/questions/73115286/python-while-trying-to-calculate-rsirelative-strength-index-stock-indicator) RSI helps to identify potentially overbought or oversold conditions currently in the market. 
- Signal Generation: Combines EMA crossover and RSI thresholds for buy/sell signals.
- Risk Management: Added Stop-loss and take-profit mechanisms to protect and secure gains immediately.
- Enhanced Logging: Detailed logging includes timestamps, action types, and prices for better traceability.

 ------------------------------------------------------------------------------------------ 

**Implementation

1. Calculate Indicators

```python
def calculate_indicators(data):
    # Exponential Moving Averages
    data['EMA_12'] = data['Close'].ewm(span=12, adjust=False).mean()
    data['EMA_26'] = data['Close'].ewm(span=26, adjust=False).mean()
    # Relative Strength Index
    delta = data['Close'].diff()
    gain = (delta.where(delta > 0, 0)).rolling(window=14).mean()
    loss = (-delta.where(delta < 0, 0)).rolling(window=14).mean()
    rs = gain / loss
    data['RSI'] = 100 - (100 / (1 + rs))
    return data
```

Detailed explanation- 

EMA_12 and EMA_26 - These are exponential moving averages calculated over 12 and 26 days, respectively. EMA reacts more quickly to price changes than SMA due to its focus on recent prices. RSI also known as The Relative Strength Index measures the magnitude of recent price changes to evaluate overbought or oversold conditions in the price of a stock. Here, it's calculated over a 14-day period. Values below 30 indicate oversold conditions (potentially undervalued), and above 70 suggest overbought conditions (potentially overvalued).

------------------------------------------------------------------------------------------

2. Generate Signals
   
```python
def generate_signals(data):
    data['Signal'] = 0
    buy_signal = (data['EMA_12'] > data['EMA_26']) & (data['RSI'] < 30)
    sell_signal = (data['EMA_12'] < data['EMA_26']) & (data['RSI'] > 70)
    data.loc[buy_signal, 'Signal'] = 1
    data.loc[sell_signal, 'Signal'] = -1
    data['Position'] = data['Signal'].replace(to_replace=0, method='ffill')
    return data
```

Detailed explanation-

Buy and Sell Signals are determined by the crossing of EMAs and the RSI levels, Buy Signal are generated when the short-term EMA (12 days) crosses above the long-term EMA (26 days) and the RSI is below 30. Sell Signal are generated when the short-term EMA falls below the long-term EMA and the RSI exceeds 70. This holds the current position, either holding (1 for buy, -1 for sell) or neutral (0), and carries forward the last non-zero signal until a new signal changes it.

------------------------------------------------------------------------------------------


3. Backtest with Risk Management


```python
def backtest(data, initial_balance=10000):
    balance = initial_balance
    position = 0
    stop_loss = 0.95
    take_profit = 1.10
    entry_price = 0

    for i, row in data.iterrows():
        # Buy or sell logic based on signals
        # Stop-loss and take-profit execution
        ...
    return balance
```

Detailed explanation-

Loop Through Data Iterates over each row (each day's trading data) to execute trading decisions based on the previously generated signals. Stop-loss and Take-profit are set as a percentage of the entry price. Stop-loss is at 95% of entry price, If the price drops to this level, the position is sold to limit further losses. Take-profit at 110% of entry price, If the price reaches this level, the position is sold to lock in profits.

Logging: Logs every buy, sell, stop loss, and take profit event with detailed information.

------------------------------------------------------------------------------------------

4. Logging Configuration
   
```python
logging.basicConfig(filename='trading_bot.log', level=logging.INFO, format='%(asctime)s - %(levelname)s - %(message)s')
```

Detailed explanation- 

This line of code configures Python's logging module to write logs to a file trading_bot.log, including timestamps, logging levels, and messages, providing a detailed record of all trading activities and decisions.

------------------------------------------------------------------------------------------

>[!NOTE]
>Make sure to download the new (Updated_bot(2023) and paste that code and run it, I have made few changes in it that i have thoroughly discussed above. There is alot of room for improvement still, and as I keep learning new concepts new calculation strategies, Ill definetly make sure to update this Documentation again.

------------------------------------------------------------------------------------------
> Overall Strategy followed

>  Strategy Development:

1. **Strategy Development:**
   - **EMA** (Exponential Moving Average)  
     or  
   - **RSI** (Relative Strength Index)

2. **Data Collection** (Yahoo Finance)

3. **Data Preprocessing**
   - Calculate **EMA**.

4. **Python Part to Implement Signal**  
   - When to buy or sell.

5. **Back-testing**  
   - Python part using **backtrader**  
   - Analytic part of trade / bot and:

6. **Deploy and Monitor.**

---
### Additional Notes:
- **MACD** (Moving Average Convergence Divergence)

------------------------------------------------------------------------------------------

>[!CAUTION]
> Make sure you trade only with "paper trading" (Fake Money) when evaluating åerformance. Using real funds to trade with this bot, is at your own risk.
