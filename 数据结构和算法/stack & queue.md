## Stack & Queue

* Stack :先入后出，添加删除皆为O(1)
* Queue :先入先出，添加删除皆为O(1)
* Prority Queue :插入O（1）。取出O(logN)
底层实现的数据结构较为多样和复杂：heap

## tips:
- 滑动窗口解法往往用双端队列解决，例题.[滑动窗口最大值](https://leetcode-cn.com/problems/sliding-window-maximum/)
- 括号问题，符合先进后出，利用栈解决,例题[有效的括号](https://leetcode-cn.com/problems/valid-parentheses/)
- 单调栈：维持一个递增或递减的栈，剥离属性得到模型后发现例题.[柱状图中最大矩形](https://leetcode-cn.com/problems/largest-rectangle-in-histogram/),可以用单调栈解决
- 哨兵技巧：可以巧妙解决栈为空的判断情况，加入哨兵可以使得代码更简洁、巧妙 例题 .[柱状图中最大矩形](https://leetcode-cn.com/problems/largest-rectangle-in-histogram/)
-   最小栈问题内部往往维护了一个辅助栈