# CLI Expense Tracker

A lightweight, zero-dependency Python command-line utility for tracking daily expenses and managing personal budgets.

## Features
- **Fast CLI Logging:** Log expenses directly from your terminal.
- **Categorization:** Tag items by category (e.g., Food, Utilities, Transport).
- **Persistent Storage:** Local CSV-based data logging.
- **Zero External Dependencies:** Runs natively using standard Python 3 libraries.

## Quick Start

### Add an Expense
```bash
python tracker.py add -c "Food" -a 15.50 -d "Lunch"
```

### View All Expenses & Summary
```bash
python tracker.py list
```

## Requirements
- Python 3.6+