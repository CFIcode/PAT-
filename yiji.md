#PAT (Basic Level) Practise （中文）

@(PAT)

[toc]

##1001. 害死人不偿命的(3n+1)猜想 (15)


卡拉兹(Callatz)猜想：

对任何一个自然数n，如果它是偶数，那么把它砍掉一半；如果它是奇数，那么把(3n+1)砍掉一半。这样一直反复砍下去，最后一定在某一步得到n=1。卡拉兹在1950年的世界数学家大会上公布了这个猜想，传说当时耶鲁大学师生齐动员，拼命想证明这个貌似很傻很天真的命题，结果闹得学生们无心学业，一心只证(3n+1)，以至于有人说这是一个阴谋，卡拉兹是在蓄意延缓美国数学界教学与科研的进展……

我们今天的题目不是证明卡拉兹猜想，而是对给定的任一不超过1000的正整数n，简单地数一下，需要多少步（砍几下）才能得到n=1？

**输入格式：**每个测试输入包含1个测试用例，即给出自然数n的值。

**输出格式：**输出从n计算到1需要的步数。

**输入样例：**
3
**输出样例：**
5
***
```
#include<iostream>
using namespace std;

int main(){
    int n;
    cin>>n;
    int count = 0;

    while(n!=1){
        if(n%2==0){
            n/=2;
            count++;
        }else{
            n = (3*n+1)/2;
            count++;
        }
    }
    cout<<count;
    system("pause");
    return 0;
}
```
***
##1002. 写出这个数 (20)


读入一个自然数n，计算其各位数字之和，用汉语拼音写出和的每一位数字。

**输入格式：**每个测试输入包含1个测试用例，即给出自然数n的值。这里保证n小于10100。

**输出格式：**在一行内输出n的各位数字之和的每一位，拼音数字间有1 空格，但一行中最后一个拼音数字后没有空格。

**输入样例：**
1234567890987654321123456789
**输出样例：**
yi san wu
***
```
#include<iostream>
#include<string>
#include<stack>
using namespace std;

int main(){
	string input;
	string output;
	cin>>input;//你输入的数字
	stack<int>st;
	int sum = 0;//各位数之和
	string spell[10]={"ling","yi","er","san","si","wu","liu","qi","ba","jiu"};
	for(int i=0;i<input.size();i++){
		sum = sum+input[i]-48;
	}
	while(sum!=0){//入栈
		st.push(sum%10);
		sum=sum/10;
	}
	while(!st.empty()){//判断栈是否为空
		output = output+spell[st.top()]+" ";
		st.pop();
	}
	output = output.substr(0,output.size()-1);
	cout<<output;
	system("pause");
	return 0;
}

```
***
##1003. 我要通过！(20)


“**答案正确**”是自动判题系统给出的最令人欢喜的回复。本题属于PAT的“**答案正确**”大派送 —— 只要读入的字符串满足下列条件，系统就输出“**答案正确**”，否则输出“答案错误”。

得到“**答案正确**”的条件是：

1. 字符串中必须仅有P, A, T这三种字符，不可以包含其它字符；
2. 任意形如 xPATx 的字符串都可以获得“**答案正确**”，其中 x 或者是空字符串，或者是仅由字母 A 组成的字符串；
3. 如果 aPbTc 是正确的，那么 aPbATca 也是正确的，其中 a, b, c 均或者是空字符串，或者是仅由字母 A 组成的字符串。

现在就请你为PAT写一个自动裁判程序，判定哪些字符串是可以获得“**答案正确**”的。
**输入格式：** 每个测试输入包含1个测试用例。第1行给出一个自然数n (<10)，是需要检测的字符串个数。接下来每个字符串占一行，字符串长度不超过100，且不包含空格。

**输出格式：**每个字符串的检测结果占一行，如果该字符串可以获得“**答案正确**”，则输出YES，否则输出NO。

**输入样例：**
8
PAT
PAAT
AAPATAA
AAPAATAAAA
xPATx
PT
Whatever
APAAATAA
**输出样例：**
YES
YES
YES
YES
NO
NO
NO
NO

