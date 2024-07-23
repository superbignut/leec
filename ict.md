#### 1. 字母转换
描述： 写出一个程序，接受一个十六进制的数，输出该数值的十进制表示。

+ 第一个知识点是scanf在读取字符串时，会忽略前置回车和空格，
+ 第二个知识点是str_len在string.h头文件中
+ 这个题就是不断的 sum = sum*16 + str[i] 
+ 结果卡在了sum += 上了， 好sb

    #include<iostream>
    #include<math.h>
    #include<string.h>

    using namespace std;

    int main() {
        char str[100];
        int sum= 0, len = 0;

        // scanf 会忽略所有的前置 \n 和 空格
        while(scanf("%s", str) != EOF){
            sum = 0;
            len = strlen(str);
            for(int i=2; i< len; ++i){
                if(str[i] >= 'A' && str[i] <= 'F'){
                    sum = sum * 16 + (str[i] - 'A' + 10);
                }
                else {
                    sum = sum * 16 + (str[i] - '0');
                }
            }

            cout << sum << endl;
        }
       return 0;
    }

#### 2. 两数之和
描述：给定一个整数数组 nums 和一个整数目标值 target，请你在该数组中找出 和为目标值 target  的那 两个 整数，并返回它们的数组下标。

+ 就是利用map很容易做,做过的题，通过使用map避免重复查找
+     vector<int> twoSum(vector<int>& nums, int target) {
          
          unordered_map<int, int> ma; // (target - this, this->index)

          for(int i = 0; i< nums.size(); ++i){

              if(ma.find(nums[i])   != ma.end()){
                  ma[target - nums[i]] = i;
                  continue;
              } else{
                  return {ma[nums[i]], i};
              }
          }
      }

#### 3.明明的随机数
描述： 明明生成了N个1到500之间的随机整数。请你删去其中重复的数字，即相同的数字只保留一个，把其余相同的数去掉，然后再把这些数从小到大排序，按照排好的顺序输出。
+ 就是利用了set的有序行和不重复的特点做了
+      set<int> se;
        int N, temp;
        while (scanf("%d", &N) != EOF) {
            for (int i = 0 ; i < N; ++i) {
                scanf("%d", &temp);
                se.insert(temp);
            }
            for (const auto& item : se) {
                cout << item << endl;
            }
        }
#### 4.字符个数统计
描述：编写一个函数，计算字符串中含有的不同字符的个数。字符在 ASCII 码范围内( 0~127 ，包括 0 和 127 )，换行表示结束符，不算在字符里。不在范围内的不作统计。多个相同的字符只计算一次
+ 可以scanf读入， 也可以fgets， getline读入
+ 插入set就好
+ int main()
{
    set<char> se;
    char s[100];

    fgets(s, 100, stdin);

    

    for(int i = 0; s[i] != '\n'; ++i){
        std::cout << s[i] << " ";
    }

    return 0;
}

#### 5.跳台阶
描述：一只青蛙一次可以跳上1级台阶，也可以跳上2级。求该青蛙跳上一个 n 级的台阶总共有多少种跳法（先后次序不同算不同的结果）。

+ 做法一，使用递归，费勃拉器数列，需要逆向思维的想
  int help(int n){
    if(n == 1) return 1;
    if(n == 2) return 2;

    return help(n-1) + help(n-2);
}
int main()
{
    int n;
    while (scanf("%d", &n) != -1)
    {
        std::cout << help(n);
    }
    return 0;
}


+ 做法2 使用数组存起来一些数据，比纯使用递归要节省计算
+ int ff[50] = {1,2};

int jumpFloor(int number)
{
    if (number == 1)
        return 1;
    if (number == 2)
        return 2;
    if (ff[number] != 0)
        return ff[number];
    return ff[number] = jumpFloor(number - 1) + jumpFloor(number - 2);

}

+ 做法3 动态规划，好简单 dp[i] = dp[i-1] + dp[i-2]
+ int main()
{
    int a, b, c;
    a = 1;
    b = 2;
    
    int n;
    scanf("%d", &n);
    for(int i = 3; i<= n ; ++i){
        c = a + b;
        a = b;
        b = c;
    }
    std::cout << c << " ";
    return 0;
}


#### 6.坐标移动
描述: 开发一个坐标计算工具， A表示向左移动，D表示向右移动，W表示向上移动，S表示向下移动。从（0,0）点开始移动，从输入字符串里面读取一些坐标，并将最终输入结果输出到输出文件里面。

输入：

合法坐标为A(或者D或者W或者S) + 数字（两位以内）

