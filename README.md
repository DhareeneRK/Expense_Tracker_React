# Expense Tracker (ReactJS)
## Date:24/05/25

## AIM
To develop a simple Expense Tracker application using React that allows users to manage their personal finances by adding, viewing, and deleting income and expense transactions, while dynamically calculating the current balance, total income, and total expenses.

## ALGORITHM
### STEP 1: Initialize the Project
Create a new React app using:

npx create-react-app expense-tracker
or

npm create vite@latest expense-tracker --template react

Open the project in a code editor like VS Code.

### Step 2: Setup State
Define a state variable to store transactions:

Example: const [transactions, setTransactions] = useState([])

Define state variables for the form inputs:

const [text, setText] = useState("")

const [amount, setAmount] = useState("")

### Step 3: Add a New Transaction
Create a form with two inputs:

Text input for description

Number input for amount

On form submit:

Validate input

Create a new transaction object

Add the object to the transactions array using setTransactions.

### Step 4: Display Transaction List

Use map() to render each transaction in a list.

Conditionally style each item based on amount > 0 (income) or amount < 0 (expense).

Add a delete button next to each transaction.

### Step 5: Calculate and Display Summary

Use reduce() to calculate:

Total Balance: sum of all amounts

Total Income: sum of all positive amounts

Total Expenses: sum of all negative amounts

Display these values at the top of the UI.

### Step 6: Delete a Transaction

When delete is clicked:

Use filter() to remove the transaction from the array by id.

Update the state using setTransactions.

### Step 7: Style the Application

Use basic CSS to style:

Balance summary

Income/expense totals

Form inputs

Transaction list (with color coding)

## PROGRAM
# App.js
```
import React, { useState } from 'react';
import './App.css';

function App() {
  const [transactions, setTransactions] = useState([]);
  const [description, setDescription] = useState('');
  const [amount, setAmount] = useState('');

  const addTransaction = (e) => {
    e.preventDefault();
    if (!description || !amount) return;

    const newTransaction = {
      id: Date.now(),
      description,
      amount: parseFloat(amount)
    };
    setTransactions([newTransaction, ...transactions]);
    setDescription('');
    setAmount('');
  };

  const deleteTransaction = (id) => {
    setTransactions(transactions.filter(tx => tx.id !== id));
  };

  const getBalance = () => {
    return transactions.reduce((acc, tx) => acc + tx.amount, 0).toFixed(2);
  };

  const getIncome = () => {
    return transactions
      .filter(tx => tx.amount > 0)
      .reduce((acc, tx) => acc + tx.amount, 0)
      .toFixed(2);
  };

  const getExpenses = () => {
    return transactions
      .filter(tx => tx.amount < 0)
      .reduce((acc, tx) => acc + tx.amount, 0)
      .toFixed(2);
  };

  return (
    <div className="container">
      <h2>Expense Tracker</h2>

      <div className="balance">
        <h3>Your Balance</h3>
        <h1>${getBalance()}</h1>
      </div>

      <div className="summary">
        <div className="income">
          <h4>Income</h4>
          <p className="money plus">${getIncome()}</p>
        </div>
        <div className="expense">
          <h4>Expense</h4>
          <p className="money minus">${Math.abs(getExpenses())}</p>
        </div>
      </div>

      <form onSubmit={addTransaction}>
        <h3>Add New Transaction</h3>
        <div className="form-control">
          <label>Description</label>
          <input
            type="text"
            placeholder="Enter description..."
            value={description}
            onChange={(e) => setDescription(e.target.value)}
          />
        </div>
        <div className="form-control">
          <label>Amount (positive = income, negative = expense)</label>
          <input
            type="number"
            placeholder="Enter amount..."
            value={amount}
            onChange={(e) => setAmount(e.target.value)}
          />
        </div>
        <button className="btn">Add Transaction</button>
      </form>

      <h3>Transaction History</h3>
      <ul className="list">
        {transactions.map(tx => (
          <li key={tx.id} className={tx.amount > 0 ? 'plus' : 'minus'}>
            {tx.description}
            <span>${tx.amount.toFixed(2)}</span>
            <button className="delete-btn" onClick={() => deleteTransaction(tx.id)}>x</button>
          </li>
        ))}
      </ul>
        <footer className="footer">
          <p>Dhareene R K (212222040035)</p>
        </footer>
      </div>
  );
}

export default App;

```
# App.css
```
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  background: #f4f4f4;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}

.container {
  max-width: 400px;
  margin: 30px auto;
  padding: 20px;
  background: #fff;
  border-radius: 8px;
  box-shadow: 0 0 10px #ccc;
}

h2 {
  text-align: center;
  margin-bottom: 20px;
}

.balance h3 {
  margin: 0;
  color: #666;
}

.balance h1 {
  margin: 10px 0;
  font-size: 2rem;
}

.summary {
  display: flex;
  justify-content: space-between;
  margin: 20px 0;
}

.income, .expense {
  width: 48%;
  padding: 10px;
  border: 1px solid #ddd;
  text-align: center;
  border-radius: 5px;
}

.money {
  font-size: 1.2rem;
  font-weight: bold;
}

.plus {
  color: green;
}

.minus {
  color: red;
}

form {
  margin-bottom: 20px;
}

.form-control {
  margin-bottom: 10px;
}

.form-control label {
  display: block;
  margin-bottom: 5px;
}

.form-control input {
  width: 100%;
  padding: 8px;
  border: 1px solid #bbb;
  border-radius: 5px;
}

.btn {
  width: 100%;
  padding: 10px;
  background: #007bff;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  font-weight: bold;
}

.btn:hover {
  background: #0056b3;
}

.list {
  list-style-type: none;
  padding: 0;
}

.list li {
  display: flex;
  justify-content: space-between;
  padding: 10px;
  border-right: 5px solid;
  background: #f9f9f9;
  margin-bottom: 10px;
  border-radius: 5px;
}

.list li.plus {
  border-color: green;
}

.list li.minus {
  border-color: red;
}

.delete-btn {
  background: transparent;
  border: none;
  color: #888;
  font-weight: bold;
  cursor: pointer;
  margin-left: 10px;
}

@media (max-width: 500px) {
  .container {
    margin: 10px;
    padding: 15px;
  }

  .summary {
    flex-direction: column;
  }

  .income, .expense {
    width: 100%;
    margin-bottom: 10px;
  }
}
.footer {
  text-align: center;
  margin-top: 20px;
  padding: 10px;
  font-size: 0.9rem;
  color: #666;
  border-top: 1px solid #ddd;
}

```
## OUTPUT

![web1 245](https://github.com/user-attachments/assets/1caf30e4-cbc5-417b-babd-6cd7832c244e)
![web2 245](https://github.com/user-attachments/assets/bdf3f03a-0553-41b5-bba0-b55558aa6a2a)


## RESULT
A fully functional React-based Expense Tracker application was successfully developed. 
