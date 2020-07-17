# CoffeeMachine
class CoffeeMachine:
    coffee_key = {
        "1": "espresso", "2": "latte", "3": "cappuccino"
    }

    coffee_cost = {
        "espresso": 4, "latte": 7, "cappuccino": 6
    }

    coffee_water_measure = {
        "espresso": 250, "latte": 350, "cappuccino": 200
    }

    coffee_milk_measure = {
        "espresso": 0, "latte": 75, "cappuccino": 100
    }

    coffee_beans_measure = {
        "espresso": 16, "latte": 20, "cappuccino": 12
    }

    def __init__(self):
        self.status = None
        self.water = 400
        self.milk = 540
        self.coffee_beans = 120
        self.coffee_cups = 9
        self.money = 550
        self.out_of_contents = []

    def order_coffee(self):
        print("Write action (buy, fill, take, remaining, exit):")
        action_type = input()
        if action_type == "buy":
            self.buy_coffee()
        elif action_type == "fill":
            self.fill_coffee_machine()
        elif action_type == "take":
            self.withdraw_money()
        elif action_type == "remaining":
            self.print_current_status()
        elif action_type == "exit":
            return action_type

    def buy_coffee(self):
        print("What do you want to buy? 1 - espresso, 2 - latte, 3 - cappuccino, back - to main menu:")
        coffee_type = input()
        if coffee_type != "back":
            self.prepare_coffee(CoffeeMachine.coffee_key[coffee_type])

    def print_current_status(self):
        print("The coffee machine has:")
        print(self.water, "of water")
        print(self.milk, "of milk")
        print(self.coffee_beans, "of coffee beans")
        print(self.coffee_cups, "of disposable cups")
        print(self.money, "of money")

    def is_coffee_possible(self, coffee_type):
        is_possible = True
        global out_of_contents
        out_of_contents = []
        if self.coffee_cups >= 1:
            water_cups = self.water // CoffeeMachine.coffee_water_measure[coffee_type]
            milk_cups = self.milk // CoffeeMachine.coffee_milk_measure[
                coffee_type] if CoffeeMachine.coffee_milk_measure[coffee_type] != 0 else 1
            coffee_beans_cups = self.coffee_beans // CoffeeMachine.coffee_beans_measure[coffee_type]
            if water_cups < 1:
                out_of_contents.append("water")
                is_possible = False
            if milk_cups < 1:
                out_of_contents.append("milk")
                is_possible = False
            if coffee_beans_cups < 1:
                out_of_contents.append("coffee beans")
                is_possible = False
        else:
            out_of_contents.append("cups")
            is_possible = False

        return is_possible

    def prepare_coffee(self, coffee_type):
        if(self.is_coffee_possible(coffee_type)):
            print("I have enough resources, making you a coffee!")
            self.money += CoffeeMachine.coffee_cost[coffee_type]
            self.water -= CoffeeMachine.coffee_water_measure[coffee_type]
            self.milk -= CoffeeMachine.coffee_milk_measure[coffee_type]
            self.coffee_beans -= CoffeeMachine.coffee_beans_measure[coffee_type]
            self.coffee_cups -= 1
        else:
            error_message = 'Sorry, not enough '
            for content in out_of_contents:
                error_message += content + ','
            error_message = error_message.rstrip(',')
            error_message += '!'
            print(error_message)

    def fill_coffee_machine(self):
        print("Write how many ml of water do you want to add:")
        self.water += abs(int(input()))

        print("Write how many ml of milk do you want to add:")
        self.milk += abs(int(input()))

        print("Write how many grams of coffee beans do you want to add:")
        self.coffee_beans += abs(int(input()))

        print("Write how many disposable cups of coffee do you want to add:")
        self.coffee_cups += abs(int(input()))

    def withdraw_money(self):
        print("I gave you $" + str(self.money))
        self.money = 0


my_machine = CoffeeMachine()
while True:
    action = my_machine.order_coffee()
    if action == "exit":
        break
