[28. 找出字符串中第一个匹配项的下标 - 力扣（LeetCode）](https://leetcode.cn/problems/find-the-index-of-the-first-occurrence-in-a-string/description/)
```java
//kmp做法：主串不回溯。利用已经部分匹配的结果，也就是最长相等前后缀
//前缀是指不包含最后一个字符的所有以第一个字符开头的连续子串。后缀是指不包含第一个字符的所有以最后一个字符结尾的连续子串。
//此处按照前缀表不减一，不右移的具体实现，即按照原本的前缀表进行实现
class Solution {
    public int[] getNext(String needle){
        int n=needle.length();
        int[] next=new int[n];
        //第一步：初始化
        //j表示前缀末尾，i表示后缀末尾，此时j的值就是最长相等前后缀的值
        next[0]=0;
        int j=0;
        // 求next数组的过程，其实就是在使用kmp方法去做:每次求next【i】，可看作一次匹配。此时的主串就相当于是【0，i】，模式串就是【0，j】，【0，j-1】已经发现在当前时匹配的部分，所以j还代表当前已经匹配部分的长度。
        //根据上面说的，其实主串为只有一个字母的时候已经匹配过了
        for(int i=1;i<needle.length();i++){
            //该次匹配过程发现字符不匹配时，那么就要利用已经匹配的部分，也就是next[j-1]，去继续进行比较，当然也有可能跳了好几次都还没有匹配，一直不匹配，所以是while
            //第二步：不相等时
            while(j>0 &&needle.charAt(j)!=needle.charAt(i))
                j=next[j-1];
            //该次匹配过程发现字符匹配时，说明多了一个字符匹配，那么肯定最长相等前后缀个数+1.所以j++；j代表着当前匹配部分的长度
            //第三步：相等时
            if(needle.charAt(j)==needle.charAt(i))
                j++;
            //第四步，对next数组进行赋值
            next[i]=j;
        }
        return next;
    }
    public int strStr(String haystack, String needle) {
        int[] next=getNext(needle);
        int j=0;
        //利用kmp算法查找前后缀，不过此时i是从0开始，因为getNext数组是对next[0]进行了初始化，所以从1开始，而查找子串是从下表为0开始查找的
        for(int i=0;i<haystack.length();i++){
            while(j>0 && haystack.charAt(i)!=needle.charAt(j))
                j=next[j-1];
            if(haystack.charAt(i)==needle.charAt(j))
                j++;
            if(j==needle.length())
                return i-j+1;
        }
        return -1;
    }
}
```