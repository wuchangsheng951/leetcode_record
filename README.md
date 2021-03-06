# leetcode record for Huiming and NaNa

## <a name="Catalogue"/>Catalogue
I love nana
* Easy
    * [1. Two Sum](#1)
    * [2. 整数反转](#2)
    * [1365. how-many-numbers-are-smaller-than-the-current-number](#1365)
    * [4.  Linked List Cycle](#4)
    * [771.  jewels-and-stones](#771)
    * [876. Middle of the Linked List](#876)
    * [565. Array Nesting](#565)
    * [881. Boats to Save People](#881)


## <a name="1"> 1.Two Sum
-------------------------
### Question:
>Given an array of integers, return indices of the two numbers such that they add up to a >specific target.You may assume that each input would have exactly one solution, and you may >not use the same element twice.

### Example:

```
        Given nums = [2, 7, 11, 15], target = 9,
        Because nums[0] + nums[1] = 2 + 7 = 9,
        return [0, 1].
```

### Thinking
> put all numbers into a dictionary. if target - element in list  = another element in list 
>We use a dictionary to record the index of the list. the first element must be in the >dictionary because of the final list is consist by two numbers.

### Solution:

```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        dic = {}
        for i in range(len(nums)):
            if target - nums[i] in dic:
                return [dic[target-nums[i]],i]
            dic[nums[i]] = i
```

[back to Catalogue](#Catalogue)

-------------------------

    

## <a name="2"> 2. 整数反转
-------------------------
### Question:
>给出一个 32 位的有符号整数，你需要将这个整数中每位上的数字进行反转。

### Example:

```
        示例 1:

输入: 123
输出: 321
 示例 2:

输入: -123
输出: -321
示例 3:

输入: 120
输出: 21
```

### 注意：
> 假设我们的环境只能存储得下 32 位的有符号整数，则其数值范围为 [−231,  231 − 1]。请根据这个假设，如果反转后整数溢出那么就返回 0。
### Solution:

```python
class Solution:
    def reverse_force(self, x: int) -> int:
        if -10 < x < 10:
            return x
        str_x = str(x)
        if str_x[0] != "-":
            str_x = str_x[::-1]
            x = int(str_x)
        else:
            str_x = str_x[:0:-1]
            x = int(str_x)
            x = -x
        return x if -2147483648 < x < 2147483647 else 0

        def reverse_better(
        self, 
        x: int) -> int:
       
        
        y, res = abs(x), 0
        # 则其数值范围为 [−2^31,  2^31 − 1]
        boundry = (1<<31) -1 if x>0 else 1<<31
        while y != 0:
            res = res*10 +y%10
            if res > boundry :
                return 0
            y //=10
        return res if x >0 else -res



```

[back to Catalogue](#Catalogue)

-------------------------


## <a name="1365"> 1365. how-many-numbers-are-smaller-than-the-current-number
-------------------------
### Question:
>Given the array nums, for each nums[i] find out how many numbers in the array are smaller than it. That is, for each nums[i] you have to count the number of valid j's such that j != i and nums[j] < nums[i].

Return the answer in an array.

### Example:

```
       Input: nums = [8,1,2,2,3]
        Output: [4,0,1,1,3]
        Explanation: 
        For nums[0]=8 there exist four smaller numbers than it (1, 2, 2 and 3). 
        For nums[1]=1 does not exist any smaller number than it.
        For nums[2]=2 there exist one smaller number than it (1). 
        For nums[3]=2 there exist one smaller number than it (1). 
        For nums[4]=3 there exist three smaller numbers than it (1, 2 and 2).
```

### Thinking
> two for loop
### Solution:

```python
class Solution:
    def smallerNumbersThanCurrent(self, nums: List[int]) -> List[int]:
        result = []
        for value in nums:
            num = len([i for i in nums if i < value ])           
            result.append(num)
        return result
```

[back to Catalogue](#Catalogue)

## <a name="4"> 4. Linked List Cycle
### Question:
>给定一个链表，判断链表中是否有环。

>为了表示给定链表中的环，我们使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。 如果 pos 是 -1，则在该链表中没有环。



### Example:

       示例 1：

输入：head = [3,2,0,-4], pos = 1
输出：true
解释：链表中有一个环，其尾部连接到第二个节点。

示例 2：

输入：head = [1,2], pos = 0
输出：true
解释：链表中有一个环，其尾部连接到第一个节点。

示例 3：

输入：head = [1], pos = -1
输出：false
解释：链表中没有环。


### Solution:

```python
法一：借助辅助空间

class Solution:
    def hasCycle(self, head: ListNode) -> bool:
        a = set() 
        while head:
            
            if head in a:
                return True

            a.add(head)
            head = head.next
        return False
```

>理解： 用一个set数据结构存储每个节点地址;
时间复杂度O(n*1)：访问每个节点O(n)+存储每个节点O(1);
空间复杂度O(n)

法二： 快慢指针法
``` c++
class Solution:
    def hasCycle(self, head: ListNode) -> bool:
        slow = fast = head
        # if not head：  # 没必要这样写可以加入while循环判断更简洁
        #     return False

        while fast and fast.next:  # 防止head为空和出现空指针的next的情况
            slow = slow.next
            fast = fast.next.next
            if slow is fast:
                return True

        return False
```

>理解：
思路：定义一个快指针和慢指针,每次快指针走2步，慢指针走1步，判断快指针是否与慢指针重逢
时间复杂度O(n+k)：
情况一：链表部分成环O(n)；
部分成环时，快指针先于慢指针进入环，慢指针进环时间O(n)；当快慢指针都进入环，假设环节点数量为K,当快慢指针第一次相遇时，快指针至少已经在环内已经比慢指针多走一圈(多走的这一圈是当慢指针进入后开始算的)，时间O(k)； 综上，时间复杂度O(k+n)，即为O(n)

>情况二：链表完全成环O(n)
        同起点，第一次相遇时，快指针已经在环内已经比慢指针多走一圈；时间复杂度O(n)

法三：递归标记法
``` c++
class Solution:
    def hasCycle(self, head: ListNode) -> bool:
        if not head:
            return False

        if head == 0xcafebabe:
            return True

        head.val = 0xcafebabe
        return self.hasCycle(head.next)

)


```

>理解：
思路：把访问过的值都进行赋值，如果链表完全成环，则必会出现重复值
时间复杂度：O(n);访问O(n)+赋值O(1)
空间复杂度：O(1)

[back to Catalogue](#Catalogue)

-------------------------
## <a name="771"> 771. Jewels and Stones
### Question:
>You're given strings J representing the types of stones that are jewels, and S representing the stones you have.  Each character in S is a type of stone you have.  You want to know how many of the stones you have are also jewels.

The letters in J are guaranteed distinct, and all characters in J and S are letters. Letters are case sensitive, so "a" is considered a different type of stone from "A".

### Example:

>Example 1:

Input: J = "aA", S = "aAAbbbb"
Output: 3
Example 2:

Input: J = "z", S = "ZZ"
Output: 0

### Thinking:
>too easy

### Solution:

```python
class Solution:
    def numJewelsInStones(self, J: str, S: str) -> int:
        J = list(J)
        # S = list(S)
        num = 0
        for i in J:
            num += S.count(i)
        return num
```
-------------------------

## <a name="876"> 876.Middle of the Linked List


### Question:

>Given a non-empty, singly linked list with head node head, return a middle node of linked list.

If there are two middle nodes, return the second middle node.

### Example:
#### Example 1:

>Input: [1,2,3,4,5]
Output: Node 3 from this list (Serialization: [3,4,5])
The returned node has value 3.  (The judge's serialization of this node is [3,4,5]).
Note that we returned a ListNode object ans, such that:
ans.val = 3, ans.next.val = 4, ans.next.next.val = 5, and ans.next.next.next = NULL.

#### Example 2:

>Input: [1,2,3,4,5,6]
Output: Node 4 from this list (Serialization: [4,5,6])
Since the list has two middle nodes with values 3 and 4, we return the second one.


### Thinking 

>The fast pointer's speed is 2 times  faster than slow one. So when the faster point get the end. The slow one will reach to the middle.

### Solution:


```c++

class Solution {
public:
    ListNode* middleNode(ListNode* head) {
        ListNode* slow = head;
        ListNode* fast = head;
        while (fast != NULL && fast->next != NULL) {
            slow = slow->next;
            fast = fast->next->next;
        }
        return slow;
    }
};
```
## <a name="565"> 565.Array Nesting

### Question:

>A zero-indexed array A of length N contains all integers from 0 to N-1. Find and return the longest length of set S, where S[i] = {A[i], A[A[i]], A[A[A[i]]], ... } subjected to the rule below.
Suppose the first element in S starts with the selection of element A[i] of index = i, the next element in S should be A[A[i]], and then A[A[A[i]]]… By that analogy, we stop adding right before a duplicate element occurs in S.

### Example:

>Input: A = [5,4,0,3,1,6,2]
Output: 4
Explanation: 
A[0] = 5, A[1] = 4, A[2] = 0, A[3] = 3, A[4] = 1, A[5] = 6, A[6] = 2.
One of the longest S[K]:
S[0] = {A[0], A[5], A[6], A[2]} = {5, 6, 2, 0}


### Thinking

by visited array recording the node

### code:

```python
class Solution:

    depth = 0 
    def dfs(self,nums, start, visited):
        start = nums[start]
        if start in visited:
            return 
        else:
            # visited.append(start)
            self.depth += 1            
            return self.dfs(nums,start,visited)
    
    
    def arrayNesting(self, nums: List[int]) -> int:
        length_list = []
        maxd = -1
        for i in range(len(nums)):
            self.depth = 1 
            visited = [nums[i]]
            self.dfs(nums,nums[i], visited)
            maxd = max(maxd,self.depth)
                
        return  maxd
```


-------------------------



## <a name="881"> 881.Boats to Save People


### Question:

>The i-th person has weight people[i], and each boat can carry a maximum weight of limit.
Each boat carries at most 2 people at the same time, provided the sum of the weight of those people is at most limit.
Return the minimum number of boats to carry every given person.  (It is guaranteed each person can be carried by a boat.)

### Example:
>Input: people = [1,2], limit = 3
Output: 1
Explanation: 1 boat (1, 2)
>Input: people = [3,2,2,1], limit = 3
Output: 3
Explanation: 3 boats (1, 2), (2) and (3)
>Input: people = [3,5,3,4], limit = 5
Output: 4
Explanation: 4 boats (3), (3), (4), (5)
>note:1 <= people.length <= 50000
1 <= people[i] <= limit <= 30000


### Thinking: 

>first sorted the array and use iteration to remove people from waiting array.


### Code:

```python
def calualate(people,limit):

    if people[-1] == limit:
        people.pop(-1)
        return people
    if limit - people[-1] > 0  :
        left = limit - people[-1]
        people.pop(-1)
        if len(people) == 0:
            return people
        if left - people[0] < 0 :
            return people

        while(left > 0 and len(people)>0):
            left = left - people[0]
            
            if left < 0 or len(people) == 0:
                break
            people.pop(0)
        return people
    
def numRescueBoats( people, limit) -> int:
    people = sorted(people)
    
    count = 0
    while(len(people)>0):
        print(people)

        people = calualate(people,limit)
        count+=1
    return count
    
```

-------------------------
