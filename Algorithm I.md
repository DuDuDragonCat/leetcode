# Algorithm i

- Update: 20210925
- Ques: 704, 278, 189, 283, 167, 344, 557, 876, 19, 3, 567

### 704. Binary Search

Same Ques Sol:
  1. 278. First Bad Version
  2. 35. Search Insert Position

``` Python
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        left, right = 0, len(nums)-1
        while left <= right:
            mid = left + (right-left)//2
            if nums[mid] == target:
                return mid
            if target > nums[mid]:
                left = mid + 1
            else:
                right = mid - 1
        return -1
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        # 只要L, R 就可以。
        ind_left, ind_half, ind_right = 0, floor(len(nums)/2), len(nums)
        while True:
            if nums[ind_half]==target:
                return ind_half
            elif nums[ind_half]<target:
                ind_left = ind_half
            else:
                ind_right = ind_half
            if ind_left > ind_right:
                break
            ind_half = floor((ind_right-ind_left)/2)

            if ind_half >= ind_right or ind_half <= ind_left:
                break
        return -1
```

### 189. Rotate Array

一開始的想法是把需要挪動到最後的數列搬到最後，之後把挪動的數列砍了。

``` Python
class Solution:
    def rotate(self, nums: List[int], k: int) -> None:
        k = k % len(nums)
        if k==0 : return
        tmpInd = len(nums)-k
        nums.extend(nums[ :tmpInd])
        del nums[ :tmpInd]
```

這解法是讓array中每個元素都項後面k個元素做替換。此方法和上個方法相比空間用的比較少。

``` Python
class Solution:
    def rotate(self, nums: List[int], k: int) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """
        k = k % len(nums)
        count = 0
        start = 0
        while count < len(nums):
            current = start
            prev = nums[start] # 先存好啟始的值
            while True:
                # 下個替換的位置
                next = (current + k) % len(nums)
                # 存好要替換的值後，進行替換。
                temp = nums[next]
                nums[next] = prev
                prev = temp
                # 更改替換的位置
                current = next
                count += 1

                if start == current:
                    break
            # 每次替換完一輪後，將啟始位置增加一。
            start += 1
```

### 283. Move Zeroes

最簡單的想法就是刪除0後，在array後面刪除一樣多的0。

``` Python
class Solution:
    def moveZeroes(self, nums: List[int]) -> None:
        if len(nums)==1: return
        zero_count = 0
        check_ind = 0
        while check_ind < len(nums):
            if nums[check_ind]==0:
                zero_count += 1
                del nums[check_ind]
            else:
                check_ind += 1
        nums.extend([0]*zero_count)
# 更簡潔的寫法
class Solution:
    def moveZeroes(self, nums: List[int]) -> None:
        count=nums.count(0)
        nums[:]=[i for i in nums if i != 0]
        nums+=[0]*count
```

two pointer的概念，將不是0的值依序往前換。

``` Python
class Solution:
    def moveZeroes(self, nums: List[int]) -> None:
        non_zero = 0
        for i in range(len(nums)):
            if nums[i] != 0:
                nums[non_zero],nums[i] = nums[i],nums[non_zero]
                non_zero +=1
```

### 167. Two Sum II - Input array is sorted

利用dict尋找時只需要O(1)，和two-pointer比不過需要額外的空間。

``` Python
class Solution:
    def twoSum(self, numbers: List[int], target: int) -> List[int]:
        dic = {}
        for i, num in enumerate(numbers):
            if target-num in dic:
                return [dic[target-num]+1, i+1]
            dic[num] = i
```

### 557. Reverse Words in a String III

Same Ques Sol:
    1. 344. Reverse String

334和557核心想法為透過two pointer替換左右兩邊的字元，這樣可以在O(n)內完成。

``` Python
while l <= r: # <=遇到字串為奇數時才會處理中間的值。
    tmpList[l], tmpList[r] = tmpStr[r], tmpStr[l]
    l += 1
    r -= 1
```

