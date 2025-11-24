# odin-linked-lists

This repository presents an answer to the [Project: Linked Lists](https://www.theodinproject.com/lessons/javascript-linked-lists) task - to create a singly linked list data structure using JavaScript.

## Implemented functions

Firstly, a factory function is created for initializing a node with following attributes: `value` and `nextNode`, which is accessed externally by `node.next`. Since JavaScript always accesses variables by reference rether than by value, the assignment of the reference to the node can be done as simple as `node1.next = node2`. Finnaly, `toString` converts the node into a string object using its `value`.

The linked list factory declares three attribute: `head`, `tail` and `size`, with `size` attribute being used for indicating a size of the list, that is the amount of nodes of list. This allows, if it's needed to get the size of the list, to not traverse the entire list and counting the nodes every time. A better practice, however, would be creating getter functions for all of these attributes rather than exposing them.

`prepend`, `append`, `at`, `pop`, `insertAt` and `removeAt` all run in constant time ($$O(1)$$). `contains` and `find` functions's running time is linear to the size of the list ($$O(n)$$) - the worst-case scenario occurs when the node with specified `value` is `tail`. This goes in line with worst-case running times described in [Big O Cheatsheet](https://www.bigocheatsheet.com/).

> [!NOTE]
> Although `at`, `pop`, `insertAt` and `removeAt` require traversing the list just like `contains` and `find`, their running times are independent of the size of the list.

The full code snippet implementing the data structure is presented below.
```js
function createLinkedList() {
  function createNode(v) {
    let value = v || null;
    let nextNode = null;

    function toString() {
      return value.toString();
    }

    return {
      value,
      next: nextNode,
      toString,
    };
  }

  function append(value) {
    let newNode = createNode(value);
    if (tail) {
      tail.next = newNode;
      tail = newNode;
    } else {
      head = newNode;
      tail = newNode;
    }
    size++;
  }

  function prepend(value) {
    let newNode = createNode(value);
    if (head) {
      newNode.next = head;
    } else {
      tail = newNode;
    }
    head = newNode;
    size++;
  }

  function at(index) {
    if (index < 0 || index >= size) {
      return null; // Index out of bounds
    }
    let currentNode = head;
    for (let i = 0; i < index; i++) {
      currentNode = currentNode.next;
    }
    return currentNode;
  }

  function pop() {
    if (!head) return; // Exit if the list is empty
    if (size === 1) {
      head = null;
      tail = null;
    } else {
      let currentNode = head;
      while (currentNode.next !== tail) {
        currentNode = currentNode.next;
      }
      currentNode.next = null;
      tail = currentNode;
    }
    size--;
  }

  function contains(value) {
    let currentNode = head;
    while (currentNode !== null) {
      if (currentNode.value === value) {
        return true;
      }
      currentNode = currentNode.next;
    }
    return false;
  }

  function find(value) {
    let currentNode = head;
    let index = 0;
    while (currentNode !== null) {
      if (currentNode.value === value) {
        return index;
      }
      index++;
      currentNode = currentNode.next;
    }
    return null;
  }

  function insertAt(value, index) {
    if (index <= 0) {
      prepend(value);
      return;
    }
    if (index >= size) {
      append(value); // Insert at the end if index is out of bounds
      return;
    }

    let predecessor = head;
    for (let i = 0; i < index - 1; i++) {
      predecessor = predecessor.next;
    }

    let newNode = createNode(value);
    newNode.next = predecessor.next;
    predecessor.next = newNode;
    size++;
  }

  function removeAt(index) {
    if (index < 0 || index >= size) {
      console.error("Index out of bounds");
      return;
    }

    if (index === 0) {
      head = head.next;
      if (size === 1) {
        tail = null;
      }
    } else {
      let predecessor = head;
      for (let i = 0; i < index - 1; i++) {
        predecessor = predecessor.next;
      }

      let toRemove = predecessor.next;
      predecessor.next = toRemove.next;

      if (index === size - 1) {
        tail = predecessor;
      }
    }

    size--;
  }

  function toString() {
    if (!head) return '( null )';

    let str = '';
    let currentNode = head;
    while (currentNode !== null) {
      str += `( ${currentNode.toString()} ) -> `;
      currentNode = currentNode.next;
    }

    str += '( null )';

    return str;
  }

  let head = null;
  let tail = null;
  let size = 0;

  return {
    append,
    prepend,
    size,
    head,
    tail,
    at,
    pop,
    contains,
    find,
    toString,
    insertAt,
    removeAt,
  };
}
```
