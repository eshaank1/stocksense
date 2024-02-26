---
toc: true
comments: false
layout: post
title: StockSense-Data
type: hacks
permalink: /stocksensedata
---


<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Stock Data Fetcher</title>
    <style>
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            margin: 0;
            padding: 20px;
            background-color: #e8f0fe; /* Light blue background */
        }
        h1 {
            color: #005a87; /* Darker blue for the main heading */
        }
        h2 {
            color: #007bff; /* Bright blue for subheadings */
        }
        input[type="text"], button {
            padding: 10px;
            margin: 10px 0;
            border-radius: 5px;
            border: 1px solid #ccc;
            box-shadow: 0px 0px 5px #aaa; /* Subtle shadow for depth */
        }
        input[type="text"] {
            width: calc(100% - 22px); /* Adjust width to ensure it is responsive */
            box-sizing: border-box; /* Ensures padding does not affect width */
        }
        button {
            background-color: #28a745; /* Green background for the button */
            color: white;
            cursor: pointer;
            transition: background-color 0.3s ease; /* Smooth transition for hover effect */
            animation: pulse 1s infinite; /* Apply animation */
        }
        button:hover {
            background-color: #218838; /* Darker green on hover */
            animation: none; /* Disable animation on hover */
        }
        @keyframes pulse {
            0% {
                transform: scale(1);
            }
            50% {
                transform: scale(1.1);
            }
            100% {
                transform: scale(1);
            }
        }
        #stockData {
            background-color: #ffffff;
            padding: 20px;
            margin-top: 20px;
            border-radius: 5px;
            box-shadow: 0 0 10px #cccccc; /* More pronounced shadow for the result box */
        }
        #stockData p {
            color: black; /* Set text color to black */
        }
        p {
            color: #6c757d; /* Dark gray for text to improve readability */
        }
    </style>
</head>
<body>
    <p>Open: Refers to the price of a stock at the beginning of a trading session. It represents the first transaction or trade of the day for a particular stock.</p>
    <p>High: Represents the highest price at which a stock was traded during a specific period, typically within a trading day.</p>
    <p>Low: Indicates the lowest price at which a stock was traded during a specific period, usually within a trading day.</p>
    <p>Close: Refers to the final price at which a stock was traded at the end of a trading session. It represents the last transaction or trade of the day for a particular stock.</p>
    <p>Volume: Represents the total number of shares of a particular stock that were traded during a specific period, such as a trading day. It indicates the level of activity or liquidity in the market for that stock.</p>
    <h1>Stock Data Fetcher - Anagha</h1>
    <input type="text" id="stockSymbol" placeholder="Enter Stock Symbol (e.g., AAPL)">
    <button onclick="fetchStockData()">Fetch Stock Data</button>

    <div id="stockData"></div>

    <script>
        async function fetchStockData() {
            const symbol = document.getElementById('stockSymbol').value;
            console.log(`Fetching data for symbol: ${symbol}`);
            
            const response = await fetch(`http://localhost:8055/api/stockchart/chart/${symbol}`);

            if (response.ok) {
                const data = await response.json();
                displayStockData(data);
            } else {
                console.error('Failed to fetch stock data.');
                document.getElementById('stockData').innerHTML = 'Failed to fetch stock data.';
            }
        }

        function displayStockData(data) {
            if (!data['Time Series (Daily)']) {
                document.getElementById('stockData').innerHTML = 'No data available for this symbol.';
                return;
            }

            const timeSeries = data['Time Series (Daily)'];
            const latestDate = Object.keys(timeSeries)[0];
            const latestData = timeSeries[latestDate];

            const content = `
                <p>Date: ${latestDate}</p>
                <p>Open: ${latestData['1. open']}</p>
                <p>High: ${latestData['2. high']}</p>
                <p>Low: ${latestData['3. low']}</p>
                <p>Close: ${latestData['4. close']}</p>
                <p>Volume: ${latestData['5. volume']}</p>
            `;

            document.getElementById('stockData').innerHTML = content;
        }
    </script>
</body>
</html>

<style> 
@import url('https://fonts.googleapis.com/css2?family=Roboto');
h1{ text-align: center; font-size: 50px; color: #0352fc; font-family: 'Roboto', serif;}
h2{ text-align: center; font-size: 25px; color: #0352fc;}
body{ text-align: center; font-size: 15px; font-family: 'Roboto', serif; background: black; }
</style>