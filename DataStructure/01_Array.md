# 数组Array

<!-- GFM-TOC -->
* [1. 有序数组的 Two Sum](#1-有序数组的-two-sum)
* [2. 两数平方和](#2-两数平方和)
* [3. 反转字符串中的元音字符](#3-反转字符串中的元音字符)
* [4. 回文字符串](#4-回文字符串)
* [5. 归并两个有序数组](#5-归并两个有序数组)
* [6. 判断链表是否存在环](#6-判断链表是否存在环)
* [7. 最长子序列](#7-最长子序列)
* [1. 盛最多水的容器](#1-盛最多水的容器-container-with-most-water)
* [2. 移动零](#2-移动零)
* [3. 爬楼梯](#3-爬楼梯)
* [4.三数之和(高频老题)](#4-三数之和(高频老题))

<!-- GFM-TOC -->


# 1. 盛最多水的容器 container-with-most-water
11\. 盛最多水的容器

[Leetcode](https://leetcode.com/problems/container-with-most-water/)/[力扣](https://leetcode-cn.com/problems/container-with-most-water/)/

```html
输入：[1,8,6,2,5,4,8,3,7]
输出：49
```

题目描述：给你 n 个非负整数 a1，a2，...，an，每个数代表坐标中的一个点 (i, ai) 。在坐标内画 n 条垂直线，垂直线 i 的两个端点分别为 (i, ai) 和 (i, 0)。找出其中的两条线，使得它们与 x 轴共同构成的容器可以容纳最多的水。

说明：你不能倾斜容器，且 n 的值至少为 2。

思路：
使用双指针，一个指针指向值较小的元素，一个指针指向值较大的元素。指向较小元素的指针从头向尾遍历，指向较大元素的指针从尾向头遍历。

- 如果两个指针指向元素的和 sum == target，那么得到要求的结果；
- 如果 sum > target，移动较大的元素，使 sum 变小一些；
- 如果 sum < target，移动较小的元素，使 sum 变大一些。

数组中的元素最多遍历一次，时间复杂度为 O(N)。只使用了两个额外变量，空间复杂度为  O(1)。

<div align="center"> <img src="./question_11.jpg" > </div><br>

```java
    public int maxArea(int[] height) {
        int max = 0;
        for (int i = 0, j = height.length - 1; i < j; ) {
            int minHeight = height[i] < height[j] ? height[i++] : height[j--];
            int area = (j - i + 1) * minHeight;
            max = Math.max(max, area);
        }
        return max;
    }
    public int maxArea1(int[] height) {
        int max = 0;
        for (int i = 0; i < height.length - 1; i++) {
            for (int j = i + 1; j < height.length; j++) {
                int area = (j - i) * Math.min(height[i], height[j]);
                max = Math.max(max, area);
            }
        }
        return max;
    }
```
# 2. 移动零
283\. 移动零

[Leetcode](https://leetcode.com/problems/climbing-stairs/)/[力扣](https://leetcode-cn.com/problems/climbing-stairs/)/

给定一个数组 nums，编写一个函数将所有 0 移动到数组的末尾，同时保持非零元素的相对顺序。

```html
输入: [0,1,0,3,12]
输出: [1,3,12,0,0]
说明:必须在原数组上操作，不能拷贝额外的数组；尽量减少操作次数。
```

解题思路：用下标j记录非零元素的下标

```java
// 4.swap
    public void moveZeroes(int[] nums) {
        int j = 0;
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] != 0) {
                int tmp = nums[i];
                nums[i] = nums[j];
                nums[j] = tmp;
                j++;
            }
        }

    }

    // 3. index none-zero
    public void moveZeroes3(int[] nums) {
        int j = 0;
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] != 0) {
                nums[j] = nums[i];
                if (i != j) {
                    nums[i] = 0;
                }
                j++;
            }
        }
    }

    // 2. create a new array, i=0: none-zero, j=nums.length-1, zero
    public void moveZeroes2(int[] nums) {
        int[] newNums = new int[nums.length];
        int start = 0, end = nums.length - 1;
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] != 0) {
                newNums[start++] = nums[i];
            } else {
                newNums[end--] = nums[i];
            }
        }

    }

    // 1. count the nums of zeros, move none zeros
    public void moveZeroes1(int[] nums) {
        int zerosNum = 0, index = 0;
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] == 0) {
                zerosNum++;
            } else {
                nums[index++] = nums[i];
            }
        }
        for (int i = index; i < nums.length; i++) {
            nums[i] = 0;
        }

    }
```

# 3. 爬楼梯
70\. 爬楼梯

[Leetcode](https://leetcode.com/problems/climbing-stairs/)/[力扣](https://leetcode-cn.com/problems/climbing-stairs/)/

假设你正在爬楼梯。需要 n 阶你才能到达楼顶。
每次你可以爬 1 或 2 个台阶。你有多少种不同的方法可以爬到楼顶呢？
注意：给定 n 是一个正整数。

```html
输入： 2
输出： 2
解释： 有两种方法可以爬到楼顶。
1.  1 阶 + 1 阶
2.  2 阶
```
解题思路：这其实就是一个斐波那契数列，最简便的是用2个数字来存最新的2个数字即可，往后迭代；或者用一个数组；不要用递归，时间复杂度高，会超时，也不是面试官推崇的做法。

```java
public int climbStairs(int n) {
        if (n <= 2) return n;
        int f1 = 1, f2 = 2, tmp = 0;
        for (int i = 3; i <= n; i++) {
            tmp = f1 + f2;
            f1 = f2;
            f2 = tmp;
        }
        return f2;
    }
    public int climbStairs1(int n) {
        if (n <= 2) return n;
        int[] ret = new int[n];
        ret[0] = 1;
        ret[1] = 2;
        for (int i = 2; i <= n - 1; i++) {
            ret[i] = ret[i - 2] + ret[i - 1];
        }
        return ret[n - 1];
    }
```


# 4. 三数之和
15\. 三数之和

[Leetcode](https://leetcode.com/problems/3sum/)/[力扣]https://leetcode-cn.com/problems/3sum/)/

给你一个包含 n 个整数的数组 nums，判断 nums 中是否存在三个元素 a，b，c ，使得 a + b + c = 0 ？请你找出所有满足条件且不重复的三元组。
注意：答案中不可以包含重复的三元组。

```html
给定数组 nums = [-1, 0, 1, 2, -1, -4]，
满足要求的三元组集合为：
[
  [-1, 0, 1],
  [-1, -1, 2]
]
```

解题思路：先排序，然后用左右夹逼法，用set存可以去重

```java
public List<List<Integer>> threeSum(int[] nums) {
        if (nums == null || nums.length <= 2) {
            return Collections.emptyList();
        }
        Arrays.sort(nums);
        Set<List<Integer>> set = new LinkedHashSet<>();
        for (int i = 0; i < nums.length - 2; i++) {
            int left = i + 1;
            int right = nums.length - 1;
            while (left < right) {
                int sum = nums[i] + nums[left] + nums[right];
                if (sum < 0) {
                    left++;
                } else if (sum > 0) {
                    right--;
                } else {
                    List<Integer> list = Arrays.asList(nums[i], nums[left], nums[right]);
                    set.add(list);
                    left++;
                    right--;
                }
            }
        }
        return new ArrayList<>(set);

    }
    // 2. hash
    public List<List<Integer>> threeSum(int[] nums) {
        if (nums == null || nums.length <= 2) {
            return Collections.emptyList();
        }
        Arrays.sort(nums);
        Set<List<Integer>> set = new LinkedHashSet<>();
        for (int i = 0; i < nums.length - 2; i++) {
            int target = -nums[i];
            Map<Integer, Integer> map = new HashMap<>(nums.length - i);
            for (int j = i+1; j < nums.length; j++) {
                int v = target - nums[j];
                if (map.containsKey(v)) {
                    List<Integer> value = Arrays.asList(nums[i], nums[j], v);
                    value.sort(Comparator.naturalOrder());
                    set.add(value);
                }
                map.put(nums[j], nums[j]);

            }
        }
        return new ArrayList<>(set);
    }

    // 1. 3重循环
    public List<List<Integer>> threeSum1(int[] nums) {
        if (nums == null || nums.length <= 2) {
            return Collections.emptyList();
        }
        Arrays.sort(nums);
        Set<List<Integer>> set = new LinkedHashSet<>();
        for (int i = 0; i < nums.length - 2; i++) {
            for (int j = i + 1; j < nums.length - 1; j++) {
                for (int k = j + 1; k < nums.length; k++) {
                    if (nums[i] + nums[j] + nums[k] == 0) {
                        List<Integer> value = Arrays.asList(nums[i], nums[j], nums[k]);
                        set.add(value);
                    }
                }
            }
        }
        return new ArrayList<>(set);
    }
```