``` Python
class Solution:
    def reverseWords(self, s: str) -> str:
        # God Ding
        print(s.split()[::-1]) # ['Ding', 'God']
        print(' '.join(s.split()[::-1])) #Ding God
        print(' '.join(s.split()[::-1])[::-1]) #doG gniD
        return ' '.join(s.split()[::-1])[::-1]
```

### 876. Middle of the Linked List

此方式只需要遍歷一次list，需要注意奇數與偶數，條件判斷一定要是fast and fast.next。

1. [1, 2, 3]
   - pass 1: slow: 1; fast: 1; fast.next: 2
   - pass 2: slow: 2; fast: 3; fast.next: None
   - pass 3: slow: 2; fast: 3; fast.next: None
2. [1, 2, 3, 4] `pass 3可以發現fast.next會不存在，利用and的判斷假如第一個是false就不會執行第二個了。`
   - pass 1: slow: 1; fast: 1; fast.next: 2
   - pass 2: slow: 2; fast: 3; fast.next: 4
   - pass 3: slow: 3; fast: None; fast.next: unKnown

``` Python
class Solution:
    def middleNode(self, head: Optional[ListNode]) -> Optional[ListNode]:
        slow = fast = head

        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next
        return slow
```

### 19. Remove Nth Node From End of List

分快和慢的List，快和慢的間距為n，當快的走到終點時，慢的跳過一個即可刪除第n個元素，需注意只有一個元素的情況。

``` Python
class Solution:
    def removeNthFromEnd(self, head: Optional[ListNode], n: int) -> Optional[ListNode]:
        list_fast = head
        list_slow = head
        for _ in range(n):
            list_fast = list_fast.next
        if list_fast==None:
            return(head.next)
        while list_fast.next:
            list_fast, list_slow = list_fast.next, list_slow.next
        list_slow.next = list_slow.next.next
        return(head)
```

### 3. Longest Substring Without Repeating Characters

透過改變start的位置來得知到目前字串中字元位置的長度。

``` Python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        charMap = {}
        start = 0
        longestSubstringLength = 0
        for end, character in enumerate(s):
            if character in charMap and charMap[character] >= start:
                # 要是character在charMap中，且存入的index大於或是start的位置，則將start的位置改變。
                start = charMap[character] +1
            else:
                longestSubstringLength = max(longestSubstringLength, end - start + 1)
            charMap[character] = end
        return longestSubstringLength
```

### 567. Permutation in String

此方法是O(len(s1)*len(s2))

``` Python
class Solution:
    def checkInclusion(self, s1: str, s2: str) -> bool:
        lenS1 = len(s1)
        s1Map = {}
        for ind, char in enumerate(s1):
            if char in s1Map:
                s1Map[char] += 1
            else:
                s1Map[char] = 1

        for start in range(len(s2)-lenS1+1):
            end = start + lenS1
            if s2[start] not in s1Map:
                continue
            tmpS = s2[start:end]
            tmpMap = {}
            for ind, char in enumerate(tmpS):
                if char in tmpMap:
                    tmpMap[char] += 1
                else:
                    tmpMap[char] = 1
            if tmpMap==s1Map:
                return True
        return False
```

每次維持當下的window slide是比較好的方式。

此方法是O(len(s2))

``` Python
class Solution:
    def checkInclusion(self, s1: str, s2: str) -> bool:
        n = len(s1)
        m = len(s2)

        store = [0] * 26
        for c in s1:
            store[ord('a') - ord(c)] += 1

        counter = [0] * 26
        for j in range(0, n):
            counter[ord('a') - ord(s2[j])] += 1

        if counter == store:
            return True

        for j in range(n, m):
            print(j)
            counter[ord('a') - ord(s2[j])] += 1
            counter[ord('a') - ord(s2[j - n])] -=  1
            if counter == store:
                return True
        return False
```