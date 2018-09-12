# Google

1. Using binary search to find index to insert a value into a sorted list

```java
        public int binarySearchIndex(List<Integer> list, int target) {
            if(list.size() == 0) return 0;
            int start = 0, end = list.size() - 1;
            if(list.get(end) < target) return end + 1; // handle corner case
            if(list.get(start) >= target) return start; // handle corner case

            while(start < end) {
                int mid = start + (end - start) / 2;
                if(target > list.get(mid)) start = mid + 1;
                else end = mid;
            }
            return end;
        }
```



