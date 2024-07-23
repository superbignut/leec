#### 1.接雨水
描述: 给一个数组height，问总能接多少雨水：

+ 用dp求解，找规律 + 边界条件
+ 
        vector<int> leftMax(n);
        leftMax[0] = height[0];
        for (int i = 1; i < n; ++i) {
            leftMax[i] = max(leftMax[i - 1], height[i]);
        }


#### 2.无重复字符的最长字串
描述： 找到字符串中不重复的最长部分

+  使用 unordered_set : find, erase, insert, 并配合left指向最左侧边界，维护了一个类似于**队列**的数据结构：
  
        int left = 0;
        for(int i = 0;i< s.size(); ++i) {
            while(window.find(s[i]) != window.end()) {
                window.erase(s[left]);
                left ++;
            }
            window.insert(s[i]);
        }
#### 3.找到字符串中另一个字符串的异位词
描述： 找到异位词的位置

+ 使用 vec 来存储 异位词中各个字母的数量, 进而可以直接使用 == 比较两个 vec 是否相等。也是维护了一个窗口。

        for(int i = 0; i < slen - plen; ++i ) {
            vecs[s[i] -'a'] --;
            vecs[s[i + plen] -'a'] ++;

            if (vecp == vecs) {
                ret.push_back(i+1);
            }
#### 4.和为 K 的子数组
描述： 找到大数组中求和为k的子数组

+ 创建了一个前缀和Vec， 加速了遍历求和的过程，注意的地方为，presum[0]=0;
  
        presum[0]  = 0;
        // 先计算presum 数组
        for(int i =0;i< nums_len ; ++i) {
            presum[i+1] = presum[i] + nums[i];
        }

        for( int l =0; l <= nums_len; ++l) {
            for(int r = l+1 ; r <= nums_len; ++r) {

                if (presum[r] - presum[l] == k) {
                    ret ++;
                }
            }
        }
+ 第二种解法是用map的形式， 在计算前缀和的遍历时，就开始，在之前的已经计算过的前缀和中，寻找是否有 相应的 map[pre - k] 存在，进而也达到了遍历的效果

        unordered_map<int, int> mp;

        mp[0] = 1;
        int ret = 0, pre = 0;

        for(auto &item : nums ) {
            pre += item;
            if(mp.find(pre - k) != mp.end())
            {
                ret += mp[pre- k];
            }
            mp[pre] ++;
        }
#### 5.最大子数组和
描述： 找出一个具有最大和的连续子数组

+ 最经典的dp问题，再次看到后还是会卡住，主要问题是，没有将子问题建立好，子问题是：

+ dp[i]等于 以i结尾的 最大字数组和。
+ 
        int dp， maxdp = nums[0];
        
        for(int i=1; i< nums.size(); ++i) {
            dp = max(dp + nums[i], nums[i]);
            maxdp = max(maxdp, dp);
        }

#### 6.合并区间
描述： 合并[[1,3],[2,6],[8,10],[15,18]] 的重复区间

+ 先排序，再合并，主要是排序比较重要，c++里就是要写一个lambda函数

        std::sort(merg.begin(), merg.end(),[](vector<int> a, vector<int> b){return a[0] < b[0];});
#### 7.翻转数组
描述： 给定一个整数数组 nums，将数组中的元素向右轮转 k 个位置，其中 k 是非负数。

+ 经典的问题，和经典的技巧，先全部翻转，再翻转两个切片。其中的reverse函数，也很精炼。

        void reverse(vector<int>& nums, int start, int end){
            while(start < end) {
                swap(nums[start], nums[end]);
                start ++;
                end --;
            }
        }
        void rotate(vector<int>& nums, int k) {
            int len = k % nums.size();
            this->reverse(nums, 0, nums.size()-1);
            this->reverse(nums, 0, len-1);
            this->reverse(nums, len, nums.size()-1);        
    }
#### 8.除自身以外数组的乘积
描述： 不用除法，计算除当前位置的元素之外的，左右两边所有元素的乘积。

+ 计算出一个前缀数组和一个后缀数组，相乘即可。感觉这类题大多的目的是减小时间复杂度，反而空间复杂度没那么重要。

        for(int i=1; i< nums.size(); ++i) {
            left[i] = left[i-1] * nums[i-1];
        }

        for(int i = nums.size()-2; i >=0; --i) {
            right[i] = right[i+1] * nums[i+1];
        }
#### 9. 矩阵置零
描述：给定一个 m x n 的矩阵，如果一个元素为 0 ，则将其所在行和列的所有元素都设为 0 。使用 原地 算法。

+ 额外建立一个m 和n 的数组，用来存储第i行和第j列是否有0元素存在。最优存储空间的没看。

        for(int i =0; i< m; ++i){
            for(int j = 0;j<n;++j){
                if(matrix[i][j] == 0){
                    mm[i] = 0;
                    nn[j] = 0;
                }
            }
        }
#### 10.螺旋矩阵
描述： 给你一个 m 行 n 列的矩阵 matrix ，请按照 顺时针螺旋顺序 ，输出返回矩阵中的所有元素。

+ 其实就是一个转圈的问题，但是边界判断不好判定，题解使用不断更新 u d l r的方法来判断是否结束，并控制循环，好巧妙。while( true ) 也好巧妙。

        while(true){

            for(int i =l; i<= r; ++i) {
                ret.push_back(matrix[u][i]);
            }
            ++ u;
            if(u > d) break;

            for(int j = u; j<= d;++j){
                ret.push_back(matrix[j][r]);
            }
            -- r;
            if(r < l) break;

            for(int i =r; i>= l; --i) {
                ret.push_back(matrix[d][i]);
            }
            -- d;
            if(d < u) break;

            for(int j = d; j>= u; --j) {
                ret.push_back(matrix[j][l]);
            }
            ++ l;
            if(l> r) break;
        }
#### 11.旋转图像
描述：给定一个 n × n 的二维矩阵 matrix 表示一个图像。请你将图像顺时针旋转 90 度。

+ 有是一个比较巧妙的做法，先对角线折叠一次，再树直 折叠一次即可。
  
        for(int i =0; i< m_size; ++i) { //对角线
            int temp;
            for(int j = 0; j<=i; ++j){
                if(i ==j ) continue;

                temp = matrix[i][j];
                matrix[i][j] = matrix[j][i];
                matrix[j][i] = temp;

            }

        }

        for(int i =0; i< m_size; ++i) { //树直
            int temp;
            for(int j = 0; j<m_size / 2; ++j){
                                
                temp = matrix[i][j];
                matrix[i][j] = matrix[i][m_size - 1 - j];
                matrix[i][m_size - 1 - j] = temp;

            }

        }

#### 12.搜索递增的二维矩阵
描述：编写一个高效的算法来搜索 m x n 矩阵 matrix 中的一个目标值 target 。该矩阵具有以下特性：

每行的元素从左到右升序排列。

每列的元素从上到下升序排列。

+ 学到一个函数的用法,lower_bound(begin, end, target, comp), 可以返回第一个不小于target的指针， 或者自定义比较函数，并且这个算法应该是内部采用的二分法，所以使用的前提是，序列中所有true的元素一定要在false元素的左边。
  
        for(auto & row : matrix) {
            auto ans = lower_bound(row.begin(), row.end(), target);
            if(ans != row.end() && *(--ans) == target ) return true;
        }

#### 13.相交链表
描述：给你两个单链表的头节点 headA 和 headB ，请你找出并返回两个单链表相交的起始节点。如果两个链表不存在相交节点，返回 null 。

+ 思路是，当一个链的节点走到NULL的时候，就从另一个节点头开始，这样可以保证，当遍历的 m+ n次之后，这两个指针一定会相遇，如果是有重合点的话。如果没有重合节点的话，也会在NULL上重合。比较关键的思想就是当跑完自己的长度，再去跑对方的长度，最后一定会遇到共同终点，如果有重合就一定会遇到重合点。太牛了。

        while(a_ptr != b_ptr){
            a_ptr = a_ptr == NULL ? headB : a_ptr->next;
            b_ptr = b_ptr == NULL ? headA : b_ptr->next;

            if((a_ptr == headA || b_ptr == headB ) && a_ptr != b_ptr) return NULL; // 可以去掉
        }
    
#### 14.反转链表
描述：给你单链表的头节点 head ，请你反转链表，并返回反转后的链表。

+ 其实就是要存储遍历节点的next节点，否则会被忘掉。


        ListNode *reverseList(ListNode *head)
        {
            ListNode *ret = nullptr;
            ListNode *temp = head;

            while (temp)
            {
                ListNode * next = temp->next; // 存储next
                temp->next = ret;
                ret = temp;

                temp = next;
                
            }
            return ret;
        }
#### 15.回文链表
描述：给你一个单链表的头节点 head ，请你判断该链表是否为回文链表。如果是，返回 true ；否则，返回 false 

+ 竟然可以用数组存起来。

        vector<int> temp;
        while(head){
            temp.emplace_back(head->val);
            head = head ->next;
        }
        int i = 0;
        int j = temp.size() - 1;
        
        for(int k = 0; k< temp.size() / 2; ++k){
            if(temp[i] != temp[j]) return false;
            ++i;
            --j;

        }

#### 16.环形链表
描述：给你一个链表的头节点 head ，判断链表中是否有环。如果链表中有某个节点，可以通过连续跟踪 next 指针再次到达，则链表中存在环。 

+ 第一种方法，使用unordered_map和 count方法

        unordered_set<ListNode*> seen;
        while(head) {
            if(seen.count(head)) {
                return true;

            }
            seen.insert(head);
            head = head ->next;

        }
        return false;

+ 快慢指针，如果有环的话，可以证明，一定可以相遇。

        ListNode * fast = head->next;
        ListNode * slow = head;

        while(true){
            if(fast == NULL || fast->next == NULL) {
                return false;
            }
            if(fast == slow){

                return true;
            }
        slow = slow->next;
        fast = fast->next->next;
        }
+ 值的注意的是，一直卡在提交报错上

        fast == NULL || fast->next == NULL
+ 后来发现是这两个判断的顺序导致的，一定要注意！！！！


#### 17.合并两个有序链表
描述： 将两个升序链表合并为一个新的 升序 链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。 

+ 思路没问题，但实现上，第一个节点的实现一定要理解：


        ListNode *prehead = new ListNode(-1); // 好重要节点
        ListNode *iter = prehead;

        while (list1 != nullptr && list2 != nullptr)
        {
            if (list1->val <= list2->val)
            {

                iter->next = list1;
                list1 = list1->next;
            }
            else
            {
                iter->next = list2;
                list2 = list2->next;
            }
            iter = iter->next;
        }
        iter->next = list1 == nullptr ? list2 : list1;
        iter->next = list2 == nullptr ? list1 : list2;
        return prehead->next;

#### 18.两数相加
描述： 给你两个 非空 的链表，表示两个非负的整数。它们每位数字都是按照 逆序 的方式存储的，并且每个节点只能存储 一位 数字。

请你将两个数相加，并以相同形式返回一个表示和的链表。

+ 直接过了，没看题解nb


        ListNode *addTwoNumbers(ListNode *l1, ListNode *l2)
        {
            
            int temp = 0;
            ListNode * ret = new ListNode(-1);
            ListNode * iter = ret;
            while(l2 != nullptr || l1 != nullptr)
            {
                int a = l1 == nullptr ? 0 : l1 ->val;
                int b = l2 == nullptr ? 0 : l2 ->val;

                int sum = a + b +temp;
                temp = sum / 10;

                ListNode * node = new ListNode( sum % 10, nullptr);
                iter->next = node;
                iter = node;

                l1 = l1 != nullptr ?  l1->next : l1;
                l2 = l2 != nullptr ?  l2->next : l2;
            }
            if (temp != 0) {
                ListNode * node = new ListNode( temp, nullptr);
                iter->next = node;
            }
            return ret->next;
#### 19.删除链表的倒数第 N 个结点
描述：给你一个链表，删除链表的倒数第 n 个结点，并且返回链表的头结点。 最开始理解成了删除后面的所有节点，但其实也差不多的，

+ 也是双指针直接过了：

        ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode * l1 = head, * l2 = head;

        
        for(int i = 0 ;i<= n; ++i) {
            if(l2 == nullptr) return head->next;
            l2 = l2->next;
        }   

        while(l2 != nullptr){
            l1 = l1->next;
            l2 = l2->next;
        }
        l1->next = l1->next->next;
        return head;
    }

#### 20. 两两交换链表中的节点
描述：迭代挺简单的

        if(head == nullptr || head->next == nullptr) return head;
        auto l1 = head;
        auto l2 = head->next;

        l1->next = l2->next;
        l2->next = l1;

        head = l2;

        while(l1->next != nullptr){
            auto temp =l1;

            l2 = l1->next->next;
            l1 = l1->next;

            if(l2 != nullptr){
                l1->next = l2->next;
                temp->next = l2;
                l2->next = l1;
            } else {
                break;
            }
        }
        
#### 21.随机链表的复制

描述： 给你一个长度为 n 的链表，每个节点包含一个额外增加的随机指针 random ，该指针可以指向链表中的任何节点或空节点。构造这个链表的 深拷贝


+ 用递归好自然的解决了问题，但是值得注意的是，递归很可能会陷入到死循环，辅助的那么map也是十分重要。

        unordered_map<Node*, Node*> mp;
        Node* copyRandomList(Node* head) {
        
        if(head == nullptr) return nullptr;

        if(mp.count(head) == 0) {

            auto new_head = new Node(head->val);

            mp[head] = new_head; //放在这里不会陷入死循环。

            new_head->next =  copyRandomList(head->next);
            new_head->random = copyRandomList(head->random);

            // mp[head] = new_head; 放在这里会陷入死循环。
            
            
        }
        return mp[head];
    }

#### 22.排序链表
描述: 给你链表的头结点 head ，请将其按 升序 排列并返回 排序后的链表 。

+ 第一种解法，正常的归并，时间复杂度 n（每一层要排序n次） * （log n） 层， 就像一个二叉树一样。空间复杂度logn，说是就是递归调用的深度

        ListNode *sortList(ListNode *head)
        {
            if(head == nullptr) return nullptr;
            return sortList(head, nullptr);
        }
        ListNode *sortList(ListNode *head, ListNode *tail)
        {
            if (head->next == tail)
            {
                head->next = nullptr; // 这里很关键， 将问题转化的很干净
                return head;
            }
            auto slow = head, fast = head;
            while (fast != tail)
            {
                slow = slow->next;
                fast = fast->next;

                if (fast != tail)
                {
                    fast = fast->next;
                }
            }
            return merge(sortList(head, slow), sortList(slow, tail));
        }

        ListNode *merge(ListNode *head1, ListNode *head2) // 这里就是升序链表的合并题
        {
            ListNode * pre = new ListNode(-1);
            auto iter = pre;
            auto l1 = head1, l2 = head2;
            while(l1 != nullptr && l2 != nullptr) {
                if(l1->val <= l2->val){
                    iter->next = l1;
                    l1 = l1->next;
                }
                else {
                    iter->next = l2;
                    l2 = l2->next;
                }
                iter = iter->next;
            }
            iter->next = l1 == nullptr ? l2  : l1;
            return pre->next;
        }    

+ 第二种解法：
好复杂的写法，但也确实很干净， 时间复杂度 nlogn , 空间不使用 递归，空间复杂度可以这么想，本来使用递归的时候，是二叉树高度logn， 每一层排序是n， 现在的话，最外层sub_len也是logn，然后我们把
里面的那个整个while和里面的merge看成一个整体，就会知道，里面，如果忽略掉那几个for循环的话，其实也是给这n个数排序一遍，也是n的复杂度。所以加在一起就是 n * log n.代码可能需要背一下。

        ListNode *sortList(ListNode *head)
        {
            int len = 0;
            auto temp_head = head;
            while (temp_head != nullptr)
            {
                len++;
                temp_head = temp_head->next;
            }
            首先找到长度

            ListNode *dummy_head = new ListNode(0, head); // 常规写法

            for (int sub_len = 1; sub_len < len; sub_len *= 2) 
            {
                auto temp = dummy_head->next;
                auto pre = dummy_head;
                while (temp != nullptr)
                {
                    auto head1 = temp;
                    for (int i = 1; i < sub_len && temp->next != nullptr; ++i)// 这里统一删掉temp->next = null的情况， 在循环外统一添加，确实很方便！！！
                    {
                        temp = temp->next;
                    }
                    auto tail = temp->next;  // 统一添加nullptr
                    temp->next = nullptr;
                    temp = tail;

                    auto head2 = temp; 
                    for (int i = 1; i < sub_len && temp != nullptr && temp->next != nullptr; ++i) // 这里统一删掉temp->next = null的情况，而且还要考虑当前不是nullptr
                    {
                        temp = temp->next;
                    }
                    ListNode* tail2 = nullptr; // 这里由于temp 可能是nullptr，所以写法和tail1不一样
                    if(temp != nullptr){
                        tail2 = temp->next;
                        temp->next = nullptr;
                        temp = tail2;
                    }
                    pre->next = merge(head1, head2);
                    while(pre->next != nullptr){
                        pre = pre->next;
                    }
                    temp = tail2; // tail2放在if里面定义就会导致这里看不到。
                }
            }
            return dummy_head->next;
        }

#### 23.LRU缓存 
描述请你设计并实现一个满足  LRU (最近最少使用) 缓存 约束的数据结构。

+ 这个题，到不是有多难，而是将问题拆分的很清晰，双向链表，map，链表中的存储内容，remove_node, add_to_head, move_to_head, remove_tail， 伪head和tail节点等等， 很有用的一题。
+ 

        struct DLinkedNode {
            int key, value;

            DLinkedNode * prev, *next;

            DLinkedNode(): key(0), value(0), prev(nullptr), next(nullptr){} // 创建一个双向链表节点

            DLinkedNode(int _key, int _value) : key(_key), value(_value), prev(nullptr), next(nullptr){}
        };

        class LRUCache
        
        {

        private:
            unordered_map<int, DLinkedNode*> cache;     // 用于统计是否有这个节点， 得到指针

            DLinkedNode* head;                          // head和tail都是伪节点

            DLinkedNode* tail;

            int size;

            int capacity;

        public:
            LRUCache(int _capacity) {

                capacity = _capacity;
                size = 0;
                head = new DLinkedNode();
                tail = new DLinkedNode();
                head->next = tail;          // 初始化
                tail->next = head;
            }
            
            int get(int key) {
                if(cache.count(key) == 0){
                    return -1;
                }
                DLinkedNode* node = cache[key];
                move_to_head(node);
                return node->value;
            }
            
            void put(int key, int value) {
                if(cache.count(key) == 0){
                    
                    auto node = new DLinkedNode(key, value);
                    add_to_head(node);
                    cache[key] = node;
                    size ++;
                    
                    if(size > capacity){
                        auto re_node = remove_tail();       ; 容量超了，移除最后一个
                        size --;
                        cache.erase(re_node->key);          ; map 删除
                        delete re_node;                     ; 指针 删除 new 和delete 是c++
                    }
                }
                else {
                    move_to_head(cache[key]);
                    cache[key]->value = value;
                }
            }

            void remove_node(DLinkedNode* node){
                node->prev->next = node->next;
                node->next->prev = node->prev;

            }

            void add_to_head(DLinkedNode* node){
                node->next = head->next;
                node->next->prev =  node;
                head ->next = node;
                node->prev = head;



            }
            void move_to_head(DLinkedNode* node)  {
                remove_node(node);
                add_to_head(node);

            }
            DLinkedNode* remove_tail(){
                auto node = tail->prev;
                remove_node(node);
                return node;
            }

        };

#### 24.中序遍历

描述： 

+ 方法1 递归，时间复杂度 On， 空间复杂度来自于栈的深度，最深的时候是n

        private:
            vector<int> ret;

        public:
            vector<int> inorderTraversal(TreeNode *root)
            {
                if(root == nullptr) return ret;

                if(root->left!= nullptr) inorderTraversal(root->left);
                ret.push_back(root->val);
                if(root->right!= nullptr) inorderTraversal(root->right);
            
                return ret;
            }
+ 方法2 迭代 ,好巧妙的写法。先找到最左侧最left的点然后，不断的配合一个栈的入栈和出栈，没有一句废话

        vector<int> inorderTraversal(TreeNode *root)
        {
            vector<int> ret;
            stack<TreeNode*> stk;

            while(root != nullptr || !stk.empty()){

                while(root!=nullptr){
                    stk.push(root); // push 
                    root = root->left;
                }
                root = stk.top();
                stk.pop(); // pop
                ret.push_back(root->val);

                root = root->right; //

            }
            return ret;
        }

#### 25.二叉树的最大深度
描述： 如题， 使用递归直接做了

+ 递归

        int maxDepth(TreeNode* root) {
            if(root == nullptr) return 0;

            int l = maxDepth(root->left);
            int r = maxDepth(root->right);

            return max(l, r) + 1;
        }

#### 26.翻转二叉树

描述： 给你一棵二叉树的根节点 root ，翻转这棵二叉树，并返回其根节点。

+ 直接递归

        TreeNode* invertTree(TreeNode* root) {
        
            if(root == nullptr)return nullptr;

            auto l = root->left;
            auto r = root->right;

            root->left = invertTree(r);
            root->right = invertTree(l);

            return root;
            }

#### 27. 对称二叉树
描述： 给你一个二叉树的根节点 root ， 检查它是否轴对称。

+ 递归，不同于一般的递归题；并不一定迭代就一定是一个参数，这题就是用两个参数求解，

        bool isSymmetric(TreeNode* root) {
            return check(root->left, root->right);
        }

        bool check(TreeNode* l, TreeNode* r) {
            if(l == nullptr && r== nullptr) return true;

            if(l == nullptr || r == nullptr) return false;

            if(l->val == r->val && check(l->left, r->right) && check(l->right, r->left)) return true;// 好关键的判断
            
            return false;
        }

+ 迭代 ，还是用栈来实现

        bool isSymmetric(TreeNode* root) {
            return check(root->left, root->right);
        }

        bool check(TreeNode* l, TreeNode* r) {
            stack<TreeNode*> stk;  // 用栈来模拟

            stk.push(l);
            stk.push(r);

            while(!stk.empty()) {
                auto node1 = stk.top();
                stk.pop();
                auto node2 = stk.top();
                stk.pop();

                if(node1 != nullptr && node2 == nullptr) return false;
                if(node2 != nullptr && node1 == nullptr) return false;
                if(node1 ==nullptr &&  node2 == nullptr) continue;            
                if( node1->val != node2->val) return false;

                stk.push(node1->left);
                stk.push(node2->right);

                stk.push(node1->right);
                stk.push(node2->left);
            }
            return true;
        }

#### 28. 二叉树的直径
描述： 给你一棵二叉树的根节点，返回该树的 直径 。

二叉树的 直径 是指树中任意两个节点之间最长路径的 长度 。这条路径可能经过也可能不经过根节点 root 。

+ 其实就是左右子树的最大和， 递归秒了

        int d_len = 0;

        int diameterOfBinaryTree(TreeNode* root) {
            max_sum(root);
            return d_len -2;
        }


        int max_sum(TreeNode* root) { //好关键的 int


            if(root == nullptr) return 0;

            int l = max_sum(root->left)+ 1;

            int r = max_sum(root->right) + 1;

            if(l + r > d_len) d_len = l + r;

            return max(l ,r);

        }

#### 29. 层序遍历

+ 使用队列，秒，题解上称这个为层序遍历

        vector<vector<int>> levelOrder(TreeNode *root)
        {
            
            vector<int> temp_ret;
            vector<vector<int>> ret;
            if (root == nullptr)
                return ret;

            queue<TreeNode *> que;

            que.push(root);

            while (!que.empty())
            {
                temp_ret.clear(); // 这里记一下size会方便遍历
                vector<TreeNode*> temp_node;
                
                while (!que.empty())
                {
                    auto front = que.front();
                    que.pop();
                    temp_node.push_back(front);
                }

                for(auto &item : temp_node) {
                    temp_ret.push_back(item->val);

                    if(item->left != nullptr) que.push(item->left);

                    if(item->right != nullptr) que.push(item->right);
                }
                ret.push_back(temp_ret);
            }
            return ret;
        }
#### 30. 将有序数组转换为二叉搜索树
描述：给你一个整数数组 nums ，其中元素已经按 升序 排列，请你将其转换为一棵 平衡二叉搜索树。

+ 类似于二分发的感觉，用递归实现

        TreeNode *sortedArrayToBST(vector<int> &nums)
            {   

                return help(0, nums.size()-1, nums);

            }

            TreeNode*  help(int l, int r, vector<int> &nums){

                if(r == l ) return new TreeNode(nums[l]); // 题解这里用的是 l > l 做判定， 会更合适

                int mid = (l + r) / 2;

                // left = l, mid-1
                // right = mid+ 1, r

                auto root = new TreeNode(nums[mid]);

                root->left = mid -1 >= l ? help(l, mid-1, nums) : nullptr;
                root->right = mid + 1 <= r ? help(mid+1, r, nums) : nullptr;

                return root;

            }
        };        

