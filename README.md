# 202568
寻找两个正序数组的中位数
答案
 
#include <vector>
using namespace std;

double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
    int m = nums1.size(), n = nums2.size();
    if (m > n) return findMedianSortedArrays(nums2, nums1); // 保证nums1更短，减少计算量
    int left = 0, right = m, half = (m + n + 1) / 2; // half为划分后左半部分总长度
    while (left <= right) {
        int i = (left + right) / 2; // nums1的分割点
        int j = half - i; // nums2的分割点
        
        // 处理越界情况（用INT_MIN/MAX模拟）
        int nums1Left = (i == 0) ? INT_MIN : nums1[i-1];
        int nums1Right = (i == m) ? INT_MAX : nums1[i];
        int nums2Left = (j == 0) ? INT_MIN : nums2[j-1];
        int nums2Right = (j == n) ? INT_MAX : nums2[j];
        
        if (nums1Left <= nums2Right && nums2Left <= nums1Right) { // 找到合法分割
            if ((m + n) % 2 == 0) { // 偶数长度，取左右最小值的平均
                return (max(nums1Left, nums2Left) + min(nums1Right, nums2Right)) / 2.0;
            } else { // 奇数长度，取左半部分最大值
                return max(nums1Left, nums2Left);
            }
        } else if (nums1Left > nums2Right) { // nums1左半部分过大，左移i
            right = i - 1;
        } else { // nums2左半部分过大，右移i
            left = i + 1;
        }
    }
    return 0.0; // 不会执行到这里
}
 
 
题解
 
核心思路：二分查找分割点
 
将两个有序数组合并后的中位数问题转化为寻找第k小元素的问题，通过二分法在较短数组上搜索分割点，使得两个数组分割后左半部分元素均小于等于右半部分，从而快速定位中位数。
 
算法步骤：
 
1. 保证nums1更短：若nums2更短则交换数组，减少二分次数。
2. 确定分割目标：设总长度为 m+n ，左半部分需包含 half=(m+n+1)/2 个元素（奇数时左半多一个）。
3. 二分搜索分割点：
- 在nums1中二分查找分割点 i ，则nums2分割点为 j=half-i 。
- 检查分割点两侧元素是否满足 nums1左 ≤ nums2右 且 nums2左 ≤ nums1右 ：
- 若满足，根据总长度奇偶性计算中位数。
- 若不满足，调整 i 的范围（左半部分过大则左移，反之右移）。
 
复杂度分析：
 
- 时间复杂度：O(log(min(m,n)))，二分次数由较短数组长度决定，满足O(log(m+n))要求。
- 空间复杂度：O(1)，仅使用常数额外空间。
 
关键点：
 
- 越界处理：用INT_MIN/MAX模拟分割点越界时的虚拟元素，避免数组越界判断。
- 奇偶统一处理：通过 (m+n+1)/2 将奇偶情况统一为左半部分最大值的计算，简化代码逻辑。