坐标之间以;分隔。

非法坐标点需要进行丢弃。如AA10;  A1A;  $%$;  YAD; 等。

下面是一个简单的例子 如：

A10;S20;W10;D30;X;A1A;B10A11;;A10;

+ 做法难点在与，怎么快速处理字符串，获取有用输入，题解的使用 vector<string> ,并且使用substr的方法进行遍历 需要学习
+   for (int i = 0; i < len; ++i) // 这里的这个判断好关键，逻辑就是，for循环遍历字符串，使用一个变量sub_str计算从有效字符开始到当前的具体，因此可以使用sub_str进行直接提取
    {
        if (s[i] != ';')
        {
        sublen++;
        continue;
        }
        vec.push_back(s.substr(i - sublen, sublen)); // 把每个子串都放到vector中
        sublen = 0;
    }
+ 第二个就是 因为只能是一位数字和两位数字，可以用 子串的长度， 和 第二位和第三位是不是数字 进行严格判定，不合格就不进行操作了，关键在严格判断这
+ for (int i = 0; i < vec.size(); ++i)
  {

    int num = 0;

    if (vec[i].size() == 3 && vec[i][1] >= '0' && vec[i][1] <= '9' && vec[i][2] >= '0' && vec[i][2] <= '9')
    {
      num = (vec[i][1] - '0') * 10 + (vec[i][2] - '0');
    }

    if (vec[i].size() == 2 && vec[i][1] >= '0' && vec[i][1] <= '9')
    {
      num = (vec[i][1] - '0');
    }

    // 其余的情况 num 都是 0
    
    if (vec[i][0] == 'A')
    {
      x -= num;
    }
    else if (vec[i][0] == 'D')
    {
      x += num;
    }
    else if (vec[i][0] == 'S')
    {
      y -= num;
    }
    else if (vec[i][0] == 'W')
    {
      y += num;
    }
    else
    {
      continue;
    }
  }
main函数的输入也值得一看：

    int main()
    {

    string s;
    while (cin >> s)
    {
        help(s);
    }

    return 0;
    }



#### 7.密码验证组合
描述： 密码要求:

1.长度超过8位

2.包括大小写字母.数字.其它符号,以上四种至少三种

3.不能有长度大于2的包含公共元素的子串重复 （注：其他符号不含空格或换行）

数据范围：输入的字符串长度满足 
1
≤
�
≤
100
 
1≤n≤100

+  核心地方在于字串的比较，这里只用比较长度为3的字串就可以了，用的是compare()函数
void help(string s)
{
  int len = 0;
  int a[] = {0, 0, 0, 0};
  int sub_len = 3;

  if (s.size() <= 8)
  {
    cout << "NG" << endl;
    return;
  }

  for (const auto &item : s)
  {
    if (isdigit(item))
    {
      a[0] = 1;
    }
    else if (islower(item))
    {
      a[1] = 1;
    }
    else if (isupper(item))
    {
      a[2] = 1;
    }
    else if (item == ' ' || item == '\n')
    {
      cout << "NG" << endl;
      return;
    }
    else
    {
      a[3] = 1;
    }
  }
  if (a[0] + a[1]+a[2]+a[3] >= 3)
  {
  }
  else
  {
    cout << "NG" << endl;
    return;
  }

  for (int i = 0; i <= s.length() - sub_len; ++i)
  {
    for (int j = i+1; j <= s.length() - sub_len; ++j)
    {
      if (s.compare(i, sub_len, s, j, sub_len) == 0) // 字符串还能这么比较吗！！！！ 第一个参数是起始位置，第二个是字符串长度
      {
        cout << "NG" << endl;
        return;
      }
    }
  }
  cout << "OK" << endl;
  return;
}
int main()
{

  string s;

  while (getline(cin, s)) 
  {
    help(s);
  }
  return 0;
}
+ 第二个方法是用set， 每次插入长度为3的字串，每次插入前看一下和之前的是不是有一样的，有一样的就是重复了
        if (sets.find(tmp) == sets.end()) {
            sets.insert(tmp);
        }else {
            return false;
        }
#### 8.删除字符串中出现次数最少的字符
描述：实现删除字符串中出现次数最少的字符，若出现次数最少的字符有多个，则把出现次数最少的字符都删除。输出删除这些单词后的字符串，字符串中其它字符保持原来的顺序。

数据范围：输入的字符串长度满足 1≤n≤20  ，保证输入的字符串中仅出现小写字母 

