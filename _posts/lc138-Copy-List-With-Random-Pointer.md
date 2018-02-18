---
title: lc138 Copy List With Random Pointer
date: 2016-10-30 11:11:04
tags: [lc-hard, linked list]
categories: leetcode
---

最開始用的Naive版本效率很低，想法是用vector記錄pointer地址，然後對每個原list里的random指針，在vector里找到對應index，於是知道了新duplicate的鏈錶中對應的地址，assign給新鏈錶節點的random指針。每次都要find，時間複雜度O(N^2)。

```c++
class Solution {
public:
    RandomListNode *copyRandomList(RandomListNode *head) {
        if (head == nullptr) return nullptr;
        RandomListNode *p = head, *prev = nullptr;
        vector<RandomListNode *> v1, v2;
        while (p != NULL) {
            v1.push_back(p);
            RandomListNode *tmp;
            tmp = new RandomListNode(p -> label);
            v2.push_back(tmp);
            if (prev!= nullptr) prev -> next = tmp;
            prev = tmp;
            p = p -> next;
        }
        p = head;
        RandomListNode *cur = v2[0];
        while (p != NULL) {
            if (p->random != nullptr) {
                auto pos = find(v1.begin(), v1.end(), p->random);
                cur -> random = v2[pos-v1.begin()];
            }
            else cur -> random = nullptr;
            p = p -> next;
            cur = cur -> next;
        }
        return v2[0];
    }
};
```

Discussion有兩種有趣的做法，都是修改原來的鏈錶，一種是將原鏈錶和新複製的用next串起來，這樣可以根據next找到原來節點對應的複製節點，從而assign random；另外一種我更喜歡的做法是修改原來鏈錶的random指向複製節點，複製節點的random先暫時指向原來random指針指向的節點，再循環一遍更新為原來節點的新random指針指向的複製節點。最後再做恢復。時間都是O(N)，除複製節點外空間前者O(1)，後者O(N)。前者通過複製鏈錶的next指向原來的next節點節省了空間，但後者的空間無法節省下來因為更新完複製節點的random以後原來節點的random無法不依靠外界信息restore了。本質其實類似。實現了一下：

```c++
class Solution {
public:
    RandomListNode *copyRandomList(RandomListNode *head) {
        if (head == nullptr) return nullptr;
        RandomListNode *p = head, *prev = nullptr, *newHead = nullptr;
        vector<RandomListNode *> v;
        int cnt = 0;
        while (p != NULL) {
            RandomListNode *tmp = new RandomListNode(p -> label);
            v.push_back(p -> random);
            tmp -> random = p -> random;
            if (!cnt++) newHead = tmp;
            p -> random = tmp;
            if (prev!= nullptr) prev -> next = tmp;
            prev = tmp;
            p = p -> next;
        }
        p = newHead;
        while (p != NULL) {
            if (p -> random) p -> random = p -> random -> random;
            p = p -> next;
        }
        p = head;
        int i = 0;
        while (p != NULL) {
            p -> random = v[i++];
            p = p -> next;
        }
        return newHead;
    }
};
```

用遞歸來完成：

```c++
class Solution {
    map<RandomListNode*, RandomListNode*> mp;
public:
    RandomListNode *copyRandomList(RandomListNode *head) {
        if (head == nullptr) return nullptr;
        if (mp[head]!=nullptr) return mp[head];
        mp[head] = new RandomListNode(head->label);
        mp[head] -> next = copyRandomList(head->next);
        mp[head] -> random = copyRandomList(head->random);
        return mp[head];
    }
};
```