***
这道题目对题意的理解也是个问题，其实是把符合条件2的格式，变成aPbTc的形式，比如对于字符串"PAT"（a=null，b=A，c=null）它符合条件2的要求，那么就可以按照aPbATca的形式写，也就是"PAAT"，再接下去是"PAAAT"。另如"APATA"（a=A，b=A，c=A），接下去是"APAATAA"，再接下去是"APAAATAAA"。对于任何从这样扩展而来的字符串，b的初始值一定是A，而c的初始值与a相同，对于"aPbTc"的下一个其实是"aPAATaa"，再下一个是"aPAAATaaa"，再下一个是"aPAAAATaaaa"，规律就出来了。在P前面A的个数*P与T之间A的个数等于T后面A的个数，若符合即答案正确，反之错误。
```
#include<iostream>
#include<string>

using namespace std;


//三个条件：
bool onlyPAT(string s){//第一个条件,只能含有P,A,T三个字符
	for(int i=0;i<s.size();i++){
		if(s[i]!='P'&&s[i]!='A'&&s[i]!='T'){
			return false;
		}
	}
	return true;
}

bool PTOnce(string s){//第二个条件,P,T只能出现一次
	int pTimes=0;
	int tTimes=0;
	for(int i=0;i<s.size();i++){
		if(s[i]=='P'){
			pTimes++;
		}
		if(s[i]=='T'){
			tTimes++;
		}
	}
	if(pTimes!=1||tTimes!=1){
			return false;
		}
	return true;
}
bool AInMiddle(string s){//PT中间至少一个A
	return(s.find('T')-s.find('P')>=2);
}

bool conditionThree(string s){//条件三,在P前面A的个数*P与T之间A的个数等于T后面A的个数
	int firstNum,secondNum,thirdNum;
	firstNum = s.find('P');
	secondNum=s.find('T')-s.find('P')-1;
	thirdNum=s.size()-1-s.find('T');
	if (thirdNum==firstNum*secondNum)
    {
		return true;
    }
		return false;
}

bool totalJudge(string s){//综合判断
	if(!onlyPAT(s)||!PTOnce(s)){
		return false;
	}
	if(!AInMiddle(s)){
		return false;
	}
	if(!conditionThree(s)){
		return false;
	}
	return true;
}


int main(){
	int n;
	cin>>n;
	for(int i=0;i<n;i++){
		string s;
		cin>>s;
		if(totalJudge(s)){
			cout<<"YES";
		}else{
			cout<<"NO";
		}
		if(i!=n-1){
			cout<<endl;
		}
	}
	system("pause");
	return 0;

}
```
***

##1004. 成绩排名 (20)


读入n名学生的姓名、学号、成绩，分别输出成绩最高和成绩最低学生的姓名和学号。

**输入格式：**每个测试输入包含1个测试用例，格式为

  第1行：正整数n
  第2行：第1个学生的姓名 学号 成绩
  第3行：第2个学生的姓名 学号 成绩
  ... ... ...
  第n+1行：第n个学生的姓名 学号 成绩
其中姓名和学号均为不超过10个字符的字符串，成绩为0到100之间的一个整数，这里保证在一组测试用例中没有两个学生的成绩是相同的。
**输出格式：**对每个测试用例输出2行，第1行是成绩最高学生的姓名和学号，第2行是成绩最低学生的姓名和学号，字符串间有1空格。

**输入样例：**
3
Joe Math990112 89
Mike CS991301 100
Mary EE990830 95
**输出样例：**
Mike CS991301
Joe Math990112
***
```
#include<iostream>
#include<string>
using namespace std;
int main()
{
  int n;
  string inputname,inputid;
  int inputscore,currentmax=0,currentmin=100;
    string max_name_id,min_name_id;
  cin>>n;
  for(int i=0;i<n;i++)
  {
    cin>>inputname>>inputid>>inputscore;
    if(inputscore>=currentmax)
    {
      currentmax=inputscore;
      max_name_id=inputname+" "+inputid;
    }
    if(inputscore<=currentmin)
    {
      currentmin=inputscore;
      min_name_id=inputname+" "+inputid;
    }
  }
  cout<<max_name_id<<endl<<min_name_id;
  return 0;
}
```
***
##1005. 继续(3n+1)猜想 (25)


卡拉兹(Callatz)猜想已经在1001中给出了描述。在这个题目里，情况稍微有些复杂。

当我们验证卡拉兹猜想的时候，为了避免重复计算，可以记录下递推过程中遇到的每一个数。例如对n=3进行验证的时候，我们需要计算3、5、8、4、2、1，则当我们对n=5、8、4、2进行验证的时候，就可以直接判定卡拉兹猜想的真伪，而不需要重复计算，因为这4个数已经在验证3的时候遇到过了，我们称5、8、4、2是被3“覆盖”的数。我们称一个数列中的某个数n为“关键数”，如果n不能被数列中的其他数字所覆盖。

