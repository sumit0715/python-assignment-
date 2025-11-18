# python-assignment-
import json  # For saving/loading data to/from a file
import os  # For checking if file exists

# File to store budget data
DATA_FILE = 'budget_data.json'

# Function to load data from file
def load_data():
    """Loads budget data from JSON file if it exists."""
    if os.path.exists(DATA_FILE):
        try:
            with open(DATA_FILE, 'r') as f:
                return json.load(f)
        except json.JSONDecodeError:
            print("Error: Corrupted data file. Starting fresh.")
    return {'transactions': []}  # Default structure: list of transactions

# Function to save data to file
def save_data(data):
    """Saves budget data to JSON file."""
    try:
        with open(DATA_FILE, 'w') as f:
            json.dump(data, f, indent=4)
        print("Data saved successfully.")
    except Exception as e:
        print(f"Error saving data: {e}")

# Function to add a transaction
def add_transaction(data):
    """Adds a new income or expense transaction."""
    try:
        trans_type = input("Enter type (income/expense): ").strip().lower()
        if trans_type not in ['income', 'expense']:
            print("Invalid type. Must be 'income' or 'expense'.")
            return
        
        amount = float(input("Enter amount: "))
        category = input("Enter category (e.g., food, salary): ").strip()
        description = input("Enter description: ").strip()
        
        transaction = {
            'type': trans_type,
            'amount': amount,
            'category': category,
            'description': description
        }
        data['transactions'].append(transaction)
        print("Transaction added.")
    except ValueError:
        print("Invalid amount. Please enter a number.")

# Function to view summary
def view_summary(data):
    """Displays a summary of total income, expenses, and balance."""
    transactions = data['transactions']
    total_income = sum(t['amount'] for t in transactions if t['type'] == 'income')
    total_expenses = sum(t['amount'] for t in transactions if t['type'] == 'expense')
    balance = total_income - total_expenses
    
    print(f"\n--- Budget Summary ---")
    print(f"Total Income: ${total_income:.2f}")
    print(f"Total Expenses: ${total_expenses:.2f}")
    print(f"Balance: ${balance:.2f}")

# Function to view all transactions
def view_transactions(data):
    """Displays all transactions."""
    transactions = data['transactions']
    if not transactions:
        print("No transactions yet.")
        return
    
    print("\n--- All Transactions ---")
    for i, t in enumerate(transactions, 1):
        print(f"{i}. {t['type'].capitalize()}: ${t['amount']:.2f} ({t['category']}) - {t['description']}")

# Main program loop
def main():
    data = load_data()
    
    while True:
        print("\nPersonal Budget Tracker")
        print("1. Add Transaction")
        print("2. View Summary")
        print("3. View All Transactions")
        print("4. Save and Quit")
        
        choice = input("Enter choice (1-4): ").strip()
        
        if choice == '1':
            add_transaction(data)
        elif choice == '2':
            view_summary(data)
        elif choice == '3':
            view_transactions(data)
        elif choice == '4':
            save_data(data)
            print("Goodbye!")
            break
        else:
            print("Invalid choice. Please select 1-4.")

if __name__ == "__main__":
    main()
