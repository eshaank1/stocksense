---
toc: true
comments: false
layout: post
title: CryptoSense
type: hacks
---

<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Cryptocurrency Data Fetcher</title>
    <style>
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            margin: 0;
            padding: 20px;
            background-color: #f5f5f5; /* Light gray background */
        }
        h1, h2 {
            color: #005a87; /* Dark blue for headings */
            margin-bottom: 10px;
        }
        p {
            color: #444; /* Dark gray for paragraphs */
            line-height: 1.5;
            margin-bottom: 15px;
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
            background-color: #007bff; /* Blue background for the button */
            color: white;
            cursor: pointer;
            transition: background-color 0.3s ease; /* Smooth transition for hover effect */
            animation: pulse 1s infinite; /* Apply animation */
            border: none; /* Remove border */
        }
        button:hover {
            background-color: #0056b3; /* Darker blue on hover */
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
        #cryptoData {
            background-color: #ffffff;
            padding: 20px;
            margin-top: 20px;
            border-radius: 5px;
            box-shadow: 0 0 10px #cccccc; /* More pronounced shadow for the result box */
        }
        #cryptoData p {
            color: #333; /* Dark gray for text */
            margin: 5px 0;
        }
    </style>
</head>
<body>
    <p>Open: Refers to the price of a cryptocurrency at the beginning of a trading session. It represents the first transaction or trade of the day for a particular cryptocurrency.</p>
    <p>High: Represents the highest price at which a cryptocurrency was traded during a specific period, typically within a trading day.</p>
    <p>Low: Indicates the lowest price at which a cryptocurrency was traded during a specific period, usually within a trading day.</p>
    <p>Close: Refers to the final price at which a cryptocurrency was traded at the end of a trading session. It represents the last transaction or trade of the day for a particular cryptocurrency.</p>
    <p>Volume: Represents the total number of units of a particular cryptocurrency that were traded during a specific period, such as a trading day. It indicates the level of activity or liquidity in the market for that cryptocurrency.</p>
    <h1>Cryptocurrency Data Fetcher - Eshaan</h1>
    <input type="text" id="cryptoSymbol" placeholder="Enter Cryptocurrency Symbol (XRP, BTC, ...)">
    <button onclick="fetchCryptoData()">Fetch Crypto Data</button>

    <div id="cryptoData"></div>

    <script>
        async function fetchCryptoData() {
            const symbol = document.getElementById('cryptoSymbol').value;
            console.log(`Fetching data for symbol: ${symbol}`);
            
            const response = await fetch(`https://www.alphavantage.co/query?function=DIGITAL_CURRENCY_DAILY&symbol=${symbol}&market=USD&apikey=NN5Z6YJMAC2LMUNP`);

            if (response.ok) {
                const data = await response.json();
                displayCryptoData(data);
            } else {
                console.error('Failed to fetch cryptocurrency data.');
                document.getElementById('cryptoData').innerHTML = 'Failed to fetch cryptocurrency data.';
            }
        }

        function displayCryptoData(data) {
            if (!data['Time Series (Digital Currency Daily)']) {
                document.getElementById('cryptoData').innerHTML = 'No data available for this symbol.';
                return;
            }

            const timeSeries = data['Time Series (Digital Currency Daily)'];
            const latestDate = Object.keys(timeSeries)[0];
            const latestData = timeSeries[latestDate];

            const content = `
                <p>Date: ${latestDate}</p>
                <p>Open: ${latestData['1a. open (USD)']}</p>
                <p>High: ${latestData['2a. high (USD)']}</p>
                <p>Low: ${latestData['3a. low (USD)']}</p>
                <p>Close: ${latestData['4a. close (USD)']}</p>
                <p>Volume: ${latestData['5. volume']}</p>
            `;

            document.getElementById('cryptoData').innerHTML = content;
        }
    </script>
</body>
</html>
