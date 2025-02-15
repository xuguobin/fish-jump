//You are given an array of positive integers w where w[i] describes the weight 
//of ith index (0-indexed). 
//
// We need to call the function pickIndex() which randomly returns an integer in
// the range [0, w.length - 1]. pickIndex() should return the integer proportional
// to its weight in the w array. For example, for w = [1, 3], the probability of p
//icking the index 0 is 1 / (1 + 3) = 0.25 (i.e 25%) while the probability of pick
//ing the index 1 is 3 / (1 + 3) = 0.75 (i.e 75%). 
//
// More formally, the probability of picking index i is w[i] / sum(w). 
//
// 
// Example 1: 
//
// 
//Input
//["Solution","pickIndex"]
//[[[1]],[]]
//Output
//[null,0]
//
//Explanation
//Solution solution = new Solution([1]);
//solution.pickIndex(); // return 0. Since there is only one single element on t
//he array the only option is to return the first element.
// 
//
// Example 2: 
//
// 
//Input
//["Solution","pickIndex","pickIndex","pickIndex","pickIndex","pickIndex"]
//[[[1,3]],[],[],[],[],[]]
//Output
//[null,1,1,1,1,0]
//
//Explanation
//Solution solution = new Solution([1, 3]);
//solution.pickIndex(); // return 1. It's returning the second element (index = 
//1) that has probability of 3/4.
//solution.pickIndex(); // return 1
//solution.pickIndex(); // return 1
//solution.pickIndex(); // return 1
//solution.pickIndex(); // return 0. It's returning the first element (index = 0
//) that has probability of 1/4.
//
//Since this is a randomization problem, multiple answers are allowed so the fol
//lowing outputs can be considered correct :
//[null,1,1,1,1,0]
//[null,1,1,1,1,1]
//[null,1,1,1,0,0]
//[null,1,1,1,0,1]
//[null,1,0,1,0,0]
//......
//and so on.
// 
//
// 
// Constraints: 
//
// 
// 1 <= w.length <= 10000 
// 1 <= w[i] <= 10^5 
// pickIndex will be called at most 10000 times. 
// 
// Related Topics 二分查找 Random 
// 👍 89 👎 0

package leetcode.editor.cn;

import java.util.Random;

//java:Random Pick with Weight
public class P528RandomPickWithWeight {
    public static void main(String[] args) {
        Solution solution = new P528RandomPickWithWeight().new Solution(null);
    }

    class Solution {

        private int[] weightSum;

        private Random random;

        public Solution(int[] w) {
            this.weightSum = new int[w.length];
            this.random = new Random();
            weightSum[0] = w[0];
            for (int i = 1; i < w.length; i++)
                weightSum[i] = w[i] + weightSum[i - 1];
        }

        public int pickIndex() {
            int target = random.nextInt(weightSum[weightSum.length - 1] - weightSum[0] + 1) + weightSum[0];
            int minIndex = 0, maxIndex = weightSum.length - 1;
            while (minIndex <= maxIndex) {
                int idx = (minIndex + maxIndex) >> 1;
                if (target == weightSum[idx]) {
                    return idx;
                } else if (target < weightSum[idx]) {
                    if (idx == 0 || target > weightSum[idx - 1]) {
                        return idx;
                    } else {
                        maxIndex = idx - 1;
                    }
                } else {
                    minIndex = idx + 1;
                }
            }
            return minIndex;
        }
    }

/**
 * Your Solution object will be instantiated and called as such:
 * Solution obj = new Solution(w);
 * int param_1 = obj.pickIndex();
 */
}