#### 31. 验证二叉搜索树

+ 递归， 要维护一个区间 （l，r）传给每一个递归函数，当下次更新左右子树的时候，分别更新 l 和 r的值为 root->val, 这样可以保证左子树的区间在不断向左变小，右子树区间向右变小， 好关键的递归思想，基于区间划分，这也是搜索二叉树的数学核心。


        bool isValidBST(TreeNode *root)
        {
            return helper(LONG_MIN, LONG_MAX, root);
        }

        bool helper(long long l ,long long r, TreeNode* root){

            if(root == nullptr)return true;

            if(root->val <= l || root->val >= r) return false;

            return helper(l, root->val, root->left) && helper(root->val, r, root->right); //核心

        }
+ 用中序遍历做：这个循环代替递归的模板好关键阿。

        bool isValidBST(TreeNode *root)
        {
            
            stack<TreeNode*> stk;
            long long  pre = LONG_MIN; // 这里也是 ， longlong可以过 
            while(!stk.empty() || root!= nullptr){

            
                while(root != nullptr){
                    stk.push(root);
                    root = root->left;
                }

                root = stk.top();
                stk.pop();
                if(pre >= root->val) return false;
                pre = root->val;

                root = root->right;

            }
            return true;
            
        }

#### 32. 二叉搜索树中第K小的元素
描述： 定一个二叉搜索树的根节点 root ，和一个整数 k ，请你设计一个算法查找其中第 k 个最小元素（从 1 开始计数）。

