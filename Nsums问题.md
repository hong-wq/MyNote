[TOC]

# Nsums问题

## 1.两数之和

[数组无序、且求下标](https://leetcode.cn/problems/two-sum/)

* 哈希法

  本质为在数组里查找快速找到target - num的位置

  ```c++
  class Solution {
  public:
      vector<int> twoSum(vector<int>& nums, int target) {
          unordered_map<int,int>mp;
          for(int i = 0; i < nums.size(); i++){
              auto it = mp.find(target- nums[i]);
              if(it != mp.end()){
                  return{it->second,i};
              }
              mp[nums[i]] = i;
          }
          return {};
      }
  };
  ```

[数组有序](https://leetcode.cn/problems/kLl5u1/submissions/)

* sort + 双指针

  ```c++
  class Solution {
  public:
      vector<int> twoSum(vector<int>& numbers, int target) {
          int l = 0, r = numbers.size() - 1;
          while(l < r){
              int sum = numbers[l] + numbers[r];
              if(sum < target){
                  l++;
              }else if(sum > target){
                  r--;
              }else{
                  return {l,r};
              }
          } 
          return {};
  
      }
  };
  ```

  

## 2.nsum问题模板

* 双指针 + 递归

* 要sort

  ```c++
  class Solution {
  public:
      vector<vector<int>>nSum(vector<int>&nums, long long target, int n, int start){
          vector<vector<int>>res;
          if(n > nums.size() || n < 2) return res;
          if(n == 2){
              int l = start, r = nums.size() - 1;
              while(l < r){
                  int lnum = nums[l], rnum = nums[r];
                  long long sum = lnum + rnum;
                  if(sum < target){
                      l++;
                  }else if(sum > target){
                      r--;
                  }else{
                      res.push_back({lnum, rnum});
                      while(l < r && nums[l] == lnum) l++;//去重逻辑,相等就往后走
                      while(l < r && nums[r] == rnum) r--;
                  }
              }
          }else{
              for(int i = start; i < nums.size(); i++){
                  int num = nums[i];
                  vector<vector<int>>nres = nSum(nums,target - num,n - 1,i + 1);
                  for(auto &tuple : nres){
                      tuple.push_back(num);
                      res.push_back(tuple);
                  }
                  while(i + 1 < nums.size() && nums[i] == nums[i + 1]) i++;//去重逻辑
              }
          }
          return res;  
  }
      vector<vector<int>> fourSum(vector<int>& nums, int target) {
          sort(nums.begin(), nums.end());
          //return nSum(nums,target,4,0);
      }
  };
  ```

  