+ 做法1 因为最后要输出，所以不需要创建新的字符串，只需要把满足条件的答案输出即可
  int main()
  {

    string s;
    int sum[22]; // 用于统计出现的字符的数目
    while (getline(cin, s))
    {
      memset(sum, 0, 4 * 22);
      // help(s);
      for(const auto & item : s){
        sum[item-'a']++;
      }



      int min = 22;

      for(int i = 0; i < s.length(); ++i){
        if(sum[s[i] - 'a'] < min){
          min = sum[s[i]-'a'];
        }
      }

      for(int i = 0 ;i < s.length(); ++i){
        if(sum[s[i] - 'a'] != min){
          cout << s[i];
        }

      }
      cout << endl;
    }
    return 0;
  }

+ 做法2 创建一个空的string 每次 += 新的字符
      for(int i = 0 ;i < s.length(); ++i){
      if(sum[s[i] - 'a'] != min){
        sss += s[i];
      }
    }

  
#### 9.整数和IP地址的转换
描述
原理：ip地址的每段可以看成是一个0-255的整数，把每段拆分成一个二进制形式组合起来，然后把这个二进制数转变成
一个长整数。
举例：一个ip地址为10.0.3.193
每段数字             相对应的二进制数
10                   00001010
0                    00000000
3                    00000011
193                  11000001

组合起来即为：00001010 00000000 00000011 11000001,转换为10进制数就是：167773121，即该IP地址转换后的数字就是它了。

数据范围：保证输入的是合法的 IP 序列

输入描述：
输入 
1 输入IP地址
2 输入10进制型的IP地址

输出描述：
输出
1 输出转换成10进制的IP地址
2 输出转换后的IP地址

+ 这里没想到可以用scanf这么输入，但是有个问题是 不写！= -1 就会出错，写了就没问题， 而且这个输入模式还是很关键的
+   while (scanf("%lld\"%lld\"%lld\"%lld", &a, &b, &c, &d) != -1) 如果遇到了“ 引号输入，可以用\ 进行代替
    int main()
  {

    string s;
    long long int a, b, c, d;

    // cout << 193 + 3 * 256 + 10 * 256 * 256 * 256 << " ";
    long long int num;
    while (scanf("%lld.%lld.%lld.%lld", &a, &b, &c, &d) != -1)
    {
      scanf("%lld", &num);

      long long int ans1 = d + c * 256 + b * 256 * 256 + a * 256 * 256 * 256;


      cout <<ans1 << endl;

      a = num / (256 * 256 * 256);

      b = (num % (256 * 256 * 256)) / (256 * 256);

      long long int b_mod = (num % (256 * 256 * 256)) % (256 * 256);

      c = b_mod / 256;

      d = b_mod % 256;


      cout << a << "." << b << "." << c << "." << d << endl;
    }
    return 0;
  }
+ 方法2 没有向我一样 / 256, 而是用左移 和右移的方法：
int main()
{

  string s;
  long long int a, b, c, d;

  // cout << 193 + 3 * 256 + 10 * 256 * 256 * 256 << " ";
  long long int num;
  while (scanf("%lld.%lld.%lld.%lld", &a, &b, &c, &d) != -1)
  {
    scanf("%lld", &num);

    long long int ans1 = d + c * 256 + b * 256 * 256 + a * 256 * 256 * 256;

    cout << ans1 << endl;

    a = num >> 24;

    num = num - (a << 24);

    b = num >> 16;

    num = num - (b << 16);

    long long int b_mod = (num % (256 * 256 * 256)) % (256 * 256);

    c = num >> 8;

    num = num - (b << 8);

    d = num;

    cout << a << "." << b << "." << c << "." << d << endl;
  }
  return 0;
}
#### 10.输入整形数组和排序标识符
描述：输入整型数组和排序标识，对其元素按照升序或降序进行排序

数据范围： 
1
≤
�
≤
1000
 
1≤n≤1000  ，元素大小满足 
0
≤
�
�
�
≤
100000
 
0≤val≤100000 
+ 这里就是用的sort进行的排序，但是具体的冒泡，归并，二分，还是要自己写一下， 机试过了的吧

int main()
{ 
  int n, temp;
  int order;
  vector<int> vec;
  while (cin >> n)
  {
    for(int i = 0 ; i< n; ++i){
      scanf("%d", &temp);
      vec.push_back(temp);
    }

    scanf("%d", &order);

    if(order == 0){
      sort(vec.begin(), vec.end(),[](auto a, auto b){ return a < b;}); // 稳定排序可以是stable_sort
    }else{
      sort(vec.begin(), vec.end(),[](auto a, auto b){ return a > b;});
    }
    
    for(const auto & item : vec){
      std::cout << item << " ";
    }
    std::cout << std::endl;
  }
  return 0;
}