+ 题解，中序遍历找到第k个，模板题


#### 33. 二叉树的右视图
描述： 给定一个二叉树的 根节点 root，想象自己站在它的右侧，按照从顶部到底部的顺序，返回从右侧所能看到的节点值。

+ 层序遍历返回每层最后一个节点

        vector<int> rightSideView(TreeNode* root) {
            vector<int> ret;

            if(root == nullptr) return ret;
            queue<TreeNode*> que;

            que.push(root);

            while(!que.empty()) {
                int temp_size = que.size(); //这里很关键，用于对每一层的循环进行计数
                
                while( temp_size != 0) {

                    auto temp = que.front();
                    if(temp_size == 1) ret.push_back(temp->val);

                    que.pop();

                    if(temp->left!=nullptr) que.push(temp->left);
                    if(temp->right!=nullptr) que.push(temp->right);

                    temp_size --;
                }
            }
            return ret;
        }
+ ***dfs*** 还有深度优先的解法， 就是递归， 并传递一个vector的引用，好厉害的写法


        class Solution {
            void dfs(TreeNode* root, int depth, vector<int>& ans) {
                if (!root) return;
                if (depth == ans.size()) ans.push_back(root->val);
                dfs(root->right, ++depth, ans);
                dfs(root->left, depth, ans);
            }
            public:
                vector<int> rightSideView(TreeNode* root) {
                    vector<int> ans;
                    dfs(root, 0, ans);
                    return ans;
                }
            };
    
