## leetcode  
[19. Remove Nth Node From End of List](https://leetcode.com/problems/reverse-linked-list/)

### Description
Given a linked list, remove the n-th node from the end of list and return its head.  
### Example:  
Given linked list: 1->2->3->4->5, and n = 2.  
After removing the second node from the end, the linked list becomes 1->2->3->5.  
### Note:  
Given n will always be valid.  
### Follow up:  
Could you do this in one pass?  

### 思路    
这道题目让我们移除链表倒数第n个结点，且给出的n是永远有效的，即n不会大于链表长度，要求只能用一次遍历解决。  
我们使用两个指针cur和front,先让cur走n步，如果cur为空，则表示n为链表长度，需要移除头结点，直接返回head.next即可；  
如果cur不为空，让front指向头结点，font和cur指针同时向后移动，当cur.next为空时，表示走到链表尾部，此时font指向的是需要移除结点的前结点，移除该结点即可，font.next = font.next.next。

```Java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode cur = head;
        ListNode font = head;
        for (int i = 0; i < n; i++) {
            cur = cur.next;
        }
        if (cur == null) {
            return head.next;
        }
        while(cur.next != null) {
            cur = cur.next;
            font = font.next;
        }
        
        font.next = font.next.next;
        return head;
    }
}
```
```C
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     struct ListNode *next;
 * };
 */
struct ListNode* removeNthFromEnd(struct ListNode* head, int n){
    struct ListNode* cur = head;
    struct ListNode* font = head;
    for (int i = 0; i < n; i++) {
        cur = cur->next;
    }
    if (cur == NULL) {
        return head->next;
    }
    while(cur->next != NULL) {
        font = font->next;
        cur = cur->next;
    }
    font->next = font->next->next;
    return head;
}
```