#### 11.字符串的逆序

+ 不知道的情况就反着输出，但看题解发现还有，reserve函数可以用
int main()
{

  string s;
  while (getline(cin, s))
  {

    reverse(s.begin(), s.end());
    std::cout << s <<endl;
    for (int i = s.length() - 1; i >= 0; --i)
    {
      std::cout << s[i];
    }
    std::cout << endl;
  }

  return 0;
}



#### 12. 合并表记录

数据表记录包含表索引index和数值value（int范围的正整数），请对表索引相同的记录进行合并，即将相同索引的数值进行求和运算，输出按照index值升序进行输出。


提示:
0 <= index <= 11111111
1 <= value <= 100000

输入描述：
先输入键值对的个数n（1 <= n <= 500）
接下来n行每行输入成对的index和value值，以空格隔开

输出描述：
输出合并后的键值对（多行）



+ 就是用map就自动升序了，

{

  map<int,int> mp;

  int num;
  int index, value;

  while(scanf("%d", &num) != -1){
    for(int i= 0; i< num; ++i){
      scanf("%d %d",&index, &value);
      mp[index] += value;
    }

    for(const auto& item : mp){
      std::cout << item.first<< " " << item.second << endl;

    }

  }

  return 0;
}

+ 然后这里尝试了一下，逆序输出也是可以的
    for (auto i = mp.end(); i != mp.begin();)
    {
      std::cout << (--i)->first << " " << i->second << endl; // 好关键的写法，是不是逆序还可以给index加一个负号
    }



#### 13. 字符串排序

+ 用set的话不能保证遇到重复的字符， 所以改用map
+ 还有要注意的是，scanf之后，后面的回车会被getline吞掉，用cin就没有这个问题
int main()
{
  map<string, int> se;

  int num;
  string s;

  while(scanf("%d", &num) != -1){
    // getchar();
    for(int i = 0;i < num; ++i){

      // getline(cin,s);
      cin >> s;
      se[s] ++;

    }
    for(const auto & item: se){
      for(int i= 0; i< item.second; ++i){ // 遍历重复字符串
        std::cout << item.first << endl;  
      }
    
    }
  }
  return 0;
}


+ 题解里面还有排序做的


int main(){
    int n;
    cin >> n;
    vector<string> strs; //字符串数组
    string s;
    for(int i = 0; i < n; i++){ //输入n个字符串
        cin >> s;
        strs.push_back(s); //堆排序
    }
    for(int i = 0; i < n; i++) // n 次冒泡
            for(int j = 1; j < n; j++)
                if(strs[j] < strs[j - 1]){//比较并冒泡
                    string temp = strs[j];  //交换
                    strs[j] = strs[j - 1];
                    strs[j - 1] = temp;
                }
    for(int i = 0; i < n; i++) //输出
        cout << strs[i] << endl;
    return 0;
}



#### 14 数据分类处理
描述：
描述
信息社会，有海量的数据需要分析处理，比如公安局分析身份证号码、 QQ 用户、手机号码、银行帐号等信息及活动记录。

采集输入大数据和分类规则，通过大数据分类处理程序，将大数据分类输出。

输入描述：
一组输入整数序列I和一组规则整数序列R，I和R序列的第一个整数为序列的个数（个数不包含第一个整数）；整数范围为0~(2^31)-1，序列个数不限

输出描述：
从R依次中取出R<i>，对I进行处理，找到满足条件的I： 

I整数对应的数字需要连续包含R<i>对应的数字。比如R<i>为23，I为231，那么I包含了R<i>，条件满足 。 

按R<i>从小到大的顺序:

(1)先输出R<i>； 

(2)再输出满足条件的I的个数； 

(3)然后输出满足条件的I在I序列中的位置索引(从0开始)； 

(4)最后再输出I。 

附加条件： 

(1)R<i>需要从小到大排序。相同的R<i>只需要输出索引小的以及满足条件的I，索引大的需要过滤掉 

(2)如果没有满足条件的I，对应的R<i>不用输出 

(3)最后需要在输出序列的第一个整数位置记录后续整数序列的个数(不包含“个数”本身)

 

序列I：15,123,456,786,453,46,7,5,3,665,453456,745,456,786,453,123（第一个15表明后续有15个整数） 

序列R：5,6,3,6,3,0（第一个5表明后续有5个整数） 

输出：30, 3,6,0,123,3,453,7,3,9,453456,13,453,14,123,6,7,1,456,2,786,4,46,8,665,9,453456,11,456,12,786


