


## 基础解
1. 遍历两个链表将它们分别转化成整数
2.对两个整数求和
3.将和再转换成链表

- fail case: 越界

```js
/**
 * Definition for singly-linked list.
 * function ListNode(val, next) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.next = (next===undefined ? null : next)
 * }
 */
/**
 * @param {ListNode} l1
 * @param {ListNode} l2
 * @return {ListNode}
 */
var addTwoNumbers = function(l1, l2) {
    let digit = 0, // 0表示各位，1表示十位
        n1 =  0 // 链表对应的整数

    while(l1 !== null){
        const pow = Math.pow(10, digit)
        n1 += l1.val * pow
        l1 = l1.next // 移动到下一个节点
        digit++ // 位数加+
    }

    digit = 0 // 重置位数
    let n2 =  0
     while(l2 !== null){
        const pow = Math.pow(10, digit)
        n2 += l2.val * pow
        l2 = l2.next // 移动到下一个节点
        digit++ // 位数加+
    }
    let sum = n1 + n2
    const head = new ListNode()
    let curr = head
    if (sum === 0) {
        head.next = new ListNode(0)
        return head.next
    }
    while(sum > 0) {
        const val = sum % 10 // 每次取出当前最低位
        curr.next = new ListNode(val) // 创建新节点，插入链表尾部
        curr = curr.next // 链表尾指针移动
        sum = Math.floor(sum / 10)
    }
    return head.next
 };
```

## 更优解


leetcode 67 371 445 989
