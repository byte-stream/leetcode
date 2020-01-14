# [4. 寻找两个有序数组的中位数](https://leetcode-cn.com/problems/median-of-two-sorted-arrays/)

## 题目

给定两个大小为 m 和 n 的有序数组 nums1 和 nums2。

请你找出这两个有序数组的中位数，并且要求算法的时间复杂度为 O(log(m + n))。

你可以假设 nums1 和 nums2 不会同时为空。

**示例 1:**

```java
nums1 = [1, 3]
nums2 = [2]

则中位数是 2.0
```

**示例 2:**

```java
nums1 = [1, 2]
nums2 = [3, 4]

则中位数是 (2 + 3)/2 = 2.5
```



## 解题方法及思路

题解的方法：递归法
为了解决这个问题，我们需要理解 “中位数的作用是什么”。在统计中，中位数被用来：

> 将一个集合划分为两个长度相等的子集，其中一个子集中的元素总是大于另一个子集中的元素。



使小的那段的最大的值 >=大的这段的最小值,即是中位数.

```java
  /*
    * 1.首先，让我们在任一位置 i 将 A(长度为m) 划分成两个部分：
    *            leftA            |                rightA
    *   A[0],A[1],...      A[i-1] |  A[i],A[i+1],...A[m - 1]
    *
    * 由于A有m个元素，所以有m + 1中划分方式(i = 0 ~ m)
    *
    * 我们知道len(leftA) = i, len(rightA) = m - i;
    * 注意：当i = 0时，leftA是空集，而当i = m时，rightA为空集。
    *
    * 2.采用同样的方式，将B也划分为两部分：
    *            leftB            |                rightB
    *   B[0],B[1],...      B[j-1] |   B[j],B[j+1],...B[n - 1]
    *  我们知道len(leftA) = j, len(rightA) = n - j;
    *
    *  将leftA和leftB放入一个集合，将rightA和rightB放入一个集合。再把这两个集合分别命名为leftPart和rightPart。
    *
    *            leftPart         |                rightPart
    *   A[0],A[1],...      A[i-1] |  A[i],A[i+1],...A[m - 1]
    *   B[0],B[1],...      B[j-1] |  B[j],B[j+1],...B[n - 1]
    *
    *   如果我们可以确认：
    *   1.len(leftPart) = len(rightPart); =====> 该条件在m+n为奇数时，该推理不成立
    *   2.max(leftPart) <= min(rightPart);
    *
    *   median = (max(leftPart) + min(rightPart)) / 2;  目标结果
    *
    *   要确保这两个条件满足：
    *   1.i + j = m - i + n - j(或m - i + n - j + 1)  如果n >= m。只需要使i = 0 ~ m，j = (m+n+1)/2-i =====> 该条件在m+n为奇数/偶数时，该推理都成立
    *   2.B[j] >= A[i-1] 并且 A[i] >= B[j-1]
    *
    *   注意:
    *   1.临界条件：i=0,j=0,i=m,j=n。需要考虑
    *   2.为什么n >= m ? 由于0 <= i <= m且j = (m+n+1)/2-i,必须确保j不能为负数。
    *
    *   按照以下步骤进行二叉树搜索
    *   1.设imin = 0,imax = m，然后开始在[imin,imax]中进行搜索
    *   2.令i = (imin+imax) / 2, j = (m+n+1)/2-i
    *   3.现在我们有len(leftPart) = len(rightPart)。而我们只会遇到三种情况：
    *
    *      ①.B[j] >= A[i-1] 并且 A[i] >= B[j-1]  满足条件
    *      ②.B[j-1] > A[i]。此时应该把i增大。 即imin = i + 1;
    *      ③.A[i-1] > B[j]。此时应该把i减小。 即imax = i - 1;
    *
    * */

    public double findMedianSortedArrays(int[] A, int[] B) {
        int m = A.length;
        int n = B.length;
        if (m > n) { // to ensure m<=n
            int[] temp = A; A = B; B = temp;
            int tmp = m; m = n; n = tmp;
        }
        int iMin = 0, iMax = m, halfLen = (m + n + 1) / 2;
        while (iMin <= iMax) {
            int i = (iMin + iMax) / 2;
            int j = halfLen - i;
            if (i < iMax && B[j - 1] > A[i]) {
                iMin = i + 1; // i is too small
            } else if (i > iMin && A[i - 1] > B[j]) {
                iMax = i - 1; // i is too big
            } else { // i is perfect
                int maxLeft;
                if (i == 0) {//A分成的leftA(空集) 和 rightA(A的全部)  所以leftPart = leftA(空集) + leftB,故maxLeft = B[j-1]。
                    maxLeft = B[j - 1];
                } else if (j == 0) { //B分成的leftB(空集) 和 rightB(B的全部)  所以leftPart = leftA + leftB(空集),故maxLeft = A[i-1]。
                    maxLeft = A[i - 1];
                } else { //排除上述两种特殊情况，正常比较
                    maxLeft = Math.max(A[i - 1], B[j - 1]);
                }
                if ((m + n) % 2 == 1) { //奇数，中位数正好是maxLeft
                    return maxLeft;
                }
                //偶数
                int minRight;
                if (i == m) {//A分成的leftA(A的全部) 和 rightA(空集)  所以rightPart = rightA(空集) + rightB,故minRight = B[j]。
                    minRight = B[j];
                } else if (j == n) {//B分成的leftB(B的全部) 和 rightB(空集)  所以rightPart = rightA + rightB(空集),故minRight = A[i]。
                    minRight = A[i];
                } else {//排除上述两种特殊情况，正常比较
                    minRight = Math.min(B[j], A[i]);
                }

                return (maxLeft + minRight) / 2.0;
            }
        }
        return 0.0;
    }
```

