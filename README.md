# golang_find_duplicate_number

Given an array of integers `nums` containing `n + 1` integers where each integer is in the range `[1, n]` inclusive.

There is only **one repeated number** in `nums`, return *this repeated number*.

You must solve the problem **without** modifying the array `nums` and uses only constant extra space.

## Examples

**Example 1:**

```
Input: nums = [1,3,4,2,2]
Output: 2

```

**Example 2:**

```
Input: nums = [3,1,3,4,2]
Output: 3

```

**Constraints:**

- `1 <= n <= $10^5$`
- `nums.length == n + 1`
- `1 <= nums[i] <= n`
- All the integers in `nums` appear only **once** except for **precisely one integer** which appears **two or more** times.

**Follow up:**

- How can we prove that at least one duplicate number must exist in `nums`?
- Can you solve the problem in linear runtime complexity?

## 解析

題目要假設一個 array 的值都是在 [1, n] 並且有 n+1個，並且只有一個重複的 value

而這個重複的 value 可能會出現兩次以上

最直覺的作法是先把 array 根據大小做排序

做完排序後 就可以由小到大檢查重複出現的值了 這樣會是 O(nlogn)

而要把找出重複值的演算法時間降低到 O(n)

需要透過這題目的限制看出來一些特性

1 所有值都在 [1,n]，並且有 n+1 個直， 這代表可以把每個值對應到一個 index

把每個 index 當作一個值， 而每個 value 看成一個指下一個 index 的指標

2 如果有重複的值，則這個串列會形成一個循環

![duplicate_number_in_list.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/25a73eb8-6390-4ef2-83e7-61a5286e3ab4/duplicate_number_in_list.png)

要找出循環可以透過 [Floyd's algorithm](https://en.wikipedia.org/wiki/Cycle_detection#Tortoise_and_hare)

只要讓兩個值一前一後

其中一個只走目前指向的下一步，另外一個走到目前指向下的下兩步

只要有循環代表前後一定會重和

而因為有 cycle 最長是 n -1 所以是時間複雜度是 O(n)

## 程式碼

```go
func findDuplicate(nums []int) int {
    slow, fast := nums[0], nums[0]
    slow = nums[slow]
    fast = nums[nums[fast]]
    for slow != fast {
        slow = nums[slow]
        fast = nums[nums[fast]]
    }
    
    slow = nums[0]
    for slow != fast {
        slow = nums[slow]
        fast = nums[fast]
    }
    return fast
}
```

## 困難點

1. 如果要在linear time 去解決這個問題，需要根據這個題目的限制去看出這個題目的關聯
2. 需要知道[Floyd's algorithm](https://en.wikipedia.org/wiki/Cycle_detection#Tortoise_and_hare)
3. 需要看出index 與值之間的關鏈

## Solve Point

- [x]  Understand what problem this question would like to solve
- [x]  Analysis Complexity