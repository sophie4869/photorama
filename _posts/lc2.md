---
title: lc2-Add Two Numbers
date: 2015-08-30 21:00:17
tags: [lc-medium, linked-list]
categories: leetcode
---

```c++

class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        int carry=0;
        ListNode* res,*headptr;
        res=new ListNode(0);
        headptr=res;
        while(l1!=NULL||l2!=NULL){
            int sum = (l1==NULL?0:l1->val)+(l2==NULL?0:l2->val)+carry;
            carry = sum/10;
            res->next=new ListNode(sum%10);
            res=res->next;
            l1=l1==NULL?NULL:l1->next;
            l2=l2==NULL?NULL:l2->next;
        }
        if(carry) res->next=new ListNode(1);
        return headptr->next;
    }
};
```