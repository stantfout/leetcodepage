# 和为S的连续正数序列

## [题目描述](https://leetcode-cn.com/problems/he-wei-sde-lian-xu-zheng-shu-xu-lie-lcof/)

小明很喜欢数学,有一天他在做数学作业时,要求计算出9~16的和,他马上就写出了正确答案是100。但是他并不满足于此,他在想究竟有多少种连续的正数序列的和为100(至少包括两个数)。没多久,他就得到另一组连续正数和为100的序列:18,19,20,21,22。现在把问题交给你,你能不能也很快的找出所有和为S的连续正数序列? Good Luck!

## 思路
### 滑动窗口(双指针)
初始滑动窗口大小为1，开始(left)为1，结束(right)为1，通过等差数列求和公式可以求得滑动窗口的和，与目标target进行比较，如果等于target，则创建一个数组保存答案，如果小于target则说明应该扩大滑动窗口的和，则增加right，如果大于target，则说明应该减少滑动窗口的和，则增加left。

```java
class Solution {
    public int[][] findContinuousSequence(int target) {
        List<int[]> res = new ArrayList<>();
        int left = 1;
        int right = 1;
        int sum = 1;//滑动窗口[left,right]的和
        //当(left <= target / 2)时，就可以退出循环了，因为不存在满足条件的序列了
        while (left <= target / 2) {
            if (sum > target) {
                sum -= left;
                left++;
            } else if (sum < target) {
                right++;
                sum += right;
            } else {
                int[] temp = new int[right - left + 1];
                for (int i = left; i <= right; i++) {
                    temp[i - left] = i;
                }
                res.add(temp);
                sum -= left;
                left++;
            }
        }
        return res.toArray(new int[res.size()][]);
    }
}
```


### 通过求和公式来解
求和公式
> (a1 + a1 + n - 1) * n / 2 = target

化简得
> 2 * a1 * n + n * n - n = 2 * target

target已知，此时只要遍历a1或者遍历n，都可求得另一个的值，只要判断是否为正整数就可以判断是否存在答案。

如果选择遍历a1，在求n的过程中要用到sqrt，如果遍历n,则不需要，所以选择遍历n。

```java
class Solution {
    public int[][] findContinuousSequence(int target) {
        List<int[]> list = new ArrayList<>();
        for (int n = 2; 2 * target + n - n * n > 0; n++) {
            int a1 = 2 * target + n - n * n;
            if (a1 % (2 * n) == 0) {
                a1 /= (2 * n);
                int arr[] = new int[n];
                arr[0] = a1;
                for (int i = 1; i < n; i++) {
                    arr[i] = arr[i - 1] + 1;
                }
                list.add(0,arr);
            }
        }
        return list.toArray(new int[list.size()][]);
    }
}
```