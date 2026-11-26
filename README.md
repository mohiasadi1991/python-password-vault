
import json
from pathlib import Path

FILE = Path("accounts.json")

def load_accounts():
    if FILE.exists():
        try:
            return json.loads(FILE.read_text(encoding="utf-8"))
        except json.JSONDecodeError:
            print("Warning: Invalid data file.")
    return []

def save_accounts(accounts):
    FILE.write_text(
        json.dumps(accounts, indent=4, ensure_ascii=False),
        encoding="utf-8"
    )

def add_account(accounts):
    website = input("Website: ").strip()
    username = input("Username: ").strip()

    if not website or not username:
        print("Website and username are required.")
        return

    accounts.append({
        "website": website,
        "username": username
    })

    save_accounts(accounts)
    print("Account record added!")

def show_accounts(accounts):
    if not accounts:
        print("No account records found.")
        return

    print("\n=== ACCOUNT RECORDS ===")
    for index, account in enumerate(accounts, 1):
        print(
            f"{index}. {account['website']} "
            f"- {account['username']}"
        )

def search_account(accounts):
    keyword = input("Website to search: ").strip().lower()

    results = [
        account for account in accounts
        if keyword in account["website"].lower()
    ]

    if results:
        for account in results:
            print(
                f"{account['website']} - "
                f"{account['username']}"
            )
    else:
        print("No matching records found.")

def delete_account(accounts):
    show_accounts(accounts)

    if not accounts:
        return

    try:
        number = int(input("Record number to delete: "))

        if 1 <= number <= len(accounts):
            removed = accounts.pop(number - 1)
            save_accounts(accounts)
            print(f"Deleted record for {removed['website']}.")
        else:
            print("Invalid record number.")

    except ValueError:
        print("Please enter a valid number.")

def main():
    accounts = load_accounts()

    while True:
        print("\n=== PYTHON ACCOUNT MANAGER ===")
        print("1. Add account")
        print("2. Show accounts")
        print("3. Search account")
        print("4. Delete account")
        print("5. Exit")

        choice = input("Choose an option: ").strip()

        if choice == "1":
            add_account(accounts)
        elif choice == "2":
            show_accounts(accounts)
        elif choice == "3":
            search_account(accounts)
        elif choice == "4":
            delete_account(accounts)
        elif choice == "5":
            print("Goodbye!")
            break
        else:
            print("Invalid choice.")

if __name__ == "__main__":
    main()