#### 34. 二叉树展开为链表
描述： 给你二叉树的根结点 root ，请你将它展开为一个单链表：
展开后的单链表应该同样使用 TreeNode ，其中 right 子指针指向链表中下一个结点，而左子指针始终为 null 。
展开后的单链表应该与二叉树 先序遍历 顺序相同。

+ 先序遍历把节点的顺序存其来，然后再遍历修改

        void flatten(TreeNode* root) {
            vector<TreeNode*> list;
            if(root == nullptr) return;
            preorder(root, list);
            int size= list.size();

            for(int i = 0; i< size - 1; ++i) {
                list[i]->left = nullptr;
                list[i]->right = list[i+1];
            }
            list[size-1]->left = list[size-1]->right = nullptr;
        }

        void preorder(TreeNode* root, vector<TreeNode*> &list){
            if(root == nullptr) return;
            list.push_back(root);
            preorder(root->left, list);     // 先序遍历
            preorder(root->right, list);
        }


#### 35. 从前序与中序遍历序列构造二叉树

描述： 给定两个整数数组 preorder 和 inorder ，其中 preorder 是二叉树的先序遍历， inorder 是同一棵树的中序遍历，请构造二叉树并返回其根节点。

+ 递归做，首先是可以使用map快速查找节点，其次是要注意中间计算左子树的数量的时候，用temp是不对的，没有理由认为temp可以更新prel和prel

        TreeNode *buildTree(vector<int> &preorder, vector<int> &inorder)
        {
            int n = preorder.size();

            for (int i = 0; i < n; ++i)
            {

                index[inorder[i]] = i;              // 需要知道，某一个节点在中序中的位置，进而将中序拆成左右两部分
            }   

            return help(preorder, inorder, 0, n - 1, 0, n - 1); // 这个写法记住应该就行
        }
        TreeNode *help(vector<int> &pre, vector<int> &in, int prel, int prer, int inl, int inr)
        {
            if(prel > prer) return nullptr;

            auto root = new TreeNode(pre[prel]);

            int temp = index[root->val];

            int numl = temp - inl; // Important!!!
            

            root->left = help(pre, in, prel + 1, prel + numl, inl, temp - 1);

            root->right = help(pre, in, prel+ numl + 1, prer,temp + 1, inr);

            return root;

        } 


#### 36. 路径总和
描述： 给定一个二叉树的根节点 root ，和一个整数 targetSum ，求该二叉树里节点值之和等于 targetSum 的 路径 的数目。

路径 不需要从根节点开始，也不需要在叶子节点结束，但是路径方向必须是向下的（只能从父节点到子节点）

+ 这里用到的不只是创建一个递归函数，而是本身也进行递归，也就是两种递归：好nice的题！！！！！

        int pathSum(TreeNode* root, int targetSum) {

            if(root == nullptr) return 0;
            int ret = 0;

            ret += help(root, targetSum); // 使用help进行递归， 如果只做到这里就是计算从根节点开始，有多少路径，使用双递归就是任何节点开始了

            ret += pathSum(root->left, targetSum);  // 对左右子树使用 pathSum 再进行递归
            ret += pathSum(root->right, targetSum);
            return ret;
        }

        int help(TreeNode* root, long target){

            if(root == nullptr)return 0;

            int ret = 0;

            long temp = target - root->val;
            if(temp == 0) ret += 1;

            ret += help(root->left, temp);
            ret += help(root->right, temp);

            return ret;
        }

#### 37. 二叉树的最近公共祖先
描述： 给定一个二叉树, 找到该树中两个指定节点的最近公共祖先。

+ 我的做法还是和上题一样采用的双迭代。但是题解似乎是单迭代

        TreeNode * node =nullptr;
        TreeNode *lowestCommonAncestor(TreeNode *root, TreeNode *p, TreeNode *q)
        {

            if(root == nullptr) return nullptr;
            int ret = 0;
            
            ret += help(root, p, q);

            if(ret == 2 ) {
                node = root;
            }
            lowestCommonAncestor(root->left, p, q);
            lowestCommonAncestor(root->right, p, q);
            return node;
        }

        int help(TreeNode *root, TreeNode *p, TreeNode *q)
        {
            if(root==nullptr)return 0;
            int ret = 0;

            if (root == p || root == q)
            {
                ret++;
            }

            ret += help(root->left, p, q);
            ret += help(root->right, p, q);

            return ret;
        }

#### 38.岛屿数量
描述： 给你一个由 '1'（陆地）和 '0'（水）组成的的二维网格，请你计算网格中岛屿的数量。

+ 遍历，每次遍历上下左右，dfs 递归时会把连起来的上下左右都变成0, 再看最外层计算了几次dfs。


        for(int i = 0; i < m; ++i){
            for(int j =0; j < n; ++j){

                if(grid[i][j] == '1') {
                    ret ++;                 // 看看计算了几次， 和橘子还不一样
                    dfs(i, j, grid);
                }
            }
        }
        return ret;
    
        void dfs(int row, int column, vector<vector<char>> &grid){
            grid[row][column] = 0;
            if(row >0 && grid[row-1][column] == '1') dfs(row-1, column, grid);
            if(row < _row - 1 && grid[row+1][column] == '1') dfs(row+1, column, grid);
            if(column>0 && grid[row][column-1] == '1') dfs(row, column-1, grid);
            if(column < _col -1 && grid[row][column+1] == '1') dfs(row, column +1, grid);
        }
#### 39. 腐烂的橘子
描述：给定的 m x n 网格 grid 中，每个单元格可以有以下三个值之一：

值 0 代表空单元格；
值 1 代表新鲜橘子；
值 2 代表腐烂的橘子。
每分钟，腐烂的橘子 周围 4 个方向上相邻 的新鲜橘子都会腐烂。

返回 直到单元格中没有新鲜橘子为止所必须经过的最小分钟数。如果不可能，返回 -1 


