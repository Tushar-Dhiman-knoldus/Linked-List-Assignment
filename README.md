# Linked-List-Assignment

### Problem Statement

Create a generic class for a linked list which would have 4 methods - insert element, delete element, search element, traverse all the elements. 
Perform all the operations for various types like String, Int, Long.


### Source Code for the Problem

```
package com.knoldus.linkedlist

import scala.annotation.tailrec
import scala.util.Try

class Node[T](val data: T, var next: Node[T])

class LinkedList[T] {
  var head: Node[T] = null

  // Inserting data into the linked list.
  def insert(data: T): Unit = {
    @tailrec
    def helper(node: Node[T]): Unit = {
      if (node.next == null) {
        node.next = new Node[T](data, null)
      } else {
        helper(node.next)
      }
    }
    if (head == null) {
      head = new Node(data, null)
    } else {
      helper(head)
    }
  }

  // For deleting the element from the Linked List
  def delete(data: T): Unit = {
    @tailrec
    def helper(node: Node[T]): Unit = {
      if (node == null || node.next == null) return

      if (node.next.data == data) {
        node.next = node.next.next
      } else {
        helper(node.next)
      }
    }
    if (head != null && head.data == data) {
      head = head.next
    } else {
      helper(head)
    }
  }

  // Traverse the linked list and applies a function to each element
  def traverse(functionForTraverse: T => Unit): Unit = {
    @tailrec
    def helper(node: Node[T]): Unit = {
      if (node != null) {

        // Exception handled
        Try(functionForTraverse(node.data)) match {
          case scala.util.Success(_) => helper(node.next)
          case scala.util.Failure(e) => println(s"An exception occurred while traversing the linked list: ${e.getMessage}")
        }
      }
    }
    helper(head)
  }

  // For Searching the element in the linked list
  def search(data: T): Option[Node[T]] = {
    @tailrec
    def helper(node: Node[T]): Option[Node[T]] = {
      if (node == null) {
        None
      } else if (node.data == data) {
        Some(node)
      } else {
        helper(node.next)
      }
    }
    helper(head)
  }
}
```


### Test Case For The Source Code

```
package com.knoldus.linkedlist
import org.scalatest.funsuite.AnyFunSuite

class LinkedListSpec extends AnyFunSuite {

  test("insert and traverse elements in LinkedList") {
    val list = new LinkedList[Int]
    list.insert(1)
    list.insert(2)
    list.insert(3)
    assert(list.head.data == 1)
    assert(list.head.next.data == 2)
    assert(list.head.next.next.data == 3)

    val result = scala.collection.mutable.ListBuffer.empty[Int]
    list.traverse { data => result += data }
    assert(result == Seq(1, 2, 3))
  }

  test("delete element from LinkedList") {
    val list = new LinkedList[Int]
    list.insert(1)
    list.insert(2)
    list.insert(3)
    list.delete(2)
    assert(list.head.data == 1)
    assert(list.head.next.data == 3)

    val result = scala.collection.mutable.ListBuffer.empty[Int]
    list.traverse { data => result += data }
    assert(result == Seq(1, 3))
  }

  test("search for element in LinkedList") {
    val list = new LinkedList[String]
    list.insert("apple")
    list.insert("banana")
    list.insert("cherry")
    val node = list.search("banana")
    assert(node.isDefined)
    assert(node.get.data == "banana")
  }
}
```