+ 反正就是好麻烦的题， 最后比较关键的就是 find 函数在vec中寻找值，string 去f ind sub_string 还有 pair的使用

int main()
{
  int numi, numr;

  while (scanf("%d", &numi) != -1)
  {
    vector<string> I;
    string s;
    
    string temp;

    for (int i = 0; i < numi; ++i)
    {
      cin >> temp;
      I.push_back(temp);
    }
    scanf("%d", &numr);

    int temp_r;

    vector<int> see_int;

    for (int i = 0; i < numr; ++i)
    {
      cin >> temp_r;

      if (find(see_int.begin(), see_int.end(), temp_r) == see_int.end()) // find 也能去vector中寻找
      {
        see_int.push_back(temp_r);
      }
    }
    sort(see_int.begin(), see_int.end()); // 寻找

    vector<string> see;

    for (const auto &item : see_int)
    {
      see.push_back(to_string(item)); // to_String 方便去 寻找字串，这里要注意，这个to_string只能用在整数上，不要用在char上
    }

    int all = 0;

    vector<int> output;

    map<string, vector<pair<int, string>>> mp; //这个还是很吊的

    for (const auto &item : see)
    {
      // 对任何一个r， 遍历I， 看看是否有这个字串
      vector<pair<int, string>> temp_vec;
      for (int i = 0; i < I.size(); ++i)
      {
        if (I[i].find(item) != -1)
        {
          temp_vec.push_back(make_pair(i, I[i])); // make_pair 存储一对数字
        }
      }
      mp[item] = temp_vec;

      if (temp_vec.size() != 0)
      {
        all += (temp_vec.size() * 2 + 2);
      }
    }

    cout << all << " ";

    for (const auto &item : see)
    {
      if (mp[item].size() != 0)
      {
        cout << item << " " << mp[item].size() << " ";
      }
      for (const auto &vvv : mp[item])
      {
        cout << vvv.first << " " << vvv.second << " ";
      }
    }
    std::cout << endl;
  }
  return 0;
}


#### 15. 查找兄弟单词
定义一个单词的“兄弟单词”为：交换该单词字母顺序（注：可以交换任意次），而不添加、删除、修改原有的字母就能生成的单词。
兄弟单词要求和原来的单词不同。例如： ab 和 ba 是兄弟单词。 ab 和 ab 则不是兄弟单词。
现在给定你 n 个单词，另外再给你一个单词 x ，让你寻找 x 的兄弟单词里，按字典序排列后的第 k 个单词是什么？
注意：字典中可能有重复单词。

查找给的单词中 有那些是 最后一个单词的兄弟单词

+ 做法，没什么复杂的，sort的地方卡了一下最后不需要return *a > *b 直接 return a> b就行
+ 第二个卡在了 最后的ans[k-1] ans有可能是空的
bool help(string s, int*xxx)
{
  int ooo[26]{0};
  for(const auto & item: s){
      ooo[item-'a'] ++;
    }
  for(int i = 0; i< 26; ++i){
    if(xxx[i] != ooo[i]){
      return false;
    }
  }
  return true;

}

int main()
{
  int xxx[26]{0};
  int num, k;

  string s;
  vector<string> vec;

  while (scanf("%d", &num) != -1)
  {
    string temp;
    for (int i = 0; i < num; ++i)
    {
      cin >> temp;
      vec.push_back(temp);
    }

    cin >> s;

    cin >> k;

    vector<string> ans;


    for(const auto & item: s){
      xxx[item-'a'] ++;
    }

    for(int i = 0; i< vec.size(); ++i){
      if(vec[i] != s && vec[i].size() == s.size() && help(vec[i], xxx) == true){
        ans.push_back(vec[i]);
      }
    }

    sort(ans.begin(), ans.end(), [](auto a,auto b){ return a > b;});

    cout << ans.size() << endl;
    if (ans.size() == 0){


    } else{

      cout << ans[k-1] << endl; // 这里如果不判断会越界报错
    }
  }

  return 0;
}


#### 16 NC37 合并区间
描述
给出一组区间，请合并所有重叠的区间。
请保证合并后的区间按区间起点升序排列。



