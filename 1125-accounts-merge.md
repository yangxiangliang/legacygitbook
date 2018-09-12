# 721. Accounts Merge

> Given a list`accounts`, each element`accounts[i]`is a list of strings, where the first element`accounts[i][0]`is a name, and the rest of the elements are emails representing emails of the account.
>
> Now, we would like to merge these accounts. Two accounts definitely belong to the same person if there is some email that is common to both accounts. Note that even if two accounts have the same name, they may belong to different people as people could have the same name. A person can have any number of accounts initially, but all of their accounts definitely have the same name.
>
> After merging the accounts, return the accounts in the following format: the first element of each account is the name, and the rest of the elements are emails **in sorted order**. The accounts themselves can be returned in any order.
>
> **Example 1:**
>
> ```
> Input: 
> accounts = [["John", "johnsmith@mail.com", "john00@mail.com"], ["John", "johnnybravo@mail.com"], ["John", "johnsmith@mail.com", "john_newyork@mail.com"], ["Mary", "mary@mail.com"]]
> Output: [["John", 'john00@mail.com', 'john_newyork@mail.com', 'johnsmith@mail.com'],  ["John", "johnnybravo@mail.com"], ["Mary", "mary@mail.com"]]
> Explanation: 
> The first and third John's are the same person as they have the common email "johnsmith@mail.com".
> The second John and Mary are different people as none of their email addresses are used by other accounts.
> We could return these lists in any order, for example the answer [['Mary', 'mary@mail.com'], ['John', 'johnnybravo@mail.com'], 
> ['John', 'john00@mail.com', 'john_newyork@mail.com', 'johnsmith@mail.com']] would still be accepted.
> ```
>
> **Note:**
>
> 1. The length of`accounts`will be in the range`[1, 1000]`.
>
> 2. The length of`accounts[i]`will be in the range`[1, 10]`.
>
> 3. The length of`accounts[i][j]`will be in the range`[1, 30]`.

### 思路

1. 这里就是要找如果有2个email list中有相同的email，那么它们就可以union成一个account，因为题目中保证了一个account肯定是同样的name，所以这里不用考虑name的问题。那么union的感觉，可以用union find的思维来解决。
2. 感觉这里难点是怎么来构造parent节点和child节点，可以让同一个account里面的email都在可以reach到同一个parent email上，从而可以union到同一个account中。
3. 其实这里某一个account list里面所有的email都是“同等地位”的，没有什么差别。只要任何某一个email能找到与其他account里面某个email相同，那么就可以合并这2个account。所以初始化的时候，可以先把任意选一个email list里面的email作为其他的parent，这里就选择每个account 里面的第一个email 作为其他的parent。**\(可以想象每个email都有一个edge连着email，这个题目也就是找connected component的感觉\)**

### Code

```java
class Solution {
    public List<List<String>> accountsMerge(List<List<String>> accounts) {
        // 记录email 与 其parent的匹配关系
        HashMap<String, String> parents = new HashMap();
        // 记录email与其account name的mapping,便于后面look up
        HashMap<String, String> owners = new HashMap();

        for(List<String> account : accounts) {
            for(int i = 1; i < account.size(); i ++) {
                // 初始化，把每个list 里面 头一个email 作为其他 parent
                parents.put(account.get(i), account.get(1));
                owners.put(account.get(i), account.get(0));
            }
        }

        // 关键：update 每个account list的parent之间的联系，如果是同一个account，那么parent node之间也会相连，会找到同一个parent
        for(List<String> account : accounts) {
            String p = findParent(parents, account.get(1));
            for(int i = 2; i < account.size(); i ++) {
                parents.put(findParent(parents, account.get(i)), p);
            }
        }

        // using TreeSet, 因为最后输出的email list需要sorted order
        HashMap<String, TreeSet<String>> union = new HashMap();
        for(List<String> account : accounts) {
            for(int i = 1; i < account.size(); i ++) {
                String p = findParent(parents, account.get(i));
                if (!union.containsKey(p)) union.put(p, new TreeSet());
                // add all emails related to same parent into union set
                union.get(p).add(account.get(i));
            }
        }

        List<List<String>> rst = new ArrayList();
        for(String p : union.keySet()) {
            List<String> tmp = new ArrayList();
            tmp.add(owners.get(p));
            tmp.addAll(union.get(p));
            rst.add(tmp);
        }
        return rst;
    }

    private String findParent(HashMap<String, String> parents, String email) {
        while(parents.get(email) != email) email = parents.get(email);
        return email;
    }
}
```



