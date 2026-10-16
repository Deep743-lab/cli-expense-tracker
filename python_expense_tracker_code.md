import argparse
import csv
import os
from datetime import datetime

DATA_FILE = "expenses.csv"


def init_db():
    if not os.path.exists(DATA_FILE):
        with open(DATA_FILE, mode="w", newline="") as file:
            writer = csv.writer(file)
            writer.writerow(["ID", "Date", "Category", "Amount", "Description"])


def get_next_id():
    if not os.path.exists(DATA_FILE):
        return 1
    with open(DATA_FILE, mode="r") as file:
        reader = list(csv.reader(file))
        if len(reader) <= 1:
            return 1
        return int(reader[-1][0]) + 1


def add_expense(category, amount, description):
    init_db()
    expense_id = get_next_id()
    date_str = datetime.now().strftime("%Y-%m-%d %H:%M")
    with open(DATA_FILE, mode="a", newline="") as file:
        writer = csv.writer(file)
        writer.writerow([expense_id, date_str, category, amount, description])
    print(f"✅ Added Expense #{expense_id}: [{category}] ${amount} - {description}")


def list_expenses():
    if not os.path.exists(DATA_FILE):
        print("No expenses recorded yet.")
        return
    with open(DATA_FILE, mode="r") as file:
        reader = csv.reader(file)
        header = next(reader, None)
        print(f"\n{'ID':<5} {'Date':<18} {'Category':<15} {'Amount':<10} {'Description'}")
        print("-" * 65)
        total = 0.0
        for row in reader:
            print(f"{row[0]:<5} {row[1]:<18} {row[2]:<15} ${float(row[3]):<9.2f} {row[4]}")
            total += float(row[3])
        print("-" * 65)
        print(f"Total Spent: ${total:.2f}\n")


def main():
    parser = argparse.ArgumentParser(description="CLI Personal Expense Tracker")
    subparsers = parser.add_subparsers(dest="command")

    # Add command
    add_parser = subparsers.add_parser("add", help="Add a new expense")
    add_parser.add_argument("--category", "-c", required=True, help="Category (e.g. Food, Transport)")
    add_parser.add_argument("--amount", "-a", type=float, required=True, help="Amount spent")
    add_parser.add_argument("--desc", "-d", default="", help="Short description")

    # List command
    subparsers.add_parser("list", help="List all expenses")

    args = parser.parse_args()

    if args.command == "add":
        add_expense(args.category, args.amount, args.desc)
    elif args.command == "list":
        list_expenses()
    else:
        parser.print_help()


if __name__ == "__main__":
    main()