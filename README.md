<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Click Cash App</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            padding: 50px;
            background-color: #f9f9f9;
        }
        #withdrawForm {
            margin-top: 20px;
        }
        input {
            margin: 10px;
            padding: 10px;
            width: 200px;
        }
        button {
            padding: 10px 20px;
            background-color: #007bff;
            color: white;
            border: none;
            cursor: pointer;
        }
        button:hover {
            background-color: #0056b3;
        }
    </style>
</head>
<body>
    <h1>Welcome to Click Cash!</h1>
    <button id="earnButton">Click to Earn £0.50!</button>
    <p id="earnings">Total Earnings: £0.00</p>

    <form id="withdrawForm">
        <input type="number" id="withdrawAmount" placeholder="Enter amount to withdraw" required>
        <button type="submit">Withdraw</button>
    </form>

    <script>
        let totalEarnings = 0;

        document.getElementById('earnButton').addEventListener('click', () => {
            totalEarnings += 0.50;
            document.getElementById('earnings').innerText = `Total Earnings: £${totalEarnings.toFixed(2)}`;
        });

        document.getElementById('withdrawForm').addEventListener('submit', async (event) => {
            event.preventDefault(); // Prevent default form submission

            const amount = document.getElementById('withdrawAmount').value;

            // Call your backend to create a withdrawal
            const response = await fetch('/withdraw', {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json',
                },
                body: JSON.stringify({ amount }),
            });

            const data = await response.json();
            if (response.ok) {
                alert('Withdrawal successful!');
            } else {
                alert(`Error: ${data.error}`);
            }
        });
    </script>
</body>
</html>
const express = require('express');
const bodyParser = require('body-parser');
const cors = require('cors');
const path = require('path'); // Import path module

const app = express();
const PORT = process.env.PORT || 3000;

app.use(cors());
app.use(bodyParser.json());
app.use(express.static(path.join(__dirname))); // Serve static files from the root directory

// Route to serve the HTML page
app.get('/', (req, res) => {
    res.sendFile(path.join(__dirname, 'index.html')); // Send the index.html file
});

// Endpoint to handle withdrawal requests
app.post('/withdraw', (req, res) => {
    const { amount } = req.body; // Get amount to withdraw
    console.log(`Withdrawal request for: £${amount}`); // Placeholder for actual withdrawal logic
    // Here you can add your Stripe logic for handling withdrawals
    res.status(200).send({ success: true });
});

app.listen(PORT, () => {
    console.log(`Server running on port ${PORT}`);
});
