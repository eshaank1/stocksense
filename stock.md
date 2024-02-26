---
toc: true
comments: false
layout: post
title: StockSense
type: hacks
permalink: /stocksense
---

<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Stock Manager</title>
    <style>
    body {
        font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        margin: 40px;
        background-color: #eceff1; /* Softer overall background */
        color: #333;
    }
    h2 {
        color: #546e7a; /* Warmer heading color */
    }
    form {
        background-color: #000000; /* Warmer form background */
        padding: 20px;
        border-radius: 8px;
        box-shadow: 0 2px 4px rgba(0,0,0,0.2); /* Softer shadow for depth */
        margin-bottom: 20px;
    }
    table {
        width: 100%;
        border-collapse: collapse;
        background-color: #b0bec5; /* Warmer table background */
        box-shadow: 0 2px 4px rgba(0,0,0,0.2); /* Consistent shadow with form */
    }
    th, td {
        text-align: left;
        padding: 12px;
        border-bottom: 1px solid #78909c; /* Warmer border color */
    }
    th {
        background-color: #546e7a; /* Warm header background */
        color: #ffffff;
    }
    tr:hover {
        background-color: #cfd8dc; /* Softer hover effect */
    }
    input[type="text"], input[type="number"] {
        width: calc(100% - 22px);
        padding: 10px;
        margin-bottom: 10px;
        border: 1px solid #78909c; /* Harmonized border color */
        border-radius: 4px;
        background-color: #eceff1; /* Input background matching body */
    }
    button {
        background-color: #546e7a; /* Harmonized button color */
        color: white;
        padding: 10px 15px;
        border: none;
        border-radius: 4px;
        cursor: pointer;
        transition: background-color 0.3s;
    }
    button:hover {
        background-color: #455a64; /* Darker on hover for interactivity */
    }
    .profit, .loss {
        font-weight: bold; /* Make profit/loss more prominent */
    }
    #totalInvestment, #overallBalance {
        font-size: 18px;
        margin-top: 20px;
        padding: 10px;
        border-radius: 8px;
        box-shadow: 0 2px 4px rgba(0,0,0,0.2);
        display: inline-block;
        background-color: #000000; /* Background for balance indicators */
    }
    .profit {
        color: #28a745; /* Keeping profit color for clear positive indication */
    }
    .loss {
        color: #dc3545; /* Keeping loss color for clear negative indication */
    }
    /* Keeping dynamic coloring functionality for #overallBalance */
    </style>


</head>
<body>
    <h2>Financial Log - Miheer</h2>
    <form id="addStockForm">
        <input type="text" id="companyName" placeholder="Company Name" required />
        <input type="number" id="shares" placeholder="Shares" required />
        <input type="number" step="0.01" id="purchasePrice" placeholder="Purchase Price" required />
        <input type="number" step="0.01" id="currentPrice" placeholder="Current Price" required />
        <button type="submit">Log Purchase</button>
    </form>

    <h2>Stocks</h2>
    <table id="stocksTable">
        <thead>
            <tr>
                <th>Company Name</th>
                <th>Shares</th>
                <th>Purchase Price</th>
                <th>Current Price</th>
                <th>Profit/Loss</th>
            </tr>
        </thead>
        <tbody>
            <!-- Stocks will be added here -->
        </tbody>
    </table>

    <!-- Add a section for overall balance -->
    <div id="overallBalance">Overall Balance: $0.00</div>

    <script>
    document.getElementById('addStockForm').onsubmit = async function(e) {
        e.preventDefault();
        const companyName = document.getElementById('companyName').value;
        const shares = document.getElementById('shares').value;
        const purchasePrice = document.getElementById('purchasePrice').value;
        const currentPrice = document.getElementById('currentPrice').value;

        try {
            const response = await fetch('http://127.0.0.1:8055/api/stock/stocks', {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json'
                },
                body: JSON.stringify({
                    company_name: companyName,
                    shares: parseInt(shares, 10),
                    purchase_price: parseFloat(purchasePrice),
                    current_price: parseFloat(currentPrice)
                })
            });

            if (response.ok) {
                alert('Stock added successfully!');
                fetchStocks(); // Refresh the stock list
            } else {
                alert('Failed to add stock. Please check if your backend is running and CORS is configured.');
            }
        } catch (error) {
            console.error('Error adding stock:', error);
            alert('Error communicating with the backend. Make sure your server is running and accessible.');
        }
    };

    async function fetchStocks() {
        let overallBalance = 0;

        try {
            const response = await fetch('http://127.0.0.1:8055/api/stock/stocks');
            const stocks = await response.json();
            const stocksTableBody = document.getElementById('stocksTable').getElementsByTagName('tbody')[0];
            stocksTableBody.innerHTML = ''; // Clear existing stocks
            stocks.forEach(stock => {
                const row = stocksTableBody.insertRow();
                const profitLoss = (stock.current_price - stock.purchase_price) * stock.shares;
                overallBalance += profitLoss; // Update overall balance

                const profitLossClass = profitLoss >= 0 ? 'profit' : 'loss';

                row.insertCell(0).textContent = stock.company_name;
                row.insertCell(1).textContent = stock.shares;
                row.insertCell(2).textContent = stock.purchase_price;
                row.insertCell(3).textContent = stock.current_price || 'N/A';
                const profitLossCell = row.insertCell(4);
                profitLossCell.textContent = profitLoss.toFixed(2);
                profitLossCell.classList.add(profitLossClass);
            });

            // Update the overall balance display
            document.getElementById('overallBalance').textContent = `Overall Balance: $${overallBalance.toFixed(2)}`;
            document.getElementById('overallBalance').classList.add(overallBalance >= 0 ? 'profit' : 'loss');

            // Dynamic message box logic
            let balanceMessage = overallBalance >= 0 ? "Congratulations on the recent successes in your investment journey! Your strategic decisions and patience are paying off, and it's a testament to your hard work and savvy. While it's important to celebrate your achievements, it's equally crucial to remain grounded. The market is very volatile, and you need to continue to make smart decisions with your money. Overconfidence can make you lose very quickly." : "We understand that the current situation with your investments might be disheartening, but remember, the journey of investing is akin to navigating the vast and unpredictable ocean. Just as the seasoned sailor uses the storms to learn and navigate better, this can be an invaluable moment for growth and resilience. Consider choosing safer investments and looking more towards the future than just short term goals. You can also try to get finacial help from someone who may know more than you.";
            const messageBox = document.getElementById('dynamicMessageBox') || document.createElement('div');
            messageBox.id = 'dynamicMessageBox';
            messageBox.textContent = balanceMessage;
            messageBox.style.marginTop = '20px';
            messageBox.style.fontWeight = 'bold';
            messageBox.style.color = overallBalance >= 0 ? '#28a745' : '#dc3545';
            const overallBalanceElement = document.getElementById('overallBalance');
            overallBalanceElement.after(messageBox);

        } catch (error) {
            console.error('Error fetching stocks:', error);
            alert('Error communicating with the backend. Make sure your server is running and accessible.');
        }
    }
    
    // Initial fetch of stocks
    fetchStocks();
</script>

</body>
</html>
