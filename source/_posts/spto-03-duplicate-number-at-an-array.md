---
title: spto-03-duplicate-number-at-an-array
date: 2019-07-22 11:23:19
tags: spto
---

题目一：找出数组中重复的数字

在一个长度为n的数组里的所有数字都在0~n-1的范围内。数组中某些数字是重复的，但不知道有几个数字是重复的，也不知道每个数字重复了几次，请找出数组里任意一个重复的数字。

##### code in java: 

```java
public class question03 {
    private static ArrayList<Integer> duplicate(int[] numbers) {
        ArrayList<Integer> ans = new ArrayList<>();
        if (numbers == null || numbers.length == 0) {
            System.out.print("is null");
            return null;
        }
        for (int i = 0;i < numbers.length;++i) {
            if(numbers[i] < 0 || numbers[i] > numbers.length) {
                System.out.print("get over");
                return null;
            }
        }
        for (int i = 0;i < numbers.length;++i) {
            while (numbers[i] != i) {
                if (numbers[i] == numbers[numbers[i]]) {
                    if(!ans.contains(numbers[i]))
                        ans.add(numbers[i]);
                    break;
                }
                int temp = numbers[i];
                numbers[i] = numbers[temp];
                numbers[temp] = temp;
            }
        }
    return ans;
	}
    public static void main(String[] args) {
        int[] list = {2, 3, 1, 0, 2, 5, 3,1,1};
        ArrayList<Integer> al = duplicate(list);
        for (int i : al) {
            System.out.print(i+ " ");
        }
    }

}
```

