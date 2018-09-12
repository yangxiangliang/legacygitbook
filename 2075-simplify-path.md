# 71. Simplify Path

> Given an absolute path for a file \(Unix-style\), simplify it.
>
> For example,
>
> **path **=`"/home/"`, =&gt;`"/home"`
>
> **path **=`"/a/./b/../../c/"`, =&gt;`"/c"`
>
> `Corner Cases:`
>
> * Did you consider the case where **path =**`"/../"`?  In this case, you should return "/".
>
> * Another corner case is the path might contain multiple slashes`'/'`together, such as`"/home//foo/"`. In this case, you should ignore redundant slashes and return`"/home/foo"`.

### 思路

用stack记录每个目录 directory的名字，用 split\("/"\) 把输入 string 分开。如果是".." ， 则Pop 处stack顶端元素，如果是"." 或则 empty，则skip掉即可。有些Corner case 需要仔细处理，比如最后result string是空的时候，还是要return “/”。

### Code

```java
class Solution {
    public String simplifyPath(String path) {
        Stack<String> stack = new Stack();
        for(String dir : path.split("/")) {
            if(dir.equals("..")) {
                if(stack.size() > 0) stack.pop();
            } 
            else if (!dir.equals(".") && dir.length() > 0) stack.push(dir);
        }
        
        String rst = "";
        while(!stack.isEmpty()) rst = "/" + stack.pop() + rst;
        return rst.length() == 0 ? "/" : rst;
    }
}
```



