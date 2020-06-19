# 1.盛最多水的容器
给你 n 个非负整数 a1，a2，...，an，每个数代表坐标中的一个点 (i, ai) 。在坐标内画 n 条垂直线，垂直线 i 的两个端点分别为 (i, ai) 和 (i, 0)。
找出其中的两条线，使得它们与 x 轴共同构成的容器可以容纳最多的水。<br>
示例:<br>
输入：[1,8,6,2,5,4,8,3,7]<br>
输出：49
* 思路:双指针的思路,第一次看到这个题很难去想到双指针的思路.数组两端同时开始便利,并记录面积的最大值,移动的原则:谁的值小,谁就移动.<br>
``` cpp
class Solution
{
public:
    int maxArea(vector<int> &height)
    {
        if (height.size() < 2)
        {
            return 0;
        }
        int ans = 0;
        //取两端指针
        auto left = height.begin();
        auto right = height.end() - 1;
        while (left < right)
        {
            int area = (right - left) * std::min(*left, *right);
            ans = std::max(area, ans);
            //判断应该移动哪边的指针
            (*left > *right) ? right-- : left++;
        }
        return ans;
    }
};
```

# 2.三数之和
给你一个包含 n 个整数的数组 nums，判断 nums 中是否存在三个元素 a，b，c ，使得 a + b + c = 0 ？请你找出所有满足条件且不重复的三元组。<br>
注意：答案中不可以包含重复的三元组。<br>
示例:<br>
给定数组 nums = [-1, 0, 1, 2, -1, -4]，<br>

满足要求的三元组集合为：<br>
[                    <br>
  [-1, 0, 1],         <br>
  [-1, -1, 2]         <br>
]                     <br>

* 思路:还是双指针的思路,先进行排序,但是因为有三个元素,选取一个元素作为固定值,进行遍历,然后此固定值向右移动一个单位,继续遍历.若三数和大于0,
移动右指针,小于0,移动左指针.但是三个元素都要考虑跳过重复值.这里我写的跳过重复值部分代码有些繁琐,应该可以整合为一个判定条件.<br>

``` cpp
class Solution
{
public:
    vector<vector<int>> threeSum(vector<int> &nums)
    {

        std::vector<std::vector<int>> result;
        //判定条件
        if (nums.size() < 3)
        {
            return result;
        }
        //排序
        sort(nums.begin(), nums.end());
        //取地址
        int *unmoved_ptr = &nums[0];
        int *end_ptr = &nums[nums.size() - 1];
        int value = nums[0];
        while (unmoved_ptr <= (end_ptr - 2))
        {
            //如果固定值大于0,退出循环,没有继续的必要了
            if (*unmoved_ptr > 0)
            {
                break;
            }
            int *left = unmoved_ptr + 1;
            int *right = end_ptr;
            while (right > left)
            {
                int sum = *unmoved_ptr + *left + *right;
                if (sum > 0)
                {
                    right--;
                    //跳过重复值
                    while (*right == *(right + 1) && right > left)
                    {
                        right--;
                    }
                }
                if (sum < 0)
                {
                    left++;
                    //跳过重复值
                    while (*left == *(left - 1) && right > left)
                    {
                        left++;
                    }
                }
                if (sum == 0)
                {
                    std::vector<int> tmp = {*unmoved_ptr, *left, *right};
                    result.emplace_back(tmp);
                    left++;
                    right--;
                    //跳过重复值
                    while (*right == *(right + 1) && right > left)
                    {
                        right--;
                    }
                    while (*left == *(left - 1) && right > left)
                    {
                        left++;
                    }
                }
            }
            value = *unmoved_ptr;
            unmoved_ptr++;
            //跳过重复值
            while (*unmoved_ptr == value && unmoved_ptr <= (end_ptr - 2))
            {
                unmoved_ptr++;
            }
        }
        return result;
    }
};
```
# 3.移除元素
给你一个数组 nums 和一个值 val，你需要 原地 移除所有数值等于 val 的元素，并返回移除后数组的新长度。

不要使用额外的数组空间，你必须仅使用 O(1) 额外空间并 原地 修改输入数组。

元素的顺序可以改变。你不需要考虑数组中超出新长度后面的元素。<br>
示例:<br>
给定 nums = [0,1,2,2,3,0,4,2], val = 2,<br>

函数应该返回新的长度 5, 并且 nums 中的前五个元素为 0, 1, 3, 0, 4。<br>

注意这五个元素可为任意顺序。<br>

你不需要考虑数组中超出新长度后面的元素。<br>
思路:这道题也是引用了双指针的思路,定义一个快指针,一个慢指针.同样也可以直接调用vector中的对应函数进行操作.<br>
``` cpp
class Solution
{
public:
    int removeElement(vector<int> &nums, int val)
    {
        int i = 0;
        for (int j = 0; j < nums.size(); j++)
        {
            if (nums[j] != val)
            {
                nums[i] = nums[j];
                i++;
            }
        }
        return i;
    }
};
```
# 4.搜索插入位置
给定一个排序数组和一个目标值，在数组中找到目标值，并返回其索引。如果目标值不存在于数组中，返回它将会被按顺序插入的位置。<br>
你可以假设数组中无重复元素。<br>
示例:<br>
输入: [1,3,5,6], 5 <br>
输出: 2

