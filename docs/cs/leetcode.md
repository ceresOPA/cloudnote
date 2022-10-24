# leetcode刷题笔记

> 鉴于自己无论是简单还是难的题目，都老是会犯各种细节错误，而且还屡教不改，因此进行记录

## 一刷

### 数组篇

#### 1. 二分查找

力扣链接：https://leetcode.cn/problems/binary-search/

```c++
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int low = 0;
        int high = nums.size()-1;
        int mid;
        while(low<=high){   // 错误点1，不要忘记加 = 
            mid = (low+high)/2;
            // mid = low + ((high - low) / 2);// 防止溢出 等同于(low + high)/2
            if(target == nums[mid]){
                return mid;
            }else if(target > nums[mid]){
                low = mid+1; //错误点2，不要写成low = mid
            }else{
                high = mid-1; //错误点2，不要写成high = mid
            }
        }
        return -1;
    }
};
```

犯了两个错误：

- while循环的判断条件，一开始没有加 = ，导致在遇到 [5] 查找 5时，low为0，high为0，low与high相等没有进入循环，而出现返回值为-1的情况。而且在low和high的更新中，也是会存在low和high相等的情况下但还没有进行（low+high）/2下标判断的情况，所以一定不能忘记等于号。（PS：<font color="red">取不取 = 取决于 一开始选择的 区间，如果high直接取nums.size()，则不用加 = </font>,如果取 =，则由于nums.size()对应的下标是不存在的，所以可能会出现下标溢出的问题）
- 在更新low和high时，不要忘记给mid加1或减1，因为mid下标的元素显然已经不可能等于target了，而且当遇到low和high都等于mid时，这时不去加1或减1，就没法出现low>high的情况，从而结束循环了。

关于(low+high)/2的溢出问题：

- 如果出现low+high之和大于int的最大值时，就会出现溢出

#### 2. 移除元素

力扣链接：https://leetcode.cn/problems/remove-element/

```c++
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int tmp;
        int count = 0;
        for(int i=0;i<nums.size();i++){
            for(int j=0;j<nums.size()-1;j++){
                if(nums[j]==val){
                    tmp = nums[j+1];
                    nums[j+1] = nums[j];
                    nums[j] = tmp;
                }
            }
        }
        // printf("%d\n",count);
        
        for(int i=0;i<nums.size();i++){
            if(nums[i]==val){
                count++;
            }
        }
        
        return nums.size()-count;

    }
};
```

这个答案写的很垃圾，一开始想的是既然要原地更新数组，那就把要删除的元素值放到后面去，所以就采取了类似于冒泡排序的思想，就类似于假定等于val值的这个元素是最大的，然后就一直交换往后排这样，得到的时间复杂度为O(n^2)。这样确实可以写出来，但是这相当鸡肋，因为其实以最直观的方法来解决的话，就对于数组要移除一个元素的方法，那不就是把后面的元素往前面移动然后覆盖要移除的元素不就完事了，这种暴力解的复杂度就是O(n^2)，所以我的方法属于是多此一举了。

当然如果使用到 双指针 的话，那在O(n)的时间内就能够完成！！

```c++
// 前移覆盖元素
/**
简单题都要卡很久，脑子都没打开
**/
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int size = nums.size();
        for(int i=0;i<size;i++){
            if(nums[i]==val){    
                for(int j=i;j<size-1;j++){
                    nums[j] = nums[j+1];
                }
                i--; // 因为前移了元素，因此此时i下标位置的元素并没有进行判断，不然的话直接到i+1，就把i下标元素给过掉了
                size--; // 相当于更新数组，直接把原来最后位置的元素给忽略不存在
            }
            
        }
        return size;
    }
};
```



#### 3. 有序数组的平方

### 链表篇

#### 1. 移除链表元素



#### 2. 设计链表

力扣链接：https://leetcode.cn/problems/design-linked-list/

