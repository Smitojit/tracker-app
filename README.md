# tracker-app
import json

class Expense:
    def __init__(self, date, description, amount):
        self.date = date
        self.description = description
        self.amount = amount

    def to_dict(self):
        """Convert the Expense object to a dictionary for JSON serialization."""
        return {
            'date': self.date,
            'description': self.description,
            'amount': self.amount
        }

    @classmethod
    def from_dict(cls, data):
        """Create an Expense object from a dictionary (for loading from JSON)."""
        return cls(data['date'], data['description'], data['amount'])

class ExpenseTracker:
    def __init__(self, filename="expenses.json"):
        self.expenses = []
        self.filename = filename
        self.load_expenses()

    def add_expense(self, expense):
        self.expenses.append(expense)
        self.save_expenses()

    def remove_expense(self, index):
        if 0 <= index < len(self.expenses):
            del self.expenses[index]
            self.save_expenses()
            print("Expense removed successfully.")
        else:
            print("Invalid expense index.")

    def view_expense(self):
        if len(self.expenses) == 0:
            print("No expense found.")
        else:
            print("Expense List:")
            for i, expense in enumerate(self.expenses, start=1):
                print(f"{i}. Date: {expense.date}, Description: {expense.description}, Amount: {expense.amount:.2f}")

    def total_expense(self):
        total = sum(expense.amount for expense in self.expenses)
        print(f"Total Expenses: {total:.2f}")

    def save_expenses(self):
        """Save expenses to a JSON file."""
        with open(self.filename, 'w') as file:
            json_data = [expense.to_dict() for expense in self.expenses]
            json.dump(json_data, file)

    def load_expenses(self):
        """Load expenses from the JSON file."""
        try:
            with open(self.filename, 'r') as file:
                json_data = json.load(file)
                self.expenses = [Expense.from_dict(expense) for expense in json_data]
        except FileNotFoundError:
            # If file does not exist, no data is loaded (i.e., start fresh)
            self.expenses = []


def main():
    tracker = ExpenseTracker()

    while True:
        print("\nExpense Tracker Menu:")
        print("1. Add Expense")
        print("2. Remove Expense")
        print("3. View Expense")
        print("4. Total Expenses")
        print("5. Exit")

        choice = int(input("Enter your choice (1-5): "))

        if choice == 1:
            date = input("Enter the date: ")
            description = input("Enter the description: ")
            amount = float(input("Enter the amount: "))
            expense = Expense(date, description, amount)
            tracker.add_expense(expense)  # Add the expense to the tracker
            print("Expense added successfully.")
        elif choice == 2:
            index = int(input("Enter the expense index to remove: ")) - 1
            tracker.remove_expense(index)
        elif choice == 3:
            tracker.view_expense()
        elif choice == 4:
            tracker.total_expense()
        elif choice == 5:
            print("Exit")
            break
        else:
            print("Invalid choice, please try again.")

if __name__ == "__main__":
    main()
