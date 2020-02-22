# Two-Pointer Search
Two pointer search algorithm are normally used to refer to searching that use two pointer in one for/while loop over the given data structure. Therefore, this part of algorithm gives linear performance as of O(n). While, it does not refer to situation such as searching a pair of items in an array that sums up to a given target value, then two nested for loops are needed to search all possible pairs. There are different ways to put these two pointers:
1. **Equi-directional**: Both start from the beginning: we have **slow-faster pointer**, **sliding window algorithm**.
2. **Opposite-directional**: One at the start and the other at the end, they move close to each other and meet in the middle.

In order to use two pointer most times the data structure needs to be ordered in some way, and decrease the time complexity from O(n^2) or O(n^3) of two/three nested for/while loops to O(n) of just one loop with two pointers and search each item just one time. In some cases, the time complexity is highly dependable on the data and the criteria we set.

## Slow-fast pointer
**Find middle node of linked list**. The simplest example of slow-fast pointer application is to get the middle node of a given linked list.
```
Example 1 (odd length)
Input: [1,2,3,4,5]
Output: Node 3 from this list (Serialization: [3,4,5])

Example 2 (even length)
Input: [1,2,3,4,5,6]
Output: Node 4 from this list (Serialization: [4,5,6])
```
We have place two pointer simultaneously at the head node, each one moves at different paces, the slow pointer moves one step and the fast moves two steps instead. When the fast pointer reached the end , the slow pointer will stop at the middle. For the loop, we only to check on the faster pointer, make sure fast pointer and fast.next is not. None, so that we can successfully visit the fast.next.next. When the length is odd, fast pointer will point at the end node because fast.next is None, when its even, fast pointer will point at None node, it terminates because fast is None.
```python
def middleNode(self, head):
  slow, fast = head, head
  while fast and fast.next:
    fast = fast.next.next
    slow = slow.next
  return slow  
```

## Floyd's Cycle Detection(Floyd's Tortoise and Hare)
Given a linked list which has a cycle. To check the existence of the cycle is quite simple. We do exactly the same as traveling by the slow and fast pointer, each at one and two steps. The code is pretty much the same with only difference been that after we change the fast and slow pointer, we check if they are same node. If true, a cycle is detected, else not.
```python
def hasCylce(self, head):
  slow = fast = head
  while fast and fast.next:
    slow = slow.next
    fast = fast.next.next
    if slow == fast :
      return True
  return False    
```
In order to know the starting node of the cycle. Here, we set the distance of the starting node of the cycle from the head is x, and y is the distance from the start node to the slow and fast pointer's node and z is the remaining distance from the meeting point to the start node.
Now, let's try to device the algorithm. Both slow and fast pointer starts at position 0, the node index they travel each step is: [0,1,2,3,...,k] and [0,2,4,6,...,2k] for slow and fast pointer respectively. Therefore the total distance traveled by the slow pointer is half of the distance travelled by slow pointer to be ds = x + y, and for the fast pointer df = x + y + z + y = x + 2y + z. With the relation 2*ds = df. We will eventually get x = z. Therefore, by moving slow pointer to the start of the linked list after meeting point and making both slow and faster move to one node at a time, they will meet at the starting node of the cycle.
```python
def detectCyle(self, head):
  slow = fast = head
  byCyle = False
  while fast and fast.next:
    slow = slow.next
    fast = fast.next.next
    if slow = fast:
      byCyle = True
      break
  if not bCycle:
    return None
  while fast and slow != fast:
    slow = slow.next
    fast = fast.next

  return slow        
```

## Opposite-directional Two pointer
Two pointer is usually used for searching a pair in the array. There are cases the data is organized in a way that we can search all the result space by placing two pointers each at the start and rear of the array and move them to each other and eventually meet and terminate the search process. The search target should help us decide which pointer to move at that step. This way, each item in the array is guaranteed to be visited at most time by one of the two pointers, thus making the time complexity to be O(n). Binary search used the technique of two pointers too, the left and right pointer together decides the current searching space, but it erase of half searching space at each step instead.
