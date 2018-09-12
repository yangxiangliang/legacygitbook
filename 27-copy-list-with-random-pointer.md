# 138. Copy List with Random Pointer

> A linked list is given such that each node contains an additional random pointer which could point to any node in the list or null.
>
> Return a deep copy of the list.

### Code 1\)

先用HashMap记录copy每个node，及copy node的label 和original的一样，用HashMap一一对应起来

```java
/**
 * Definition for singly-linked list with a random pointer.
 * class RandomListNode {
 *     int label;
 *     RandomListNode next, random;
 *     RandomListNode(int x) { this.label = x; }
 * };
 */
public class Solution {
    public RandomListNode copyRandomList(RandomListNode head) {
        if (head == null) return null;
        HashMap<RandomListNode, RandomListNode> map = new HashMap();
        RandomListNode cur = head;
        // copy all nodes and put them in hashmap
        while (cur != null) {
            // in case there is cycle in list
            if (map.containsKey(cur)) break;
            RandomListNode copy = new RandomListNode (cur.label);
            map.put(cur, copy);
            cur = cur.next;
        }
        // assign next and random node for them if any
        for (RandomListNode node: map.keySet()) {
            if (node.next != null) map.get(node).next = map.get(node.next);
            if (node.random != null) map.get(node).random = map.get(node.random);
        }
        return map.get(head);
    }
}
```

### Code 2\)

上面相当于scan了所有nodes 2遍，其实只需要scan一遍即可。就是从head 一直往next走，因为有HashMap记录original node和copy node的关系，每次copy当前current node的时候，同时检查其 current node的next node和random node是不是也被copy了，如果没有，也把其copy入HashMap即可。

```java
/**
 * Definition for singly-linked list with a random pointer.
 * class RandomListNode {
 *     int label;
 *     RandomListNode next, random;
 *     RandomListNode(int x) { this.label = x; }
 * };
 */
public class Solution {
    public RandomListNode copyRandomList(RandomListNode head) {
        if(head == null) return null;
        HashMap<RandomListNode, RandomListNode> map = new HashMap();
        RandomListNode node = head;
        while(node != null) {
            if(!map.containsKey(node)) map.put(node, new RandomListNode(node.label));
            if(node.next != null && !map.containsKey(node.next)) map.put(node.next, new RandomListNode(node.next.label));
            if(node.random != null && !map.containsKey(node.random)) map.put(node.random, new RandomListNode(node.random.label));
            map.get(node).next = map.get(node.next);
            map.get(node).random = map.get(node.random);
            node = node.next;
        }
        return map.get(head);
    }
}
```



