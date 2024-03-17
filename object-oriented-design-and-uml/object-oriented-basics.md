<h1 align="center">Object Oriented Basics</h1>

Object-oriented programming (OOP) is a style of programming that focuses on using objects to design and build applications. Contrary to procedure-oriented programming where programs are designed as blocks of statements to manipulate data, OOP organizes the program to combine data and functionality and wrap it inside something called an “Object”.

If you have never used an object-oriented programming language before, you will need to learn a few basic concepts before you can begin writing any code. This chapter will introduce some basic concepts of OOP:

* **Objects:** Objects represent a real-world entity and the basic building block of OOP. For example, an Online Shopping System will have objects such as shopping cart, customer, product item, etc.

* **Class:** Class is the prototype or blueprint of an object. It is a template definition of the attributes and methods of an object. For example, in the Online Shopping System, the Customer object will have attributes like shipping address, credit card, etc., and methods for placing an order, canceling an order, etc.

**A Simple Code Snippet of Class and Object**

**Class Code Snippet:**

```go

// ShoppingCart represents a shopping cart.
type ShoppingCart struct {
	total float64
	items map[string]int
}

// NewShoppingCart creates a new shopping cart instance.
func NewShoppingCart() *ShoppingCart {
	return &ShoppingCart{
		total: 0,
		items: make(map[string]int),
	}
}

// AddItem adds an item to the shopping cart.
func (sc *ShoppingCart) AddItem(itemName string, quantity int, price float64) {
	sc.total += float64(quantity) * price
	sc.items[itemName] += quantity
}

// RemoveItem removes an item from the shopping cart.
func (sc *ShoppingCart) RemoveItem(itemName string, quantity int, price float64) string {
	_, ok := sc.items[itemName]
	if ok == false {
		return fmt.Sprintf("the %v item is not present in cart", itemName)
	}
	if sc.items[itemName] < quantity {
		return fmt.Sprintf("only %v  %v item are there in the cart", quantity, itemName)
	}
	sc.total -= float64(quantity) * price
	if quantity == sc.items[itemName] {
		delete(sc.items, itemName)
	} else {
		sc.items[itemName] -= quantity
	}
	return ""
}

// Checkout processes the checkout for the shopping cart.
func (sc *ShoppingCart) Checkout(cashPaid float64) string {
	if cashPaid < sc.total {
		return fmt.Sprintf("You paid %v but cart amount is %v", cashPaid, sc.total)
	}
	balance := cashPaid - sc.total
	return fmt.Sprintf("Exchange amount: %v", balance)
}
```

**Object and it's Uses Code Snippet:**
```go
# Driver code
func main() {
	// Example usage
	cart := NewShoppingCart()
	cart.AddItem("apple", 3, 1.50)
	cart.AddItem("banana", 2, 0.75)

	fmt.Println("Total:", cart.total)
	fmt.Println("Items:", cart.items)

	err := cart.RemoveItem("orange", 1, 1.50)
	if err != "" {
		fmt.Println("the err is : ", err)
	}

	fmt.Println("Total after removal:", cart.total)
	fmt.Println("Items after removal:", cart.items)

	fmt.Println(cart.Checkout(5.00))
}
```

**Response:**
```
Total: 6
Items: map[apple:3 banana:2]
the err is :  the orange item is not present in cart
Total after removal: 6
Items after removal: map[apple:3 banana:2]
You paid 5 but cart amount is 6
``` 

<p align="center">
    <img src="/media-files/oop-principles.svg" alt="OOP Principles">
</p>

The four principles of object-oriented programming are encapsulation, abstraction, inheritance, and polymorphism.

* **Encapsulation:**
  -  Encapsulation is the mechanism of binding the data together and hiding it from the outside world. Encapsulation is achieved when each object keeps its state private so that other objects don’t have direct access to its state. Instead, they can access this state only through a set of public functions.
  -  Encapsulation in Go is achieved through the use of struct fields and methods. Go doesn't have traditional access control keywords like public, private, or protected as seen in languages like Java or C++. Instead, it uses naming conventions and package-level scoping to determine visibility. 

