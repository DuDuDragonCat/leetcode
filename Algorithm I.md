# Algorithm i

- Update: 20210919
- Ques: 704, 278

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

