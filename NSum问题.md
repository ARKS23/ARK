# 数之和问题

### 只有唯一答案，可以排序+双指针
```C++
vector<int> twoSum(vector<int>& nums, int target) {
    // 先对数组排序
    sort(nums.begin(), nums.end());
    // 左右指针
    int lo = 0, hi = nums.size() - 1;
    while (lo < hi) {
        int sum = nums[lo] + nums[hi];
        // 根据 sum 和 target 的比较，移动左右指针
        if (sum < target) {
            lo++;
        } else if (sum > target) {
            hi--;
        } else if (sum == target) {
            return {lo, hi};
        }
    }
    return {};
}
```

### 答案不唯一，排序+双指针+去重
```C++
vector<vector<int>> twoSum(vector<int>& nums, int target) {
    vector<vector<int>> result;
    // 先对数组排序
    sort(nums.begin(), nums.end());
    // 左右指针
    int L = 0, R = nums.size() - 1;
    while (L < R) {
        int left = nums[L];
        int right = nums[R];
        int sum = left + right;
        if (sum == target) { //去重
            result.push_back({left, right});
            while (L < R && nums[L] == left) 
                L++;
            while (L < R && nums[R] == right)
                R--;
        }
        else if (sum < target) { //优化去重
            while (L < R && nums[L] == left)
                L++;
        }
        else {          //优化去重
            while (L < R && nums[R] == right)
                R--;
        }
    }
    return result;
}
```

### 三数之和
- 在两数之和的基础上穷举第一个数字，并去重
```C++
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        vector<vector<int>> result;
        for (int i = 0; i < nums.size(); ++i) {
            vector<vector<int>> temp = twoSum(nums, i + 1, 0 - nums[i]);
            for (auto &t : temp) {
                t.push_back(nums[i]);
                result.push_back(t);
            }
            while (i < nums.size() - 1 && nums[i] == nums[i + 1]) ++i; //去重
        }
        return result;
    }

    vector<vector<int>> twoSum(vector<int>& nums, int start,int target) {
        vector<vector<int>> result;
        int L = start, R = nums.size() - 1;
        while (L < R) {
            int left = nums[L];
            int right = nums[R];
            int sum = left + right;
            if (sum == target) {
                result.push_back({left, right});
                while (L < R && left == nums[L]) L++;
                while (L < R && right == nums[R]) R--;
            }
            else if (sum < target) 
                while (L < R && left == nums[L]) L++;
            else
                while (L < R && right == nums[R]) R--;
        }
        return result;
    }
};
```

### 四数之和
- 在三数的基础上改装，和之前同理.更改起始索引和去重步骤
```C++
class Solution {
public:
    vector<vector<int>> fourSum(vector<int>& nums, int target) {
        sort(nums.begin(), nums.end());
        vector<vector<int>> result;
        for (int i = 0; i < nums.size(); ++i) {
            vector<vector<int>> temp = threeSum(nums, i + 1, target - nums[i]); //更改起始索引
            for (auto &t : temp) {
                t.push_back(nums[i]);
                result.push_back(t);
            }
            while (i < nums.size() - 1 && nums[i] == nums[i + 1]) ++i; //去重
        }
        return result;
    }

private:
    vector<vector<int>> threeSum(vector<int>& nums, int start, int target) {
        vector<vector<int>> result;
        for (int i = start; i < nums.size(); ++i) {
            vector<vector<int>> temp = twoSum(nums, i + 1, target - nums[i]);//更改起始索引
            for (auto &t : temp) {
                t.push_back(nums[i]);
                result.push_back(t);
            }
            while (i < nums.size() - 1 && nums[i] == nums[i + 1]) ++i; //去重
        }
        return result;
    }

    vector<vector<int>> twoSum(vector<int>& nums, int start,int target) {
        vector<vector<int>> result;
        int L = start, R = nums.size() - 1;
        while (L < R) {
            int left = nums[L];
            int right = nums[R];
            int sum = left + right; //三种情况去重
            if (sum == target) {
                result.push_back({left, right});
                while (L < R && left == nums[L]) L++;
                while (L < R && right == nums[R]) R--;
            }
            else if (sum < target)
                while (L < R && left == nums[L]) L++;
            else
                while (L < R && right == nums[R]) R--;
        }
        return result;
    }
};
```