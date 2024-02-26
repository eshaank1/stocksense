---
toc: false
comments: false
layout: post
title: CryptoSense
type: hacks
permalink: cryptosense
---

<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Cryptocurrency Data Fetcher</title>
    <style> 

</style>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Roboto');
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            margin: 0;
            padding: 20px;
            background-color: #f5f5f5; /* Light gray background */
        }
        h1{ text-align: center; font-size: 50px; color: #0352fc; font-family: 'Roboto', serif;}
        h2{ text-align: center; font-size: 25px; color: #0352fc;}
        p{ text-align: center; font-size: 15px; font-family: 'Roboto', serif; background: black; }
        input[type="text"], button {
            padding: 10px;
            margin: 10px 0;
            border-radius: 5px;
            border: 7px solid #0352fc;
            box-shadow: 0px 0px 5px #aaa; /* Subtle shadow for depth */
        }
        input[type="text"] {
            width: calc(100% - 22px); /* Adjust width to ensure it is responsive */
            box-sizing: border-box; /* Ensures padding does not affect width */
        }
        button {
            background-color: #000000; /* Black background for the button */
            color: white;
            cursor: pointer;
        }
        button:hover {
            background-color: #0056b3; /* Blue on hover */
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
            border: 7px solid #0352fc;
            box-shadow: 0 0 10px #cccccc; /* More pronounced shadow for the result box */
        }
        #cryptoData p {
            color: #333; /* Dark gray for text */
            margin: 5px 0;
        }
    </style>
</head>
<body>
    <h2>Info</h2>
    <p>Open: Price of a crypto at the beginning of a trading session (usually the first transaction of the day)</p>
    <p>High: Highest price the crypto was valued/traded at during the trading session (usually the day)</p>
    <p>Low: Lowest price the crypto was valued/traded at during the trading session (usually the day)</p>
    <p>Close: Price of a crypto at the end of a trading session (usually the last transaction of the day)</p>
    <p>Volume: Total number of units of a particular crypto traded within a trading session (usually the day)</p>
    <h2>Cryptocurrency Data Fetcher by Eshaan</h2>
    <h3>Enter a Cryptocurrency Symbol (XRP, BTC, ...) below:</h3>
    <input type="text" id="cryptoSymbol" placeholder=" ">
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

<style> 
@import url('https://fonts.googleapis.com/css2?family=Roboto');
h1{ text-align: center; font-size: 50px; color: #0352fc; font-family: 'Roboto', serif;}
h2{ text-align: center; font-size: 25px; color: #0352fc;}
body{ text-align: center; font-size: 15px; font-family: 'Roboto', serif; background: black; }
</style>