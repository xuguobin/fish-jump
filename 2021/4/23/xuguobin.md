    class Solution {

        private int[][] rects;
        private int[] coordinateCntSum;
        private Random random;
        private int[] res;

        public Solution(int[][] rects) {
            this.rects = rects;
            this.coordinateCntSum = new int[rects.length];
            this.res = new int[2];
            this.random = new Random();
            for (int i = 0; i < rects.length; i ++) {
                int[] rect = rects[i];
                int x1 = rect[0], y1 = rect[1];
                int x2 = rect[2], y2 = rect[3];
                int coordinateCnt = (x2 - x1 + 1) * (y2 - y1 + 1);
                coordinateCntSum[i] = i == 0 ?
                        coordinateCnt : coordinateCnt + coordinateCntSum[i - 1];
            }
        }

        public int[] pick() {
            int target = random.nextInt(coordinateCntSum[coordinateCntSum.length - 1]) + 1;
            int l = 0, r = coordinateCntSum.length - 1;
            while (l <= r) {
                int mid = (l + r) >> 1;
                int val = coordinateCntSum[mid];
                if (val == target) {
                    fillResFromRect(mid);
                    return res;
                } else if (val < target) {
                    l = mid + 1;
                } else {
                    r = mid - 1;
                }
            }
            fillResFromRect(l);
            return res;
        }

        private void fillResFromRect(int index) {
            int[] rect = rects[index];
            int x1 = rect[0], y1 = rect[1];
            int x2 = rect[2], y2 = rect[3];
            int randomX = random.nextInt(x2 - x1 + 1) + x1;
            int randomY = random.nextInt(y2 - y1 + 1) + y1;
            res[0] = randomX;
            res[1] = randomY;
        }
    }