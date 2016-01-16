![GraphKit](http://www.graphkit.io/GK/GraphKit.png)

## Welcome to GraphKit

GraphKit is a CoreData and Algorithm framework written in Swift. It's designed to simplify the complexities when working with CoreData while providing a seamless data-driven architecture for algorithm design.

## Features

- [x] Thread Safe
- [x] Store Any Data Type, Including Binary Data
- [x] Relationship Modeling
- [x] Action Modeling For Analytics
- [x] Model With Graph Theory and Set Theory
- [x] Faceted Search API
- [x] JSON Toolset
- [x] Probability Extension For Predictive Analytics
- [x] Library of Data Structures
- [x] Asynchronous / Synchronous Saving
- [x] Data-Driven Architecture
- [x] Asynchronous Model Observation
- [x] Comprehensive Unit Test Coverage
- [x] Example Projects

## Requirements

* iOS 8.0+ / Mac OS X 10.9+
* Xcode 7.2+

## Communication

- If you **need help**, use [Stack Overflow](http://stackoverflow.com/questions/tagged/graphkit). (Tag 'graphkit')
- If you'd like to **ask a general question**, use [Stack Overflow](http://stackoverflow.com/questions/tagged/graphkit).
- If you **found a bug**, _and can provide steps to reliably reproduce it_, open an issue.
- If you **have a feature request**, open an issue.
- If you **want to contribute**, submit a pull request.

## Installation
> **Embedded frameworks require a minimum deployment target of iOS 8 or OS X Mavericks (10.9).**
> - [Download GraphKit](https://github.com/CosmicMind/GraphKit/archive/master.zip)

## CocoaPods

[CocoaPods](http://cocoapods.org) is a dependency manager for Cocoa projects. You can install it with the following command:

```bash
$ gem install cocoapods
```

> CocoaPods 0.39.0+ is required to build GraphKit 4.0.0+.

To integrate GraphKit into your Xcode project using CocoaPods, specify it in your `Podfile`:

```ruby
source 'https://github.com/CocoaPods/Specs.git'
platform :ios, '8.0'
use_frameworks!

pod 'GK', '~> 4.0'
```

Then, run the following command:

```bash
$ pod install
```

## Carthage

Carthage is a decentralized dependency manager that builds your dependencies and provides you with binary frameworks.

You can install Carthage with Homebrew using the following command:

```bash
$ brew update
$ brew install carthage
```
To integrate GraphKit into your Xcode project using Carthage, specify it in your Cartfile:

```bash
github "CosmicMind/GraphKit"
```

Run `carthage update` to build the framework and drag the built `GraphKit.framework` into your Xcode project.

## Changelog

GraphKit is a growing project and will encounter changes throughout its development. It is recommended that the [Changelog](https://github.com/CosmicMind/GraphKit/wiki/Changelog) be reviewed prior to updating versions.

## Examples

* Visit the Examples directory to see example projects using GraphKit.

## A Tour  

* [Entity](#entity)
* [Relationship](#relationship)
* [Action](#action)
* [Groups](#groups)
* [Probability](#probability)
* [Data Driven](#datadriven)
* [Faceted Search](#facetedsearch)
* [JSON](#json)
* [Data Structures](#datastructures)
* [DoublyLinkedList](#doublylinkedlist)
* [Stack](#stack)
* [Queue](#queue)
* [Deque](#deque)
* [RedBlackTree](#redblacktree)
* [SortedSet](#sortedset)
* [SortedMultiSet](#sortedmultiset)
* [SortedDictionary](#sorteddictionary)
* [SortedMultiDictionary](#sortedmultidictionary)

<a name="entity"></a>
## Entity

An **Entity** is a model object that **represents a person, place, or thing**. For example, a Company, Photo, Video, User, Person, and Note. In code, the following is how this would look.

```swift
let graph: Graph = Graph()

// Create a Person Entity.
let person: Entity = Entity(type: "Person")
person["firstName"] = "Elon"
person["lastName"] = "Musk"

// Create a Company Entity.
let company: Entity = Entity(type: "Company")
company["name"] = "Tesla Motors"

graph.save()
```

[Learn More About Entities](https://github.com/CosmicMind/GraphKit/wiki/Entity)

<a name="relationship"></a>
## Relationship

A **Relationship** is a model object that **forms a relationship between two Entities**. For example, "Person is an Employee of Company". _Person_ and _Company_ are two Entities that form a Relationship -- of type _Employee_. In code, the following is how this would look.

```swift
let graph: Graph = Graph()

// Create a Person Entity.
let person: Entity = Entity(type: "Person")
person["firstName"] = "Mark"
person["lastName"] = "Zuckerberg"

// Create a Company Entity.
let company: Entity = Entity(type: "Company")
company["name"] = "Facebook"

// Create an Employee Relationship.
let employee: Relationship = Relationship(type: "Employee")
employee["startDate"] = "February 4, 2004"

// Form the relationship.
employee.subject = person
employee.object = company

graph.save()
```

Notice that information about the relationship is stored within the _Employee_ Relationship leaving both _Mark_ and _Facebook_ to form other relationships freely. This is a key principal when using Relationships.

[Learn More About Relationships](https://github.com/CosmicMind/GraphKit/wiki/Relationship)

<a name="action"></a>
## Action

An **Action** is a model object that **forms a relationship between a collection of Entity subjects and a collection of Entity objects**. For example, "Person Purchased many Books". The _Purchased_ Action captures the _Person_ Entity and _Book_ Entities in a single relationship that may be used later to ask questions like, "what Books has the Person Purchased?", or "Has the Person Purchased a Physics Book?". In code, the following is how this would look.

```swift
let graph: Graph = Graph()

let person: Entity = Entity(type: "Person")
let books: Array<Entity> = graph.searchForEntity(types: ["Book"])

let purchased: Action = Action(type: "Purchased")
purchased.addSubject(person)

for book in books {
	purchased.addObject(book)
}

graph.save()
```

[Learn More About Actions](https://github.com/CosmicMind/GraphKit/wiki/Action)

<a name="groups"></a>
## Groups

Groups are used to organize Entities, Relationships, and Actions into different collections from their types. This allows multiple types to exist in a single collection. For example, a Photo and Video Entity type may exist in a group called Media. Another example may be including a Photo and Book Entity type in a Favorites group for your users' account. Below are examples of using groups.

```swift
let graph: Graph = Graph()

let photo: Entity = Entity(type: "Photo")
photo.addGroup("Media")
photo.addGroup("Favorites")
photo.addGroup("Holiday Album")

let video: Entity = Entity(type: "Video")
video.addGroup("Media")

let book: Entity = Entity(type: "Book")
book.addGroup("Favorites")
book.addGroup("To Read")

graph.save()

// Searching groups.
let favorites: Array<Entity> = graph.searchForEntity(groups: ["Favorites"])
```

<a name="probability"></a>
## Probability

Probability is a core feature within GraphKit. Your application may be completely catered to your users' habits and usage. To demonstrate this wonderful feature, let's look at some examples:

Determining the probability of rolling a 3 using a die of 6 numbers.

```swift
let die: Array<Int> = Array<Int>(arrayLiteral: 1, 2, 3, 4, 5, 6)
print(die.probabilityOf(3)) // Output: 0.166666666666667
```

The expected value of rolling a 3 or 6 with 100 trials using a die of 6 numbers.

```swift
let die: Array<Int> = Array<Int>(arrayLiteral: 1, 2, 3, 4, 5, 6)
print(die.expectedValueOf(100, elements: 3, 6)) // Output: 33.3333333333333
```

Recommending a Physics book if the user is likely to buy a Physics book.

```swift
let graph: Graph = Graph()

// Get all the Purchased Actions.
let purchased: Array<Action> = graph.searchForAction(types: ["Purchased"])

// Determine the probability of getting a Book in the Physics group.
let probabilityOfX: Double = purchased.probabilityOf { (action: Action) in
	if let entity: Entity = action.objects.first {
		if "Book" == entity.type {
			return entity.hasGroup("Physics")
		}
	}
	return false
}

if 33.33 < probabilityOfX {
	// Recommend a Physics book.
}
```

<a name="datadriven"></a>
## Data Driven

As data moves through your application, the state of information may be observed to create a reactive experience. Below is an example of watching when a "User Clicked a Button".

```swift
// Set the UIViewController's Protocol to GraphDelegate.
let graph: Graph = Graph()
graph.delegate = self

graph.watchForAction(types: ["Clicked"])

let user: Entity = Entity(type: "User")
let clicked: Action = Action(type: "Clicked")
let button: Entity = Entity(type: "Button")

clicked.addSubject(user)
clicked.addObject(button)

graph.save()

// Delegate method.
func graphDidInsertAction(graph: Graph, action: Action) {
    switch(action.type) {
    case "Clicked":
      print(action.subjects.first?.type) // User
      print(action.objects.first?.type) // Button
    case "Swiped":
      // Handle swipe.
    default:break
    }
 }
```

<a name="facetedsearch"></a>
## Faceted Search

To explore the intricate relationships within GraphKit, the search API adopts a faceted design. The below examples show how to use the _Graph_ search API:

Searching multiple Entity types.

```swift
let graph: Graph = Graph()
let collection: Array<Entity> = graph.searchForEntity(types: ["Photo", "Video"])
```

Searching multiple Entity groups.

```swift
let graph: Graph = Graph()
let collection: Array<Entity> = graph.searchForEntity(groups: ["Media", "Favorites"])
```

Searching multiple Entity properties.

```swift
let graph: Graph = Graph()
let collection: Array<Entity> = graph.searchForEntity(properties: [(key: "name", value: "Eve"), ("age", 27)])
```

Searching multiple facets simultaneously will aggregate all results into a single collection.

```swift
let graph: Graph = Graph()
let collection: Array<Entity> = graph.searchForEntity(types: ["Photo", "Friends"], groups: ["Media", "Favorites"])
```

Filters may be used to narrow in on a search result. For example, searching a book title and group within purchases.

```swift
let graph: Graph = Graph()

let collection: Array<Action> = graph.searchForAction(types: ["Purchased"]).filter { (action: Action) -> Bool in
	if let entity: Entity = action.objects.first {
		if "Book" == entity.type && "The Holographic Universe" == entity["title"] as? String  {
			return entity.hasGroup("Physics")
		}
	}
	return false
}
```

<a name="json"></a>
## JSON

JSON is a widely used format for serializing data. GraphKit comes with a JSON toolset. Below are some examples of its usage.

```swift
// Serialize Dictionary.
let data: NSData? = JSON.serialize(["user": ["username": "daniel", "password": "abc123", "token": 123456789]])

// Parse NSData.
let j1: JSON? = JSON.parse(data!)
print(j1?["user"]?["username"]?.asString) // Output: "daniel"

// Stringify.
let stringified: String? = JSON.stringify(j1!)
print(stringified) // Output: "{\"user\":{\"password\":\"abc123\",\"token\":123456789,\"username\":\"daniel\"}}"

// Parse String.
let j2: JSON? = JSON.parse(stringified!)
print(j2?["user"]?["token"]?.asInt) // Output: 123456789
```

<a name="datastructures"></a>
## Data Structures

GraphKit comes packed with some powerful data structures to help write algorithms. The following structures are included: DoublyLinkedList, Stack, Queue, Deque, RedBlackTree, SortedSet, SortedMultiSet, SortedDictionary, and SortedMultiDictionary.

<a name="doublylinkedlist"></a>
## DoublyLinkedList

The DoublyLinkedList data structure is excellent for large growing collections of data. Below is an example of its usage.

```swift
let listA: DoublyLinkedList<Int> = DoublyLinkedList<Int>()
listA.insertAtFront(3)
listA.insertAtFront(2)
listA.insertAtFront(1)

let listB: DoublyLinkedList<Int> = DoublyLinkedList<Int>()
listB.insertAtBack(4)
listB.insertAtBack(5)
listB.insertAtBack(6)

let listC: DoublyLinkedList<Int> = listA + listB

listC.cursorToFront()
repeat {
	print(listC.cursor)
} while nil != listC.next
// Output:
// 1
// 2
// 3
// 4
// 5
// 6
```

<a name="stack"></a>
## Stack

The Stack data structure is a container of objects that are inserted and removed according to the last-in-first-out (LIFO) principle. Below is an example of its usage.

```swift
let stack: Stack<Int> = Stack<Int>()
stack.push(1)
stack.push(2)
stack.push(3)

while !stack.isEmpty {
	print(stack.pop())
}
// Output:
// 3
// 2
// 1
```

<a name="queue"></a>
## Queue

The Queue data structure is a container of objects that are inserted and removed according to the first-in-first-out (FIFO) principle. Below is an example of its usage.

```swift
let queue: Queue<Int> = Queue<Int>()
queue.enqueue(1)
queue.enqueue(2)
queue.enqueue(3)

while !queue.isEmpty {
	print(queue.dequeue())
}
// Output:
// 1
// 2
// 3
```

<a name="deque"></a>
## Deque

The Deque data structure is a container of objects that are inserted and removed according to the first-in-first-out (FIFO) and last-in-first-out (LIFO) principle. Essentially, a Deque is a Stack and Queue combined. Below are examples of its usage.

```swift
let dequeA: Deque<Int> = Deque<Int>()
dequeA.insertAtBack(1)
dequeA.insertAtBack(2)
dequeA.insertAtBack(3)

while !dequeA.isEmpty {
	print(dequeA.removeAtFront())
}
// Output:
// 1
// 2
// 3

let dequeB: Deque<Int> = Deque<Int>()
dequeB.insertAtBack(4)
dequeB.insertAtBack(5)
dequeB.insertAtBack(6)

while !dequeB.isEmpty {
	print(dequeB.removeAtBack())
}
// Output:
// 6
// 5
// 4
```

<a name="redblacktree"></a>
## RedBlackTree

A RedBlackTree is a Balanced Binary Search Tree that maintains insert, remove, update, and search operations in a complexity of O(logn). The GraphKit implementation of a RedBlackTree also includes an order-statistic, which allows the data structure to be accessed using subscripts like an array or dictionary. RedBlackTrees may store unique keys or non-unique key values. Below is an example of its usage.

```swift
let rbA: RedBlackTree<Int, Int> = RedBlackTree<Int, Int>(uniqueKeys: true)

for var i: Int = 1000; 0 < i; --i {
	rbA.insert(1, value: 1)
	rbA.insert(2, value: 2)
	rbA.insert(3, value: 3)
}
print(rbA.count) // Output: 3
```

<a name="sortedset"></a>
## SortedSet

SortedSets are a powerful data structure for algorithm and analysis design. Elements within a SortedSet are unique and insert, remove, and search operations have a complexity of O(logn). The GraphKit implementation of a SortedSet also includes an order-statistic, which allows the data structure to be accessed using an index subscript like an array. Below are examples of its usage.

```swift
let setA: SortedSet<Int> = SortedSet<Int>(elements: 1, 2, 3) // Sorted: [1, 2, 3]
let setB: SortedSet<Int> = SortedSet<Int>(elements: 4, 3, 6) // Sorted: [3, 4, 6]

let setC: SortedSet<Int> = SortedSet<Int>(elements: 7, 1, 2) // Sorted: [1, 2, 7]
let setD: SortedSet<Int> = SortedSet<Int>(elements: 1, 7) // Sorted: [1, 7]

let setE: SortedSet<Int> = SortedSet<Int>(elements: 1, 6, 7) // Sorted: [1, 6, 7]

// Union.
print((setA + setB).count) // Output: 5
print(setA.union(setB).count) // Output: 5

// Intersect.
print(setC.intersect(setD).count) // Output: 2

// Subset.
print(setD < setC) // true
print(setD.isSubsetOf(setC)) // true

// Superset.
print(setD > setC) // false
print(setD.isSupersetOf(setC)) // false

// Contains.
print(setE.contains(setA.first!)) // true

// Probability.
print(setE.probabilityOf(setA.first!, setA.last!)) // 0.333333333333333
```

<a name="sortedmultiset"></a>
## SortedMultiSet

A SortedMultiSet is identical to a SortedSet, except that a SortedMultiSet allows non-unique elements. Look at [SortedSet](#sortedset) for examples of its usage.

<a name="sorteddictionary"></a>
## SortedDictionary

A SortedDictionary is a powerful data structure that maintains a sorted set of keys with value pairs. Keys within a SortedDictionary are unique and insert, remove, update, and search operations have a complexity of O(logn).

<a name="sortedmultidictionary"></a>
## SortedMultiDictionary

A SortedMultiDictionary is identical to a SortedDictionary, except that a SortedMultiDictionary allows non-unique keys. Below is an example of its usage.

```swift
let graph: Graph = Graph()

let students: Array<Entity> = graph.searchForEntity(types: ["Student"])

let dict: SortedMultiDictionary<String, Entity> = SortedMultiDictionary<String, Entity>()

// Do something with an alphabetically SortedMultiDictionary of Student Entities.
for student in students {
	dict.insert(student["name"] as! String, value: student)
}
```

## License

Copyright (C) 2015 - 2016, Daniel Dahan and CosmicMind, Inc. <http://cosmicmind.io>. All rights reserved.

Redistribution and use in source and binary forms, with or without modification, are permitted provided that the following conditions are met:

*   Redistributions of source code must retain the above copyright notice, this     
    list of conditions and the following disclaimer.

*   Redistributions in binary form must reproduce the above copyright notice,
    this list of conditions and the following disclaimer in the documentation
    and/or other materials provided with the distribution.

*   Neither the name of GraphKit nor the names of its
    contributors may be used to endorse or promote products derived from
    this software without specific prior written permission.

THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS" AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT HOLDER OR CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.