+ 做法：就是先排序一下，到也没啥，只是时间复杂度是nlogn

    vector<Interval> merge(vector<Interval>& intervals) {
        // write code here
        
        vector<Interval> ret;

        if(intervals.size() == 0) return {};

        sort(intervals.begin(), intervals.end(),[](auto a, auto b){return a.start < b.start;});

        auto temp_interval = Interval(intervals[0].start, intervals[0].end);

        for(const auto& item : intervals){
            if(item.start>= temp_interval.start && item.start <= temp_interval.end){
                temp_interval.end = max(temp_interval.end, item.end);
                continue;
            } else{
                ret.push_back(temp_interval);
                temp_interval = item;
            }
            
        }

        ret.push_back(temp_interval);


        return ret;

    }

+ 做读取输入的时候，遇到了好多问题
vector<pair<int, int>> input;
  getchar();  // 首先是这个前两个的输入是括号的情况，如果不用这个吞掉，写成while(scanf("[[%d,%d]", &l, &r)!= -1){ 即使按下回车，这个scanf的返回值仍是0,
              // 这里的回车是会被吃进去的，和以前的scanf("%d") 不一样，%d会一直等到数字的出现，第一个字符匹配符号了，直接就匹配任何字符了
              // 所以scanf 第一个如果是特殊字符，其实和 %c 一样，一定要注意哦
  getchar(); // 先把括号吃掉，否则会有问题 

  while(scanf("%d,%d]", &l, &r)!= -1){
    input.push_back(make_pair(l ,r));
    cout << l << " " << r << endl;

    while(scanf(",[%d,%d]", &l, &r ) != 0){ // 这个第一个字符如果不是 ，那个字符是不会被吞掉的,这里要注意，第一个是如果不一样不会吞掉，第二个是不一致时会返回0
                                            // 这个输入处理的经验都是，当scanf第一个字符是特殊字符的时候，一定要注意
      input.push_back(make_pair(l ,r));
      cout <<a << " " <<  l << " " << r << "yes" << endl ;
    }



#### 17成绩排序
描述
给定一些同学的信息（名字，成绩）序列，请你将他们的信息按照成绩从高到低或从低到高的排列,相同成绩

都按先录入排列在前的规则处理。

例示：
jack      70
peter     96
Tom       70
smith     67

从高到低  成绩
peter     96
jack      70
Tom       70
smith     67

从低到高

smith     67

jack      70

Tom       70
peter     96

+ 方法这里最关键的一点是 用的稳定排序，第一次用这个
int main()
{
  int num;
  int order;
  vector<pair<string, int>> vec;

  while (cin >> num >> order)
  {
    string name;
    int score;
    for (int i = 0; i < num; ++i)
    {
      cin >> name >> score;
      vec.push_back(make_pair(name, score));
    }
    if (order == 1)
    {
      stable_sort(vec.begin(), vec.end(), [](auto a, auto b)
                  { return a.second < b.second; });
    }
    else
    {
      stable_sort(vec.begin(), vec.end(), [](auto a, auto b)
                  { return a.second > b.second; });
    }

    for (const auto &item : vec)
    {
      cout << item.first << " " << item.second << endl;
    }

    return 0;
  }
}

##### 18有效括号序列
描述
给出一个仅包含字符'(',')','{','}','['和']',的字符串，判断给出的字符串是否是合法的括号序列
括号必须以正确的顺序关闭，"()"和"()[]{}"都是合法的括号序列，但"(]"和"([)]"不合法。

+ 用栈挺简单的

bool isValid(string s)
{

  stack<char> qe;

  for(const auto & item : s){
    if(qe.size() == 0 || item=='(' || item == '[' || item == '{'){
      qe.push(item);
    }else if(item == ')'){
      if(qe.top() == '(') qe.pop();
      else return false;
    }else if(item == ']'){
      if(qe.top() == '[') qe.pop();
      else return false;

    }else if(item == '}'){
      if(qe.top() == '{') qe.pop();
      else return false;
    }
  }
  if(qe.size() != 0) return false;
  return true;
}

int main()
{

  return 0;
}


#### 19最长回文子串

描述
对于长度为n的一个字符串A（仅包含数字，大小写英文字母），请设计一个高效算法，计算其中最长回文子串的长度。
+ 字串的题，好常见，这里复杂的方法是用 从一个数往两边扩散，和两个数往两边扩散， 然后得到的复杂度n2
int getLongestPalindrome(string A) {
        int ret = 1;

        if (A.size() == 0) return 0;
        for (int i = 0; i < A.size(); ++i) {
            int temp_len = 1;
            int len = 1;
            while ((i - len) >= 0 && (i + len) < A.size() && A[i - len] == A[i + len]) {
                temp_len = len * 2 + 1;
                len ++;
            }
            ret = max(temp_len, ret);
        }

        for (int i = 0; i < A.size(); ++i) {
            int len = 0;

            while ((i - len) >= 0 && (i + len + 1) < A.size() &&
                    A[i - len] == A[i + len + 1]) {
                len ++;
            }
            ret = max(len * 2, ret);
        }
        return ret;
    }

+ 动态规划暂时不考虑






#### 20最小覆盖子串
给你一个字符串 s 、一个字符串 t 。返回 s 中涵盖 t 所有字符的最小子串。如果 s 中不存在涵盖 t 所有字符的子串，则返回空字符串 "" 。


注意：

对于 t 中重复字符，我们寻找的子字符串中该字符数量必须不少于 t 中该字符数量。
如果 s 中存在这样的子串，我们保证它是唯一的答案。


+ 由于 会有重复的字符，所以动态规划不太会写，先写了一个遍历的，因为有大小写，所以用的60， 但最后超时了
bool check(int *ss, int *tt){

  for(int i = 0;i< 60;++i){
    if(ss[i] < tt[i]) return false;
  }
  return true;
}
string minWindow(string s, string t)
{
  if(s.size() < t.size()) return "";
j
    tt[item-'A']++;
  }
  
  for(int i = 0 ;i < s.size(); ++i){
    int temp[60]{0};
    int temp_len = 0;
    for(int len = 0; i + len < s.size(); ++len){
        
      temp[s[i+len]-'A'] ++;

      if(len + 1 < t.size()) continue;
      
      if(check(temp, tt) == true){
        temp_len = len+1;
        if(temp_len < min_len){
          l =i;
        }
        min_len =min(min_len, temp_len);
        break;
      }
    }
  }
  if(min_len == s.size() + 1){
    return "";
  } else{
    return s.substr(l, min_len);
  }

}