现在给定一系列待验证的数字，我们只需要验证其中的几个关键数，就可以不必再重复验证余下的数字。你的任务就是找出这些关键数字，并按从大到小的顺序输出它们。

**输入格式：**每个测试输入包含1个测试用例，第1行给出一个正整数K(<100)，第2行给出K个互不相同的待验证的正整数n(1<n<=100)的值，数字间用空格隔开。

**输出格式：**每个测试用例的输出占一行，按从大到小的顺序输出关键数字。数字间用1个空格隔开，但一行中最后一个数字后没有空格。

**输入样例：**
6
3 5 6 7 8 11
**输出样例：**
7 6

***
```
#include<iostream>  
#include<list>  
#include<vector>  
#include<algorithm>  
using namespace std;  
bool find(list<int>li, int value)//在list里查找某个元素，找到了返回true,否则返回false  
{  
    for (auto p : li) //这个写法vs2010不能通过
    {  
        if (value == p)  
        {  
            return true;  
        }  
    }  
    return false;  
}  
int main()  
{  
    int n;//数字个数  
    int num[101];//待验证的正整数  
    int i = 0;//控制循环  
    int temp;  
    int flag = 1;//控制打印空格  
    list<int> li;//list保存被覆盖的数  
    cin >> n;  
    for (i = 0; i < n; i++)  
    {  
        cin >> num[i];//输入待验证的正整数  
        temp = num[i];  
        while (temp != 1)//计算被覆盖的数字  
        {  
            if (temp % 2 == 1)  
            {  
                temp = (3 * temp + 1) / 2;  
                li.push_back(temp); //把被覆盖的数字放进list  
            }else{  
                temp = temp / 2;  
                li.push_back(temp);  
            }  
        }  
    }  
    li.sort();//对被覆盖的数进行排序  
    li.unique();//去除重复的数字  
    sort(num, num + n);//对被验证的正整数进行排序  
    for (i =n-1; i>=0; i--)//从大到小输出  
    {  
        if (!find(li, num[i]))//如果被验证的正整数没被覆盖  
        {  
            if (flag==1)  
            {  
                cout << num[i] ;//输出  
                flag = 0;  
            }  
            else  
            {  
                cout << ' ' << num[i];  
            }  
        }  
    }  
    return 0;  
} 
```
***

##1006. 换个格式输出整数 (15)


让我们用字母B来表示“百”、字母S表示“十”，用“12...n”来表示个位数字n（<10），换个格式来输出任一个不超过3位的正整数。例如234应该被输出为BBSSS1234，因为它有2个“百”、3个“十”、以及个位的4。

**输入格式：**每个测试输入包含1个测试用例，给出正整数n（<1000）。

**输出格式：**每个测试用例的输出占一行，用规定的格式输出n。

**输入样例1：**
234
**输出样例1：**
BBSSS1234
**输入样例2：**
23
**输出样例2：**
SS123

***

```
#include<iostream>
using namespace std;

int main(){

	int ge,shi,bai;
	int input;
	cin>>input;
	ge = input%10;
	shi = (input/10)%10;
	bai = (input/100)%10;
	while(bai--){//0表示假,bai--为0时结束循环
		cout<<"B";
	}
	while(shi--){
		cout<<"S";
	}
	for(int i=1;i<=ge;i++){
		cout<<i;
	}
	system("pause");
	return 0;
	
}
```
***
##1007. 素数对猜想 (20)


让我们定义 dn 为：dn = pn+1 - pn，其中 pi 是第i个素数。显然有 d1=1 且对于n>1有 dn 是偶数。“素数对猜想”认为“存在无穷多对相邻且差为2的素数”。

现给定任意正整数N (< 105)，请计算不超过N的满足猜想的素数对的个数。

**输入格式：**每个测试输入包含1个测试用例，给出正整数N。

**输出格式：**每个测试用例的输出占一行，不超过N的满足猜想的素数对的个数。

**输入样例：**
20
**输出样例：**
4

```
#include<iostream>
#include<cmath>
using namespace std;

bool isPrime(int value){
	if(value==0||value==1){
		return false;
	}
	for(int i=2;i<=(int)sqrt((float)value);i++){//vs2010需要这么写(int)sqrt((float)value，其他只要sqrt(value)
		if(value%i==0){
			return false;
		}
	}
	return true;
}

int main(){
	int n;
	int front=2,rear=3;
	int count=0;
	cin>>n;
	for(int i=4;i<=n;i++){
		if(isPrime(i)){
			front=rear;
			rear=i;
			if(rear-front==2){
				count++;
			}
		}
	}
	cout<<count;
	
	system("pause");
	return 0;
}
```
***
##1008. 数组元素循环右移问题 (20)