``` cpp
class Solution
{
public:
    int searchInsert(vector<int> &nums, int target)
    {
        int left = 0;
        int right = nums.size();
        while (left < right)
        {
            int mid = (left + right) / 2;
            if (nums[mid] == target)
            {
                return mid;
            }
            if (nums[mid] < target)
            {
                left = mid + 1;
            }
            if (nums[mid] > target)
            {
            //这里需要注意一下，若right = nums.size() -1, 则right = mid - 1,而且while的判定条件改为left <= right
                right = mid;
            }
        }
        return left;
    }
};
```
模板二：<br>
``` cpp
class Solution
{
public:
    int searchInsert(vector<int> &nums, int target)
    {
        int left = 0;
        int right = nums.size() - 1;
        while (left <= right)
        {
            int mid = (left + right) / 2;
            if (nums[mid] == target)
            {
                return mid;
            }
            if (nums[mid] < target)
            {
                left = mid + 1;
            }
            if (nums[mid] > target)
            {
                right = mid - 1;
            }
        }
        return left;
    }
};
```
# 5.最接近的三数之和
给定一个包括 n 个整数的数组 nums 和 一个目标值 target。找出 nums 中的三个整数，使得它们的和与 target 最接近。返回这三个数的和。假定每组输入只存在唯一答案。<br>
示例：<br>
输入：nums = [-1,2,1,-4], target = 1 <br>
输出：2 <br>
解释：与 target 最接近的和是 2 (-1 + 2 + 1 = 2)<br>
思路：双指针的思路，双边同时移动，不断更新最小差值并保存对应的临时sum
``` cpp
class Solution
{
public:
    int threeSumClosest(vector<int> &nums, int target)
    {
        if (nums.size() == 3)
        {
            return nums[0] + nums[1] + nums[2];
        }
        std::sort(nums.begin(), nums.end());
        //用于保存返回的值
        int res = 0;
        int minvalue = __INT32_MAX__;
        for (int i = 0; i < nums.size(); i++)
        {
            int left = i + 1;
            int right = nums.size() - 1;
            while (left < right)
            {
                //记录临时的和
                int tmp_sum = nums[i] + nums[left] + nums[right];
                //不断更新最小值并保存对应的临时和
                if (std::fabs(tmp_sum - target) <= minvalue)
                {
                    minvalue = std::fabs(tmp_sum - target);
                    res = tmp_sum;
                }
                if (tmp_sum < target)
                {
                    left++;
                }
                else if (tmp_sum > target)
                {
                    right--;
                }
                else
                {
                    return tmp_sum;
                }
            }
        }
        return res;
    }
};
```
# 6.在排序数组中查找元素的第一个和最后一个位置
给定一个按照升序排列的整数数组 nums，和一个目标值 target。找出给定目标值在数组中的开始位置和结束位置。<br>
你的算法时间复杂度必须是 O(log n) 级别。<br>
示例：<br>
输入: nums = [5,7,7,8,8,10], target = 8 <br>
输出: [3,4] <br>
参考链接：https://leetcode-cn.com/problems/find-first-and-last-position-of-element-in-sorted-array/solution/er-fen-cha-zhao-suan-fa-xi-jie-xiang-jie-by-labula/<br>
其中详细介绍了二分查找这一类的问题,包括查找某个值是否存在，查找左边界，右边界，文章强推，所以我综合了左右边界的查找思路，写出下面的代码，暂时没有相处将左右边界一起写的方式 <br>
```cpp
class Solution
{
public:
    vector<int> searchRange(vector<int> &nums, int target)
    {
        vector<int> res;
        int left = 0;
        int right = nums.size() - 1;
        bool find = true;
        //寻找左侧边界
        while (left <= right)
        {
            int mid = (left + right) / 2;
            if (target == nums[mid])
            {
                right = mid - 1;
            }
            else if (target > nums[mid])
            {
                left = mid + 1;
            }
            else if (target < nums[mid])
            {
                right = mid - 1;
            }
        }
        if (left >= nums.size() || nums[left] != target)
        {
            find = false;
        }
        //如果没有发现左边界，说明不存在该值，直接return
        if (!find)
        {
            res = {-1, -1};
            return res;
        }
        //若存在左边界，则去寻找右边界
        else
        {
            res.emplace_back(left);
            left = 0;
            right = nums.size() - 1;
            while (left <= right)
            {
                int mid = (left + right) / 2;
                if (target == nums[mid])
                {
                    left = mid + 1;
                }
                else if (target > nums[mid])
                {
                    left = mid + 1;
                }
                else if (target < nums[mid])
                {
                    right = mid - 1;
                }
            }
            res.emplace_back(right);
        }
        return res;
    }
};
```