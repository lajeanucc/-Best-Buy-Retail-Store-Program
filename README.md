

def show_products():
    print("\nAvailable Products:")
    for name, info in products.items():
        print(f"{name} - ${info['price']} - Stock: {info['stock']}")

def add_to_cart():
    product = input("Enter product name to add: ")
    if product in products:
        if products[product]['stock'] == 0:
            print("Out of stock.")
            return
        try:
            qty = int(input("Enter quantity: "))
            if qty <= 0:
                print("Invalid quantity.")
                return
            if qty > products[product]['stock']:
                print("Not enough stock.")
                return
            if product in cart:
                cart[product] += qty
            else:
                cart[product] = qty
            products[product]['stock'] -= qty
            print("Item added.")
        except:
            print("Invalid input.")
    else:
        print("Product not found.")

def remove_from_cart():
    product = input("Enter product name to remove: ")
    if product in cart:
        try:
            qty = int(input("Enter quantity to remove: "))
            if qty <= 0 or qty > cart[product]:
                print("Invalid quantity.")
                return
            cart[product] -= qty
            products[product]['stock'] += qty
            if cart[product] == 0:
                del cart[product]
            print("Item removed.")
        except:
            print("Invalid input.")
    else:
        print("Item not in cart.")

def view_cart():
    print("\nCart:")
    total = 0
    for item in cart:
        qty = cart[item]
        price = products[item]['price']
        print(f"{item} x{qty} - ${price * qty}")
        total += price * qty
    print(f"Subtotal: ${total}")

def checkout():
    subtotal = 0
    for item in cart:
        qty = cart[item]
        price = products[item]['price']
        subtotal += price * qty
    discount = 0
    if subtotal > 5000:
        discount = subtotal * 0.05
        subtotal -= discount
    tax = subtotal * 0.10
    total = subtotal + tax
    print(f"\nSubtotal: ${subtotal}")
    print(f"Tax (10%): ${tax}")
    print(f"Total: ${total}")
    while True:
        try:
            paid = float(input("Enter amount received: "))
            if paid < total:
                print("Not enough money.")
            else:
                change = paid - total
                print_receipt(paid, change, subtotal, tax, total)
                break
        except:
            print("Invalid input.")

def print_receipt(paid, change, subtotal, tax, total):
    print("\n--- Best Buy Retail Store ---")
    for item in cart:
        qty = cart[item]
        price = products[item]['price']
        total_price = price * qty
        print(f"{item} x{qty} ${price} = ${total_price}")
    print(f"Subtotal: ${subtotal}")
    print(f"Tax: ${tax}")
    print(f"Total: ${total}")
    print(f"Paid: ${paid}")
    print(f"Change: ${change}")
    print("Thank you for shopping!\n")

def check_stock_alerts():
    for name, info in products.items():
        if info['stock'] < 5:
            print(f"Low stock alert: {name} - Only {info['stock']} left!")

products = {
    "Bread": {"price": 150, "stock": 10},
    "Milk": {"price": 200, "stock": 10},
    "Sugar": {"price": 120, "stock": 10},
    "Soap": {"price": 80, "stock": 10},
    "Toothpaste": {"price": 160, "stock": 10},
    "Butter": {"price": 220, "stock": 10},
    "Juice": {"price": 250, "stock": 10},
    "Eggs": {"price": 300, "stock": 10},
    "Rice": {"price": 400, "stock": 10},
    "Oil": {"price": 500, "stock": 10}
}

while True:
    cart = {}
    while True:
        print("\n1. Show Products")
        print("2. Add to Cart")
        print("3. Remove from Cart")
        print("4. View Cart")
        print("5. Checkout")
        print("6. Exit Transaction")
        choice = input("Select option: ")
        if choice == "1":
            show_products()
        elif choice == "2":
            add_to_cart()
        elif choice == "3":
            remove_from_cart()
        elif choice == "4":
            view_cart()
        elif choice == "5":
            checkout()
            break
        elif choice == "6":
            break
        else:
            print("Invalid option.")
    check_stock_alerts()
    cont = input("\nNew transaction? (y/n): ")
    if cont.lower() != 'y':
        break