一个数组A中存有N（N>0）个整数，在不允许使用另外数组的前提下，将每个整数循环向右移M（M>=0）个位置，即将A中的数据由（A0 A1……AN-1）变换为（AN-M …… AN-1 A0 A1……AN-M-1）（最后M个数循环移至最前面的M个位置）。如果需要考虑程序移动数据的次数尽量少，要如何设计移动的方法？

**输入格式：**每个输入包含一个测试用例，第1行输入N ( 1<=N<=100)、M（M>=0）；第2行输入N个整数，之间用空格分隔。

**输出格式：**在一行中输出循环右移M位以后的整数序列，之间用空格分隔，序列结尾不能有多余空格。

**输入样例：**
6 2
1 2 3 4 5 6
**输出样例：**
5 6 1 2 3 4

```
#include<iostream>
using namespace std;

int main(){//思路解析：假如输入123456，存入数组时存为123456123456,,然后从（n-实际移动数）开始输出，一共输出n个。
	int n,N;
	int arr[202];
	cin>>n>>N;
	for(int i=0;i<n;i++){
		cin>>arr[i];
		arr[n+i]=arr[i];
	}
	if(N>n)
		N=N%n;
	for(int j=n-N;j<2*n-N;j++){
		if(j!=2*n-N-1){
			cout<<arr[j]<<" ";
		}else{
			cout<<arr[j];
		}	
	}
	system("pause");
	return 0;

}
```

##1009. 说反话 (20)


给定一句英语，要求你编写程序，将句中所有单词的顺序颠倒输出。

**输入格式：**测试输入包含一个测试用例，在一行内给出总长度不超过80的字符串。字符串由若干单词和若干空格组成，其中单词是由英文字母（大小写有区分）组成的字符串，单词之间用1个空格分开，输入保证句子末尾没有多余的空格。

**输出格式：**每个测试用例的输出占一行，输出倒序后的句子。

**输入样例：**
Hello World Here I Come
**输出样例：**
Come I Here World Hello

```
#include<iostream>
#include<vector>
#include<string>
using namespace std;

int main(){
	string temp;
	vector<string>v;
	
	do{
	cin>>temp;//cin 遇到空格也会停
	v.push_back(temp);
	}while(getchar()!='\n');

	for(int i=v.size()-1;i>=0;i--){
		if(i==v.size()-1){
			cout<<v[i];
		}else{
			cout<<' '<<v[i];
		}	
	}

	system("pause");
	return 0;
}
```
***
##1010. 一元多项式求导 (25)


设计函数求一元多项式的导数。（注：xn（n为整数）的一阶导数为n*xn-1。）

**输入格式：**以指数递降方式输入多项式非零项系数和指数（绝对值均为不超过1000的整数）。数字间以空格分隔。

**输出格式：**以与输入相同的格式输出导数多项式非零项的系数和指数。数字间以空格分隔，但结尾不能有多余空格。注意“零多项式”的指数和系数都是0，但是表示为“0 0”。

**输入样例：**
3 4 -5 2 6 1 -2 0
**输出样例：**
12 3 -10 1 6 0

```
#include<iostream>
using namespace std;

int main(){
	int A;//A为系数
	int n;//n为指数
	int flag=1;//标记第一次输入
	do{
		cin>>A>>n;
		if(n>0){
			if(flag==1){
				cout<<A*n<<' '<<n-1;
				flag = 0;
			}else{
				cout<<' '<<A*n<<' '<<n-1;
			}
		}
		if(flag==1){
			cout<<"0 0";//若指数系数都为零，则输出0 0  
		}
	}while(getchar()!='\n');

	system("pause");
	return 0;
}
```
***
##1011. A+B和C (15)


给定区间[-2^31, 2^31]内的3个整数A、B和C，请判断A+B是否大于C。

**输入格式：**

输入第1行给出正整数T(<=10)，是测试用例的个数。随后给出T组测试用例，每组占一行，顺序给出A、B和C。整数间以空格分隔。

**输出格式：**

对每组测试用例，在一行中输出“Case #X: true”如果A+B>C，否则输出“Case #X: false”，其中X是测试用例的编号（从1开始）。

