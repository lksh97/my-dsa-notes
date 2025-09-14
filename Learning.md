```java
            if(minHeap.size() < k) 
            {
                minHeap.offer(num);
            }
            else if (num > minHeap.peek()) { // else if idhar isiliye laga hai kyunki if mein aise hi daal do lekin agar heap size k ho jaaye toh dekho peak se tab daalo
                minHeap.poll();
                minHeap.offer(num);
            }
```