**Encapsulation Code Snippet:**

```go
package main

import (
	"fmt"
)

// Person represents a person with private fields and public methods.
type Person struct {
	name string // private field, starts with small alphabet, can be only updated with in the package 
	age  int    // private field
}

// NewPerson creates a new Person instance with the provided name and age.
func NewPerson(name string, age int) *Person {
	return &Person{
		name: name,
		age:  age,
	}
}

// GetName returns the name of the person.
func (p *Person) GetName() string {
	return p.name
}

// GetAge returns the age of the person.
func (p *Person) GetAge() int {
	return p.age
}

// SetName sets the name of the person.
func (p *Person) SetName(name string) {
	p.name = name
}

// SetAge sets the age of the person.
func (p *Person) SetAge(age int) {
	p.age = age
}

func main() {
	// Create a new Person instance
	person := NewPerson("John", 30)

	// Access and modify fields using methods
	fmt.Println("Name:", person.GetName())
	fmt.Println("Age:", person.GetAge())

	person.SetName("Alice")
	person.SetAge(25)

	fmt.Println("Updated Name:", person.GetName())
	fmt.Println("Updated Age:", person.GetAge())
}

```


**Response:**
```
Name: John
Age: 30
Updated Name: Alice
Updated Age: 25
```

* **Abstraction:** Abstraction can be thought of as the natural extension of encapsulation. It means hiding all but the relevant data about an object in order to reduce the complexity of the system. In a large system, objects talk to each other, which makes it difficult to maintain a large code base; abstraction helps by hiding internal implementation details of objects and only revealing operations that are relevant to other objects.

**Abstraction Code Snippet:**

```python
from abc import ABC, abstractmethod

class Parent(ABC):
  def common(self):
    print('In common method of Parent')

  @abstractmethod
  def vary(self):
    pass

class Child1(Parent):
  def vary(self):
    print('In vary method of Child1')

class Child2(Parent):
  def vary(self):
    print('In vary method of Child2')

# object of Child1 class
child1 = Child1()
child1.common()
child1.vary()

# object of Child2 class
child2 = Child2()
child2.common()
child2.vary()
```


**Response:**
```
In common method of Parent
In vary method of Child1
In common method of Parent
In vary method of Child2
```

* **Inheritance:** Inheritance is the mechanism of creating new classes from existing ones.

**Inheritance Code Snippet:**

```python
class Person(object): 

    def __init__(self, name):
        self.name = name

    def get_name(self):
        return self.name

    def is_employee(self):
        return False
   

class Employee(Person):

    def is_employee(self): 
        return True
   
# Driver code
emp = Person("Person 1")
print("{} is employee: {}".format(emp.get_name(), emp.is_employee()))

emp = Employee("Employee 1")
print("{} is employee: {}".format(emp.get_name(), emp.is_employee()))
```


**Response:**
```
Person 1 is employee: False
Employee 1 is employee: True
```

* **Polymorphism:** Polymorphism (from Greek, meaning “many forms”) is the ability of an object to take different forms and thus, depending upon the context, to respond to the same message in different ways. Take the example of a chess game; a chess piece can take many forms, like bishop, castle, or knight and all these pieces will respond differently to the ‘move’ message.

**Polymorphism Code Snippet:**

```python
class Bishops:

    def move(self):
        print("Bishops can move diagonally")

class Knights:

    def move(self):
        print("Knights can move two squares vertically and one square horizontally, or two squares horizontally and one square vertically")

# common interface
def move_test(chess_piece):
    chess_piece.move()
# Driver code

#instantiate objects
bishop = Bishops()
knight = Knights()

# passing the object
move_test(bishop)
move_test(knight)
```


**Response:**
```
Bishops can move diagonally
Knights can move two squares vertically and one square horizontally, or two squares horizontally and one square vertically
```
