## 1

给定一个整数数组和一个目标值，找出数组中和为目标值的两个数。

你可以假设每个输入只对应一种答案，且同样的元素不能被重复利用。

示例:

给定 nums = [2, 7, 11, 15], target = 9

因为 nums[0] + nums[1] = 2 + 7 = 9
所以返回 [0, 1]

    var twoSum = function (nums, target) {
        var res = [0, 0];
        var _nums = nums;
        var _len = _nums.length;
        if (_len < 2) {
            return res;
        }
        for (var n = 0; n < _len; n++) {
            var _n = _nums[n];
            var cur = target - _n;
            var m = nums.indexOf(cur);
            if (m > -1 && n !== m) {
                res = [n, m];
                return res;
            }
        }

        return res;
    };

## 2
给定一个已按照升序排列 的有序数组，找到两个数使得它们相加之和等于目标数。

函数应该返回这两个下标值 index1 和 index2，其中 index1 必须小于 index2。

说明:

返回的下标值（index1 和 index2）不是从零开始的。
你可以假设每个输入只对应唯一的答案，而且你不可以重复使用相同的元素。
示例:

输入: numbers = [2, 7, 11, 15], target = 9
输出: [1,2]
解释: 2 与 7 之和等于目标数 9 。因此 index1 = 1, index2 = 2 。


    var twoSum = function (nums, target) {
        var res = [0, 0];

        for (var i = 0; i < nums.length; i++) {
            var m = target - nums[i];
            var index = nums.indexOf(m);
            if (index > -1 && i !== index) {
                return [i + 1, index + 1].sort(function(a, b) {
                    return a - b;
                });
            }
        }

        return res;
    };



给定一个二叉搜索树和一个目标结果，如果 BST 中存在两个元素且它们的和等于给定的目标结果，则返回 true。

案例 1:

    输入: 
        5
       / \
      3   6
     / \   \
    2   4   7

    Target = 9

    输出: True
 

案例 2:

    输入: 
        5
       / \
      3   6
     / \   \
    2   4   7

    Target = 28

    输出: False

-

    var findTarget = function (root, k) {
        if (!root) {
            return false;
        }
        let queue = [], remainders = [];
        queue.push(root);
        while (queue.length) {
            let curr = queue.shift();
            if (remainders.indexOf(curr.val) !== -1) return true;
            remainders.push(k - curr.val);
            if (curr.left) {
                queue.push(curr.left);
            }

            if (curr.right) {
                queue.push(curr.right);
            }

        }
        return false;
    };

## 3


给定两个非空链表来表示两个非负整数。位数按照逆序方式存储，它们的每个节点只存储单个数字。将两数相加返回一个新的链表。

你可以假设除了数字 0 之外，这两个数字都不会以零开头。

示例：

输入：(2 -> 4 -> 3) + (5 -> 6 -> 4)
输出：7 -> 0 -> 8
原因：342 + 465 = 807

    var addTwoNumbers = function(l1, l2) {
        var carray = 0;
        var ary = [];
        var p = l1, q = l2;
        while(p || q){
            var pv = 0, qv = 0;
            if (p && p.val) {
                pv = p.val;
            };
            if (q && q.val) {
                qv = q.val;
            };
            var tv = pv + qv;
            console.log(tv);
            if (carray > 0) {
                tv += carray;
                carray = 0;
            }
            if (tv >= 10) {
                tv -= 10;
                carray = 1;
            }
            ary.push(tv);
            p = p && p.next || null;
            q = q && q.next || null;
        }
        if (carray > 0) {
            ary.push(1);
        }
        return ary;
    };