+ 另一个做法是使用滑动窗口，第一次写，注意一下！还有一个关键的就是，如果是那些s中都没有的字符，在t中也没必要进行统计，所以用可以用map
使用滑动窗口需要证明，先移动有边界，再移动左边界的这个idea的正确性，
+ 大概想了一下，是反证法，不会有把最小窗口忽略的可能性

bool check()
{
  for (const auto &item : mp)
  {
    if (cnt.find(item.first) == cnt.end())
      return false;
    else
    {
      if (cnt[item.first] < item.second)
        return false;
    }
  }
  return true;
}

string minWindow(string s, string t)
{

  if (s.size() < t.size())
    return "";

  int min_len = s.size() + 1;

  int min_l, min_r;

  for (const auto &item : t) //初始化mp
  {
    mp[item]++;
  }

  int l = 0, r = 0;

  for (r = 0; r != t.size() - 1; ++r) // 将r放到等长的初始值 比t少一个 这里可以保证初始化完毕后肯定是flase
  {
    if (mp.find(s[r]) != mp.end())
    {
      cnt[s[r]]++;
    }
  }
  
  while (r != s.size())
  { // 先移动有边界找到满足的情况，再移动做边界，找到不满足的情况
    while (check() == false)
    {
      if (mp.find(s[r]) != mp.end())
      {
        cnt[s[r]]++;
      }
      r++;
      if(r >= s.size()) break;
    }

    while(check() == true){
      if(r - l <= min_len){
        min_len = r-l+1;
        min_l = l;
        min_r = r;
      } 

      if (mp.find(s[l]) != mp.end())
      {
        cnt[s[l]]--;
      }
      l++;
    }
    
  }
  if(min_len == s.size() +1) return "";
  else return s.substr(min_l, min_len);
}


#### 21称砝码

描述
现有n种砝码，重量互不相等，分别为 m1,m2,m3…mn ；
每种砝码对应的数量为 x1,x2,x3...xn 。现在要用这些砝码去称物体的重量(放在同一侧)，问能称出多少种不同的重量。
注：
称重重量包括 0 
+ 这个做法还是很牛的，最开始集合是0, 每加一个砝码，在所有的已知的重量的基础上 加上这个砝码

