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

* **Abstraction:**
  - Abstraction can be thought of as the natural extension of encapsulation. It means hiding all but the relevant data about an object in order to reduce the complexity of the system. In a large system, objects talk to each other, which makes it difficult to maintain a large code base; abstraction helps by hiding internal implementation details of objects and only revealing operations that are relevant to other objects.
  - Abstraction in Go refers to the practice of hiding the implementation details of a type or package and exposing only the necessary functionality to the users of that type or package. This allows users to interact with the abstraction without needing to understand how it is implemented internally.
  - In Go, abstraction can be achieved through several mechanisms:
    1. Interfaces: Interfaces define a set of method signatures. Types that implement these method signatures implicitly satisfy the interface. By defining interfaces, you can interact with objects based on their behavior rather than their concrete type, promoting abstraction.
    2. Packages: Go packages provide a way to organize code into separate, reusable units. By exporting specific functions, types, and variables from a package, you can control what is accessible to users of the package, hiding implementation details.
    3. Composite Types: Structs and arrays in Go can encapsulate multiple values into a single unit. By exposing only certain fields or methods of a struct, you can abstract away the internal representation of the data.

  - Here's a simple example demonstrating abstraction in Go using interfaces: 

**Abstraction Code Snippet:**

```go
package main

import (
	"fmt"
)

// Animal is an interface that defines the Speak method.
type Animal interface {
	Speak() string
}

// Dog is a struct representing a dog.
type Dog struct {
	Name string
}

// Speak implements the Speak method for Dog.
func (d Dog) Speak() string {
	return fmt.Sprintf("%s says: Woof!", d.Name)
}

// Cat is a struct representing a cat.
type Cat struct {
	Name string
}

// Speak implements the Speak method for Cat.
func (c Cat) Speak() string {
	return fmt.Sprintf("%s says: Meow!", c.Name)
}

func main() {
	dog := Dog{Name: "Fido"}
	cat := Cat{Name: "Whiskers"}

	animals := []Animal{dog, cat} // storing structs in the interface type

	for _, animal := range animals {
		fmt.Println(animal.Speak())
	}
}

```


**Response:**
```
Fido says: Woof!
Whiskers says: Meow!

```

* **Inheritance:**
   - Inheritance is the mechanism of creating new classes from existing ones.
   - In Go, there's no direct support for class-based inheritance as in object-oriented languages like Java or Python. However, you can achieve similar behavior using composition and embedding.

   - Composition:
                 Composition is a way of building types by combining other types. You can embed one struct within another to achieve composition. The fields and methods of the embedded struct become part of the outer struct.

**Inheritance Code Snippet:**

```go
 package main

import (
	"fmt"
)

// Animal represents an animal.
type Animal struct {
	Name string
}

// Speak method for animals.
func (a *Animal) Speak() {
	fmt.Println("Animal sound")
}

// Dog represents a dog.
type Dog struct {
	Animal // Embedding Animal into Dog
	Breed  string
}

// Speak method for dogs.
func (d *Dog) Speak() {
	fmt.Println("Woof!")
}

func main() {
	dog := Dog{
		Animal: Animal{Name: "Buddy"},
		Breed:  "Labrador",
	}

	fmt.Println("Name:", dog.Name) // Accessing embedded field
	fmt.Println("Breed:", dog.Breed)

	dog.Speak() // Call Speak method of Dog
}

```


**Response:**
```
Name: Buddy
Breed: Labrador
Woof!
```

* **Polymorphism:**
  - Polymorphism (from Greek, meaning “many forms”) is the ability of an object to take different forms and thus, depending upon the context, to respond to the same message in different ways.
  - Take the example of a chess game; we can demonstrate polymorphism by representing different types of chess pieces (e.g., pawn, rook, bishop) as different types that implement a common interface, such as Piece.

**Polymorphism Code Snippet:**

```go
package main

import "fmt"

// Piece represents a chess piece.
type Piece interface {
	Move() string
}

// Pawn represents a pawn chess piece.
type Pawn struct{}

// Move implements the Move method for Pawn.
func (p Pawn) Move() string {
	return "Forward 1 square"
}

// Rook represents a rook chess piece.
type Rook struct{}

// Move implements the Move method for Rook.
func (r Rook) Move() string {
	return "Forward, backward, left, or right any number of squares"
}

// Bishop represents a bishop chess piece.
type Bishop struct{}

// Move implements the Move method for Bishop.
func (b Bishop) Move() string {
	return "Diagonally any number of squares"
}

func main() {
	// Create an array of chess pieces
	pieces := []Piece{
		Pawn{},
		Rook{},
		Bishop{},
	}

	// Move each chess piece
	for _, piece := range pieces {
		fmt.Println(piece.Move())
	}
}

```


**Response:**
```
Forward 1 square
Forward, backward, left, or right any number of squares
Diagonally any number of squares

```
