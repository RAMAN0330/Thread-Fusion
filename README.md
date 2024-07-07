# Thread-Fusion
 A comprehensive repo for mastering threading concepts. Includes fundamentals, advanced topics, multilingual examples, best practices, and interactive tutorials to help you build efficient, threaded applications.
Thread-Fusion

```bash
  PROBLEM STATEMENT
```
 Write a program to get the number of item types with their availability and usage pattern. Then, calculate which item type is less utilized.
Note:
The item that is less utilized is calculated by min(Total item used / Total available)
Start the thread by calling the start() method and wait for all the threads to complete their processing using the join() method.
Once all the threads are completed, a display of which item type is less utilized.
Write a function called count(), this will calculate which item is less utilized.

code :
```bash
import threading
class ItemManager:
    def __init__(self):
        self.avail = {}
        self.usage = {}
        self.lock = threading.Lock()
    
    def update_avail(self, item_number, count):
        with self.lock:
            if item_number in self.avail:
                self.avail[item_number] += count
            else:
                self.avail[item_number] = count
    
    def update_usage(self, item_number, count):
        with self.lock:
            if item_number in self.usage:
                self.usage[item_number] += count
            else:
                self.usage[item_number] = count
    
    def cal_util(self):
        util_ratios = {}
        for item_number in self.avail:
            total_avail = self.avail[item_number]
            total_used = self.usage.get(item_number,0)
            if total_avail >0:
                util_ratio = total_used/total_avail
            else:
                util_ratio = float("inf")
            util_ratios[item_number] = util_ratio
            
        min_util_item = min(util_ratios,key= util_ratios.get)
        return min_util_item
def count():    
    n = int(input().strip())
    item_man = ItemManager()
    for _ in range(n):
        item_number , avail_count = map(int, input().strip().split())
        item_man.update_avail(item_number, avail_count)
        
    m = int(input().strip())
    for _ in range(m):
        item_number, usage_count = map(int, input().strip().split())
        item_man.update_usage(item_number,usage_count)
        
    less_utilizes = item_man.cal_util()
    print(f"The item type that is less utilized is {less_utilizes}")

count()
```

```bash
PROBLEM STATEMENT
```
 Write a program to print the Customer and their Order details using threading. Create a class named Customer with the following member variables/attributes:
Data Type VariableName 
Integer id
String username
String password
String name
String address
String city
Integer pincode
Integer contactNumber
String email
Order orderList
Create a list of objects of the Customer class. Get all details from the user. Print the details in a specified format, for all the orders which are given by the Customers. Write a program using threading in which each thread prints the data of the orders of customers.

code: 
```bash
import threading

class Order:
    def __init__(self, orderId, total):
        self.orderId = orderId
        self.total = total
        
    def __str__(self):
        return f"Id : {self.orderId} ,  Total Amount : {self.total}"

class Customer:
    def __init__(self, id, username, password, name, address, city, pincode, contactNumber, email, orderList):
        self.id = id
        self.username = username
        self.password = password
        self.name = name
        self.address = address
        self.city = city
        self.pincode = pincode
        self.contactNumber = contactNumber
        self.email = email
        self.orderList = orderList
        
    def __str__(self):
        details = (f"Id :  {self.id} ,  User Name :  {self.username} ,  Password :  {self.password} ,  Name :  {self.name} ,"
                   f" Address :  {self.address} ,  City :  {self.city} ,  Pincode :  {self.pincode} ,"
                   f" Contact Number :  {self.contactNumber} ,  Email :  {self.email}")
        return details

def print_orders(customer):
    print(customer)
    for order in customer.orderList:
        print(order)

def main():
    n = int(input().strip())  # Number of customers
    
    customers = []
    
    for _ in range(n):
        id = int(input().strip())
        username = input().strip()
        password = input().strip()
        name = input().strip()
        address = input().strip()
        city = input().strip()
        pincode = int(input().strip())
        contactNumber = int(input().strip())
        email = input().strip()
        
        m = int(input().strip())  # Number of orders for this customer
        orderList = []
        for _ in range(m):
            orderId = int(input().strip())
            total = int(input().strip())
            orderList.append(Order(orderId, total))
        
        customer = Customer(id, username, password, name, address, city, pincode, contactNumber, email, orderList)
        customers.append(customer)
    
    threads = []
    for customer in customers:
        t = threading.Thread(target=print_orders, args=(customer,))
        threads.append(t)
        t.start()
    
    for thread in threads:
        thread.join()

if __name__ == "__main__":
    main()
```
```bash
PROBLEM STATEMENT
```
 Write a program to get the number of events and event details. Then, calculate whether organizing the event will lead to profit or loss. The profit or loss will be calculated by a separate thread. Calculate whether profit or loss and display it with the event name.

Start the thread by calling the start() method and wait for all the threads to complete their processing using the join() method.
Once all the threads are completed, display the event name with status as profit or loss.
Write a separate method called ‘profit_or_loss(seats_booked, hall_capacity)’.
This method takes two parameters seats_booked, hall_capacity, and does the profit or loss calculation.

code :
```bash
import threading

class Event:
    def __init__(self, eventName, hallName, cost, hallCapacity, seatsBooked):
        self.eventName = eventName
        self.hallName = hallName
        self.cost = int(cost)
        self.hallCapacity = int(hallCapacity)
        self.seatsBooked = int(seatsBooked)

def profit_or_loss(event):
    profit_percentage = (event.seatsBooked / event.hallCapacity) * 100
    if profit_percentage >= 60:
        status = "Profit"
    else:
        status = "Loss"
    print(f"{event.eventName} attains {status}")

def main():
    n = int(input().strip())
    if n <= 0:
        print("Invalid Input")
        return
    
    events = []
    for _ in range(n):
        event_details = input().strip().split(',')
        if len(event_details) != 5:
            continue
        eventName, hallName, cost, hallCapacity, seatsBooked = event_details
        event = Event(eventName, hallName, cost, hallCapacity, seatsBooked)
        events.append(event)
    
    threads = []
    for event in events:
        t = threading.Thread(target=profit_or_loss, args=(event,))
        threads.append(t)
        t.start()
    
    for thread in threads:
        thread.join()

if __name__ == "__main__":
    main()
```