int main()
{
  int n;
  vector<int> weight;
  vector<int> num;

  int temp_weight, temp_num;

  while (cin >> n)
  {

    for (int i = 0; i < n; ++i)
    {
      cin >> temp_weight;
      weight.push_back(temp_weight);
    }

    for (int i = 0; i < n; ++i)
    {
      cin >> temp_num;
      num.push_back(temp_num);
    }

    int ret = 0;

    set<int> se;

    vector<int> vec;

    se.insert(0);

    for (int j = 0; j < weight.size(); ++j) // 遍历这些砝码
    {
    for (int i = 0; i < num[j]; ++i)// 遍历数量
      {
        vector<int> neww;
        for (const auto &value : se)
        {
          if(se.count(value + weight[j]) == 0){
            neww.push_back(value + weight[j]);    // 不这么写容易死循环
          }
        }
        for(const auto & item : neww){
          se.insert(item);
        }
      }
    }
    cout << se.size() << endl;
  }
  return 0;
}
#### 22背包问题


  for(int i = 1; i<6; ++i){
    for(int v = 1; v < cap; ++v){
      if(v < weight[i-1]){
        dp[i][v] = dp[i-1][v]; //如果装不下，就不用求下面的和了
      }else{
        dp[i][v] = max(dp[i-1][v-weight[i-1]] + value[i-1] , dp[i-1][v]); // int dp[6][55]{0}; //6个物品装入55容量的背包的最大价值 dp[0][k]全是0
      }
    }
  }

  //dp[i][v]表示 第i件物品装进容量是v的包，可以得到的最大价值， 



#### 23面试题 08.08. 有重复字符串的排列组合
有重复字符串的排列组合。编写一种方法，计算某字符串的所有排列组合。

示例1:

 输入：S = "qqe"
 输出：["eqq","qeq","qqe"]
示例2:

 输入：S = "ab"
 输出：["ab", "ba"]
+ 我的做法是，用set 加上不断在可能的位置插入新 的char ，学到了两个收获


vector<string> permutation(string S)
{
  set<string> se;

  for (const auto &c : S)
  {
    cout << c<<endl;
    if (se.size() == 0)
    {
      // cout << to_string(c) << " "<< string(1,c) << endl; //1. to_string 不能用于char，否则会被认为是整数
      se.insert(string(1,c));
      continue;
    }

    set<string> new_se;

    for (const auto &item : se)
    {
      for (int i = 0; i <= item.size(); ++i)
      {
        string temp = item.substr(0, i) + string(1,c) + item.substr(i,item.size()-i); // 2.第二个收获是，sub_str的第一个参数可以和size想等，这时返回空
        cout  <<temp <<endl;
        new_se.insert(temp);
      }
    }
    se = new_se;
  }
  vector<string> ret;
  for (const auto &item : se)
  {
    ret.push_back(item);
  }

  return ret;
}

+ 其实还可以用dfs配合剪枝来做

#### 24 leetcode77 组合

给定两个整数 n 和 k，返回范围 [1, n] 中所有可能的 k 个数的组合。

你可以按 任何顺序 返回答案。


+ 这里这个写法，第一次见，用到了dfs，还挺利害的，所以感觉排列组合的题，好像都能这么写

public:
  int _n;
  int _k;

  vector<vector<int>> ret;

  void dfs(int cur, vector<int> &temp)
  {
    if (temp.size() == _k)
    {
      ret.push_back(temp);
      return;
    }

    if (cur > _n)
      return;

    temp.push_back(cur);

    dfs(cur + 1, temp);

    temp.pop_back();

    dfs(cur + 1, temp);
  }
  vector<vector<int>> combine(int n, int k)
  {
    _n = n;
    _k = k;

      vector<int> temp;
      dfs(1, temp);
    
    return ret;
  }


#### 28括号的最大嵌套深度

+ 做法，我用的栈，但其实用一个变量就可以了，因为一定是合法的

    int maxDepth(string s) {
        stack<int> st;

        int max_len = 0;

        for(const auto& item : s){
          if(item == '('){
            st.push(1);
            max_len = max(max_len, (int)st.size());
          }else if(item == ')'){
            st.pop();
          }
        }
        return max_len;
    }



####  29最长连续递增序列
给定一个未经排序的整数数组，找到最长且 连续递增的子序列，并返回该序列的长度。

+  以为要用双指针，其实一个遍历就够了，维护一个当前的start 写复杂了，
  int findLengthOfLCIS(vector<int> &nums)
  {
    int l = 0, r = 0;
    int ret = 1;
    while(r != nums.size()-1){
      while(nums[r+1] > nums[r])
      {
        r++;
        if(r == nums.size()-1) break;
      }

      ret = max(ret, r -l +1);
      
      if(r == nums.size()-1) break;
      l = r + 1;
      r = l;
    }
    return ret;

  }

+ 题解：
int findLengthOfLCIS(vector<int>& nums) {
        int ans = 0;
        int n = nums.size();
        int start = 0;
        for (int i = 0; i < n; i++) {
            if (i > 0 && nums[i] <= nums[i - 1]) {
                start = i;
            }
            ans = max(ans, i - start + 1);
        }
        return ans;
    }

---
  leecode 674  
---