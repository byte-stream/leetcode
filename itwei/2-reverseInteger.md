### 给出一个 32 位的有符号整数，你需要将这个整数中每位上的数字进行反转。

####  示例 1:
 输入: 123
 输出: 321

####  示例 2:
 输入: -123
 输出: -321

####  示例 3:
 输入: 120
 输出: 21

 来源：力扣（LeetCode）
 链接：https://leetcode-cn.com/problems/reverse-integer
 著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

```
public class ReverseInterger {
    public static void main(String[] args) {
        int num = 123456789;
        int result = solute(num);
        System.out.println(result);
    }

    private static int solute(int num) {
        long result = 0;
        while(num != 0){
            //每次循环
            //num%10 依次取 个十百千万..位的数
            //result*10 依次将 个位变成十百千万..位
            result = result*10 + num%10;
            //依次去掉最后一位数
            num = num/10;
        }
        return  (int)result==result? (int)result:0;
    }
}
```