**输入样例：**
4
1 2 3
2 3 4
2147483647 0 2147483646
0 -2147483648 -2147483647
**输出样例：**
Case #1: false
Case #2: true
Case #3: true
Case #4: false

```
#include<iostream>

using namespace std;

int main(){
	double a,b,c;//这边如果是int答案错误
	int n;
	cin>>n;
	for(int i=1;i<=n;i++){
		cin>>a>>b>>c;
		if((a+b)>c){
			cout<<"Case #"<<i<<": "<<"true"<<endl;
		}else{
			cout<<"Case #"<<i<<": "<<"false"<<endl;
		}
	}
	system("pause");
	return 0;
}
```
***

1012. 数字分类 (20)


给定一系列正整数，请按要求对数字进行分类，并输出以下5个数字：

A1 = 能被5整除的数字中所有偶数的和；
A2 = 将被5除后余1的数字按给出顺序进行交错求和，即计算n1-n2+n3-n4...；
A3 = 被5除后余2的数字的个数；
A4 = 被5除后余3的数字的平均数，精确到小数点后1位；
A5 = 被5除后余4的数字中最大数字。
**输入格式：**

每个输入包含1个测试用例。每个测试用例先给出一个不超过1000的正整数N，随后给出N个不超过1000的待分类的正整数。数字间以空格分隔。

**输出格式：**

对给定的N个正整数，按题目要求计算A1~A5并在一行中顺序输出。数字间以空格分隔，但行末不得有多余空格。

若其中某一类数字不存在，则在相应位置输出“N”。

**输入样例1：**
13 1 2 3 4 5 6 7 8 9 10 20 16 18
**输出样例1：**
30 11 2 9.7 9
**输入样例2：**
8 1 2 4 5 6 7 9 16
**输出样例2：**
N 11 2 N 9


```
#include<iostream>
using namespace std;

int main(){
	int A1=0,A2=0,A3=0,A5=0;
	int A2_exist = 0;
	double A4=0.0;
	int A4Num=0;
	int input,n;
	int flag=1;
	cin>>n;
	for(int i=0;i<n;i++){
		cin>>input;
		
		switch(input%5){
		case 0: 
			if(input%2==0){
				A1=A1+input;
			}
			break;
		case 1:
			A2=A2+input*flag;
			flag=-flag;
			A2_exist=1;
			break;
		case 2:
			A3++;
			break;
		case 3:
			A4=A4+input;
			A4Num++;
			break;
		case 4:
			A5 = (A5>input)?A5:input;
			break;
		}
	}
	if(A1>0){
		cout<<A1<<' ';
	}else{
		cout<<'N'<<' ';
	}

	if(A2_exist>0){
		cout<<A2<<' ';
	}else{
		cout<<'N'<<' ';
	}

	if(A3>0){
		cout<<A3<<' ';
	}else{
		cout<<'N'<<' ';
	}

	if(A4>0){
		printf("%.1f ",A4/A4Num);
	}else{
		cout<<'N'<<' ';
	}

	if(A5>0){
		cout<<A5;
	}else{
		cout<<'N';
	}
	
	
	system("pause");
	return 0;
}
```
***

##1013. 数素数 (20)


令Pi表示第i个素数。现任给两个正整数M <= N <= 104，请输出PM到PN的所有素数。

**输入格式：**

输入在一行中给出M和N，其间以空格分隔。

**输出格式：**

输出从PM到PN的所有素数，每10个数字占1行，其间以空格分隔，但行末不得有多余空格。

**输入样例：**
5 27
**输出样例：**
11 13 17 19 23 29 31 37 41 43
47 53 59 61 67 71 73 79 83 89
97 101 103

```
#include<iostream>
#include<cmath>
using namespace std;


bool isPrime(int value){
	if(value==0 || value==1){
		return false;
	}
	for(int i=2;i<=sqrt(value*1.0);i++){
		if(value%i==0){
			return false;
		}			
	}
	return true;
}
int main(){
	int m,n;
	int value=2;
	int count = 0;
	int inner_count = 0;//在[m,n]范围里的素数个数
	cin>>m>>n;

	while(count<=n){
		
		if(isPrime(value)){
			count++;
			if(count>=m&&count<=n){
				inner_count++;
				if(inner_count%10==1){//输出每行第一个数
					cout<<value;
				}else{
					cout<<' '<<value;
				}
				if(inner_count%10==0){//每十个数换一行
					cout<<endl;
				}
			}	
		}
		value++;
		
	}

	system("pause");
	return 0;
}
```
***