```c++
class MyLinkedList {
public:

    struct LinkNode{
        int val;
        struct LinkNode* next;
        LinkNode(int val):val(val),next(nullptr){}
    };

    int size;
    LinkNode* head;

    MyLinkedList() {

        head = new LinkNode(0);
        size = 0;

    }
    
    int get(int index) {

        if(index>=size||index<0){
            return -1;
        }

        LinkNode* p = head->next;

        for(int i=0;i<index;i++){
            p = p->next;
        }

        return p->val;

    }
    
    void addAtHead(int val) {

        LinkNode* newNode = new LinkNode(val);
        newNode->next = head->next;
        head->next = newNode;
        size++;
    }
    
    void addAtTail(int val) {
        LinkNode* p = head;
        while(p->next!=nullptr){
            p = p->next;
        }
        LinkNode* newNode = new LinkNode(val);
        p->next = newNode;
        size++;
    }
    
    void addAtIndex(int index, int val) {
        LinkNode* p = head;
        if(index<=size){
            for(int i=0;i<index;i++){
                p = p->next;
            }
            LinkNode* newNode = new LinkNode(val);
            newNode->next = p->next;
            p->next = newNode;
            size++;
        }
    }
    
    void deleteAtIndex(int index) {
        LinkNode* p = head;
        if(index<0||index>=size){
            return;
        }
        for(int i=0;i<index;i++){ 
                p = p->next;
            }
            LinkNode* q = p->next;
            p->next = q->next;
            delete q;
            size--;
        
    }
};
```

这道题，看起来是不难的，但是自己写的时候真的是漏洞百出，而且有时候就一个有没有加=号的问题，半天查不出来。

- for循环中，要循环几次的问题
- 边界条件的判断，什么时候才能增加，什么时候才能减少

#### 3. 翻转链表

力扣链接：https://leetcode.cn/problems/reverse-linked-list/

翻转链表的解决方法挺多的：

- 一开始想到的就是用**头插法**来新建一个链表，因为头插法得到的链表正好是个相反的，但头插法需要使用到额外的内存空间，时间复杂度为O(n)，空间复杂度也为O(n)
- 要原地改变链表的话，则可以使用到**双指针**，通过前后两个指针不断地翻转链表元素，可以让空间复杂度降到O(1)
- 双指针的方法也还可以使用递归方式进行实现，但使用递归的话，空间复杂度又会变为O(n)。除此之外，还可以使用另一种递归的方式，从链表的最后一个元素开始翻转，而不是一个个翻转过去，代码如下（理解了挺久的）

```c++
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
		// 这里的head==nullptr是针对空链表的，而head->next==nullptr是对于单个元素链表以及返回为新的头结点，因为翻转的最后一个元素就是新链表的第一个元素嘛
        if(head==nullptr||head->next==nullptr){
            return head;
        }
        ListNode* newHead = reverseList(head->next); //一直走到最后，然后再进行翻转
        head->next->next = head;
        head->next = nullptr;

        return newHead;

    }
};
```

#### 4. 两两交换链表中的节点

力扣链接：https://leetcode.cn/problems/swap-nodes-in-pairs/

```c++
class Solution {
public:
    ListNode* swapPairs(ListNode* head) {
        if(head==nullptr||head->next==nullptr){
            return head;
        }
        ListNode* p = new ListNode(0,head);
        ListNode* newHead = p;
        // 没有想到这个判断条件，太蠢了，一直用p!=nullptr这样的在试
        while(p->next!=nullptr&&p->next->next!=nullptr){
            ListNode* q = p->next;
            ListNode* r = p->next->next->next;

            p->next = q->next;
            q->next->next = q;
            q->next = r;

            p = p->next->next;

        }
        return newHead->next;
    }
};
```



### 哈希表

#### 6.赎金信

力扣链接：https://leetcode.cn/problems/ransom-note/

```c++
class Solution {
public:
    bool canConstruct(string ransomNote, string magazine) {
        vector<int> v(26);
        for(auto m:magazine){
            v[m-'a']++;
        }
        for(auto r:ransomNote){
            if(v[r-'a']<=0){
                return false;
            }
            v[r-'a']--;
        }
        return true;
    }
};
```

经过前面几道题对哈希表的学习，也是能够知道怎么简单应用了。