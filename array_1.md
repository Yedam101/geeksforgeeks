### 1. Binary Search
#### Given a sorted array of size N and an integer K, find the position(0-based indexing) at which K is present in the array using binary search.


class Solution:	
	def binarysearch(self, arr, n, k):
	    left, right = 0, n -1
	    
	    while left < right:
	       if arr[left] > k or arr[right] < k:
	           return -1
	       elif arr[left] == k:
	           return left
	       elif arr[right] == k:
	           return right
	       left, right = left+1, right-1

        return -1
	    
	def binarysearch(self, arr, n, k):	

 		if k in arr:
 		    return arr.index(k)
		else:
        	return -1
