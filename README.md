# Bus-Booking-System
#This is a simple Python program for bus ticket booking that lets users log in, book or cancel seats, check bus status, and make payments through cash (with change calculation), card, or UPI.
# Bus Ticket Booking in Python

class Bus:
    def __init__(self, busNumber, source, destination, totalSeats, availableSeats, fare):
        self.busNumber = busNumber
        self.source = source
        self.destination = destination
        self.totalSeats = totalSeats
        self.availableSeats = availableSeats
        self.fare = fare


class User:
    def __init__(self, username, password):
        self.username = username
        self.password = password


def displayMainMenu():
    print("\n=== MAIN MENU ===")
    print("ENTER -> 1. LOGIN")
    print("ENTER -> 2. EXIT")
    return int(input("Enter your choice: "))


def displayUserMenu():
    print("\n#=== USER MENU ===#")
    print("ENTER -> 1. Book a Ticket")
    print("ENTER -> 2. Cancel a Ticket")
    print("ENTER -> 3. Check Bus Status")
    print("ENTER -> 4. Logout")
    return int(input("Enter your choice: "))


def loginUser(users, username, password):
    for i, user in enumerate(users):
        if user.username == username and user.password == password:
            return i
    return -1


def payment(amount):
    print("\nHow would you like to pay?")
    print("1. Cash")
    print("2. Card")
    print("3. UPI")
    choice = int(input("Enter your choice: "))

    if choice == 1:
        given = float(input(f"Your bill is Rs.{amount}. Enter cash given: "))
        if given < amount:
            print(f"Insufficient cash! You still need Rs.{amount - given:.2f}.")
        else:
            change = given - amount
            print(f"Payment of Rs.{amount} received in CASH.")
            if change > 0:
                print(f"Please collect your change: Rs.{change:.2f}")
            print("THANK YOU for booking with us!\n")

    elif choice == 2:
        pin = input("Swipe your card and enter PIN: ")
        print(f"Payment of Rs.{amount} received via CARD.")
        print("THANK YOU for booking with us!\n")

    elif choice == 3:
        upi_id = input("Enter your UPI ID (e.g., name@upi): ")
        print(f"Payment request sent to {upi_id}.")
        print(f"Payment of Rs.{amount} received via UPI.")
        print("THANK YOU for booking with us!\n")

    else:
        print("Invalid payment option.")


def bookTicket(buses):
    print("\nAvailable Buses:")
    for bus in buses:
        print(f"{bus.busNumber}, Start: {bus.source}, End: {bus.destination}, "
              f"Seats: {bus.totalSeats}, Available: {bus.availableSeats}, Fare: {bus.fare}")

    busNumber = int(input("\nEnter Bus Number: "))
    bus = next((b for b in buses if b.busNumber == busNumber), None)

    if not bus:
        print(f"Bus with Bus Number {busNumber} not found.")
        return

    seatsToBook = int(input("Enter Number of Seats: "))
    if bus.availableSeats < seatsToBook:
        print(f"Sorry, only {bus.availableSeats} seats are available.")
    else:
        bus.availableSeats -= seatsToBook
        totalFare = seatsToBook * bus.fare
        print(f"Booking successful! {seatsToBook} seats booked on Bus Number {busNumber}.")
        print(f"Your Bill = Rs.{totalFare}")
        payment(totalFare)


def cancelTicket(buses):
    busNumber = int(input("\nEnter Bus Number: "))
    bus = next((b for b in buses if b.busNumber == busNumber), None)

    if not bus:
        print(f"Bus with Bus Number {busNumber} not found.")
        return

    seatsToCancel = int(input("Enter Number of Seats to Cancel: "))
    bookedSeats = bus.totalSeats - bus.availableSeats

    if seatsToCancel > bookedSeats:
        print("Error: You can't cancel more seats than were booked.")
    else:
        bus.availableSeats += seatsToCancel
        refundAmount = seatsToCancel * bus.fare
        print(f"Cancellation successful! {seatsToCancel} seats canceled on Bus Number {busNumber}.")
        print(f"Refund Amount = Rs.{refundAmount}")
        payment(refundAmount)


def checkBusStatus(buses):
    busNumber = int(input("\nEnter Bus Number: "))
    bus = next((b for b in buses if b.busNumber == busNumber), None)

    if bus:
        print(f"\nBus Number: {bus.busNumber}")
        print(f"Source: {bus.source}")
        print(f"Destination: {bus.destination}")
        print(f"Total Seats: {bus.totalSeats}")
        print(f"Available Seats: {bus.availableSeats}")
        print(f"Fare: {bus.fare:.2f}")
    else:
        print(f"Bus with Bus Number {busNumber} not found.")


def main():
    print(" ================================")
    print("|    WELCOME TO BUS TICKET      |")
    print("|       BOOKING SYSTEM          |")
    print("|     DESIGNED BY Sai Kiran     |")
    print(" ================================")

    users = [User("sai", "35689")]
    buses = [
        Bus(101, "Chennai", "Nellore", 50, 50, 500.0),
        Bus(102, "Kerala", "Karnataka", 40, 40, 400.0),
        Bus(103, "Gudur", "Chennai", 30, 30, 300.0)
    ]

    loggedInUserId = -1

    while True:
        if loggedInUserId == -1:
            choice = displayMainMenu()
            if choice == 1:
                username = input("Enter Username: ")
                password = input("Enter Password: ")
                loggedInUserId = loginUser(users, username, password)
                if loggedInUserId == -1:
                    print("Login failed. Please check your username and password.")
                else:
                    print(f"Login successful. Welcome, {username}!")
            elif choice == 2:
                print("Exiting the program.")
                break
            else:
                print("Invalid choice. Please try again.")
        else:
            userChoice = displayUserMenu()
            if userChoice == 1:
                bookTicket(buses)
            elif userChoice == 2:
                cancelTicket(buses)
            elif userChoice == 3:
                checkBusStatus(buses)
            elif userChoice == 4:
                print("Logging out.")
                loggedInUserId = -1
            else:
                print("Invalid choice. Please try again.")


if __name__ == "__main__":
    main()
