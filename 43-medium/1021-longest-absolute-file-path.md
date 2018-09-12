# 388. Longest Absolute File Path

> Suppose we abstract our file system by a string in the following manner:
>
> The string`"dir\n\tsubdir1\n\tsubdir2\n\t\tfile.ext"`represents:
>
> ```
> dir
>     subdir1
>     subdir2
>         file.ext
> ```
>
> The directory `dir `contains an empty sub-directory `subdir1 `and a sub-directory `subdir2 `containing a file `file.ext`.
>
> The string `"dir\n\tsubdir1\n\t\tfile1.ext\n\t\tsubsubdir1\n\tsubdir2\n\t\tsubsubdir2\n\t\t\tfile2.ext" `represents:
>
> ```
> dir
>     subdir1
>         file1.ext
>         subsubdir1
>     subdir2
>         subsubdir2
>             file2.ext
> ```
>
> The directory`dir`contains two sub-directories`subdir1`and`subdir2`.`subdir1`contains a file`file1.ext`and an empty second-level sub-directory`subsubdir1`.`subdir2`contains a second-level sub-directory`subsubdir2`containing a file`file2.ext`.
>
> We are interested in finding the longest \(number of characters\) absolute path to a file within our file system. For example, in the second example above, the longest absolute path is`"dir/subdir2/subsubdir2/file2.ext"`, and its length is`32`\(not including the double quotes\).
>
> Given a string representing the file system in the above format, return the length of the longest absolute path to file in the abstracted file system. If there is no file in the system, return`0`.
>
> **Note:**
>
> * The name of a file contains at least a "." and an extension.
>
> * The name of a directory or sub-directory will not contain a "."
>
> Time complexity required: `O(n) `where `n `is the size of the input string.
>
> Notice that `a/aa/aaa/file1.txt `is not the longest file path, if there is another path `aaaaaaaaaaaaaaaaaaaaa/sth.png`.

### 思路

首先由于有“\n”\(分行符\)将不同string分开，然后还有"\t"\(tab符\)将层次分开。一开始可以以"\n"来split input string，然后遍历split后的所有string。查看该string有几个"\t"，如果没有，则说明是第一层目录，即题目中的"dir"，如果有1个，说明在其下一层，比如"subdir1", "subdir2"，依次类推。同样需要记录到该层的absolute path的路径，这样到下一层时，可以用上一层的path + "/" + 该层目录名/文件名 即可。

### Code

```java
public class Solution {
    public int lengthLongestPath(String input) {
        int max = 0;
        HashMap<Integer, String> map = new HashMap(); // 记录到每"层"目录的absolute path
        for(String tmp : input.split("\n")) {
            // see how many "\t" in the string tmp
            int count = tmp.lastIndexOf("\t") + 1;  // tab 相当于 1个char，不是2
            tmp = tmp.substring(count); // remove "\t" 得到该层 目录/文件 名字
            if(count == 0) {
                // input string only contains file name, e.g. "file.txt"
                if(tmp.contains(".")) return tmp.length();
                map.put(count, tmp);
            }
            else {
                String current = map.get(count-1) + "/" + tmp;
                if(tmp.contains(".")) max = Math.max(max, current.length());
                else map.put(count, current);
            }
        }
        return max;
    }
}
```



