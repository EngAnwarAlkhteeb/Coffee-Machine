# ☕ Coffee Machine Program

A Python program that simulates a coffee machine with various functionalities, including making different types of coffee, checking resources, processing coins, and more.

## 🚀 Features

1. **Order Coffee**: Prompt the user to choose their desired coffee (espresso, latte, cappuccino).
2. **Turn Off Machine**: Enter "off" to turn off the coffee machine. (Secret word for maintainers)
3. **Generate Report**: Enter "report" to display a report with current resource values.
4. **Check Resources**: Ensure there are enough resources before making a drink.
5. **Process Coins**: Insert coins to pay for the selected drink.
6. **Check Transaction**: Verify if the user has inserted enough money for the chosen drink.
7. **Make Coffee**: Deduct the required resources and provide the user with their coffee.

## ⚙️ Usage

1. Run the Python script in a console or terminal.
2. Follow the prompts to order coffee, turn off the machine, generate reports, and more.
3. Enjoy your virtual coffee experience!

## 📋 The Code 

```python
# MENU = {
    "espresso": {
        "ingredients": {
            "water": 50,
            "coffee": 18,
        },
        "cost": 1.5,
    },
    "latte": {
        "ingredients": {
            "water": 200,
            "milk": 150,
            "coffee": 24,
        },
        "cost": 2.5,
    },
    "cappuccino": {
        "ingredients": {
            "water": 250,
            "milk": 100,
            "coffee": 24,
        },
        "cost": 3.0,
    }
}

profit = 0
resources = {
    "water": 300,
    "milk": 200,
    "coffee": 100,
}

# check if the user order it's enough with your order
def is_resource_sufficient(order_ingredients):
    """Return True when order can be """
    is_enough = True
    for item in order_ingredients:
        if order_ingredients[item] >= resources[item]:
            print(f"Sorry there is not enough {item}.")
            is_enough = False
    return is_enough


# Calculate the monetary value of the coins inserted. E.g. 1 quarter, 2 dimes, 1 nickel, 2
#pennies = 0.25 + 0.1 x 2 + 0.05 + 0.01 x 2 = $0.52
def process_coins():
    """Return the total calculated from coins inserted. """
    print("Please insert coins.")
    total = int(input("How many quarters: ")) * 0.25
    total += int(input("How many dimes: ")) * 0.1
    total += int(input("How many nickles: ")) * 0.05
    total += int(input("How many pinnies: ")) * 0.01
    return total


def is_transaction_succeessful(money_received, drink_cost):
    """Return True when the payment is accepted, or False when the money is insufficient. """
    if money_received >= drink_cost:
        change = round(money_received - drink_cost, 2)
        print(f"here's ${change} in change.")
        global profit
        profit += drink_cost
        return True
    else:
        print("Sorry that's not enough money. Money refunded.")
        return False


def make_coffee(drink_name, order_ingredients):
    """Deduct the required ingredients from the resources"""
    for item in order_ingredients:
        resources[item] -= order_ingredients[item]
    print(f"Here is your {drink_name} ☕. Enjoy ")


is_on = True

# check the user's input to decide what to do next
while is_on:
    # prompt user by asking " What would you like? (espresso/latte/cappuccino): "
    choice = input("What would you like? (espresso/latte/cappuccino): ")
    # Turn of the machine " off"
    if choice == "off":
        is_on = False
    # Print report
    elif choice == "report":
        print(f"water : {resources['water']}ml")
        print(f"milk : {resources['milk']}ml")
        print(f"coffee : {resources['coffee']}g")
        print(f"money : ${profit}")
    else:
        drink = MENU[choice]
        if is_resource_sufficient(drink['ingredients']):
            payment = process_coins()
            if is_transaction_succeessful(payment, drink['cost']):
                make_coffee(choice, drink['ingredients'])
