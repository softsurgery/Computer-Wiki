```python
class Solution:
    def findMedianSortedArrays(self, nums1, nums2):
        merged = sorted(nums1 + nums2)
        n = len(merged)
        mid = n // 2
        if n % 2 == 0:
            return (merged[mid - 1] + merged[mid]) / 2.0  # Force float division
        else:
            return merged[mid] * 1.0  # Also return as float
```