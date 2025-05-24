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

## OUTPUT


## RESULT
A fully functional React-based Expense Tracker application was successfully developed. 