+ 方法， 最开始想的bfs没有用栈，导致第一次循环的时候就会不停地将第一次腐烂的橘子 继续进行腐烂传递，导致输出 总是1
解决方法是使用栈， 每次先遍历一下， 将已经腐烂的橘子存在栈里， 再对栈中的橘子进行腐烂传递， 这样的话， 腐烂就是按照题目的要求来的了
并且学到的技巧是， pair 和 make_pair函数 很好用。



        while (delta != 0)
        {
            delta = 0;

            for (int i = 0; i < _m; ++i)
            {
                for (int j = 0; j < _n; ++j)
                {
                    if (grid[i][j] == 2)
                    {
                        oran.push(make_pair(i, j)); //先把腐烂的橘子存起来，再在最外层进行更新
                    }
                }
            }
            delta += bfs(grid, oran);
虽然当时第一次做题时很难受，但的确学到了新内容。


#### 40. 地平线面试题
判读从root出发到 叶节点的路径和是否等于给定值，返回所有满足条件的路径。

+ 递归做，学到的新的知识点是，vector也有pop_back()函数

        vector<vector<int>> pathSum(TreeNode *root, int targetSum)
        {
            target = targetSum;

            vector<int> path;
            help(root, 0, path);
            return ret;
        }

        void help(TreeNode *root, int sum, vector<int> &path)
        {
            if (root == nullptr)
                return;

            sum += root->val;
            path.push_back(root->val);

            if (root->left == nullptr && root->right == nullptr)
            {
                if (sum == target)
                {
                    ret.push_back(path);
                }
                return;
            }                              // review 这里应该改成 push， help， pop 这种规范的格式!!!!!!!!

            help(root->left, sum, path); // 这里确实只需要一个pop_back就可以

            help(root->right, sum, path);

            path.pop_back();
        }
#### 41课程表

+ 你这个学期必须选修 numCourses 门课程，记为 0 到 numCourses - 1 。

在选修某些课程之前需要一些先修课程。 先修课程按数组 prerequisites 给出，其中 prerequisites[i] = [ai, bi] ，表示如果要学习课程 ai 则 必须 先学习课程  bi 。

例如，先修课程对 [0, 1] 表示：想要学习课程 0 ，你需要先完成课程 1 。

+ 说白了就是判断内部有没有环，我使用的办法就是递归，最后如果递归的层数太多则失败，并配合减枝
class Solution {
public:
    vector<bool> flag;

    bool canFinish(int numCourses, vector<vector<int>>& prerequisites) {

        flag.resize(numCourses, false);
        vector<int> temp(numCourses,-1);

        vector<vector<int>> in(numCourses ,temp);
        for(const auto&item : prerequisites){
            in[item[0]][item[1]] = 0;
        }
        
        for(int i= 0; i< numCourses; ++i){
            if(flag[i] == true) continue;
            if(help(i, in, 0)==false) return false;
            flag[i] = true;
        }
        return true;
    }

    bool help(int i, const vector<vector<int>>&in, int depth){
        if(flag[i] == true) return true;
        if(depth > in.size()) return false;
        for(int j=0; j< in[i].size(); ++j){
            if( in[i][j]!= -1){
                if(help(j, in, depth+1)== false) 
                    return false;
                else
                    flag[j] = true;
            }
        }
        return true;
    }
};

+ 题解的是dfs和bfs，dfs,他的dfs使用的跳出条件就是遇到了正在访问的节点，不用向我的一样还要等待超时，
+ 他的技巧是，用一个vector<int> 的 0 1 2 分别来表示，没被访问，正在访问，访问完毕，这个还是很好用的
+ 并且他设置了一个全局变量，如果检测到全局变量就直接GG， 不用一层一层return回去

class Solution {
private:
    vector<vector<int>> edges;
    vector<int> visited;
    bool valid = true;

public:
    void dfs(int u) {
        visited[u] = 1;
        for (int v: edges[u]) {
            if (visited[v] == 0) {
                dfs(v);
                if (!valid) {
                    return;
                }
            }
            else if (visited[v] == 1) {
                valid = false;
                return;
            }
        }
        visited[u] = 2;
    }

    bool canFinish(int numCourses, vector<vector<int>>& prerequisites) {
        edges.resize(numCourses);
        visited.resize(numCourses);
        for (const auto& info: prerequisites) {
            edges[info[1]].push_back(info[0]);
        }
        for (int i = 0; i < numCourses && valid; ++i) {
            if (!visited[i]) {
                dfs(i);
            }
        }
        return valid;
    }
};

+ 第二种方法是 bfs 使用的方法是
  + 最开始入度为0的节点入栈，并创建了一个vector<int> 的in_edge 和edge 用来进行遍历和入度的表示
  + 并不断 的把 入度 为0 的加节点 去掉，最后看是不是去掉了足够多的节点
  + 这个思想还是很关键
  

#### 42课程表2
现在你总共有 numCourses 门课需要选，记为 0 到 numCourses - 1。给你一个数组 prerequisites ，其中 prerequisites[i] = [ai, bi] ，表示在选修课程 ai 前 必须 先选修 bi 。

例如，想要学习课程 0 ，你需要先完成课程 1 ，我们用一个匹配来表示：[0,1] 。
返回你为了学完所有课程所安排的学习顺序。可能会有多个正确的顺序，你只要返回 任意一种 就可以了。如果不可能完成所有课程，返回 一个空数组 。

+ 用bfs试一下，  bfs还是很简单的

class Solution {
public:
    vector<int> findOrder(int numCourses, vector<vector<int>>& prerequisites) {

        vector<vector<int>> edge(numCourses); // 复杂度体现在这里 O(m+n)
        vector<int> in(numCourses, 0);
        stack<int> st;

        for(const auto& item: prerequisites){
            edge[item[0]].push_back(item[1]);
            in[item[1]]++;
        }
    
        for(int i = 0; i< in.size(); ++i){  // in 表示入度， 也就是有多少个被指向
            if(in[i] == 0){
                st.push(i);
            }
        }

        int num = 0;
        vector<int> ret;
        while(st.empty() == false){
            auto first = st.top();
            st.pop();
            num ++;
            ret.push_back(first);
            for(const auto & item : edge[first]){       // 每次只有入度为0才能放进去，放进去后，它指向的节点减1
                in[item]--;
                if(in[item] == 0)
                    st.push(item);
            }
        }
        //cout << num <<endl;
          if(num == numCourses)
        {
            reverse(ret.begin(), ret.end());
            return ret;
        }
        else return {};
    }

    
#### 43 全排列

给定一个不含重复数字的数组 nums ，返回其 所有可能的全排列 。你可以 按任意顺序 返回答案。

+  感觉自己的办法有点蠢阿，pop_back一直不知道放在哪里合适
class Solution {
public:

    set<vector<int>> ret_set;
    vector<vector<int>> ret;
    int _num;

    void help(int index, vector<int>&temp, vector<int>& nums){
        temp.push_back(nums[index]);

        if(temp.size() == _num){
            ret_set.insert(temp);
            return;
                                            
        }
        
        for(int i = 0; i< _num; ++i){
            if(find(temp.begin(), temp.end(),nums[i]) == temp.end()){
                help(i, temp, nums);
                temp.pop_back();                // 应该放在这里吗？？
            }

        }   
        
    }
    vector<vector<int>> permute(vector<int>& nums) {
        _num = nums.size();
        
        for(int i = 0; i< _num; ++i)
        {
            vector<int> temp;
            help(i, temp, nums);
        }
        

        for(const auto& item: ret_set){
            ret.push_back(item);
        }
        return ret;
    }
};


+ 上面的写法好乱，整理之后，收获大大， 在全排列也就是不适用index的基础上， 增加一次查找

class Solution {
public:

    set<vector<int>> ret_set;
    vector<vector<int>> ret;
    int _num;

    void help(vector<int>&temp, vector<int>& nums){
        
        if(temp.size() == _num){
            ret_set.insert(temp);                       // 这里甚至都不用set， 直接push_back就行，因为是按顺序遍历的
            return;
        }
        
        for(int i = 0; i< _num; ++i){
            if(find(temp.begin(), temp.end(),nums[i]) == temp.end()){

                temp.push_back(nums[i]);                // 这三行是回溯法的核心 添加+递归+退出 n* n!
                help(temp, nums);
                temp.pop_back();
            }
        }   
        
    }
    vector<vector<int>> permute(vector<int>& nums) {
        _num = nums.size();
        
            
        vector<int> temp;
        help(temp, nums);

        for(const auto& item: ret_set){                 //所以这些也是多余的了
            ret.push_back(item);
        }
        return ret;
    }
};


#### 44子集
+ 给你一个整数数组 nums ，数组中的元素 互不相同 。返回该数组所有可能的
子集
（幂集）。

解集 不能 包含重复的子集。你可以按 任意顺序 返回解集。

+ 添加了一个限定条件， index， 让每次搜索的顺序从index之后搜索， 并且把ret的pushback 放到了循环里面

class Solution {
public:

    vector<vector<int>> ret;


    void help(int index, vector<int> &temp, vector<int> & nums){

        
        for(int i = index;i < nums.size(); ++i){
            if(find(temp.begin(), temp.end(), nums[i]) != temp.end()){
                temp.push_back(nums[i]);
                ret.push_back(temp);
                help(i+1, temp, nums);
                temp.pop_back();
            }
            
        }

    }


    vector<vector<int>> subsets(vector<int>& nums) {
        vector<int> temp;
        help(0, temp, nums);
        ret.push_back({});
        return ret;
    }
};

+ 看一下题解，可以用更简单的方法，就是普通的dfs，每一个节点有放与不放两个状态，用一个index进行遍历就ok

class Solution
{
public:
    vector<vector<int>> ret;

void help(int index, vector<int> &temp, vector<int> &nums)
    {
        if (index == nums.size()){
            ret.push_back(temp);
            return;
        }

        temp.push_back(nums[index]);    // 放
        help(index + 1, temp, nums);

        temp.pop_back();                // 不放
        help(index + 1, temp, nums);    
    }

    vector<vector<int>> subsets(vector<int> &nums)
    {
        vector<int> temp;
        help(0, temp, nums);
        return ret;
    }
};

#### 45电话号码的字母组合
描述：给定一个仅包含数字 2-9 的字符串，返回所有它能表示的字母组合。答案可以按 任意顺序 返回。

给出数字到字母的映射如下（与电话按键相同）。注意 1 不对应任何字母。

+ 我的方法就是类似于之前 称砝码的题，每次都在原有的基础上 进行修改

class Solution
{
public:
    vector<string> letterCombinations(string digits)
    {

        map<char, string> mp;
        mp['1'] = "";
        mp['2'] = "abc";
        mp['3'] = "def";
        mp['4'] = "ghi";
        mp['5'] = "jkl";
        mp['6'] = "mno";
        mp['7'] = "pqrs";
        mp['8'] = "tuv";
        mp['9'] = "wxyz";

        vector<string> ret(1, "");
        if(digits=="") return {};
        for (const auto &ch : digits)
        {
            vector<string> temp;

            for (const auto &item : mp[ch]) // 遍历新加字符串
            {
                for(auto & ori: ret){   // 遍历原有字符串

                    temp.push_back(ori + string(1,item) ); // 构造新的字符串， 把字符转换为字符串的函数！
                }
                
            }

            ret = temp;
        }
        return ret;
    }

};


#### 46 组合总和

+ 给你一个 无重复元素 的整数数组 candidates 和一个目标整数 target ，找出 candidates 中可以使数字和为目标数 target 的 所有 不同组合 ，并以列表形式返回。你可以按 任意顺序 返回这些组合。

candidates 中的 同一个 数字可以 无限制重复被选取 。如果至少一个数字的被选数量不同，则两种组合是不同的。 

+ 就是dfs的拓展，比较常见的思路就是，如果是组合，就加一个index 防止回头即可，不加index 就是纯排列

class Solution {
public:
    vector<vector<int>> ret;

    void help(vector<int>& candidates, int target, vector<int> &temp,int index){

        if(target == 0){
            ret.push_back(temp);
            return;
        }else if(target < 0){
            return;
        }

        for(int i = index; i< candidates.size(); ++i){
            temp.push_back(candidates[i]);
            help(candidates, target - candidates[i], temp, i);
            temp.pop_back();
        }
    }
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {   // 这个target也挺关键的

        vector<int> temp;                   // 辅助数组
        help(candidates, target, temp, 0);
        return ret;
    }
};

#### 47阿里云1
+ 输入一个字符串，字符串中的
+ a li yun 分别代表一个权重， 求 给定字符串的总权重，
+ aliyun 的总权重是： (a al li liu liyu iyun yun) * 1 + (ali aliy aliyu liyun) * 2 + (aliyun) * 3 = 7 + 8 + 3 = 18
+ aba = (a ab ba a) * 1 + aba * 2 = 4 + 2 = 6

这里应该是对于任何一个子串，查找有几个 a li 和yun ，进而给权重， 不能用简单的find，

#### 48阿里云2

+ 输入 n 和 k
+ 是否有 从1到n的排列，满足 所有相邻两个数的绝对值中的最大值 = k

我的做法是先把所有排列求出来，再检测是否有答案，但是超时了， 那得怎么做呢




#### 49 括号生成

+ 数字 n 代表生成括号的对数，请你设计一个函数，用于能够生成所有可能的并且 有效的 括号组合。

+ 递归做的， 还行， 主要就是有个加入右括号的前提条件，类似于对遍历树的节点的时候的判定一样
class Solution {
public:
	int  _n;
	vector<string> ret;

	void help(string &s, int num_l, int num_r){
		
		if(s.size() == _n*2){
			ret.push_back(s);
			return;
		}

		if(num_r < num_l && num_r < _n){
			s = s + string(1, ')');
			help(s, num_l, num_r + 1);
			s.pop_back();
		}

		if(num_l < _n) {
			s = s + string(1, '(');
			help(s, num_l+1, num_r);
			s.pop_back();
		}


	}
    vector<string> generateParenthesis(int n) {
		_n = n;
		string s;
		help(s, 0, 0);
		return ret;
    }
};


#### 50 腾讯LRU 最近最少使用

描述： // 实现 LRUCache 类：
// 运用你所掌握的数据结构，设计和实现一个  LRU (最近最少使用) 缓存机制 。
// 实现 LRUCache 类：

// LRUCache(int capacity) //以正整数作为容量 capacity 初始化 LRU 缓存
// int get(int key) //如果关键字 key 存在于缓存中，则返回关键字的值，否则返回 -1 。
// void put(int key, int value) //如果关键字已经存在，则变更其数据值；如果关键字不存在，则插入该组「关键字-值」。当缓存容量达到上限时，它应该在写入新数据之前删除最久未使用的数据值，从而为新的数据值留出空间



struct node{
    int key;
    int val;
    node* next;
};


#### 单词搜索

给定一个 m x n 二维字符网格 board 和一个字符串单词 word 。如果 word 存在于网格中，返回 true ；否则，返回 false 。

单词必须按照字母顺序，通过相邻的单元格内的字母构成，其中“相邻”单元格是那些水平相邻或垂直相邻的单元格。同一个单元格内的字母不允许被重复使用。

+ 做法就是dfs，一个一个找，找到当前符合的，就遍历上下左右，看看是否符合下一个，这里还要标记已经遍历过的位置，二维vec初始化很有用，但是题解似乎也可以不使用那个visited数组
+ 挺经典的题


class Solution {
public:
    int _m;
    int _n;
    int _len;
    bool exist(vector<vector<char>>& board, string word) {
        _m = board.size();
        _n = board[0].size();

        _len = word.size();

        for(int i = 0; i< _m; ++i){
            for(int j = 0; j< _n; ++j){
                
                vector<vector<char>> visited(_m, vector<char>(_n, ' ')); // 为每次dfs传建了一个 是否访问过的flag

                if(help(board, visited, word, 0, i,j) == true){
                    return true;
                }


            }
        }
        return false;

    }

    bool help(vector<vector<char>>& board, vector<vector<char>>& visited, string word, int deep, int x, int y){

        if(deep == word.size()) return true;
        
        if(x < 0 || x >= _m) return false;

        if(y < 0 || y >= _n) return false;

        if(visited[x][y] != ' ') return false;

        if(board[x][y] != word[deep]) return false;

        visited[x][y] = '!';

        bool ans = false;


        ans = ans || help(board, visited, word, deep + 1, x, y-1);

        ans = ans || help(board, visited, word, deep + 1, x, y+1);

        ans = ans || help(board, visited, word, deep + 1, x+1, y);

        ans = ans || help(board, visited, word, deep + 1, x-1, y);

        visited[x][y] = ' ';
        return ans;
    }
};

#### 分割回文串

描述： 给你一个字符串 s，请你将 s 分割成一些子串，使每个子串都是回文串。返回 s 所有可能的分割方案。

+ 首先用动态规划将 _circle[i][j] 是不是回文字串判断出来，太关键了
+ 判断出来之后，就只要dfs就可以了

class Solution {
public:
    vector<vector<bool>> _circ;
    int _len;
    string _s;

    vector<vector<string>> ret;

    vector<vector<string>> partition(string s) {

        _circ = vector<vector<bool>>(s.length(), vector<bool>(s.length(), true));
        _len = s.length();
        _s = s;
        
        vector<string> temp;
        help(_circ);
        dfs(0, temp);

        return ret;
    }
    void dfs(int deep, vector<string> & temp){
        if(deep >= _len) {
            ret.push_back(temp);
            return;
        }
        for(int i = deep; i < _len; ++i){
            if(_circ[deep][i] == true){
                temp.push_back(_s.substr(deep,i-deep+1));
                dfs(i+1, temp);
                temp.pop_back();
            }
        }
    }

    void help(vector<vector<bool>> circ){
        for(int i =_len-1; i>=0; --i){ // 这里其实是从len -2 开始的，因为j的判定， len-1一定被排除在外了
            for(int j=i+1; j< _len; ++j){

                if(_s[i] == _s[j] && _circ[i+1][j-1]){
                    _circ[i][j] = true;
                }else{
                    _circ[i][j] = false;
                }
            }
        }
    }
};



#### 并查集，好朋友

+ 有 n 个城市，其中一些彼此相连，另一些没有相连。如果城市 a 与城市 b 直接相连，且城市 b 与城市 c 直接相连，那么城市 a 与城市 c 间接相连。

省份 是一组直接或间接相连的城市，组内不含其他没有相连的城市。

给你一个 n x n 的矩阵 isConnected ，其中 isConnected[i][j] = 1 表示第 i 个城市和第 j 个城市直接相连，而 isConnected[i][j] = 0 表示二者不直接相连。

返回矩阵中 省份 的数量。


+ 做法就是一个fa数组，最开始进行初始化，后来不断的查找最顶部的父节点，每次更新的时候不是更新当前的节点，而是更新最顶部的节点的父节点

class Solution {
public:

    int _n;
    vector<int> fa;

    int findfa(int a){

        while(fa[a] != a)
            a = fa[a];
        return a;
    }

    void unionfa(int a, int b){

        //int faa, fab;
        while(fa[a] != a)
            a = fa[a];
        //faa = a;

        while(fa[b] != b)
            b = fa[b];
        //fab = b;
        if(a != b)
            fa[b] = a; 
    }

    int findCircleNum(vector<vector<int>>& isConnected) {
        _n = isConnected.size();
        fa.resize(_n);
        for(int i= 0; i< _n ;++i){
            fa[i] = i;
        }

        for(int i = 0;i < _n; ++i){
            for(int j = 0;j < _n; ++j){
                if(i==j) continue;
                if(isConnected[i][j] != 0){
                    if(findfa(i) != findfa(j)){
                        unionfa(i, j);
                    }
                }
            }
        }

        int num = 0;
        for(int i =0; i< _n; ++i){
            if(fa[i] == i)            
                num ++;
        }
        return num;

    }
};

#### 并查集 连通网络的操作次数

+ 用以太网线缆将 n 台计算机连接成一个网络，计算机的编号从 0 到 n-1。线缆用 connections 表示，其中 connections[i] = [a, b] 连接了计算机 a 和 b。

网络中的任何一台计算机都可以通过网络直接或者间接访问同一个网络中其他任意一台计算机。

给你这个计算机网络的初始布线 connections，你可以拔开任意两台直连计算机之间的线缆，并用它连接一对未直连的计算机。请你计算并返回使所有计算机都连通所需的最少操作次数。如果不可能，则返回 -1 。 

+ 思想是，找到冗余线路的条数，这里是当父节点相同时就是冗余的
+ 并和最后并查集的总数量作比较，看看够不够用
class Solution {
public:

    int _was = 0;
    int _n;
    vector<int> fa;

    int findfa(int a){
        while (fa[a] != a)
            a = fa[a];
        return a;
    }
    void unifa(int a, int b){
        fa[findfa(b)] = findfa(a);
    }


    int makeConnected(int n, vector<vector<int>>& connections) {
        _n = n;
        fa.resize(_n);                  // size的是真实的大小， capasity是分配的容量
        for(int i = 0;i< _n; ++i){
            fa[i]= i;
        }

        for(const auto & item : connections){
            if(findfa(item[0]) != findfa(item[1])){
                unifa(item[0], item[1]);
            }else{
                _was ++;                    ; 浪费了几个节点
            }
        }

        int num = 0;
        for(int i=0;i<_n; ++i){
            if(fa[i] == i) num ++;          ; 指向的是自己所以就是一个集合
        }
        
        if(_was+1 >= num){
            cout << _was << " " << num;
            return num-1;
        }
        return -1;


    }
};

#### 二分查找-搜索插入的位置

+ 可以使用递归，但题解用while循环也可以

class Solution
{
    int _target;
    int _num;
    int index;

public:
    int searchInsert(vector<int> &nums, int target)
    {

        _target = target;
        _num = nums.size();


        help(0, _num - 1, nums);
        return index;
        
    }

    void help(int l, int r, vector<int> &nums)
    {

        if (l > r)
        {
            index = l;
            return;
        }

        int mid = (l + r) / 2;

        if (_target > nums[mid]) //  <= is attentioned.
        {
            help(mid + 1, r, nums);
        }
        else
        {
            help(l, mid - 1, nums);
        }
    }
};


+ 循环好简单阿， 每次只用l就行的

public:
    int searchInsert(vector<int> &nums, int target)
    {

        int l = 0;
        int r = nums.size() -1;
        while(l <= r){
            int mid = (r + l) / 2;
            if(target > nums[mid] ) // 这里的判定条件总是， target是否比mid大， 所以最后才总是l 关键
                l = mid + 1;
            else 
                r = mid - 1;
        }
        return l;
    }
class Solution {
public:
    int searchInsert(vector<int> &nums, int target)
    {
        int l =0, r = nums.size()-1;

        
        while(l <= r){
            int mid = (l + r) / 2;

            if(nums[mid] < target){
                l = mid + 1;    // l的左边一定小于target 关键！！！！！！！！！！！！！！！！！！
            } else{
                r = mid - 1;    // r的右边一定大与等于target， r最后一定等于l-1 ！！！！！！！！！！！！
            }
        }
        return l;
    }
};

#### 二分查找-在排序数组中查找元素的第一个和最后一个位置

+ 描述： 给你一个按照非递减顺序排列的整数数组 nums，和一个目标值 target。请你找出给定目标值在数组中的开始位置和结束位置。

如果数组中不存在目标值 target，返回 [-1, -1]。

你必须设计并实现时间复杂度为 O(log n) 的算法解决此问题。


+ 其实就是那个二分法的l的指向是第一个不小于target的位置，这个思想很关键， 还有一个就是特判要记得写

class Solution {
public:
    vector<int> searchRange(vector<int>& nums, int target) {
        int l = 0, r = nums.size()-1;
        if(nums.size()==0) return {-1, -1};
        int mid;
        while(l <= r){
            mid = (l + r) / 2;
            if(nums[mid] < target){
                l = mid + 1;
            }else{
                r = mid -1;
            }
        }

        if(l<=nums.size()-1 && nums[l] == target){
            int p = l;

            while(p <= nums.size()-1 && nums[p] == target){
                ++p;
            }
            return {l, p-1};

        }
        return {-1, -1};
        
    }
};

####  二分法-搜索旋转排序数组


整数数组 nums 按升序排列，数组中的值 互不相同 。

在传递给函数之前，nums 在预先未知的某个下标 k（0 <= k < nums.length）上进行了 旋转，使数组变为 [nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]]（下标 从 0 开始 计数）。例如， [0,1,2,4,5,6,7] 在下标 3 处经旋转后可能变为 [4,5,6,7,0,1,2] 。

给你 旋转后 的数组 nums 和一个整数 target ，如果 nums 中存在这个目标值 target ，则返回它的下标，否则返回 -1 。

你必须设计一个时间复杂度为 O(log n) 的算法解决此问题


+ 思路， 这个题采用的是一个比较简单的思路， 就是还是二分法的框架，其中 经常要去判断 target 和 nums0 的大小关系， 进而可以判断出到底在左边那一片，还是右边那一片
class Solution {
public:
    int search(vector<int>& nums, int target) {

        if(nums.size() == 0) return -1;
        int l = 0, r = nums.size()-1, mid;

        int nums0 = nums[0]; // 这里是怕 nums0 被修改，所以提前保存了一下
        while(l <= r){
            mid = (l + r) / 2;

            if(target >= nums0){ // on the left
                if(nums[mid]  < nums0) nums[mid] = target+ 999;

                if(nums[mid] < target){
                    l = mid + 1;                    
                }   
                else{
                    r = mid -1;
                }
            }else{
                if(nums[mid]  >= nums0) nums[mid] = target- 999; // 这个等号要留意

                if(nums[mid] < target){
                    l = mid + 1;                    
                }   
                else{
                    r = mid -1;
                }
            }
        }
        if(l <= nums.size()-1 && nums[l] == target) return l;
        else return -1;

    }
};


#### 二分法-搜索旋转后的最小值

+ 思路其实和之前类似，还更好做了，找到不合适的可以强行赋值 9999 或者-9999，总体框架还是二分法的框架


class Solution {
public:
    int findMin(vector<int>& nums) {
        if(nums.size() <=1) return nums[0];

        if (nums[0] < nums[nums.size() -1]) return nums[0];

        int l = 0, r = nums.size()-1, mid;

        int target = -8999;
        int num0 = nums[0];

        while(l <= r){
            mid = (l + r) / 2;
            if(nums[mid] >= num0) nums[mid] = -9999;
            if(nums[mid] < target){
                l = mid + 1;
            } else{
                r = mid -1;
            }

        }

        return nums[l];
    }
};


####  有效的括号

+ 给定一个只包括 '('，')'，'{'，'}'，'['，']' 的字符串 s ，判断字符串是否有效。

+ 确实用栈最直观


class Solution {
public:
    bool isValid(string s) {
        stack<char> stk;

        for(int i = 0; i< s.size(); ++i){
            if(s[i] == '(' || s[i] == '{' || s[i] == '['){
                stk.push(s[i]);
            }else if(stk.empty() == true){
                return false;
            }
            else if (s[i] == ')') {
                if(stk.top() == '(') stk.pop();
                else return false;
            }
            else if (s[i] == ']') {
                if(stk.top() == '[') stk.pop();
                else return false;
            }
            else if (s[i] == '}') {
                if(stk.top() == '{') stk.pop();
                else return false;
            }
        }
        if(stk.empty() == false) return false;
        else return true;
    }
};

#### 最小栈


+ 设计一个支持 push ，pop ，top 操作，并能在常数时间内检索到最小元素的栈。

实现 MinStack 类:

MinStack() 初始化堆栈对象。
void push(int val) 将元素val推入堆栈。
void pop() 删除堆栈顶部的元素。
int top() 获取堆栈顶部的元素。
int getMin() 获取堆栈中的最小元素。


+ 思路就是，插入栈时，同时还要计算当前的最小值进栈，可以是但的的栈，也可以是 std::pair



class MinStack {
public:

    stack<int> st;
    stack<int> st_min;
    MinStack() {
        
    }
    
    void push(int val) {
        if(st_min.empty()){
            st.push(val);
            st_min.push(val);
        }
        else{
            st.push(val);
            st_min.push(min(st_min.top(), val));
        }
    }
    
    void pop() {
        st.pop();
        st_min.pop();
    }
    
    int top() {
        return st.top();
    }
    
    int getMin() {
        return st_min.top();
    }
};


#### 字符串解码
 s = "3[a]2[bc]"

 s = "3[a2[c]]"
+ 这个一上来的思路就是递归,但是几个条件的判断确实很让人发晕, 辅助栈的思路应该也是类似的,就是要保存括号前面的mutil 和当下的res

+ 然后就是python 的字符到int的转换不能 用减法

class Solution:
    def decodeString(self, s: str) -> str:
        
        def help(s, i):
            res = ""
            mutil = 0
            
            while i < len(s):
                if s[i] >= '0' and s[i] <= '9':
                    mutil = mutil * 10 + int(s[i])
                elif s[i] == '[':
                    temps, tempi =  help(s, i+1)
                    res += mutil * temps   # 这里很关键
                    i = tempi
                    mutil = 0
                elif s[i] == ']':
                    return res, i
                else:
                    res += s[i]
                i += 1
            return  res
    
        return help(s, 0)
    # s = "3[a]2[bc]"


#### 每日温度

+ 给定一个整数数组 temperatures ，表示每天的温度，返回一个数组 answer ，其中 answer[i] 是指对于第 i 天，下一个更高温度出现在几天后。如果气温在这之后都不会升高，请在该位置用 0 来代替。


+ 把index入栈，遍历，每次比较当前的温度是否大于 栈内的温度， 大于的 都出栈，然后出栈的时候更新结果

class Solution {
public:
    vector<int> dailyTemperatures(vector<int>& temperatures) {
        // index依次入栈，只有遇到大的，才出栈， 出栈的时候修改ans，，，真nice的思路
        stack<int> stk;
        vector<int> ans(temperatures.size(), 0);
        for(int i = 0; i< temperatures.size();++i){
            if(stk.empty()== true){
                stk.push(i);
            }else{
                if(temperatures[i] <= temperatures[stk.top()]){
                    stk.push(i);
                }else{
                    while(stk.empty() == false && temperatures[stk.top()] < temperatures[i]){
                        int number = stk.top();
                        stk.pop();
                        ans[number] = i - number;
                    }
                    stk.push(i);
                }
            }
        }
        return ans;
        
    }
};


#### 数组中的第K个最大元素


+ 补了一点快速排序的知识

    int partition(vector<int> &nums, int left, int right){ // 用来找到中间节点
        
        int i = left, j = right;
        while(i < j){
            while(i < j && nums[j] >= nums[left]) j--; // 如果j走到和i重复的位置，则 没有实际的交换
            while(i < j && nums[i] <= nums[left]) i++;  // 如果 i走到和j重复的位置上，会发生交换 left <-> i=j
            
            swap(nums[i], nums[j]);
        }
        swap(nums[i], nums[left]);
        return i;
    }
    void quicksort(vector<int> &nums, int left, int right){

        if(left >= right) return;

        int mid = partition(nums, left, right);
        quicksort(nums, left, mid -1);
        quicksort(nums, mid + 1, right);
    }

用快排会超时，题解要对快排进行修改，基本思想是，mid的位置总是不变的,可以作为入手点，每次只排又需要的地方，也就是左边或者右边

+ 堆

+ 感觉堆排序好快， 堆排序的主要分为两步，第一步是建堆，然后是不断的保存根节点和重建，
+ 建堆的复杂度是n， 总复杂度是nlogn             

        void maxHeap(vector<int>&a, int i, int heapSize){
            // 因为第一个节点是0, 所以用l、r和heapSize 比较是正确的
            int l = i * 2 + 1, r = i * 2 + 2, largest = i;
            if(l < heapSize && a[l] > a[largest]){
                // left son exist
                largest = l;
            }
            if(r < heapSize && a[r] > a[largest]){
                // left son exist
                largest = r;
            }
            if(largest != i){
                swap(a[i], a[largest]);
                maxHeap(a, largest, heapSize);
            }
        }

        void buildMaxHeap(vector<int>&a, int heapSize){
            // heapSize / 2 正好就是最右下的那个根节点
            for(int i = heapSize / 2; i >= 0; --i){
                maxHeap(a, i, heapSize); // alter heap
            }

        }

        void findK(vector<int>&a, int k){
            int heapSize = a.size();
            buildMaxHeap(a, heapSize); // 建堆
            for(int i = 1; i<k; ++i){
                
                swap(a[0], a[heapSize-1]);
                --heapSize;
                maxHeap(a, 0, heapSize);
            }

        }

#### 前 K 个高频元素

+ 先用mp把频率统计出来，再用堆做，这种题的名字一般都是，最大的k个，最小的k个这种字样，用堆做就比较合适

class Solution {
public:

    unordered_map<int, int> mp;


    void helpHeap(vector<int> &a, int i, int size){
        int l = 2*i+1, r=2*i+2, largest=i;

        if(l < size && mp[a[l]] > mp[a[largest]]){
            largest = l;
        }
        if(r < size && mp[a[r]] > mp[a[largest]]){
            largest = r;
        }
        if(largest != i){
            swap(a[i], a[largest]);
            helpHeap(a, largest, size);
        }
    }

    void buildheap(vector<int> &a, int size){
        for(int i = size/2; i>=0; --i)
            helpHeap(a, i, size);
    }
    vector<int> topKFrequent(vector<int>& nums, int k) {
        

        for(int i = 0; i< nums.size(); ++i){
            mp[nums[i]] += 1;
        }
        vector<int> temp;
        vector<int> ret;

        for(const auto & item: mp){
            temp.push_back(item.first);
        }

        int heapSize = mp.size();
        buildheap(temp, heapSize);

        for(int i= 0; i<k;++i){
            ret.push_back(temp[0]);
            temp[0] = temp[heapSize -1];
            --heapSize;
            helpHeap(temp, 0, heapSize);
        }
        
        return ret;
    }
 

};