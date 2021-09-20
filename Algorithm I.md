# Algorithm i

- Update: 20210920
- Ques: 704, 278, 189

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
        """
        Do not return anything, modify nums in-place instead.
        """
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