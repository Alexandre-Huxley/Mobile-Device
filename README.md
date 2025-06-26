# Mobile-Device
# Master Degree
# Mobile Devices, SIM cards and Accessories Shop

# Task 1 – Setting up the system.
# Objective: 
# - Use appropriate data structures to store item code, description, and price for devices, SIM cards, and accessories.
# - Allow the customer to choose a specific phone or tablet.
# - Allow phone customers to choose SIM Free or Pay As You Go.
# - Allow the customer to choose a standard or luxury case.
# - Allow the customer to choose chargers (none, one, or both).
# - Calculate the total price of the transaction.
# - Output a list of items purchased and the total price.

devices = [
    {"code": "BPCM", "desc": "Compact", "price": 29.99, "type": "phone"},
    {"code": "BPSH", "desc": "Clam shell", "price": 49.99, "type": "phone"},
    {"code": "RPSS", "desc": "Robo phone - 5inch 64GB memory", "price": 199.99, "type": "phone"},
    {"code": "RPLL", "desc": "Robo phone - 6inch 256GB memory", "price": 499.99, "type": "phone"},
    {"code": "YPLS", "desc": "Y-phone standard 6 inch 64GB memory", "price": 549.99, "type": "phone"},
    {"code": "YPLL", "desc": "Y-phone deluxe 6 inch 256GB memory", "price": 649.99, "type": "phone"},
    {"code": "RTMS", "desc": "RoboTab - 8 inch screen 64GB memory", "price": 149.99, "type": "tablet"},
    {"code": "RTML", "desc": "RoboTab - 10 inch screen 128GB memory", "price": 299.99, "type": "tablet"},
    {"code": "YTLM", "desc": "Y-tab standard - 10 inch screen 128GB memory", "price": 499.99, "type": "tablet"},
    {"code": "YTLL", "desc": "Y-tab deluxe - 10 inch screen 256GB memory", "price": 599.99, "type": "tablet"}
]

sim_options = [
    {"code": "SMNO", "desc": "Sim free (no SIM card)", "price": 0.00},
    {"code": "SMPG", "desc": "Pay as you go (with SIM card)", "price": 9.99}
]

cases = [
    {"code": "CSST", "desc": "Standard", "price": 0.00},
    {"code": "CSLX", "desc": "Luxury", "price": 50.00}
]

chargers = [
    {"code": "CGCR", "desc": "Car", "price": 19.99},
    {"code": "CGHM", "desc": "Home", "price": 15.99}
]

def choose_option(options, prompt):
    # Prompts the user to select an option from a list, validates input.
    while True:
        print(prompt)
        for idx, item in enumerate(options, 1):
            print(f"{idx}. {item['desc']} (${item['price']:.2f})")
        choice = input("Enter the number of your choice: ")
        if choice.isdigit() and 1 <= int(choice) <= len(options):
            return options[int(choice)-1]
        else:
            print("Invalid input. Please try again.")

def yes_no(prompt):
    # Prompts the user for a yes/no answer, validates input.
    while True:
        answer = input(prompt + " (y/n): ").strip().lower()
        if answer in ['y', 'n']:
            return answer == 'y'
        print("Invalid input. Please enter 'y' or 'n'.")

def main():
    # Task 2 – Allow a customer to order multiple mobile devices.
    # Objective:
    # - Offer the customer the opportunity to purchase additional devices.
    # - For each device, repeat the selection and calculation steps.
    # - Calculate a running total for the customer.
    # - Output the total to be paid when finished.

    print("Welcome to the Mobile Device Shop!")
    total = 0.0
    items_purchased = []
    device_count = 0
    discount_total = 0.0

    while True:
        print("\nAvailable devices:")
        for idx, d in enumerate(devices, 1):
            print(f"{idx}. {d['desc']} ({d['code']}) - ${d['price']:.2f}")
        device = choose_option(devices, "Choose a phone or tablet:")
        device_count += 1
        device_price = device['price']
        item_list = [f"{device['desc']} ({device['code']}) - ${device_price:.2f}"]

        # SIM options only for phones
        sim_price = 0.0
        if device['type'] == "phone":
            sim = choose_option(sim_options, "Choose SIM option for your phone:")
            sim_price = sim['price']
            item_list.append(f"{sim['desc']} ({sim['code']}) - ${sim_price:.2f}")

        # Case selection
        case = choose_option(cases, "Choose a case:")
        case_price = case['price']
        item_list.append(f"{case['desc']} ({case['code']}) - ${case_price:.2f}")

        # Charger selection
        charger_total = 0.0
        for charger in chargers:
            if yes_no(f"Do you want to add a {charger['desc']} charger?"):
                charger_total += charger['price']
                item_list.append(f"{charger['desc']} charger ({charger['code']}) - ${charger['price']:.2f}")

        # Calculate subtotal for this device
        subtotal = device_price + sim_price + case_price + charger_total

        # Task 3 – Offering discounts.
        # Objective:
        # - Apply a 10% discount to every additional phone or tablet purchased.
        # - Output the new total and the amount saved.
        discount = 0.0
        if device_count > 1:
            discount = subtotal * 0.10
            subtotal -= discount
            discount_total += discount

        total += subtotal
        items_purchased.append((item_list, subtotal, discount))

        # Ask if customer wants another device
        if not yes_no("Would you like to purchase another device?"):
            break

    # Output summary
    print("\n--- Purchase Summary ---")
    for idx, (item_list, subtotal, discount) in enumerate(items_purchased, 1):
        print(f"\nDevice {idx}:")
        for item in item_list:
            print("  -", item)
        if discount > 0:
            print(f"  Discount applied: -${discount:.2f}")
        print(f"  Subtotal: ${subtotal:.2f}")

    print(f"\nTotal to pay: ${total:.2f}")
    if discount_total > 0:
        print(f"You saved: ${discount_total:.2f} with discounts!")

if __name__ == "__main__":
    main()
