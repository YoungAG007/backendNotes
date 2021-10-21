## 1.稀疏数组与队列

### 1.1稀疏数组（Sparse  Array）

用数组实现稀疏矩阵，数组为n*3大小

arr[0] [0]为矩阵的row（行数）

arr[0] [1]为矩阵的col（列数）

arr[0] [2]为矩阵的有效元素个数

arr[i] [j] = k代表在矩阵的第i行第j列的元素值为k

------

### 1.2队列（queue）

有序列表，可用数组与链表（linkedList中的removeFirst和addLast方法）实现，遵循先进先出，在尾部添加，在头部删除

#### 1.2.1用环形数组实现队列

```java
//使用数组模拟环形队列   留一个为空
class CircleArrayQueue {
    private int maxSize;
    private int front;
    private int rear;
    private int[] arr;  //模拟队列

    public CircleArrayQueue(int arrMaxSize) {
        maxSize = arrMaxSize;
        arr = new int[maxSize];
        front = 0; //指向队列头部
        rear = 0;//指向队列尾部的后一个
    }

    public boolean isFull() {
        return (rear + 1) % maxSize == front;
    }

    public boolean isEmpty() {
        return front == rear;
    }

    public void addQueue(int data) {
        if (isFull()) {
            System.out.println("队列满");
            return;
        }
        arr[rear] = data;
        rear++;
        rear %= maxSize;
    }

    public int getQueue() {
        if (isEmpty()) {
            throw new RuntimeException("队列空");
        }
        int value = front;
        front++;
        front %= maxSize;
        return arr[value];
    }

    public void showQueue() {
        if (isEmpty()) {
            System.out.println("队列空");
            return;
        }
        // i < front+队列中元素个数（(rear+maxSize-front)%maxSize）
        for (int i = front; i < front + size(); i++) {  
            System.out.printf("arr[%d]=%d\n", i % maxSize, arr[i % maxSize]);
        }
    }

    public int size() {
        return (rear + maxSize - front) % maxSize;
    }

    //显示队列头部数据
    public int headqueue() {
        if (isEmpty()) {
            throw new RuntimeException("队列空");
        }
        return arr[front];
    }
}
```





## 2.链表

![image-20210322165146288](Data structure and algorithm.assets/image-20210322165146288.png)

### 2.1单链表

结点类（每个对象就是一个结点）

```java
class HeroNode {
    public int no;
    public String name;
    public String nickName;
    public HeroNode next;

    public HeroNode(int no, String name, String nickName) {
        this.no = no;
        this.name = name;
        this.nickName = nickName;
    }
}
```



管理结点的类（添加，删除，查找等等）

```java
class SingleLinkedList {
    //头结点，不存放数据
    private HeroNode head = new HeroNode(0, "", "");
    int nodeNum = 0;
    
    //添加结点到单向链表,使用一个辅助结点（头结点不能动）
    //按序号添加
    public void add(HeroNode heroNode) {
        HeroNode temp = head;
        while (true) {
            if (heroNode.no == temp.no) {       //已经有该序号
                System.out.println("已有此人");
                break;
            } else if (temp.next == null) {     //temp后为空直接添加
                temp.next = heroNode;
                break;
            } else if (heroNode.no < temp.next.no) {
                //向后遍历，遇见no比temp.next小的停止，插入temp与temp.next之间
                heroNode.next = temp.next;
                temp.next = heroNode;
                break;
            }
            temp = temp.next;
        }
    }
```



单链表反转

- 先定义一个新结点  private HeroNode reverseHead = new HeroNode(0, "", "")；

- 从头到尾遍历原来的链表，每遍历一个，将其插入reverseHead后；

- head.next = reverseHead。

```java
public static void reverse(HeroNode head) {
    if (head.next == null || head.next.next == null) {
        return;
    }
    HeroNode reverseHead =new HeroNode(0,"","");
    HeroNode temp = head.next;
    HeroNode temp1 = null;
    while (temp != null) {
        //for (int i = 0; i < getNodeNum(head); i++) {
        temp1 = temp.next;
        temp.next = reverseHead.next;
        reverseHead.next = temp;
        temp = temp1;
    }
    head.next = reverseHead.next;
}
```



逆序打印单链表

- 创建一个栈，将各结点压入栈，先进后出
- 遍历这个栈然后输出

```java
public static void reversePrint(HeroNode head) {
    if (head.next == null || head.next.next == null) {
        return;
    }
    //创建一个栈，将各结点压入栈，先进后出
    Stack<HeroNode> stack = new Stack<HeroNode>();
    HeroNode cur = head.next;
    while (cur != null) {
        stack.push(cur);    //相当于add
        cur = cur.next;
    }

    while (stack.size() > 0) {
        System.out.println(stack.pop());    //重写了toString方法
    }
}
```



### 2.2双向链表

每个结点除了next对象属性还有pre对象属性，指向前一个结点；

双向链表可以向前或者向后查找，可以自我删除。



### 2.3Josephu问题

设编号为 1，2，… n 的 n 个人围坐一圈，约定编号k（1<=k<=n）的人从 1 开始报数，数到m 的那个人出列，它的下一位又从 1 开始报数，数到 m 的那个人又出列，依次类推，直到所有人出列为止，由此产生一个出队编号的序列。

用不带头结点的单向循环链表解决。

- 结点类：get/setnext
- 管理结点的类：first指向第一个结点
- 构建一个单向环形链表：先创建第一个结点，让first指向他形成环形，每加入一个保持环形。
- 出圈：创建一个helper先遍历指向链表最后一个结点，当helper=first即到最后一个出圈的。





## 3.栈

stack，先进后出。

### 3.1数组模拟栈

```java
class ArrayStack {
    private int maxSize;
    private int[] stack;
    private int top = -1;   //栈顶

    public ArrayStack(int maxSize) {
        this.maxSize = maxSize;
        stack = new int[this.maxSize];
    }

    //栈满
    public boolean isFull() {
        return top == maxSize - 1;
    }

    //栈空
    public boolean isEmpty() {
        return top == -1;
    }

    //入栈
    public void push(int value) {
        if (isFull()) {
            System.out.println("栈满");
            return;
        }
        top++;
        stack[top] = value;
    }

    //出栈
    public int pop() {
        if (isEmpty()) {
            throw new RuntimeException("栈空");
        }
        int value = stack[top];
        top--;
        return value;
    }

    //遍历栈(从栈顶开始显示数据)
    public void list() {
        if (isEmpty()) {
            System.out.println("栈空");
            return;
        }
        for (int i = top; i >= 0; i--) {
            System.out.printf("stack[%d]=%d\n", i, stack[i]);
        }
    }
}
```

### 3.2栈实现中缀表达式计算器

- Stack类继承于Vector类，Vector：作为List接口的古老实现类；线程安全的，效率低；底层使用Object[] elementData存储。

- 中缀表达式：即常见的表达式：3+2*6-2

- 思路：

  1. 将String类型的字符串表达式分隔开存入一个List（多位数用String.match(正则表达式)）

  2. 创建一个数栈和符号栈，遍历这个List

  3. 发现数字直接入数栈

  4. 发现符号分两种情况：

     若当前符号栈为空或当前操作符大于栈顶操作符的优先级，直接入栈

     若当前操作符小于或等于栈顶操作符的优先级，从数栈pop出两个数，从符号栈pop出一个符号，进行计算，结果入数栈，再将当前操作符入符号栈（栈顶元素-或/次顶元素）。

  5. 当表达式遍历完，顺序从数栈或者符号栈取出数和符号计算，结果入数栈，最后得到结果

```java
public class Calculator {
    public static void main(String[] args) {
        String expression = "30+2*6-2";

        //一个数字栈，一个符号栈
        ArrayStack numStack = new ArrayStack(10);
        ArrayStack operStack = new ArrayStack(10);
        //定义需要的相关变量
        int index = 0;  //用于扫描
        int num1 = 0;
        int num2 = 0;
        int oper = 0;
        int res = 0;
        char ch = ' ';  //将每次扫描得到的char保存到ch
        String keepNum = "";    //用于拼接多位数

        while (true) {
            ch = expression.substring(index, index + 1).charAt(0);
            if (operStack.isOper(ch)) {     //如果是是运算符
                if (!operStack.isEmpty()) {  //符号栈不为空
                    //当前操作符优先级小于或者等于栈中的，需要从数字栈pop出两个数
                    //再从符号栈中pop出一个符号，进行计算，结果入数字栈，然后将当前操作符入栈
                    if (operStack.Priority(ch) <= operStack.Priority(operStack.peek())) {
                        num1 = numStack.pop();
                        num2 = numStack.pop();
                        oper = operStack.pop();
                        res = numStack.cal(num1, num2, oper);
                        numStack.push(res);
                        operStack.push(ch);
                    } else {
                        //当前操作符优先级大于栈中的，直接入符号栈
                        operStack.push(ch);
                    }
                } else {                    //符号栈为空直接入栈
                    operStack.push(ch);
                }
            } else {
                //如果是数，判断是否是多位数
                //需要向后再看
                //需要定义一个字符串变量用于拼接
                keepNum += ch;

                //当ch是最后一位直接入栈
                if (index == expression.length() - 1) {
                    numStack.push(ch - 48);
                }else {
                    //多次拼接多位数的index++在while外
                    if (operStack.isOper(expression.substring(index + 1, index + 2).charAt(0))) {
                        //如果后一位是符号，入栈
                        numStack.push(Integer.parseInt(keepNum));
                        keepNum = "";   //keepNum清空
                    }
                }

            }
            //让index+1，并判断是否到expression的最后
            index++;
            if (index >= expression.length()) {
                break;
            }
        }

        //扫描完毕后，就顺序的从数字栈和符号栈pop出相应的数和符号，并运行
        while (true) {
            //当符号栈为空，数字栈只有一个数字（结果）
            if (operStack.isEmpty()) {
                break;
            }
            num1 = numStack.pop();
            num2 = numStack.pop();
            oper = operStack.pop();
            res = numStack.cal(num1, num2, oper);
            numStack.push(res);
        }

        System.out.printf("表达式%s = %d", expression, numStack.pop());
    }

}

class ArrayStack {
    private int maxSize;
    private int[] stack;
    private int top = -1;   //栈顶

    public ArrayStack(int maxSize) {
        this.maxSize = maxSize;
        stack = new int[this.maxSize];
    }

    //栈满
    public boolean isFull() {
        return top == maxSize - 1;
    }

    //栈空
    public boolean isEmpty() {
        return top == -1;
    }

    //入栈
    public void push(int value) {
        if (isFull()) {
            System.out.println("栈满");
            return;
        }
        top++;
        stack[top] = value;
    }

    //出栈
    public int pop() {
        if (isEmpty()) {
            throw new RuntimeException("栈空");
        }
        int value = stack[top];
        top--;
        return value;
    }

    //遍历栈(从栈顶开始显示数据)
    public void list() {
        if (isEmpty()) {
            System.out.println("栈空");
            return;
        }
        for (int i = top; i >= 0; i--) {
            System.out.printf("stack[%d]=%d\n", i, stack[i]);
        }
    }

    //返回运算符的优先级，优先级使用数字表示
    //数字越大，优先级越高
    public int Priority(int oper) {
        if (oper == '*' || oper == '/') {
            return 1;
        } else if (oper == '+' || oper == '-') {
            return 0;
        } else {
            return -1;
        }
    }

    //判断是不是运算符
    public boolean isOper(char val) {
        return val == '+' || val == '-' || val == '*' || val == '/';
    }

    //计算方法
    public int cal(int num1, int num2, int oper) {
        int res = 0;
        switch (oper) {
            case '+':
                res = num1 + num2;
                break;
            case '-':
                res = num2 - num1;
                break;
            case '*':
                res = num1 * num2;
                break;
            case '/':
                res = num2 / num1;
                break;
        }
        return res;
    }

    //看一眼栈顶的值，不弹出
    public int peek() {
        return stack[top];
    }
}
```

### 3.2栈实现逆波兰计算器

- 前缀表达式：又称**波兰式**，前缀表达式的运算符位于操作数之前，(3+4)×5-6 对应的前缀表达式就是 - × + 3 4 5 6
- 计算器求值方式：**从右至左**扫描表达式，遇到数字时，将数字压入堆栈，遇到运算符时，弹出栈顶的两个数，用运算符对它们做相应的计算（栈顶元素 和 次顶元素），并将结果入栈；重复上述过程直到表达式最左端，最后运算得出的值即为表达式的结果（栈顶元素-或/次顶元素）
- 后缀表达式：又称**逆波兰表达式**,与前缀表达式相似，只是运算符位于操作数之后， (3+4)×5-6 对应的后缀表达式就是 3 4 + 5 × 6 –
- 计算器求值方式：**从左至右**扫描表达式，遇到数字时，将数字压入堆栈，遇到运算符时，弹出栈顶的两个数，用运算符对它们做相应的计算（次顶元素 和 栈顶元素），并将结果入栈；重复上述过程直到表达式最右端，最后运算得出的值即为表达式的结果（次顶元素-或/栈顶元素）



```java
public static void main(String[] args) {
    //"3 4 + 5 * 6"  =  (3+4)*5-6
    String suffixExpression = "3 4 + 5 * 20 -";

    List<String> rpnList = getListString(suffixExpression);
    System.out.println("rpnList=" + rpnList);

    System.out.println(calculate(rpnList));
}

//将suffixExpression按空格分割存入数组
public static List<String> getListString(String suffixExpression) {
    String[] split = suffixExpression.split(" ");
    List<String> list = new ArrayList<String>();
    for (String ele : split) {
        list.add(ele);
    }
    return list;

}

public static int calculate(List<String> ls) {
    //创建一个栈
    Stack<String> stack = new Stack<String>();
    //遍历ls
    for (String item : ls) {
        //使用正则表达式取出数
        if (item.matches("\\d+")) { //匹配的是多位数
            stack.push(item);
        } else {
            //pop出两位数，运算并入栈
            int num2 = Integer.parseInt(stack.pop());
            int num1 = Integer.parseInt(stack.pop());
            int res = 0;
            if (item.equals("+")) {
                res = num1 + num2;
            } else if (item.equals("-")) {
                res = num1 - num2;
            } else if (item.equals("*")) {
                res = num1 * num2;
            } else if (item.equals("/")) {
                res = num1 / num2;
            }
            stack.push(String.valueOf(res));
            //stack.push(res + "");
        }
    }

    //最后留在里面的就是结果
    return Integer.parseInt(stack.pop());
}
```

### 3.4中缀表达式转后缀表达式

1. 初始化两个栈，运算符栈s1与储存中间结果的栈s2

2. 从左到右扫描中缀表达式

3. 遇到操作数压s2

4. 遇到运算符，比较其与s1栈顶的运算符的优先级：

   若s1为空或者栈顶为"(",直接入栈

   若优先级比栈顶运算符高，直接入栈

   否则，将s1栈顶的运算符弹出压入到s2，再次重复与s1栈顶的比较操作

5. 遇到括号：

   左括号"("直接压入s1

   右括号")"时，依次弹出s1的运算符并压入s2，直到遇到左括号，将这对括号舍弃

6. 扫描完毕后，将s1中剩余的运算符依次弹出并压入s2

7. 依次弹出s2的元素，结果的逆序即为对应的后缀表达式

如将  **"1+((2+3)*4)-5"**  转化为  **"1 2 3 + 4 * + 5 -"**

```java
public class InvPolandNotation {
    public static void main(String[] args) {
        String str = "1+((2+3)*4)-5";
        List<String> list = getList(str);
        System.out.println(list);
        String trans = trans(list);
        System.out.println(trans);
    }

    //将中缀表达式转化为List储存(包含多位数)
    public static List<String> getList(String str){
        String numKeep = "";
        List<String> list = new ArrayList<>();
        for (int i = 0; i < str.length(); i++) {
            if (str.charAt(i) < 48 || str.charAt(i) > 57) {
                list.add("" + str.charAt(i));
            } else {
                while (i < str.length() && (str.charAt(i) >= 48 && str.charAt(i) <= 57)) {
                    numKeep = numKeep + str.substring(i, i + 1);
                    i++;
                }
                list.add(numKeep);
                numKeep = "";
                i--;    //重要
            }
        }
        return list;
    }

    public static String trans(List<String> list) {
        Stack<String> s1 = new Stack<>();
        Stack<String> s2 = new Stack<>();
        String invPolandNotation = "";
        for (String str : list) {
            if (str.equals("+") || str.equals("-") || str.equals("*") || str.equals("/")) {
                while (!(s1.isEmpty()) && !(s1.peek().equals("(")) && !(priority(str) > priority(s1.peek()))) {
                    s2.push(s1.pop());
                }
                s1.push(str);
            } else if (str.equals("(")) {
                s1.push(str);
            } else if (str.equals(")")) {
                while (!(s1.peek().equals("("))) {
                    s2.push(s1.pop());
                }
                s1.pop();
            } else {
                s2.push(str);
            }
        }

        while (!s1.isEmpty())
            s2.push(s1.pop());

        //反转
        while (!s2.empty()) {
            s1.push(s2.pop());
        }

        while (!s1.empty()) {
            invPolandNotation += s1.pop();
            invPolandNotation += " ";
        }

        int length = invPolandNotation.length();
        invPolandNotation = invPolandNotation.substring(0, length - 2);
        return invPolandNotation;
    }

    //返回运算符的优先级，优先级使用数字表示
    //数字越大，优先级越高
    public static int priority(String oper) {
        if (oper == "*" || oper == "/") {
            return 1;
        } else if (oper == "+" || oper == "-") {
            return 0;
        } else {
            return -1;
        }
    }
}
```

完整的逆波兰计算器包括：

1. 支持+-*/()
2. 支持多位数、小数
3. 过滤任何空白字符，包括换行符，制表符等等



## 4.递归

调用规则：1.当程序执行一个方法时，就会开辟一个独立的空间（在栈内）。

2.每个空间的数据（局部变量）是独立的，如果方法中使用的是引用数据类型，则共享该数据。

3.当一个方法执行完毕，或者遇到return，就会返回，遵守谁调用，就将结果返回给谁。

### 4.1迷宫问题



```java
public class MiGong {
    public static void main(String[] args) {
        //二维数组模拟迷宫
        int[][] map = new int[8][7];
        //1-墙、挡板
        for (int i = 0; i < 7; i++) {
            map[0][i] = 1;
            map[7][i] = 1;
        }
        for (int i = 0; i < 7; i++) {
            map[i][0] = 1;
            map[i][6] = 1;
        }
        map[3][1] = 1;
        map[3][2] = 1;

        setWay(map, 1, 1);

        for (int i = 0; i < 8; i++) {
            for (int j = 0; j < 7; j++) {
                System.out.print(map[i][j]+" ");
            }
            System.out.println();
        }
    }

    /**
    * @Description: map[6][5]表示通路找到，当map[i][j]为0则未走过，
     * 为2则通路走过，为3表示走过但是走不通
     * 策略：下-右-上-左，走不通再回溯
    * @Param: [int[][] 表示地图, int 位置, int]
    * @return: boolean
    */
    public static boolean setWay(int[][] map, int i, int j) {
        if (map[6][5] == 2) {   //通路已经找到
            return true;
        } else {
            if (map[i][j] == 0) {   //如果当前这个点还没有走过
                map[i][j] = 2;
                if (setWay(map, i + 1, j)) {        //向下走
                    return true;
                } else if (setWay(map, i, j + 1)) { //向右走
                    return true;
                } else if (setWay(map, i - 1, j)) { //向上走
                    return true;
                } else if (setWay(map, i, j - 1)) { //向左走
                    return true;
                } else {    //说明该点走不通
                    map[i][j] = 3;
                    return false;
                }
            } else {    //map[i][j] != 0
                return false;
            }
        }
    }
}
```

### 4.2八皇后问题

8x8棋盘上，任意两个皇后都不能处于同一行、同一列或者同一斜线上（暴力遍历回溯）。

```java
public class Queue8 {

    int max = 8;
    int[] arr = new int[max];
    int methodNum = 0;

    public static void main(String[] args) {
        Queue8 queue8 = new Queue8();
        queue8.place(0);
        System.out.println(queue8.methodNum);
    }

    //放置第n个皇后
    private void place(int n) {
        if (n == max) {
            print();
            methodNum += 1;
            return;
        }
        //依次放入皇后
        //i表示放到哪一列
        for (int i = 0; i < max; i++) {
            //先把当前的皇后，放到该行第一列
            arr[n] = i;
            if (judge(n)) { //不冲突
                place(n + 1);
            }//冲突，继续执行arr[n]=i;
        }
    }

    //输出皇后摆放位置
    private void print() {
        for (int i = 0; i < arr.length; i++) {
            System.out.print(arr[i]+" ");
        }
        System.out.println();
        System.out.println();
    }

    //判断是否冲突（同一行，同一列，同一斜线）
    //n-第n个皇后
    private boolean judge(int n) {
        for (int i = 0; i < n; i++) {
            if (arr[i] == arr[n] || Math.abs(n - i) == Math.abs(arr[n] - arr[i])) {
                return false;
            }
        }
        return true;
    }
}
```



## 5.排序

### 5.1时间复杂度

- 时间频度：一个算法中语句的执行次数，记为T(n)

- 时间复杂度：O(f(n))，f(n)是T(n)的同量级函数

  用常数1代替T(n)中的加法常数

  只保留T(n)中最高阶项

  去掉T(n)中最高阶项的系数

- 常见的时间复杂度（一般讨论最坏情况下的时间复杂度）：

  1）常数阶O(1)：没有循环等复杂结构

  2）对数阶O(log2 n)：

```java
int i=1;
while(i<n) {
	i = i * 2;
}
```

​		3）线性阶O(n)：

```java
for(int i=1; i<n; i++) {
	sum+=i;
}
```

​	4）线性对数阶O(nlog N)：对数阶外嵌套一个线性阶

​	5）平方阶O(n^2)：线性阶外嵌套一个线性阶

​	6）立方阶和K次方阶同理

- 空间复杂度：算法所耗费的存储空间（不是主要的）



### 5.2冒泡排序BubbleSorting

```java
public class BubbleSort {
    public static void main(String[] args) {

        int arr[] = new int[10000];
        for (int i = 0; i < arr.length; i++) {
            arr[i] = (int) (Math.random() * 10000);
        }
        int temp;

        for (int i = 0; i < arr.length - 1; i++) {
            boolean flag = true;    //可提前结束冒泡

            for (int j = 0; j < arr.length - 1 - i; j++) {
                if (arr[j] < arr[j + 1]) {
                    temp = arr[j];
                    arr[j] = arr[j + 1];
                    arr[j + 1] = temp;
                    flag = false;
                }
            }
            if (flag) {
                break;
            }
        }
        System.out.println(Arrays.toString(arr));
    }
}
```



### 5.3简单选择排序SelectSorting

- 第一次从arr[0]到arr[n-1]中选取最小值后，再与arr[0]交换
- 第二次从arr[1]到arr[n-1]中选取最小值后，再与arr[1]交换
- 以此类推
- 速度比冒泡排序快

```java
public class SelectSorting {
    public static void main(String[] args) {
        int arr[] = new int[10000];
        for (int i = 0; i < arr.length; i++) {
            arr[i] = (int) (Math.random() * 10000);
        }
        int temp = 0;

        for (int i = 0; i < arr.length - 1; i++) {
            int minSign = i;

            for (int j = i; j < arr.length; j++) {
                if (arr[j] > arr[minSign]) {
                    minSign = j;
                }
            }

            temp = arr[i];
            arr[i] = arr[minSign];
            arr[minSign] = temp;
        }

        System.out.println(Arrays.toString(arr));
    }
}
```



### 5.4直接插入排序InsertSorting

将n个待排元素看成一个有序表和一个无序表，刚开始有序表只有一个元素而无序表有n-1个元素，依次取出无序表的元素并与有序表元素比较后插入

```java
public class InsertSorting {
    public static void main(String[] args) {
        int arr[] = new int[10000];
        for (int i = 0; i < arr.length; i++) {
            arr[i] = (int) (Math.random() * 10000);
        }
        int temp;

        for (int i = 1; i < arr.length; i++) {
            temp = arr[i];
            int index = i - 1;  //index+1为要插入的有序表的位置
            while (index >= 0 && temp < arr[index]) {
                arr[index + 1] = arr[index];
                index--;
            }
            arr[index + 1] = temp;
        }

        System.out.println(Arrays.toString(arr));
    }
}
```



### 5.5希尔排序ShellSorting

是一种插入排序（缩小增量排序），把数据按下标的一定增量分组，对每组使用直接插入排序，随着增量减小，每组包含的数据越多，当增量减少至1时，整个数据被分为一组（再进行一次排序），完成排序，**是对直接插入排序的改进**

分为：

- 交换法：每组内部采用冒泡排序（速度很慢）
- 移动法：每组内部采用插入排序

![image-20210511152306229](Data structure and algorithm.assets/image-20210511152306229.png)

```java
public class ShellSorting {
    public static void main(String[] args) {
        int arr[] = new int[10000];
        for (int i = 0; i < arr.length; i++) {
            arr[i] = (int) (Math.random() * 10000);
        }
        int temp = 0;
        

        //gap代表分组步长(初始组数为总数的一半，组数一直除2直至只有一组)
        for (int gap = arr.length / 2; gap > 0; gap /= 2) {
            //遍历组里所有元素(所有组的无序表类似冒泡的操作)
            for (int i = gap; i < arr.length; i++) {
                //每遍历到组内的一个元素，进行一次冒泡操作
                //每次操作将当前的最小值冒泡到组内最前端
                //遍历完后组内整个冒泡操作也做完变成有序的了
                for (int j = i - gap; j >= 0; j -= gap) {
                    if (arr[j] > arr[j + gap]) {
                        temp = arr[j];
                        arr[j] = arr[j + gap];
                        arr[j + gap] = temp;
                    }
                }
            }
        }

        //gap代表分组步长(初始组数为总数的一半，组数一直除2直至只有一组)
        for (int gap = arr.length / 2; gap > 0; gap /= 2) {
            //遍历所有组(所有组的无序表进行插入排序)
            for (int i = gap; i < arr.length; i++) {
                int j = i;
                temp = arr[i];
                while (j - gap >= 0 && temp < arr[j - gap]) {
                    //移动
                    arr[j] = arr[j - gap];
                    j -= gap;
                }
                //退出while循环时，找到temp的插入位置
                arr[j] = temp;
            }
        }

        System.out.println(Arrays.toString(arr));
    }
}
```



### 5.6快速排序QuickSorting

- 选取一个基准值，经过一次排序，基准值左边的数据比基准值都要小或者相等，基准值右边的数据比基准值都要大或者相等
- 一次排序中，基准值的值不变，但位置可能会发生变化
- 一次排序完成后，该基准值不再参与排序，左边与右边进行迭代递归
- ![image-20210511152655933](Data structure and algorithm.assets/image-20210511152655933.png)

```java
public class QuickSorting {
    public static void main(String[] args) {
        //int arr[] = new int[10000];
        //for (int i = 0; i < arr.length; i++) {
        //    arr[i] = (int) (Math.random() * 10000);
        //}
        int arr[] = {1, 2, 3, 10, 8, 9, 8, 18, 5};
        int temp = 0;
        quicksorting(arr,0, arr.length-1);
        System.out.println(Arrays.toString(arr));
    }

    public static void quicksorting(int[] arr, int left, int right) {
        int l = left;   //左下标
        int r = right;  //右下标
        int temp;
        int pivot = arr[(left + right) / 2];    //中间值
        //while循环目的是让比pivot值小的(或等于)放到左边，大的(或等于)放到右边
        //与pivot值相同的不一定靠在一起
        while (l < r) {
            //在pivot的左边找到一个大于等于pivot的值
            //最坏情况是其本身
            while (arr[l] < pivot) {
                l += 1;
            }
            //在pivot的右边找到一个小于等于pivot的值
            //最坏情况是其本身
            while (arr[r] > pivot) {
                r -= 1;
            }
            //如果左右都没找到符合的值,提前跳出
            //即pivot左边全是小于其的值，右边全是大于其的值
            if (l == r) {
                break;
            }
            //找到了进行交换
            temp = arr[l];
            arr[l] = arr[r];
            arr[r] = temp;
            //下面两个if是防止有多个与pivot相等的值陷入死循环
            //若只有一个pivot，交换也没关系，只是改变了pivot的位置
            //如果交换完后发现arr[l] == pivot，r前移，即之前的arr[r] == pivot
            if (arr[l] == pivot) {
                r -= 1;
            }

            //如果交换完后发现arr[r] == pivot，l后移，
            if (arr[r] == pivot) {
                l += 1;
            }
        }
        //如果l == r（也必然是l==r上面的循环才会结束）；
        //必须l++，r--，否则出现栈溢出（已经排好的pivot值不参与接下来的排序了）
        //此时l == r也就是pivot的下标，但可能较前发生了变化（有多个值与pivot相等）
        if (l == r) {
            l++;
            r--;
        }
        //向左递归
        if (left < r) { //递归终止条件
            quicksorting(arr, left, r);
        }
        //向右递归
        if (right > l) {    //递归终止条件
            quicksorting(arr, l, right);
        }
    }
}
```



### 5.7归并排序MergeSorting

分治思想（divide and conquer）

![image-20210803204705136](Data structure and algorithm.assets/image-20210803204705136.png)

```java
public class MergeSorting {
    public static void main(String[] args) {
        int arr[] = new int[10000];
        for (int i = 0; i < arr.length; i++) {
            arr[i] = (int) (Math.random() * 10000);
        }
        int[] temp = new int[arr.length];
        mergeSort(arr, 0, arr.length - 1, temp);

        System.out.println(Arrays.toString(arr));
    }

    public static void mergeSort(int[] arr, int left, int right, int[] temp) {
        if (left < right) { //（终止条件）直至全部分成只包含一个数据的有序序列
            int mid = (left + right) / 2;   //中间索引
            //向左递归分解
            mergeSort(arr, left, mid, temp);
            //向右递归分解
            mergeSort(arr, mid+1, right, temp);
            //最后一次将两个元素向左向右分解开，无法再递归分解，开始从栈顶开始回溯合并
            //merge()的参数即为当前调用mergeSort()的参数
            merge(arr, left, mid, right, temp);
        }
    }
    

    /**
     * @param arr: 排序的原始数组
     * @param left: 左边有序序列的初始索引
     * @param mid: 中间索引(左边有序序列的末尾)
     * @param right: 右边索引
     * @param temp: 做中转的数组
     * @return int
     * @Description: 合并的方法
     */
    public static void merge(int[] arr, int left, int mid, int right, int[] temp) {
        int i = left;   //左边有序序列的初始索引
        int j = mid + 1;    //右边有序序列的初始索引
        int t = left;      //temp数组的当前索引
        //（一）先把左右两边的数据按大小顺序填充到temp数组
        //直到左右两边有序序列有一边处理完毕为止
        //极限情况，一个数据也是有序序列
        while (i <= mid && j <= right) {
            if (arr[i] <= arr[j]) {
                temp[t] = arr[i];
                t++;
                i++;
            } else {
                temp[t] = arr[j];
                t++;
                j++;
            }
        }
        //（二）把有剩余数据的一边依次填充到temp数组
        while (i <= mid) {  //左边有序序列还有剩余元素
            temp[t] = arr[i];
            t++;
            i++;
        }
        while (j <= right) { //右边有序序列还有剩余元素
            temp[t] = arr[j];
            t++;
            j++;
        }
        //（三）将temp元素拷贝到arr(并不是每次拷贝所有)
        t = left;
        while (t <= right) {
            arr[t] = temp[t];
            t++;
        }
    }
}
```



### 5.8基数排序RadixSorting

- 属于分配式排序（distribution sort），是桶排序的扩展，属于效率高的稳定性排序法
- 经典的牺牲空间换时间的算法，占用内存大，容易造成OutOfMemoryError
- 基本思想：将所有待比较的数据统一为同样的数位长度，数位较短的数前面补0，然后，从最低位开始，依次进行一次排序，一直到最高位排序完成，数列就变成一个有序序列。
- 有负数的数组需要对其改进

![image-20210512125946805](Data structure and algorithm.assets/image-20210512125946805.png)

![image-20210512125905584](Data structure and algorithm.assets/image-20210512125905584.png)

```java
public class RadixSorting {
    public static void main(String[] args) {
        int arr[] = new int[10000];
        for (int i = 0; i < arr.length; i++) {
            arr[i] = (int) (Math.random() * 10000);
        }
        radixSort(arr);
        System.out.println(Arrays.toString(arr));
    }

    public static void radixSort(int[] arr) {
        //先得到最大值的位数
        int max = arr[0];
        for (int i = 1; i < arr.length; i++) {
            if (arr[i] > max) {
                max = arr[i];
            }
        }
        int maxLength = (max + "").length();
        //定义一个二维数组，表示10个桶(0~9)，一个一位数组则是一个桶
        //空间换时间的经典算法
        int[][] bucket = new int[10][arr.length];
        //一个一维数组记录各个桶每次放入数据的个数
        int[] bucketEleCounts = new int[10];
        //开始对所有数位进行基数排序
        for (int i = 0; i < maxLength; i++) {
            for (int j = 0; j < arr.length; j++) {
                int digitOfElement = arr[j] / (int) Math.pow(10, i) % 10;
                //放到对应的桶中
                bucket[digitOfElement][bucketEleCounts[digitOfElement]] = j;
                bucketEleCounts[digitOfElement]++;
            }
            int index = 0;  //arr数据的指示
            //按照桶的顺序，将数据放回原来数组
            for (int k = 0; k < bucket.length; k++) {
                for (int x = 0; x < bucketEleCounts[k]; x++) {
                    arr[index] = bucket[k][x];
                    index++;
                }
                //每个桶操作完后，记录桶内数据个数的值需要置0
                bucketEleCounts[k] = 0;
            }
        }
    }
}
```



### 5.9常用排序算法总结和对比

![image-20210729094323286](Data structure and algorithm.assets/image-20210729094323286.png)

![image-20210512132412367](Data structure and algorithm.assets/image-20210512132412367.png)

**相关术语解释：**

​	1)**稳定**：如果a原本在b前面，而a=b，排序之后a仍然在b的前面；

​	2)**不稳定**：如果a原本在b的前面，而a=b，排序之后a可能会出现在b的后面；

​	3)**内排序**：所有排序操作都在内存中完成；

​	4)**外排序**：由于数据太大，因此把数据放在磁盘中，而排序通过磁盘和内存的数据传输才能进行；

​	5)**时间复杂度：** 一个算法执行所耗费的时间。

​	6)**空间复杂度**：运行完一个程序所需内存的大小。

​	7)**n:** 数据规模

​	8)**k:** “桶”的个数

​	9)**In-place:**  不占用额外内存

​	10)**Out-place:** 占用额外内存



## 6.查找

### 6.1线性查找

- 有序数组或无序数组都可使用
- 暴力遍历

```java
public class SeqSearch {
    public static void main(String[] args) {
        int arr[] = {1, 9, 11, -1, 34, 89};
        int index = seqSearch(arr, 11);
        if (index == -1) {
            System.out.println("NotFound！");
        } else {
            System.out.println("index = " + index);
        }
    }
    
    /**
     * @param arr: 
     * @param value: 
     * @return int: 
     * @Description: 查找到一个数据就返回
     */
    public static int seqSearch(int[] arr, int value) {
        //线性查找逐一比对
        for (int i = 0; i < arr.length; i++) {
            if (arr[i] == value) {
                return i;
            }
        }
        return -1;
    }
}
```

### 6.2二分查找

- 要求有序数组
- 递归查找

```java
public class BinarySearch {
    public static void main(String[] args) {
        int[] arr = {1, 6, 7, 9, 12, 16, 19, 23, 24, 28, 36, 39, 42};
        int index = binarySearch(arr, 0, arr.length - 1, 9);
        if (index == -1) {
            System.out.println("NotFound！");
        } else {
            System.out.println("index = " + index);
        }

    }

    public static int binarySearch(int[] arr, int left, int right, int value) {

        //递归终止条件
        if (left > right) {
            return -1;
        }
        
        int mid = (left + right) / 2;
        int midVal = arr[mid];
        if (value == midVal) {
            return mid;
        } else if (value < midVal) {
            return binarySearch(arr, left, mid - 1, value);
        } else {
            return binarySearch(arr, mid + 1, right, value);
        }
    }
}
```

- 有多个待查找的值时

```java
public class BinarySearch1 {
    public static void main(String[] args) {
        int[] arr = {1, 6, 7, 9, 12, 16, 19, 19, 19, 23, 24, 28, 36, 39, 42};
        ArrayList<Integer> integers = binarySearch1(arr, 0, arr.length - 1, 19);
        if (integers == null) {
            System.out.println("NotFound！");
        } else {
            System.out.println("index = " + integers.toString());
        }

    }

    public static ArrayList<Integer> binarySearch1(int[] arr, int left, int right, int value) {

        ArrayList<Integer> integers = new ArrayList<>();
        //递归终止条件
        if (left > right) {
            return null;
        }

        int mid = (left + right) / 2;
        int midVal = arr[mid];
        if (value == midVal) {
            int temp = mid - 1;
            while (true) {
                if (temp < 0 || arr[temp] != value) {
                    break;
                }
                integers.add(temp);
                temp--;
            }
            integers.add(mid);
            temp = mid + 1;
            while (true) {
                if (temp > arr.length - 1 || arr[temp] != value) {
                    break;
                }
                integers.add(temp);
                temp++;
            }
            return integers;
        } else if (value < midVal) {
            return binarySearch1(arr, left, mid - 1, value);
        } else {
            return binarySearch1(arr, mid + 1, right, value);
        }
    }
}
```

### 6.3插值查找

- 插值查找算法类似于二分查找，不同的是插值查找每次从**自适应**mid处开始查找
- 对于数据量大，关键字分布比较均匀的查找表来说，采用插值查找速度较快，当不均匀的情况下，不一定比二分查找好
- int mid = left + (right – left) * (findVal – arr[left]) / (arr[right] – arr[left])

```java
public class InsertValueSearch {
    public static void main(String[] args) {
        int[] arr = {1, 6, 7, 9, 12, 16, 19, 23, 24, 25, 28, 36, 39, 42};
        int index = insertValueSearch(arr, 0, arr.length - 1, 9);
        if (index == -1) {
            System.out.println("NotFound！");
        } else {
            System.out.println("index = " + index);
        }

    }

    public static int insertValueSearch(int[] arr, int left, int right, int value) {

        //递归终止条件
        if (left > right) {
            return -1;
        }
        //插值查找
        int mid = left + (value - arr[left]) / (arr[right] - arr[left]) * (right - left);
        int midVal = arr[mid];
        if (value == midVal) {
            return mid;
        } else if (value < midVal) {
            return insertValueSearch(arr, left, mid - 1, value);
        } else {
            return insertValueSearch(arr, mid + 1, right, value);
        }
    }
}
```

### 6.4斐波那契查找

- 核心思想与二分查找类似，mid取值不同，可用非递归方法实现（while+手动改left与right的值）
- 由F[k]=F[k-1]+F[k-2]可得：（F[k]-1）=（F[k-1]-1）+（F[k-2]-1）+1，只要顺序表的长度为**F[k]-1**，则可以将该表分成长度为**F[k-1]-1**和**F[k-2]-1**的两段，**mid=low+F(k-1)-1**      
- 顺序表长度n不一定刚好等于F[k]-1（F[k]-1恰好大于或等于n即可），所以需要将原来的顺序表长度n增加至F[k]-1，用顺序表最后一位的数值进行填充

```java
public class FibonacciSearch {
    static int maxSize = 20;
    public static void main(String[] args) {
        int[] arr = {1, 6, 7, 9, 12, 16, 19, 23, 24, 25, 28, 36, 39, 42};
        int index = fibSearch(arr, 19);
        if (index == -1) {
            System.out.println("NotFound！");
        } else {
            System.out.println("index = " + index);
        }
    }

    //非递归方法得到一个斐波那契数列
    public static int[] fib() {
        int[] f = new int[maxSize];
        f[0] = 1;
        f[1] = 1;
        for (int i = 2; i < maxSize; i++) {
            f[i] = f[i - 1] + f[i - 2];
        }
        return f;
    }

    public static int fibSearch(int[] arr, int value) {
        int left = 0;
        int right = arr.length - 1;
        int k = 0;  //斐波那契数列分割数值的下标
        int mid;
        int[] fib = fib();  //获取斐波那契数列
        //获取到斐波那契数列分割数值的下标,该数值要刚好大于或等于数组长度
        while (right > fib[k] - 1) {
            k++;
        }
        //f[k]-1的值可能大于arr的长度，构造新的数组，不足的由arr数组最后一位填充
        int[] temp = Arrays.copyOf(arr, fib[k]);
        for (int i = right + 1; i < temp.length; i++) {
            temp[i] = arr[right];
        }
        //使用while循环找到value值
        //手动改left与right的值，加while循环来替代递归
        while (left <= right) {
            //mid将数组分成两部分
            //fib[k] - 1 = (fib[k - 1] - 1) + (fib[k - 2] - 1) + 1;
            mid = left + fib[k - 1] - 1;
            if (value < temp[mid]) {    //向左查找
                right = mid - 1;
                //fib[k-1]-1 = (fib[k-2]-1)+(fib[k-3]-1)+1;
                k--;
            } else if (value > temp[mid]) { //向右查找
                left = mid + 1;
                //fib[k-2]-1 = (fib[k-3]-1)+(fib[k-4]-1)+1;
                k -= 2;
            } else {    //value == temp[mid]
                //扩充过的数组需要确定返回哪个
                if (mid <= right) {
                    return mid;
                } else {
                    return right;
                }
            }
        }
        return -1;
    }
}
```



## 7.哈希表

散列表（Hash table，也叫哈希表），是根据关键码值(Key value)而直接进行访问的数据结构。也就是说，它通过把关键码值映射到表中一个位置来访问记录，以加快查找的速度。这个映射函数叫做**散列函数**，存放记录的数组叫做散列表。

```java
* @description: 哈希表(数组加链表)实现雇员系统
 */
public class HashTableDemo {
    public static void main(String[] args) {
        //创建哈希表
        HashTable hashTable = new HashTable(7);
        //简单菜单
        int key;
        Scanner scanner = new Scanner(System.in);
        Boolean flag = true;
        while (flag) {
            System.out.println("1:添加雇员");
            System.out.println("2.显示雇员");
            System.out.println("3.显示雇员");
            System.out.println("4.退出");
            key = scanner.nextInt();
            switch (key) {
                case 1:
                    System.out.println("输入id");
                    int id = scanner.nextInt();
                    System.out.println("输入名字");
                    String name = scanner.next();
                    hashTable.add(new Emp(id, name));
                    break;
                case 2:
                    hashTable.list();
                    break;
                case 3:
                    System.out.println("输入id");
                    int id1 = scanner.nextInt();
                    hashTable.find(id1);
                    break;
                case 4:
                    flag = false;
                    scanner.close();
                    break;
            }
        }
    }
}

//创建哈希表，管理多条链表
class HashTable {
    private EmpLinkedList[] empLinkedLists;


    public HashTable(int size) {
        this.empLinkedLists = new EmpLinkedList[size];
        //分别初始化各条链表
        for (int i = 0; i < size; i++) {
            empLinkedLists[i] = new EmpLinkedList();
        }
    }

    //添加雇员
    public void add(Emp emp) {
        //根据id和散列函数决定其放入哪条链表
        int NO = hashFun(emp.id);
        empLinkedLists[NO].add(emp);
    }

    //遍历
    public void list() {
        for (int i = 0; i < empLinkedLists.length; i++) {
            empLinkedLists[i].list();
        }
    }

    //查找
    public Emp find(int id) {
        int NO = hashFun(id);
        Emp emp = empLinkedLists[NO].find(id);
        if (emp == null) {
            System.out.println("没有找到");
        } else {
            System.out.printf("在第%d条链表中找到id=%d的职员", NO, id);
        }
        return emp;
    }

    //散列函数
    public int hashFun(int id) {
        return id % empLinkedLists.length;
    }
}

//一个雇员
class Emp {
    public int id;
    public String name;
    public Emp next; //默认为空

    public Emp(int id, String name) {
        this.id = id;
        this.name = name;
    }
}

//一条链表
class EmpLinkedList {
    private Emp head;   //表示一个雇员

    public void add(Emp emp) {
        if (head == null) {
            head = emp;
            return;
        }
        Emp curEmp = head;
        while (true) {
            if (curEmp.next == null) {
                break;
            }
            curEmp = curEmp.next;
        }
        curEmp.next = emp;
    }

    public void list() {
        if (head == null) {
            System.out.println("空");
            return;
        }
        Emp curEmp = head;
        while (true) {
            if (curEmp.next == null) {
                System.out.printf("id=%d name=%s\t", curEmp.id, curEmp.name);
                break;
            }
            curEmp = curEmp.next;
            System.out.println();
        }
    }

    public Emp find(int id) {
        if (head == null) {
            System.out.println("空");
            return null;
        }
        Emp curEmp = head;
        while (true) {
            if (curEmp.id == id) {
                break;
            }
            if (curEmp.next == null) {
                return null;
            }
            curEmp = curEmp.next;
        }
        return curEmp;
    }
}
```



## 8.树

### 8.1 二叉树

- 树相较于顺序存储和链式存储能提高数据**存储，读取**的效率

  - 顺序存储如数组：
    - 优点：通过下标方式访问元素，速度快。**对于有序数组**，还可使用二分查找提高检索速度。
    - 缺点：如果要检索具体某个值，或者插入值(按一定顺序)**会整体移动**，效率较低
  - 链式存储：
    - 优点：在一定程度上对数组存储方式有优化(比如：插入一个数值结点，只需要将插入结点，链接到链表中即可， 删除效率也很好)。
    - 缺点：在进行检索时，效率仍然较低，比如(检索某个值，需要从头结点开始遍历) 

  ![image-20210525190112848](Data structure and algorithm.assets/image-20210525190112848.png)

- 树的常用术语(结合示意图理解):

  1)结点	2)根结点	3)父结点	4)子结点	5)叶子结点 (没有子结点的结点)	6)结点的权(结点值)	7)路径(从root结点找到该结点的路线)	8)层	9)子树	10)树的高度(最大层数)	11)森林 :多颗子树构成森林

- 树有很多种，每个结点**最多只能有两个子结点**的一种形式称为二叉树。

- 如果该二叉树的**所有叶子结点都在最后一层**，并且结点总数= 2^n -1 , n 为层数，则我们称为满二叉树。

- 如果该二叉树的所有叶子结点都在最后一层或者倒数第二层，而且最后一层的叶子结点在左边连续，倒数第二层的叶子结点在右边连续，我们称为完全二叉树。

- ![image-20210728144043481](Data structure and algorithm.assets/image-20210728144043481.png)



### 8.2 二叉树的遍历、查找与删除结点

- 遍历

  前序遍历: **先输出父结点**，再遍历左子树和右子树（判断左右子树是否存在）

  中序遍历: 先遍历左子树，**再输出父结点**，再遍历右子树

  后序遍历: 先遍历左子树，再遍历右子树，**最后输出父结点**

  **小结**: 看输出父结点的顺序，就确定是前序，中序还是后序

  <img src="Data structure and algorithm.assets/image-20210728221811659.png" alt="image-20210728221811659" style="zoom:50%;" />

  前序遍历A-B-D-F-G-H-I-E-C

  中序遍历F-D-H-G-I-B-E-A-C

  后序遍历F-H-I-G-D-E-B-C-A

- 查找

  与前、中、后序遍历类似，但在递归时需注意找到目的结点后再return

- 删除

  （如果删除的结点是叶子结点，则删除该结点，如果删除的结点是非叶子结点，则删除该子树）

  二叉树是单向的，所以是判断当前结点的子结点是否是需要删除的结点

  如果当前结点的左子结点不为空，是需要删除的结点，则将其置空，结束递归，否则向左子树递归

  如果当前结点的右子结点不为空，是需要删除的结点，则将其置空，结束递归，否则向右子树递归

  root结点没有父结点，需要单独判断

```java
* @description: 二叉树前中后序遍历与查找、删除
 */
public class BinaryTreeDemo {
    public static void main(String[] args) {
        //先创建一个二叉树
        BinaryTree binaryTree = new BinaryTree();
        HeroNode heroNode = new HeroNode(1,"sd");
        HeroNode heroNode1 = new HeroNode(2,"nc");
        HeroNode heroNode2 = new HeroNode(3,"kd");
        HeroNode heroNode3 = new HeroNode(4,"ac");
        HeroNode heroNode4 = new HeroNode(5,"ef");

        binaryTree.setRoot(heroNode);
        heroNode.setLeft(heroNode1);
        heroNode.setRight(heroNode2);
        heroNode2.setLeft(heroNode3);
        heroNode2.setRight(heroNode4);

        System.out.println("preOrder");
        binaryTree.preOrder();

        System.out.println("preOrderSearch");
        HeroNode heroNode5 = binaryTree.postOrderSearch(3);
        if (heroNode5 == null) {
            System.out.println("NotFound");
        } else {
            System.out.println(heroNode5);
        }

        System.out.println("delNode");
        binaryTree.delNode(3);
        binaryTree.preOrder();

    }
}

//定义二叉树
class BinaryTree {
    private HeroNode root;

    //定义根结点
    public void setRoot(HeroNode root) {
        this.root = root;
    }

    //前序遍历
    public void preOrder() {
        root.preOrder();
    }
    //中序遍历
    public void midOrder() {
        root.midOrder();
    }
    //后序遍历
    public void postOrder() {
        root.postOrder();
    }

    //前序遍历查找
    public HeroNode preOrderSearch(int no) {
        return root.preOrderSearch(no);
    }
    //中序遍历查找
    public HeroNode midOrderSearch(int no) {
        return root.midOrderSearch(no);
    }
    //后序遍历查找
    public HeroNode postOrderSearch(int no) {
        return root.postOrderSearch(no);
    }

    //删除结点
    public void delNode(int no) {
        if (root != null) {
            //需要对root结点单独判断，因为其无父结点
            if (root.getNo() == no) {
                root = null;
            } else {
                root.delNode(no);
            }
        } else {
            System.out.println("Empty tree!");
        }
    }
}

//树的一个结点
class HeroNode {
    private int no;
    private String name;
    private HeroNode left;
    private HeroNode right;

    public HeroNode(int no, String name) {
        this.no = no;
        this.name = name;
    }

    public int getNo() {
        return no;
    }

    public void setLeft(HeroNode left) {
        this.left = left;
    }

    public void setRight(HeroNode right) {
        this.right = right;
    }

    @Override
    public String toString() {
        return "HeroNode{" +
                "no=" + no +
                ", name='" + name + '\'' +
                '}';
    }

    //前序遍历方法
    public void preOrder() {
        System.out.println(this);
        //递归向左
        if (this.left != null) {
            this.left.preOrder();
        }
        //递归向右
        if (this.right != null) {
            this.right.preOrder();
        }
    }

    //中序遍历方法
    public void midOrder() {
        //递归向左
        if (this.left != null) {
            this.left.midOrder();
        }
        System.out.println(this);
        //递归向右
        if (this.right != null) {
            this.right.midOrder();
        }
    }

    //后序遍历方法
    public void postOrder() {
        //递归向左
        if (this.left != null) {
            this.left.postOrder();
        }
        //递归向右
        if (this.right != null) {
            this.right.postOrder();
        }
        System.out.println(this);
    }

    //前序遍历查找
    public HeroNode preOrderSearch(int no) {
        if (this.no == no) {
            return this;
        }
        if (this.left != null) {
            //找到了再return
            if (this.left.preOrderSearch(no) != null) {
                return this.left.preOrderSearch(no);
            }
        }
        if (this.right != null) {
            return this.right.preOrderSearch(no);
        }
        return null;
    }

    //中序遍历查找
    public HeroNode midOrderSearch(int no) {
        if (this.left != null) {
            //如果左边没找到
            if (this.left.midOrderSearch(no) != null) {
                return this.left.midOrderSearch(no);
            }
        }
        if (this.no == no) {
            return this;
        }
        if (this.right != null) {
            return this.right.midOrderSearch(no);
        }
        return null;
    }

    //后序遍历查找
    public HeroNode postOrderSearch(int no) {
        if (this.left != null) {
            if (this.left.postOrderSearch(no) != null) {
                return this.left.postOrderSearch(no);
            }
        }
        if (this.right != null) {
            if (this.right.postOrderSearch(no) != null) {
                return this.right.postOrderSearch(no);
            }
        }
        if (this.no == no) {
            return this;
        }
        return null;
    }

    //删除结点
    public void delNode(int no) {
        if (this.left != null && this.left.no == no) {
            this.left = null;
            return;
        }
        if (this.right != null && this.right.no == no) {
            this.right = null;
            return;
        }
        //向左、右子树递归
        if (this.left != null) {
            this.left.delNode(no);
        }
        if (this.right != null) {
            this.right.delNode(no);
        }

    }
}
```



### 8.3 顺序存储二叉树

![image-20210526202744674](Data structure and algorithm.assets/image-20210526202744674.png)

- **数组存储方式**和**树**的存储方式可以相互转换，在遍历时也可以按照**前序遍历**，**中序遍历**和**后序遍历**的方式

- 顺序存储二叉树的**特点**：

  顺序二叉树通常只考虑完全二叉树

  第n个元素的左子结点为 2 * n + 1(编号)

  第n个元素的右子结点为 2 * n + 2(编号)

  第n个元素的父结点为 (n-1) / 2(编号)

  n : 表示二叉树中的第几个元素(按0开始编号)

```java
public class ArrBinaryTreeDemo {
    public static void main(String[] args) {
        int[] arr = {1, 2, 3, 4, 5, 6, 7};
        ArrBinaryTree arrBinaryTree = new ArrBinaryTree(arr);
        arrBinaryTree.preOrder();
    }
}

class ArrBinaryTree {
    private int[] arr;

    public ArrBinaryTree(int[] arr) {
        this.arr = arr;
    }

    /**
     * @param index: 数组的下标
     * @return void:
     * @Description: 前序递归遍历
     */
    //前序遍历
    public void preOrder(int index) {
        if (arr == null && arr.length == 0) {
            System.out.println("数组为空");
        }
        System.out.println(arr[index]);
        //向左递归遍历
        if ((index * 2 + 1) < arr.length) {
            preOrder(index * 2 + 1);
        }
        //向右递归遍历
        if ((index * 2 + 2 < arr.length)) {
            preOrder(index * 2 + 2);
        }
    }

    //重载（arrBinaryTree.preOrder();即开始遍历）
    public void preOrder() {
        this.preOrder(0);
    }
}
```



### 8.4 线索化二叉树

- n个结点的二叉链表中含有n+1 【 2n-(n-1)=n+1】 个空指针域。利用二叉链表中的空指针域，存放指向该结点在**某种遍历次序**下的**前驱**和**后继**结点的指针（这种附加的指针称为"线索"），**线索二叉树**(Threaded  BinaryTree)

- 因为线索化后，各个结点指向有变化，因此**原来的遍历方式不能使用**，各个结点可以**通过线型方式遍历**，因此**无需使用递归**方式，这样也提高了遍历的效率

- 中序遍历的结果：{8, 3, 10, 1, 14, 6}

  前序遍历的结果：{1, 3, 8, 10, 6, 14}

  后序遍历的结果：{6, 14, 10, 8, 3, 1}

![image-20210526203151815](Data structure and algorithm.assets/image-20210526203151815.png)

![image-20210526203207137](Data structure and algorithm.assets/image-20210526203207137.png)

```java
public class ThreadedBinaryTreeDemo {
    public static void main(String[] args) {
        HeroNode heroNode = new HeroNode(1,"sd");
        HeroNode heroNode1 = new HeroNode(3,"nc");
        HeroNode heroNode2 = new HeroNode(6,"kd");
        HeroNode heroNode3 = new HeroNode(8,"ac");
        HeroNode heroNode4 = new HeroNode(11,"ef");
        HeroNode heroNode5 = new HeroNode(14,"jm");
        BinaryTree binaryTree = new BinaryTree();
        binaryTree.setRoot(heroNode);
        heroNode.setLeft(heroNode1);
        heroNode.setRight(heroNode2);
        heroNode1.setLeft(heroNode3);
        heroNode1.setRight(heroNode4);
        heroNode2.setLeft(heroNode5);

        //线索化二叉树
        binaryTree.threadedNodes(heroNode);
        //测试是否完成线索化(14的前驱结点是1)
        System.out.println(heroNode5.getLeft());
        //使用线索化方式遍历线索化二叉树
        System.out.println("使用线索化方式遍历线索化二叉树");
        binaryTree.threadedlist();
    }
}

//定义线索化二叉树
class BinaryTree {
    private HeroNode root;
    
    //为了实现线索化，需要创建给指向当前结点的前驱结点的指针
    //在递归进行线索化时，始终保存前驱结点
    private HeroNode pre = null;

    public void setRoot(HeroNode root) {
        this.root = root;
    }

    //编写对二叉树进行中序线索化的方法（递归）
    //先遍历左子树，再父结点，再遍历右子树
    public void threadedNodes(HeroNode node) {
        if (node == null) {
            return;
        }
        //(1)先线索化左子树
        threadedNodes(node.getLeft());
        //(2)再线索化当前结点的左指针和对应前驱结点的右指针
        if (node.getLeft() == null) {
            //左指针指向前驱结点
            node.setLeft(pre);
            node.setLeftType(1);
        }
        if (pre != null && pre.getRight() == null) {
            //让前驱结点的右指针指向当前结点
            pre.setRight(node);
            pre.setRightType(1);
        }
        //每处理一个结点后，移动前驱结点
        pre = node;
        //(3)再线索化右子树
        threadedNodes(node.getRight());
    }

    //中序遍历线索化二叉树的方法（非递归）
    public void threadedlist() {
        //定义一个变量，存储当前遍历的结点
        HeroNode node = root;
        
        //线索化后，第一个结点前驱结点为null，最后一个结点后继结点为null
        while (node != null) {
            //循环找到leftType == 1的结点，第一个找到的就是8结点
            //(被线索化处理后但没有前驱结点的点)

            while (node.getLeftType() == 0) {
                node = node.getLeft();
            }
            //打印当前这个结点
            System.out.println(node);
            //如果当前结点的右指针指的是后继结点，就一直输出
            while (node.getRightType() == 1) {
                node = node.getRight();
                System.out.println(node);
            }
            /// 若已经没有后继结点，将当前结点转换为其右结点。
            node = node.getRight();
        }
    }
}

//树的一个结点
class HeroNode {
    private int no;
    private String name;
    private HeroNode left;
    private HeroNode right;
    //0--left是左子树  1--left是前驱结点
    private int leftType;
    //0--right是右子树  1--right是后驱结点
    private int rightType;

    public HeroNode(int no, String name) {
        this.no = no;
        this.name = name;
    }

    public void setLeft(HeroNode left) {
        this.left = left;
    }

    public void setRight(HeroNode right) {
        this.right = right;
    }

    public void setLeftType(int leftType) {
        this.leftType = leftType;
    }

    public void setRightType(int rightType) {
        this.rightType = rightType;
    }

    public int getLeftType() {
        return leftType;
    }

    public int getRightType() {
        return rightType;
    }

    public HeroNode getLeft() {
        return left;
    }

    public HeroNode getRight() {
        return right;
    }

    @Override
    public String toString() {
        return "HeroNode{" +
                "no=" + no +
                ", name='" + name + '\'' +
                '}';
    }
}
```



### 8.5 堆排序

- 堆是具有以下性质的**完全二叉树**：每个结点的值都大于或等于其左右孩子结点的值，称为大顶堆, 每个结点的值都小于或等于其左右孩子结点的值，称为小顶堆

- 大顶堆举例说明：

  ![image-20210729151251530](Data structure and algorithm.assets/image-20210729151251530.png)

- 堆排序是一种**选择排序，**它的最坏，最好，平均时间复杂度均为O(nlogn)，它也是不稳定排序。

- 一般升序采用大顶堆，降序采用小顶堆。

- 堆排序基本思想（以升序为例）：
  - 首先将一个排序序列构造成一个大顶堆：
    - 从最后一个非叶子结点开始下沉，循环向前（相当于每次在堆顶加入一个新元素）
    - 每个非叶子结点的下沉是与其左右子结点比较并交换，非叶子结点的值不再交换时结束下沉
  - 此时大顶堆的堆顶元素为最大值，将其与末尾元素交换，此时待排序序列的长度-1
  - 末尾元素也是此大顶堆的一个新元素，继续向下沉得到新的大顶堆，重复这两个操作直至待排序序列的长度为1

```java
public class HeapSort {
    public static void main(String[] args) {
        int arr[] = {4, 6, 8, 5, 9};
        heapSort(arr);
    }

    /**
     * Description: 堆排序方法
     * @param arr:
     * @return void:
     */
    public static void heapSort(int arr[]) {
        int temp;
        //构造初始堆（升序采用大顶堆，降序采用小顶堆），从左到右，从下到上
        //i = arr.length / 2 - 1为最后一个非叶子结点
        for (int i = arr.length / 2 - 1; i >= 0; i--) {
            adjustHeap(arr,i, arr.length);
        }
        //将堆顶元素与末尾元素交换，此时只改变了堆顶元素的值，
        //所以从堆顶开始继续调整使其满足大顶堆的定义
        for (int j = arr.length - 1; j > 0; j--) {
            temp = arr[j];
            arr[j] = arr[0];
            arr[0] = temp;
            adjustHeap(arr, 0, j);
        }
        System.out.println(Arrays.toString(arr));

    }

    /**
     * Description: 将【i对应的非叶子结点】下沉到合适位置
     *              (与其左右子结点比较，交换后指向其左右子结点再重复直至不交换break)
     *      若从最后一个非叶子结点向前进行此操作，即可构建大顶堆或小顶堆
     * {4, 6, 8, 5, 9}  i = 1 ->  {4, 9, 8, 5, 6}
     * @param arr:
     * @param i: 非叶子结点在数组的索引
     * @param length: 表示对数组前多少元素进行调整
     * @return void:
     */
    public static void adjustHeap(int arr[], int i, int length) {

        int temp = arr[i];
        //k = i * 2 + 1是i结点的左子结点，k = i * 2 + 2是i结点的右子结点
        for (int k = i * 2 + 1; k < length; k = k * 2 + 1) {
            if (k + 1 < length && arr[k] < arr[k + 1]) {  //左子结点的值小于右子结点
                //k指向右子结点
                k++;
            }
            if (arr[k] > temp) {    //如果子结点大于父结点
                arr[i] = arr[k];
                //将i指向k(k的值已经交换给了i)继续循环比较
                i = k;
            } else {
                break;
            }
        }
        //for循环结束后，已经将i为root的子树的最大值放在了最顶上
        arr[i] = temp;  //将temp值放到调整后的位置
    }
}
```



### 8.6 赫夫曼树

- 一些概念：

  - 路径：在一棵树中，从一个结点往下可以达到的孩子或孙子结点之间的通路
  - 路径长度：若规定根结点的层数为1，则从根结点到第L层结点的路径长度为L-1
  - 结点的权：将树中结点赋给一个有着某种含义的数值，则这个数值称为该结点的权
  - 带权路径长度：从**根结点**到该结点之间的**路径长度**与**该结点的权**的乘积
  - 树的带权路径长度：树的带权路径长度规定为**所有叶子结点的带权路径长度之和**，记为WPL(weighted path length) 

- **赫夫曼树(Huffman Tree)**：树的带权路径长度(WPL)最小，权值越大的结点离根结点越近的二叉树才是最优二叉树

- 创建赫夫曼树思路：

  - 将每一个数据构成一个结点（最简单的二叉树）加入List集合，按根结点权值从小到大进行排序
  - 取出根结点权值最小的两个二叉树，组成新的二叉树加入List集合，二叉树的根结点权值为两者之和，从List集合删除这两个二叉树
  - 再次按照根结点权值从小到大进行排序，重复这两个步骤，直至List集合只剩下一个元素，就是赫夫曼树的根结点
  - 由数列 {13, 7, 8, 3, 29, 6, 1}构成的赫夫曼树

  <img src="Data structure and algorithm.assets/image-20210729223216180.png" alt="image-20210729223216180" style="zoom:50%;" />

```java
public class HuffmanTree {
    public static void main(String[] args) {
        int[] arr = {13, 7, 8, 3, 29, 6, 1};
        Node root = createHuffmanTree(arr);
        root.preOrder();
    }

    //编写一个前序遍历的方法
    public static void preOrder(Node root) {
        if (root != null) {
            root.preOrder();
        } else {
            System.out.println("空树");
        }
    }

    //创建Huffman树的方法
    public static Node createHuffmanTree(int[] arr) {
        //遍历数组，将每个元素构成一个Node，将Node放入List
        List<Node> nodes = new ArrayList<>();
        for (int value : arr) {
            nodes.add(new Node(value));
        }

        while (nodes.size() > 1) {
            //排序从小到大(底层是快排)
            Collections.sort(nodes);
            //取出根结点最小的两颗二叉树(结点)
            Node leftNode = nodes.get(0);
            Node rightNode = nodes.get(1);
            //构建一个新的二叉树(两颗二叉树根结点值相加成为新结点、父结点)
            Node parent = new Node(leftNode.value + rightNode.value);
            parent.left = leftNode;
            parent.right = rightNode;
            //从ArrayList删除已经处理过的二叉树
            nodes.remove(leftNode);
            nodes.remove(rightNode);
            //将parent加入nodes
            nodes.add(parent);
        }
        //返回Huffman树的root结点
        return nodes.get(0);
    }

    //创建结点类
    //为了让Node对象支持排序，实现Comparable接口
    static class Node implements Comparable<Node>{
        int value;  //结点权值
        Node left;
        Node right;

        //前序遍历
        public void preOrder() {
            System.out.println(this);
            if (this.left != null) {
                this.left.preOrder();
            }
            if (this.right != null) {
                this.right.preOrder();
            }
        }

        public Node(int value) {
            this.value = value;
        }

        @Override
        public String toString() {
            return "Node{" +
                    "value=" + value +
                    '}';
        }

        @Override
        public int compareTo(Node o) {
            //从小到大排序
            return this.value - o.value;
        }
    }
}
```



### 8.7 赫夫曼编码

#### a）通信领域中信息的处理方式：

- 定长编码：每个字符对应的AscII码对应的8位的二进制
- 变长编码：各个字符出现的次数进行编码（0= , 1=a, 10=i, 11=e, 100=k, 101=l, 110=o, 111=v, 1000=j, 1001=u, 1010=y, 1011=d），原则是出现次数越多的，则编码越小，但不符合前缀编码
- 赫夫曼编码：按照字符出现的次数构建一颗赫夫曼树, 次数作为权值
  - 符合前缀编码：字符的编码都不能是其他字符编码的前缀
  - 如果文件本身就是经过压缩处理的，那么使用赫夫曼编码再压缩效率不会有明显变化, 比如视频,ppt 等等
  - 赫夫曼编码是按字节来处理的，因此可以处理所有的文件(二进制文件、文本文件)
  - 如果一个文件中的内容，重复的数据不多，压缩效果也不会很明显

#### b）生成赫夫曼编码：

- 构建赫夫曼树结点类Node(属性Byte data；int weight；Node left；Node right；)

```java
//创建Node
class Node implements Comparable<Node>{

    Byte data;  //存放数据'a'->97 ' '->32
    int weight; //权值，字符数据出现的次数
    Node left;
    Node right;

    public Node(Byte data, int weight) {
        this.data = data;
        this.weight = weight;
    }

    //前序遍历
    public void preOrder() {
        System.out.println(this);
        if (this.left != null) {
            this.left.preOrder();
        }
        if (this.right != null) {
            this.right.preOrder();
        }
    }

    @Override
    public String toString() {
        return "Node{" +
                "data=" + data +
                ", weight=" + weight +
                '}';
    }

    @Override
    public int compareTo(Node o) {
        //从小到大排序
        return this.weight - o.weight;
    }
}
```

- 通过原始数据的字节数组，统计每个字符的权值（用Map统计），返回List\<Node>

```java
/**
 * Description: 接受一个字节数组，返回List
 * @param bytes:
 * @return java.util.List<huffmancode.Node>:
 */
private static List<Node> getNodes(byte[] bytes) {
    ArrayList<Node> nodes = new ArrayList<>();

    //存储每一个byte出现的次数（Map）
    HashMap<Byte, Integer> counts = new HashMap<>();
    for (byte b : bytes) {
        Integer count = counts.get(b);
        if (count == null) {
            counts.put(b, 1);
        } else {
            counts.put(b, count + 1);
        }
    }

    //把每个键值对转成一个Node对象并加入List集合
    for (Map.Entry<Byte, Integer> entry : counts.entrySet()) {
        nodes.add(new Node(entry.getKey(), entry.getValue()));
    }

    return nodes;
}
```

- 通过List\<Node>生成创建对应的Huffman树，返回代表Huffman树的根结点HuffmanRoot

```java
/**
 * Description: 通过List创建对应的Huffman树
 * @param nodes:
 * @return huffmancode.Node:
 */
private static Node createHuffmanTree(List<Node> nodes) {
    while (nodes.size() > 1) {
        //排序
        Collections.sort(nodes);
        //取出最小的两个二叉树,并创建一颗新的二叉树(只有权值，没有data)
        Node leftNode = nodes.get(0);
        Node rightNode = nodes.get(1);
        Node parent = new Node(null, leftNode.weight + rightNode.weight);
        parent.left = leftNode;
        parent.right = rightNode;
        //将新的二叉树加入List，从List删除使用过的两颗二叉树
        nodes.remove(leftNode);
        nodes.remove(rightNode);
        nodes.add(parent);
    }
    return nodes.get(0);
}
```

- 将传入Huffman树的所有叶子结点的赫夫曼编码得到，放入huffmanCodes集合Map<Byte, String>

```java
//生成赫夫曼树对应的赫夫曼编码表
//1.将赫夫曼编码表存放在Map<Byte,String>中
static Map<Byte, String> huffmanCodes = new HashMap<>();
//2.定义一个StringBuilder，存储某个叶子结点的路径
static StringBuilder stringBuilder = new StringBuilder();
//重载
private static Map<Byte, String> getCodes(Node root) {
    if (root == null) {
        return null;
    }
    getCodes(root,"",stringBuilder);
    return huffmanCodes;
}

/**
 * Description: 将传入node的所有叶子结点的赫夫曼编码得到，放入huffmanCodes集合
 * @param node: 传入结点
 * @param code: 路径：结点为左子结点：0，结点为右子结点：1
 * @param stringBuilder: 用于拼接路径
 * @return void:
 */
private static void getCodes(Node node, String code, StringBuilder stringBuilder) {
    StringBuilder stringBuilder1 = new StringBuilder(stringBuilder);
    stringBuilder1.append(code);
    if (node != null) { //如果node == null不处理
        //判断当前node是叶子结点还是非叶子结点
        if (node.data == null) {    //非叶子结点
            //递归处理
            //向左
            getCodes(node.left, "0", stringBuilder1);
            //向右
            getCodes(node.right, "1", stringBuilder1);
        } else {    //叶子结点
            huffmanCodes.put(node.data, stringBuilder1.toString());
        }
    }
}
```

#### c）使用赫夫曼编码压缩数据：

```java
/**
 * Description: 把所有方法进行封装
 * @param bytes: 原始字符串对应的字节数组
 * @return byte[]: 经过赫夫曼编码处理过的字节数组（压缩过的数组）
 */
private static byte[] huffmanZip(byte[] bytes) {
    //将字符数组的转化成一个Node对象类型(字符数据与权值)的List集合
    List<Node> nodes = getNodes(bytes);
    //创建对应的赫夫曼树
    Node huffmanTreeRoot = createHuffmanTree(nodes);
    //根据赫夫曼树创建创建赫夫曼编码
    Map<Byte, String> huffmanCodes = getCodes(huffmanTreeRoot);
    //根据赫夫曼编码压缩得到赫夫曼编码处理过的字节数组（压缩过的数组）
    byte[] huffmanCodeBytes = zip(bytes, huffmanCodes);

    return huffmanCodeBytes;
}


/**
 * Description: 将字符串对应的byte[]数组，通过生成的赫夫曼编码表,返回赫夫曼编码压缩过的byte[]
 * @param bytes: 原始字符串对应的byte[]数组
 * @param huffmanCodes: 生成的赫夫曼编码Map
 * @return byte[]: 赫夫曼编码压缩过的byte[]，如byte[0]="10101000"(补码)补码等于反码+1
 */
private static byte[] zip(byte[] bytes, Map<Byte, String> huffmanCodes) {
    //1.利用 huffmanCodes 将 bytes 转成赫夫曼编码对应的字符串
    StringBuilder stringBuilder = new StringBuilder();

    for (byte b : bytes) {
        stringBuilder.append(huffmanCodes.get(b));
    }
    //System.out.println("strstringBuilder=" + stringBuilder);

    //2.将stringBuilder="1010100010111111110..."转化为byte[]
    //每8位对应一个byte
    int len = (stringBuilder.length() + 7) / 8;
    int index = 0;  //记录是第几个byte
    byte[] huffmanCodeBytes = new byte[len];

    for (int i = 0; i < stringBuilder.length(); i += 8) {
        String strByte;
        if (i + 8 >= stringBuilder.length()) {   //处理最后的一段
            //如"0010"转化为byte会丢失前面两个00，用zeroCount记录下来
            //存储到huffmanCodeBytes最后一项
            strByte = stringBuilder.substring(i);
        } else {
            strByte = stringBuilder.substring(i, i + 8);
        }
        //将strByte转成一个Byte,放入huffmanCodeBytes
        //由于计算机存储数据时，都是以补码的形式进行存储。
        //通常看到的数却是计算机存储的补码先转换成反码，后转换成原码，再转换成十进制呈现的。
        //正数和0的补码是其本身，负数补码是最高位不变，其他位按位取反再加1
        //当int32位类型强转为byte8位类型时, 存储的是10100111(补码)而不是原码
        //补码存储："(省略24位)10101000"-> 168 ->(byte)截取后8位 -> 11011000 -> -88
        huffmanCodeBytes[index] = (byte) Integer.parseInt(strByte, 2);
        index++;
    }
    return huffmanCodeBytes;
}
```

#### d）使用赫夫曼编码解压数据：

```java
/**
 * Description: 解压
 * @param huffmanCodes: 赫夫曼编码表
 * @param huffmanBytes: 赫夫曼编码后的字节数组
 * @return byte[]: 原来字符串对应的字节数组
 */
private static byte[] deCode(Map<Byte, String> huffmanCodes, byte[] huffmanBytes) {
    //得到huffmanBytes对应的二进制字符串，形式如"1010100010111111110..."
    StringBuilder stringBuilder = new StringBuilder();
    for (int i = 0; i < huffmanBytes.length; i++) {
        //最后一个字节需要单独考虑
        if (i == huffmanBytes.length - 1) {
            stringBuilder.append(byteToBitString(false, huffmanBytes[i]));
        } else {
            stringBuilder.append(byteToBitString(true, huffmanBytes[i]));
        }
    }

    //把字符串按照赫夫曼编码解码
    //把赫夫曼编码调换，进行反向查询
    Map<String, Byte> map = new HashMap<>();
    for (Map.Entry<Byte, String> entry : huffmanCodes.entrySet()) {
        map.put(entry.getValue(), entry.getKey());
    }

    //创建集合，存放byte,再转化成数组（List长度可变）
    List<Byte> list = new ArrayList<>();
    int i = 0;  //赫夫曼编码后的字节数组索引
    while (i < stringBuilder.length()){
        int count = 1;
        boolean flag = true;
        Byte b = null;  //包装类可以存储null值
        while (flag) {
            //i不动，count++直到匹配到一个字符   [i, i + count)
            String key = stringBuilder.substring(i, i + count);
            b = map.get(key);
            if (b == null) {
                count++;
            } else {
                flag = false;
            }
        }
        list.add(b);
        i += count;
    }
    byte contentBytes[] = new byte[list.size()];
    for (int j = 0; j < contentBytes.length; j++) {
        contentBytes[j] = list.get(j);
    }
    return contentBytes;
}

/**
 * Description: 将一个byte转化为二进制字符串
 * @param flag: flase-表示不补高位（最后一个字节不补高位）
 * @param b: 传入的byte
 * @return java.lang.String: 对应的二进制的字符串（补码）
 */
private static String byteToBitString(boolean flag, byte b) {
    //将byte转化为int
    int temp = b;
    //非最后一个字节，如果是正数，不足8位，需要补高位
    if (flag) {
        temp |= 256;    //按位与 1 0000 0000
    }
    //返回的temp对应的二进制的**补码**字符串，与前面的补码相抵消
    String str = Integer.toBinaryString(temp);
    if (flag || temp < 0) { //非最后一个字节，或最后一个字节为负数
        //负数有32位，补位正数有9位，需要截取后8位
        return str.substring(str.length() - 8);
    } else {        //最后一个字节
        return str;
    }
}
```

#### e）使用赫夫曼编码压缩文件：

```java
/**
 * Description: 使用赫夫曼编码压缩文件
 * @param srcFile: 待压缩文件位置
 * @param dstFile: 成功压缩后的文件位置
 * @return void:
 */
public static void zipFile(String srcFile, String dstFile) {
    FileInputStream fis = null;
    FileOutputStream fos = null;
    ObjectOutputStream oos = null;
    try {
        //创建文件输入流
        fis = new FileInputStream(srcFile);
        //创建一个与源文件大小一样的byte[]
        byte[] bytes = new byte[fis.available()];
        //读取文件
        fis.read(bytes);
        //直接对bytes压缩
        byte[] huffmanBytes = huffmanZip(bytes);
        //创建文件输出流
        fos = new FileOutputStream(dstFile);
        //创建一个与文件输出流相关联的ObjectOutputStream
        oos = new ObjectOutputStream(fos);
        //以对象流的方式写入文件（编码表和数据）
        //目的是恢复源文件时区分编码表和数据
        oos.writeObject(huffmanBytes);
        oos.writeObject(huffmanCodes);
    } catch (IOException e) {
        e.printStackTrace();
    } finally {
        try {
            if (oos != null) {
                oos.close();
            }
            if (fos != null) {
                fos.close();
            }
            if (fis != null) {
                fis.close();
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

#### f） 使用赫夫曼编码压缩文件：

```java
/**
 * Description: 解压文件
 * @param zipFile: 待解压文件位置
 * @param dstFile: 成功解压的文件位置
 * @return void:
 */
public static void unZipFile(String zipFile, String dstFile) {
    FileInputStream fis = null;
    ObjectInputStream ois = null;
    FileOutputStream fos = null;
    try {
        //文件输入流
        fis = new FileInputStream(zipFile);
        //与文件输入流相关联的对象输入流
        ois = new ObjectInputStream(fis);
        //读取byte[]数组(读取顺序与写入顺序一致)
        byte[] huffmanBytes = (byte[])ois.readObject();
        Map<Byte,String> huffmanCodes = (Map<Byte,String>)ois.readObject();
        //解码
        byte[] bytes = deCode(huffmanCodes, huffmanBytes);
        //将bytes写入目标文件
        fos = new FileOutputStream(dstFile);
        fos.write(bytes);
    } catch (IOException e) {
        e.printStackTrace();
    } catch (ClassNotFoundException e) {
        e.printStackTrace();
    } finally {
        try {
            if (fos != null) {
                fos.close();
            }
            if (ois != null) {
                ois.close();
            }
            if (fis != null) {
                fis.close();
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```



### 8.8 二叉排序树BST

- 介绍：
  - Binary Sort(Search) Tree，对于二叉排序树的任何一个非叶子结点，要求左子结点的值比当前结点的值小，右子结点的值比当前结点的值大。

  - 在二叉排序树的构建过程中，会造成对于任何一个非叶子结点，左子树的所有结点的值比当前结点的值小，右子树的所有结点的值比当前结点的值大，相同的值一般放在右子结点。

    ![image-20210802141308608](Data structure and algorithm.assets/image-20210802141308608.png)

  - 二叉排序树相较于顺序存储和链式存储能提高数据**存储，读取**的效率

    - 顺序存储如数组：
      - 优点：通过下标方式访问元素，速度快。**对于有序数组**，还可使用二分查找提高检索速度。
      - 缺点：如果要检索具体某个值，或者插入值(按一定顺序)**会整体移动**，效率较低
    - 链式存储：
      - 优点：在一定程度上对数组存储方式有优化(比如：插入一个数值结点，只需要将插入结点，链接到链表中即可， 删除效率也很好)。
      - 缺点：在进行检索时，效率仍然较低，比如(检索某个值，需要从头结点开始遍历) 

- 二叉排序树的创建与遍历：

```java
//测试
public class BinarySortTreeDemo {
    public static void main(String[] args) {
        int[] arr = {7, 3, 10, 12, 5, 1, 9};
        BinarySortTree binarySortTree = new BinarySortTree();
        for (int i = 0; i < arr.length; i++) {
            binarySortTree.add(new Node(arr[i]));
        }

        //中序遍历二叉排序树
        System.out.println("中序遍历二叉排序树");
        binarySortTree.midOrder();
    }
}

//二叉排序树
class BinarySortTree {
    private Node root;

    //添加结点
    public void add(Node node) {
        if (root == null) {
            root = node;
        } else {
            root.add(node);
        }
    }

    //中序遍历(c)
    public void midOrder() {
        if (root != null) {
            root.midOrder();
        } else {
            System.out.println("树空");
        }
    }
}

//结点
class Node {
    int value;
    Node left;
    Node right;

    public Node(int value) {
        this.value = value;
    }

    @Override
    public String toString() {
        return "Node{" +
                "value=" + value +
                '}';
    }
    
    //添加结点的方法
    //递归的方法，满足二叉排序树的要求
    public void add(Node node) {
        if (node == null) {
            return;
        }
        //判断传入的结点与当前子树根结点值的关系
        if (node.value < this.value) {
            if (this.left == null) {    //左子结点为空
                this.left = node;
            } else {
                //递归向左子树添加
                this.left.add(node);
            }
        } else {
            if (this.right == null) {
                this.right = node;
            } else {
                //递归向右子树添加
                this.right.add(node);
            }
        }
    }

    //中序遍历
    public void midOrder() {
        if (this.left != null) {
            this.left.midOrder();
        }
        System.out.println(this);
        if (this.right != null) {
            this.right.midOrder();
        }
    }
}
```

- 二叉排序树的结点删除：
  - ![image-20210802141912503](Data structure and algorithm.assets/image-20210802141912503.png)
  - 删除叶子结点 (比如：2, 5, 9, 12)：
    - 若待删除结点是无父节点的叶子结点，即为根结点，直接将根结点置空
    - 找到待删除结点的父结点，将父结点的left或right属性置为空，
  - 删除只有一颗子树的结点 (比如：1)：
    - 若待删除结点无父结点，则root = 待删除结点.left(right)
    - 若待删除结点有父结点，判断待删除结点是父结点的左或右子结点，判断待删除结点的子结点是左或右子结点，则parent.left(right) = targetNode.left(right)
  - 删除有两颗子树的结点. (比如：7, 3，10 )：
    - 方法一：找到以待删除结点为根结点的右子树的值最小的结点，保存这个结点的值，删除这个结点，再将**这个值赋给待删除结点**。
    - 方法二：找到以待删除结点为根结点的左子树的值最大的结点，保存这个结点的值，删除这个结点，再将***这个值赋给待删除结点**。

```java
//测试
public class BinarySortTreeDemo {
    public static void main(String[] args) {
        int[] arr = {7, 3, 10, 12, 5, 1, 9};
        BinarySortTree binarySortTree = new BinarySortTree();
        for (int i = 0; i < arr.length; i++) {
            binarySortTree.add(new Node(arr[i]));
        }

        //中序遍历二叉排序树
        System.out.println("中序遍历二叉排序树");
        binarySortTree.midOrder();

        //删除叶子结点
        //System.out.println("删除结点后");
        //binarySortTree.delNode(5);
        //binarySortTree.midOrder();

        //删除只有一颗子树的结点
        //System.out.println("删除结点后");
        //binarySortTree.delNode(1);
        //binarySortTree.midOrder();

        //删除有两颗子树的结点
        System.out.println("删除结点后");
        binarySortTree.delNode(7);
        binarySortTree.midOrder();
    }
}

//二叉排序树
class BinarySortTree {
    private Node root;

    /**
     * Description: 删除以node为根节点的子树的最小结点并返回他的值
     * @param node: 当作以node为根节点的子树
     * @return int: 返回以node为根节点的子树最小结点的值
     */
    public int delRightTreeMin(Node node) {
        Node target = node;
        //找到以node为根节点的子树的最小值
        while (target.left != null) {
            target = target.left;
        }
        //删除最小结点（只是删除在树中的结构，没有删除这个类，有指针指向不会被回收）
        delNode(target.value);
        return target.value;
    }

    //删除结点
    public void delNode(int value) {
        if (root == null) {
            return;
        } else {
            Node targetNode = search(value);
            //如果没有找到要删除的结点
            if (targetNode == null) {
                return;
            }
            //如果有要删除的结点而二叉树只有根节点一个结点
            //不包括要删除的结点没有父结点而有子结点的情况
            if (root.left == null && root.right == null) {
                root = null;
                return;
            }
            //找到targetNode的父结点
            Node parent = searchParent(value);

            //1.删除的结点是叶子结点
            if (targetNode.left == null && targetNode.right == null) {
                //判断targetNode是父结点的左子结点还是右子结点
                if (parent.left != null && parent.left.value == value) {
                    parent.left = null;
                } else if (parent.right != null && parent.right.value == value) {
                    parent.right = null;
                }
            } else if (targetNode.left != null && targetNode.right != null) {
                //2.删除的结点有两颗子树
                int minVal = delRightTreeMin(targetNode.right);
                targetNode.value = minVal;
            } else {
                //3.剩余的就是删除的结点只有一颗子树
                //3.1如果要删除的结点有左子结点
                if (targetNode.left != null) {
                    //3.1.1如果targetNode是parent的左子结点
                    if (parent != null) {
                        if (parent.left.value == value) {
                            parent.left = targetNode.left;
                        } else {    //3.1.2如果targetNode是parent的右子结点
                            parent.right = targetNode.left;
                        }
                    } else {
                        root = targetNode.left;
                    }
                } else {    //3.1如果要删除的结点有右子结点
                    //3.2.1如果targetNode是parent的左子结点
                    if (parent != null) {
                        if (parent.left.value == value) {
                            parent.left = targetNode.right;
                        } else {    //3.2.2如果targetNode是parent的右子结点
                            parent.right = targetNode.right;
                        }
                    } else {
                        root = targetNode.right;
                    }
                }
            }

        }
    }

    //查找要删除结点的父结点
    public Node searchParent(int value) {
        if (root == null) {
            return null;
        } else {
            return root.searchParent(value);
        }
    }

    //查找删除结点
    public Node search(int value) {
        if (root == null) {
            return null;
        } else {
            return root.search(value);
        }
    }

    //添加结点
    public void add(Node node) {
        if (root == null) {
            root = node;
        } else {
            root.add(node);
        }
    }

    //中序遍历
    public void midOrder() {
        if (root != null) {
            root.midOrder();
        } else {
            System.out.println("树空");
        }
    }
}

//结点
class Node {
    int value;
    Node left;
    Node right;

    public Node(int value) {
        this.value = value;
    }

    @Override
    public String toString() {
        return "Node{" +
                "value=" + value +
                '}';
    }

    //查找要删除节点的父节点
    public Node searchParent(int value) {
        //如果当前结点就是要删除结点的父结点，返回
        if ((this.left != null && this.left.value == value) || (this.right != null && this.right.value == value)) {
            return this;
        } else {
            //当前查找的值小于当前结点的值，且当前左子结点不为空
            if (value < this.value && this.left != null) {
                return this.left.searchParent(value);   //向左子树递归查找
            } else if (value >= this.value && this.right != null) {
                return this.right.searchParent(value);  //向右子树递归查找
            } else {
                return null;    //没有找到父结点
            }
        }
    }


    //(前序遍历)查找要删除的结点
    public Node search(int value) {
        if (value == this.value) {
            return this;
        } else if (value < this.value) {    //向左子树递归查找
            if (this.left == null) {
                return null;
            }
            return this.left.search(value);
        } else {
            if (this.right == null) {   //向右子树递归查找
                return null;
            }
            return this.right.search(value);
        }
    }

    //添加结点的方法
    //递归的方法，满足二叉排序树的要求
    public void add(Node node) {
        if (node == null) {
            return;
        }
        //判断传入的结点与当前子树根结点值的关系
        if (node.value < this.value) {
            if (this.left == null) {    //左子结点为空
                this.left = node;
            } else {
                //递归向左子树添加
                this.left.add(node);
            }
        } else {
            if (this.right == null) {
                this.right = node;
            } else {
                //递归向右子树添加
                this.right.add(node);
            }
        }
    }

    //中序遍历
    public void midOrder() {
        if (this.left != null) {
            this.left.midOrder();
        }
        System.out.println(this);
        if (this.right != null) {
            this.right.midOrder();
        }
    }
}
```



### 8.9 平衡二叉树AVL

- 介绍：

  - 数列{1,2,3,4,5,6}，创建一颗二叉排序树(BST)，左子树全部为空，从形式上看，更像一个单链表，插入速度没有影响查询速度明显降低(因为需要依次比较)， 不能发挥BST的优势，因为每次还需要比较左子树，其查询速度比单链表还慢

    <img src="Data structure and algorithm.assets/image-20210802193440057.png" alt="image-20210802193440057" style="zoom: 67%;" />

  - 平衡二叉搜索树（Self-balancing binary search tree）又被称为AVL树，**是对二叉排序树功能的扩充**，可以**保证查询效率较高**。

  - 平衡二叉搜索树：它是一 棵空树或它的左右两个子树的高度差的绝对值不超过1，并且左右两个子树都是一棵平衡二叉树

- AVL树高度求解：

```java
//Node类的方法

//返回左子树的高度
public int leftHeight() {
    if (left == null) {
        return 0;
    } else {
        return left.height();
    }
}

//返回右子树的高度
public int rightHeight() {
    if (right == null) {
        return 0;
    } else {
        return right.height();
    }
}

//返回以当前结点为根结点的子树的高度
public int height() {
    return Math.max(left == null ? 0 : left.height(), right == null ? 0 : right.height()) + 1;
}
```

- AVL树左旋转：

  <img src="Data structure and algorithm.assets/image-20210802194040450.png" alt="image-20210802194040450" style="zoom:67%;" />

```java
//Node类的方法

//左旋转
public void leftRotate() {
    //1.以当前结点的值创建新的结点
    Node newNode = new Node(value);
    //2.把新结点的左子树设置为当前结点的左子树
    newNode.left = this.left;
    //3.把新结点的右子树设置为当前结点的右子树的左子树
    newNode.right = this.right.left;
    //4.把当前结点的值替换为右子结点的值
    this.value = this.right.value;
    //5.把当前结点的右子树设置为右子树的右子树
    this.right = this.right.right;
    //6.把当前结点的左子树设置为新结点
    this.left = newNode;
}
```

- AVL树右旋转：

<img src="Data structure and algorithm.assets/image-20210802194427681.png" alt="image-20210802194427681" style="zoom: 80%;" />

```java
//Node类的方法

//右旋转
public void rightRotate() {
    //1.以当前结点的值创建新的结点
    Node newNode = new Node(value);
    //2.把新结点的右子树设置为当前结点的右子树
    newNode.right = this.right;
    //3.把新结点的左子树设置为当前结点的左子树的右子树
    newNode.left = this.left.right;
    //4.把当前结点的值替换为左子结点的值
    this.value = this.left.value;
    //5.把当前结点的左子树设置为左子树的左子树
    this.left = this.left.left;
    //6.把当前结点的右子树设置为新结点
    this.right = newNode;
}
```

- AVL树双旋转：

![image-20210802194619951](Data structure and algorithm.assets/image-20210802194619951.png)

- 单独的进行左旋转或右旋转并不能使所有二叉树变成平衡二叉树

- 当对结点进行右旋转时，如果它的左子树的右子树高度大于它的左子树的左子树高度时，需要先将它的左子节点进行左旋转，再对当前结点进行右旋转
- 当对结点进行左旋转时，如果它的右子树的左子树高度大于它的右子树的右子树高度时，需要先将它的右子节点进行右旋转，再对当前结点进行左旋转
- 两个方向的双旋转并不同时进行

```java
//添加结点的方法
//递归的方法，满足二叉排序树的要求
public void add(Node node) {
    if (node == null) {
        return;
    }
    //判断传入的结点与当前子树根结点值的关系
    if (node.value < this.value) {
        if (this.left == null) {    //左子结点为空
            this.left = node;
        } else {
            //递归向左子树添加
            this.left.add(node);
        }
    } else {
        if (this.right == null) {
            this.right = node;
        } else {
            //递归向右子树添加
            this.right.add(node);
        }
    }

    //当添加完一个结点后，如果右子树高度-左子树高度 > 1
    if (rightHeight() - leftHeight() > 1) {
        //如果它的右子树的左子树高度大于它的右子树的右子树高度
        if (right != null && right.leftHeight() > left.rightHeight()) {
            //先对当前结点的右结点（右子树）进行右旋转
            right.rightRotate();
        }
        //再进行左旋转
        leftRotate();
        //不再进行下面右旋的操作
        return;
    }

    //当添加完一个结点后，如果左子树高度-右子树高度 > 1
    if (leftHeight() - rightHeight() > 1) {
        //如果它的左子树的右子树高度大于它的左子树的左子树高度
        if (left != null && left.rightHeight() > left.leftHeight()) {
            //先对当前结点的左结点（左子树）进行左旋转
            left.leftRotate();
        }
        //再进行右旋转
        rightRotate();
    }
}
```



### 8.10 多路查找树

#### a）二叉树与多叉树

- 二叉树的问题分析：
  - 二叉树需要加载到内存的，如果二叉树的节点很多，在构建二叉树时，需要多次进行i/o操作(海量数据存在数据库或文件中)，节点海量，构建二叉树时，速度有影响
  - 节点海量，也会造成二叉树的高度很大，会降低操作速度
- 多叉树：
  - 允许每个节点可以有更多的数据项和更多的子节点，就是多叉树（multiway tree）
  - 多叉树通过重新组织节点，减少树的高度，能对二叉树进行优化
  - <img src="Data structure and algorithm.assets/image-20210802214332593.png" alt="image-20210802214332593" style="zoom:80%;" />

#### b）2-3树

- 2-3树是最简单的B树结构，具有以下特点：
  - 2-3树的所有叶子节点都在同一层(只要是B树都满足这个条件)
  - 有两个子节点的节点（含一个关键字）叫二节点，二节点要么没有子节点，要么有两个子节点
  - 有三个子节点的节点（含两个关键字）叫三节点，三节点要么没有子节点，要么有三个子节点
  - 2-3树是由二节点和三节点构成的树。
- 2-3树构建举例{16, 24, 12, 32, 14, 26, 34, 10, 8, 28, 38, 20} **符合二叉排序树的规则**：
  - ![image-20210802214730847](Data structure and algorithm.assets/image-20210802214730847.png)
  - 当按照规则插入一个数到某个节点时，不能满足上面2-3树的三个特点，就需要拆，先向上拆，如果上层满，则拆本层，拆后仍然需要满足上面3个特点
  - 对于三节点的子树的值大小仍然遵守(BST 二叉排序树)的规则

#### c）B树

- B-tree树即B树，B即Balanced，平衡的二叉树，2-3树和2-3-4树都是B树

- B树通过**重新组织节点，降低树的高度**，并且减少i/o读写次数来提升效率

<img src="Data structure and algorithm.assets/image-20210802214917153.png" alt="image-20210802214917153" style="zoom:80%;" />

- 文件系统及数据库系统的设计者利用了磁盘预读原理，将一个节点的大小设为等于一个页(页得大小通常为4k)，这样每个节点只需要一次I/O就可以完全载入

- 将树的度M（结点的度是指子结点的个数，树的度是指最大的结点的度）设置为1024，在600亿个元素中最多只需要4次I/O操作就可以读取到想要的元素，B树(B+)广泛应用于文件存储系统以及数据库系统中

- B树的说明：

  - B树的阶：节点的最多子节点个数。比如2-3树的阶是3，2-3-4树的阶是4
  - B-树的搜索，从根结点开始，对结点内的关键字（有序）序列进行二分查找，如果命中则结束，否则进入查询关键字所属范围的儿子结点；重复，直到所对应的儿子指针为空，或已经是叶子结点
  - 关键字集合分布在整颗树中, 即叶子节点和非叶子节点都存放数据.
  - 搜索有可能在非叶子结点结束
  - 其搜索性能等价于在关键字全集内做一次二分查找

  ![image-20210802215328730](Data structure and algorithm.assets/image-20210802215328730.png)

#### d）B+树

- B+树是B树的变体，也是一种多路搜索树，是MySql一些索引的底层原理

- B+树的说明：

  - B+树的搜索与B树也基本相同，区别是B+树只有达到叶子结点才命中（B树可以在非叶子结点命中），其性能也等价于在关键字全集做一次二分查找
  - 所有关键字都出现在叶子结点的链表中（即数据只能在叶子节点【也叫稠密索引】），且链表中的关键字(数据)恰好是有序的
  - 不可能在非叶子结点命中
  - 非叶子结点相当于是叶子结点的索引（稀疏索引），叶子结点相当于是存储（关键字）数据的数据层
    更适合文件索引系统
  - B树和B+树各有自己的应用场景，不能说B+树完全比B树好，反之亦然

  ![image-20210802215452159](Data structure and algorithm.assets/image-20210802215452159.png)

#### e）B*树

- B*树是B+树的变体，在B+树的非根和非叶子结点再增加指向兄弟的指针

- B*树的说明：

  - B树定义了非叶子结点关键字个数至少为(2/3)*M，即块的最低使用率为2/3，而B+树的块的最低使用率为B+树的1/2*
  - 可以看出，B*树分配新结点的概率比B+树要低，空间使用率更高

  

![image-20210802215615450](Data structure and algorithm.assets/image-20210802215615450.png)



## 9.图

### 9.1 图的基本介绍

- 图（Graph）是一种数据结构，其中结点可以具有零个或多个相邻元素。两个结点之间的连接称为边。 结点也可以称为顶点。图可以表示多对多的关系。

- 图的常用术语：

  - 顶点(vertex)
  - 边(edge)
  - 路径
  - 无向图(右图)
  - 有向图
  - 带权图

  ![image-20210803165148688](Data structure and algorithm.assets/image-20210803165148688.png)

  ![image-20210803165214467](Data structure and algorithm.assets/image-20210803165214467.png)

  

### 9.2 图的表示方式

图的表示方式有两种：二维数组表示（邻接矩阵）与链表表示（邻接表）。

- 邻接矩阵

  邻接矩阵是表示图形中顶点之间相邻关系的矩阵，对于n个顶点的图而言，矩阵是的row和col表示的是1....n个点。0-未连通，1-连通（路径的权值）

  <img src="Data structure and algorithm.assets/image-20210803165434134.png" alt="image-20210803165434134" style="zoom: 67%;" />

- 邻接表

  邻接矩阵需要为每个顶点都分配n个边的空间，其实有很多边都是不存在,会造成空间的一定损失.

  邻接表的实现只关心存在的边，不关心不存在的边。因此没有空间浪费，邻接表由数组+链表组成

  ![image-20210803165935507](Data structure and algorithm.assets/image-20210803165935507.png) 



### 9.3 图的深度优先遍历

- 图的深度优先搜索(Depth First Search) ：

  从初始访问结点出发，首先访问第一个邻接结点，然后再以这个被访问的邻接结点作为初始结点，访问它的第一个邻接结点。这样的访问策略是**优先往纵向挖掘深入**，而不是对一个结点的所有邻接结点进行横向访问。深度优先搜索是一个**递归**的过程。

- 深度优先遍历算法步骤：

  1.访问初始结点v，并标记结点v为已访问。

  2.查找结点v的第一个邻接结点w。

  3.若w存在，则继续执行4，如果w不存在，则回到第1步，将从v的下一个结点继续。

  4.若w未被访问，对w进行深度优先遍历递归（即把w当做另一个v，然后进行步骤123）。

  5.查找结点v的w邻接结点的下一个邻接结点，转到步骤3。

![image-20210803170432781](Data structure and algorithm.assets/image-20210803170432781.png)



### 9.4 图的广度优先遍历

- 图的广度优先搜索(Broad First Search) :

  类似于一个**分层搜索**的过程，广度优先遍历需要使用一个队列以保持访问过的结点的顺序，以便按这个顺序来访问这些结点的邻接结点。（如先输出A，再将与A邻接的B、C输出，再从B开始，将与B邻接的全部输出...）

- 广度优先遍历算法步骤：

  1.访问初始结点v并标记结点v为已访问。

  2.结点v入队列。

  3.当队列非空时，继续执行，否则算法结束。

  4.出队列，取得队头结点u。

  5.查找结点u的第一个邻接结点w。

  6.若结点u的邻接结点w不存在，则转到步骤3；否则循环执行以下三个步骤：

  ​	6.1 若结点w尚未被访问，则访问结点w并标记为已访问。 

  ​	6.2 结点w入队列。

  ​	6.3 查找结点u的继w邻接结点后的下一个邻接结点w，转到步骤6。

**深度优先和广度优先的代码实现：**

```java
public class Graph {

    public static void main(String[] args) {
        int n = 5;
        String[] vertexValue = {"A", "B", "C", "D", "E"};
        //创建图对象
        Graph graph = new Graph(5);
        //循环添加顶点
        for (String vertex : vertexValue) {
            graph.insertVertex(vertex);
        }
        //添加边
        graph.insertEdge(0,1,1);
        graph.insertEdge(0,2,1);
        graph.insertEdge(1,2,1);
        graph.insertEdge(1,3,1);
        graph.insertEdge(1,4,1);

        //显示邻接矩阵
        graph.showGraph();

        //测试dfs遍历
        //System.out.println("dfs");
        //graph.dfs(graph.isVisited, 0);
        //System.out.println();


        //测试bfs遍历
        System.out.println("bfs");
        graph.bfs(graph.isVisited, 0);
    }

    //存储顶点集合
    private ArrayList<String> vertexList;
    //存储图对应的邻接矩阵
    private int[][] edges;
    //表示边的数目
    private int numOfEdges;
    //记录某个顶点是否被访问
    private boolean[] isVisited;

    //构造器
    public Graph(int n) {
        //初始化矩阵和vertexList
        edges = new int[n][n];
        vertexList = new ArrayList<String>(n);
        numOfEdges = 0;
        isVisited = new boolean[n];
    }

    //对dfs进行重载，遍历所有结点，并进行bfs
    //目的是避免空顶点（结点孤立）遍历不到
    public void bfs() {
        for (int i = 0; i < getNumOfVertex(); i++) {
            if (!isVisited[i]) {
                bfs(isVisited, i);
            }
        }
    }

    //广度优先遍历算法
    private void bfs(boolean[] isVisited, int i) {
        int u;  //表示队列头结点的下标
        int w;  //表示邻接结点的下标
        //队列，记录结点访问的顺序
        LinkedList queue = new LinkedList();
        System.out.print(getValueByIndex(i) + "->");
        //将该结点设置为访问
        isVisited[i] = true;
        //将这个结点加入队列（先进先出）
        queue.addLast(i);
        //队列非空
        while (!queue.isEmpty()) {
            u = (Integer) queue.removeFirst();
            //得到u的第一个邻接结点的下标w
            w = getFirstNeighbor(u);
            while (w != -1) {   //说明有邻接结点
                if (!isVisited[w]) {    //如果w结点没被访问
                    System.out.print(getValueByIndex(w) + "->");
                    isVisited[w] = true;
                    queue.addLast(w);
                }
                //如果w结点已经被访问过，查找属于u的下一个邻接结点
                w = getNextNeighbor(u, w);
            }
        }
    }

    //对dfs进行重载，遍历所有结点，并进行dfs
    //目的是避免空顶点（结点孤立）遍历不到
    public void dfs() {
        for (int i = 0; i < getNumOfVertex(); i++) {
            if (!isVisited[i]) {
                dfs(isVisited, i);
            }
        }
    }

    //深度优先遍历算法
    public void dfs(boolean[] isVisited, int i) {
        //首先访问该结点，输出
        System.out.print(getValueByIndex(i) + "->");
        //将该结点设置为访问
        isVisited[i] = true;
        //查找结点i的第一个邻接结点
        int w = getFirstNeighbor(i);
        while (w != -1) {   //说明有邻接结点
            if (!isVisited[w]) {    //如果w结点没被访问
                dfs(isVisited, w);
            }
            //如果w结点已被访问,查找属于i的下一个邻接结点
            w = getNextNeighbor(i, w);
        }
    }

    /**
     * Description: 根据前一个邻接结点的下标来获取下一个邻接结点
     * @param v1: 本结点的下标
     * @param v2: 前一个邻接结点的下标
     * @return int:
     */
    public int getNextNeighbor(int v1, int v2) {
        for (int i = v2 + 1; i < vertexList.size(); i++) {
            if (edges[v1][i] > 0) {
                return i;
            }
        }
        return -1;
    }

    //得到以index为下标的结点的第一个领接结点的下标
    public int getFirstNeighbor(int index) {
        for (int i = 0; i < vertexList.size(); i++) {
            if (edges[index][i] > 0) {
                return i;
            }
        }
        return -1;
    }

    //显示图对应的矩阵
    public void showGraph() {
        for (int[] link : edges) {
            System.out.println(Arrays.toString(link));
        }
    }

    //返回顶点的个数
    public int getNumOfVertex() {
        return vertexList.size();
    }

    //返回边的个数
    public int getNumOfEdges() {
        return numOfEdges;
    }

    //返回顶点i(下标)对应的数据如0->"A"  1->"B"
    public String getValueByIndex(int i) {
        return vertexList.get(i);
    }

    //返回v1，v2的边的权值
    public int getWeight(int v1, int v2) {
        return edges[v1][v2];
    }

    //插入顶点
    public void insertVertex(String vertex) {
        vertexList.add(vertex);
    }

    /**
     * Description: 添加边
     * @param v1: 表示第v1个顶点的下标
     * @param v2: 表示第v2个顶点的下标
     * @param weight: v1，v2的边的权值
     * @return void:
     */
    public void insertEdge(int v1, int v2, int weight) {
        //无向表
        edges[v1][v2] = weight;
        edges[v2][v1] = weight;
        numOfEdges++;
    }
}
```



## 10.十大算法

### 10.1 二分查找算法（非递归）

用while循环来替代递归，算法的时间复杂度不变。

```java
public class BinarySearchNoRecur {
    public static void main(String[] args) {
        int[] arr = {1, 3, 8, 10, 11, 67, 100};
        int index = binarySearch(arr, 1);
        System.out.println("index = " + 10);
    }

    /**
     * Description:
     * @param arr: 待查数组，升序排列
     * @param target: 目标值
     * @return int: 返回对应下标，没有找到返回-1
     */
    public static int binarySearch(int[] arr, int target) {
        int left = 0;
        int right = arr.length - 1;
        while (left <= right) { //继续查找
            int mid = (left + right) / 2;
            if (arr[mid] == target) {
                return mid;
            } else if (arr[mid] > target) {
                right = mid - 1;    //向左边查找
            } else {
                left = mid + 1; //向右边查找
            }
        }
        return -1;
    }
}
```



### 10.2 分治算法

- 分治法Divide and conquer algorithm在每一层递归上都有三个步骤：

  1)分解：将原问题分解为若干个规模较小，相互独立，与原问题形式相同的子问题

  2)解决：若子问题规模较小而容易被解决则直接解，否则递归地解各个子问题

  3)合并：将各个子问题的解合并为原问题的解。

- 分治算法可以求解的一些经典问题：二分搜索、大整数乘法、棋盘覆盖、**归并排序**、**快速排序**、线性时间选择、最接近点对问题、循环赛日程表、汉诺塔

- 汉诺塔问题分析：

  - 1)如果是有一个盘， A->C
  - 2)如果我们有 n >= 2 情况，我们总是可以看做是两个盘 1.最下边的盘(第n个) 2. 上面的盘(第1到n-1个)
    - 先把最上面的盘 A->B
    - 把最下边的盘 A->C
    - 把B塔的所有盘 从 B->C 
    - ![image-20210804102911540](Data structure and algorithm.assets/image-20210804102911540.png)

```java
public class DAQ {
    public static void main(String[] args) {
        hanoiTower(5,'A','B','C');
    }

    /**
     * Description: 第1到num个盘从a移动到c中间借助b
     * @param num: 要移动的汉诺塔数量
     * @param a:
     * @param b:
     * @param c:
     * @return void:
     */
    public static void hanoiTower(int num, char a, char b, char c) {
        //如果只有一盘
        if (num == 1) {
            System.out.println("第1个盘从" + a + "->" + c);
        } else {
            //1.把上面的所有盘A->B，移动过程中借助C
            hanoiTower(num - 1, a, c, b);
            //2.把最下面的盘A->C
            System.out.println("第" + num + "个盘从" + a + "->" + c);
            //3.把B上面的所有全部B->C
            hanoiTower(num - 1, b, a, c);
        }
    }
}
```



### 10.3 动态规划算法

- 动态规划算法介绍：

  - 动态规划(Dynamic Programming)算法的核心思想是：**将大问题划分为小问题**进行解决，从而一步步获取最优解的处理算法
  - 动态规划算法与分治算法类似，其基本思想也是将待求解问题分解成若干个子问题，先求解子问题，然后从这些子问题的解得到原问题的解。
  - 与分治法不同的是，适合于用动态规划求解的问题，经分解得到**子问题往往不是互相独立的**。 ( 即下一个子阶段的求解是建立在上一个子阶段的解的基础上，进行进一步的求解 )
  - 动态规划可以通过**填表**的方式来逐步推进，得到最优解.

- 01背包问题：

  - 背包问题主要是指一个给定容量的背包、若干具有一定价值和体积的物品，如何选择物品放入背包使物品的价值最大。其中又分**01背包(每个物品最多放一个)**和完全背包(完全背包指的是：每种物品都有无限件可用，可以转化为01背包问题)等

  - 问题描述：

    ![image-20210804145154514](Data structure and algorithm.assets/image-20210804145154514.png)

  - 解题思路：每次遍历到的第i个物品，根据w[i]和val[i]来确定是否需要将该物品放入背包中。即对于给定的n个物品，设val[i]、w[i]分别为第i+1(从0开始)个物品的价值和体积，C为背包的容量。再令v[i] [j]表示在前 i 个物品中能够装入容量为 j 的背包中的最大价值。

    - (1)  v[i] [0]=v[0] [j]=0; //表示填入表第一行和第一列是0

      ![image-20210804183103594](Data structure and algorithm.assets/image-20210804183103594.png)

    - (2) 当w[i-1]> j 时：令v[i] [j]=v[i-1] [j]   // 当准备加入新增的商品的容量大于当前背包的容量时，就直接使用上一个单元格的装入策略

    - (3) 当j>=w[i-1]时：// 当准备加入新增的商品的容量小于或等于当前背包的容量时

      令v[i] [j]=max{v[i-1] [j], val[i-1]+v[i-1] [j-w[i-1]]}  //新的单元格的装入策略由  1）上一个单元格的装入策略  2）当前商品加入背包与上一层商品&&背包容量减去当前商品体积的装入策略  **两者间的价值更大者**决定

      ![image-20210804183124961](Data structure and algorithm.assets/image-20210804183124961.png)

    - 背包问题最优解回溯（从最后一个解v[i] [j]开始）：

      - v[i] [j]=v[i-1] [j] 时，说明没有选择第 i 个商品，则回到v[i-1] [j]；
      - v[i] [j]=val[i-1]+v[i-1] [j-w[i-1]]时，说明装了第 i 个商品，该商品是最优解组成的一部分，随后我们得回到装该商品之前，即回到v[i-1] [j-w[i-1]]；
      - 一直遍历到i＝0结束为止，所有解的组成都会找到。

      ![image-20210804183744802](Data structure and algorithm.assets/image-20210804183744802.png)

```java
public class KnapsackProblem {
    public static void main(String[] args) {
        int[] w = {2, 3, 4, 5};  //物品的体积
        int[] val = {3, 4, 5, 6}; //物品的价值
        int n = val.length; //物品的个数
        int m = 8;  //背包的容量

        //创建二维数组，物品和背包容量的交叉表
        //v[i][j]表示在前i个物品中能放下装入容量j的背包的最大价值
        //初始化第一列第一行（默认为0）
        int[][] v = new int[n + 1][m + 1];

        //动态规划处理
        for (int i = 1; i < v.length; i++) {    //遍历所有商品
            for (int j = 1; j < v[0].length; j++) { //遍历所有背包容量
                //v[][]数组的i和j是从1开始遍历，val[]和w[]数组的i是从0开始遍历
                if (w[i - 1] > j) {
                    // 当准备加入新增的商品的容量大于当前背包的容量时，
                    // 就直接使用上一个单元格的装入策略
                    v[i][j] = v[i - 1][j];
                } else {
                    // 当准备加入新增的商品的容量小于或等于当前背包的容量时
                    // 新的单元格的装入策略由
                    // 1）上一个单元格的装入策略
                    // 2）当前商品加入背包与上一层商品&&背包容量减去当前商品体积的装入策略
                    // **两者间的价值更大者**决定
                    v[i][j] = Math.max(v[i - 1][j], val[i - 1] + v[i - 1][j - w[i - 1]]);
                }
            }
        }

        //输出v[][]物品和背包容量的交叉表
        for (int i = 0; i < v.length; i++) {
            for (int j = 0; j < v[i].length; j++) {
                System.out.print(v[i][j] + "\t");
            }
            System.out.println();
        }

        //输出最后背包内的商品
        int i = v.length - 1;    //行的最大下标
        int j = v[0].length - 1; //列的最大下标
        //从最后一项开始输出
        while (i > 0 && j > 0) {
            if (v[i][j] == v[i - 1][j]) {
                //说明没有选择第i个商品
            } else {
                //说明选择了第i个商品
                System.out.printf("第%d个商品放到背包中\n", i);
                j -= w[i - 1];
            }
            //朝上一个商品遍历
            i--;
        }
    }
}
```



### 10.4 KMP算法

- 字符串匹配问题：判断文本串"BBC ABCDAB ABCDABCDABDE"是否存在模式串"ABCDABD"，如果存在，就返回第一次出现的位置, 如果没有，则返回-1

- 暴力匹配算法：

  - 遍历文本串，i 为文本串的指针，j 为模式串的指针；

  - 如果当前字符匹配成功（即s1[i] == s2[j]），则i++，j++，继续匹配下一个字符；

  - 如果失配（即s1[i] != s2[j]），令i = i - (j - 1)，j = 0。**相当于每次失配时，i 回溯最初位置，j 被置为0**。

  - ![image-20210805145442763](Data structure and algorithm.assets/image-20210805145442763.png)

    ![image-20210805145500145](Data structure and algorithm.assets/image-20210805145500145.png)

```java
public class ViolenceMatch {
    public static void main(String[] args) {
        String str1 = "BBC ABCDAB ABCDABCDABDE";
        String str2 = "ABCDABD";
        System.out.println(violenceMatch(str1, str2));
    }

    //暴力匹配算法
    public static int violenceMatch(String str1, String str2) {
        char[] s1 = str1.toCharArray();
        char[] s2 = str2.toCharArray();

        int i = 0;  //i索引指向s1
        int j = 0;  //j索引指向s2
        //i < s1.length保证长字符串全部遍历
        //j < s2.length保证s1含有s2时就退出
        while (i < s1.length && j < s2.length) {
            if (s1[i] == s2[j]) {   //开始匹配
                i++;
                j++;
            } else {    //没有匹配成功
                i = i - (j - 1);
                j = 0;
            }
        }

        //判断是否匹配成功
        if (j == s2.length) {
            return i - j;
        }
        return -1;
    }
}
```

- KMP算法介绍：

  - Knuth-Morris-Pratt 字符串查找算法，由三个人的姓氏取名，利用之前判断过信息，通过一个next数组，保存模式串中前后最长公共子序列的长度，每次回溯时，通过next数组找到，前面匹配过的位置，省去了大量的计算时间
  - 参考：https://www.cnblogs.com/ZuoAndFutureGirl/p/9028287.html

- KMP算法：

  - 获取模式串的部分匹配表（**使用了KMP后续算法的原理**）：

    - next 数组各值的含义：代表包括当前字符在内的字符串中，有多大长度的相同前缀后缀。例如如果next [j] = k，代表包括 j 在内的字符串中有最大长度为*k* 的相同前缀后缀。
    - ![image-20210805150109856](Data structure and algorithm.assets/image-20210805150109856.png)

  - 遍历文本串，i 为文本串的指针，j 为模式串的指针；

  - 如果当前字符匹配成功（即s1[i] == s2[j]），则i++，j++，继续匹配下一个字符；

  - 如果失配（即s1[i] != s2[j]），令i 不变，j = next[j - 1]，**相当于每次失配时，i 不动，j 回溯到 前 j-1 个元素子串的相同前缀的后一个位置**。

  - ![image-20210805150738117](Data structure and algorithm.assets/image-20210805150738117.png)

    <img src="Data structure and algorithm.assets/image-20210805150752631.png" alt="image-20210805150752631"  />



```java
public class KMP {
    public static void main(String[] args) {
        String str1 = "BBC ABCDAB ABCDABCDABDE";
        String str2 = "ABCDABD";
        //str2的部分匹配表
        System.out.println(Arrays.toString(kmpNext(str2)));
        //str2在str1中的位置
        System.out.println(kmpSearch(str1, str2));
    }

    /**
     * Description: KMP算法匹配模式串在文本串中的位置
     * @param text: 待匹配的文本串
     * @param pattern: 模式串
     * @return int: 模式串在文本串中的位置，不匹配返回-1
     */
    public static int kmpSearch(String text, String pattern) {
        //获取模式串的部分匹配表
        int[] next = kmpNext(pattern);
        //遍历待匹配的文本串
        //i-待匹配的文本串的索引   j-模式串的索引
        for (int i = 0, j = 0; i < text.length(); i++) {
            //不匹配，j回溯
            if (j > 0 && text.charAt(i) != pattern.charAt(j)) {
                j = next[j - 1];
            }
            //匹配，j++
            if (text.charAt(i) == pattern.charAt(j)) {
                j++;
            }
            if (j == pattern.length()) {    //找到位置
                return i - j + 1;
            }
        }
        //遍历后没有找到
        return -1;
    }
    /**
     * Description: 获取一个字符串的部分匹配表
     * @param pattern: 模式串
     * @return int[]: int[i]代表包括当前字符在内的字符串中，相同前缀后缀的长度
     */
    public static int[] kmpNext(String pattern) {
        int[] next = new int[pattern.length()];
        //子串长度为1，部分匹配值为0
        next[0] = 0;
        //i为遍历整个字符串的索引，j是前缀索引
        for (int i = 1, j = 0; i < pattern.length(); i++) {
            //j > 0 使 j = 0时 在不满足条件时保持索引0，而不是 j = next[-1]
            //pattern.charAt(i) != pattern.charAt(j)时，过程和模式串与文本串匹配类似
            //如模式串="ABABCABABA"
            //匹配表为="0012012343"
            //在j=4(ABABC),i=9(ABABA)时不满足条件，但前面已经满足"ABAB"相同了，
            //即 j 对应的前面的一个"AB"ABC和 i 对应的后面的一个AB"AB"A已经匹配上了
            //所以j = next[j - 1];指向前面的一个"AB"的后一位继续与i比较
            while (j > 0 && pattern.charAt(i) != pattern.charAt(j)) {
                j = next[j - 1];
            }
            //
            if (pattern.charAt(i) == pattern.charAt(j)) {
                j++;
            }
            next[i] = j;
        }
        return next;
    }
}
```



### 10.5 贪心算法

- 贪心算法介绍：Greedy Algorithm，是指在对问题进行求解时，在每一步选择中都采取最好或者最优(即最有利)的选择，从而希望能够导致结果是最好或者最优的算法，所得到的结果**不一定是最优的结果**(有时候会是最优解)，但是都是相对近似(接近)最优解的结果

- 应用-集合覆盖：存在如下表的需要付费的广播台，以及广播台信号可以覆盖的地区。 **如何选择最少的广播台**，让所有的地区都可以接收到信号

  - <img src="Data structure and algorithm.assets/image-20210805193444352.png" alt="image-20210805193444352" style="zoom:80%;" />

  - 思路分析：

    - 1)遍历所有的广播电台, 找到一个**覆盖了最多未覆盖的地区**（交集）的电台(此电台可能包含一些已覆盖的地区，但没有关系）

      2)将这个电台加入到一个选择集合中，从未覆盖地区中删除已经被这个电台覆盖的地区

      3)重复第1步直到覆盖了全部的地区，选择集合就是最后结果

```java
public class GreedyAlgorithm {
    public static void main(String[] args) {
        //创建广播电台
        HashMap<String, HashSet<String>> broadcasts = new HashMap<>();

        HashSet<String> hashSet1 = new HashSet<String>();
        hashSet1.add("北京");
        hashSet1.add("上海");
        hashSet1.add("天津");

        HashSet<String> hashSet2 = new HashSet<String>();
        hashSet2.add("广州");
        hashSet2.add("北京");
        hashSet2.add("深圳");

        HashSet<String> hashSet3 = new HashSet<String>();
        hashSet3.add("成都");
        hashSet3.add("上海");
        hashSet3.add("杭州");


        HashSet<String> hashSet4 = new HashSet<String>();
        hashSet4.add("上海");
        hashSet4.add("天津");

        HashSet<String> hashSet5 = new HashSet<String>();
        hashSet5.add("杭州");
        hashSet5.add("大连");

        //加入到broadcasts的map
        broadcasts.put("K1", hashSet1);
        broadcasts.put("K2", hashSet2);
        broadcasts.put("K3", hashSet3);
        broadcasts.put("K4", hashSet4);
        broadcasts.put("K5", hashSet5);

        //allAreas 存放所有需要覆盖的地区，选择电台后，去掉交集地区
        HashSet<String> allAreas = new HashSet<String>();
        allAreas.add("北京");
        allAreas.add("上海");
        allAreas.add("天津");
        allAreas.add("广州");
        allAreas.add("深圳");
        allAreas.add("成都");
        allAreas.add("杭州");
        allAreas.add("大连");

        //创建ArrayList，存放选择的电台集合selects
        ArrayList<String> selects = new ArrayList<>();

        //定义临时的集合，在遍历过程中，存放某个电台覆盖地区与还未覆盖地区的交集地区
        HashSet<String> tempSet = new HashSet<>();

        //maxKey保存在一次遍历过程中，上述交集地区最大的电台key
        //以及上一个电台maxkey与还未覆盖地区的交集地区数量
        String maxKey = null;
        int preMaxKeyNum = 0;


        //选择的电台还没有覆盖到所有地区
        while (allAreas.size() != 0) {
            maxKey = null;
            preMaxKeyNum = 0;
            //遍历所有电台key
            for (String key : broadcasts.keySet()) {
                //每次循环清空tempSet
                tempSet.clear();
                //当前电台key能覆盖的地区加入tempSet
                HashSet<String> areas = broadcasts.get(key);
                tempSet.addAll(areas);
                //求出tempSet（当前电台key能覆盖的地区）
                //和allAreas（还未覆盖的地区）的交集赋给tempSet
                tempSet.retainAll(allAreas);
                //如果当前电台key含有的未覆盖地区的数量，比本次循环中的maxKey还多
                //体现出贪心算法的特点
                if (tempSet.size() > 0 &&
                        (maxKey == null || tempSet.size() > preMaxKeyNum)) {
                    maxKey = key;
                    preMaxKeyNum = tempSet.size();
                }
            }
            //将电台maxKey加入选择selects
            if (maxKey != null) {
                selects.add(maxKey);
                //去除allAreas中已经覆盖的地区
                allAreas.removeAll(broadcasts.get(maxKey));
            }
        }

        System.out.println(selects);

    }
}
```



### 10.6 Prim算法

- 普里姆算法是求**带权的无向连通图**的**最小生成树**(Minimum Cost Spanning Tree/MST)的算法（**最短连通**）。
- 给定一个带权的无向连通图，如何选取一棵生成树（N个顶点，一定有N-1条边），使树上所有边上权的总和为最小，这叫最小生成树 。

![image-20210807232021381](Data structure and algorithm.assets/image-20210807232021381.png)

- 应用场景-修路问题：
  - ![image-20210807233107547](Data structure and algorithm.assets/image-20210807233107547.png)
  - 各个站点的距离用边线表示(权) ，如何修路保证各个村庄都能连通，并且总的修建公路总里程最短?
- Prim算法思路分析与图解：
  - 在包含n个顶点的连通图中，找出只有(n-1)条边包含所有n个顶点的连通子图，也就是所谓的**极小连通子图**
  - 设G=(V,E)是连通网，T=(U,D)是最小生成树，V,U是顶点集合，E,D是边的集合，visited[]标记该节点是否访问过
  - 若从顶点 u 开始构造最小生成树，则从集合 V 中取出顶点 u 放入集合U中，标记顶点 u 的 visited[u]=1
  - 若集合 U 中顶点 ui 与集合 (V-U) 中的顶点 vj 之间存在边，则寻找这些边中权值最小的边，将顶点 vj 加入集合U中，将边（ui,vj）加入集合D中，标记visited[vj]=1，**这种构造方法已经保证不会构成回路**
  - 重复步骤上一个步骤，直到U与V相等，即所有顶点都被标记为访问过，此时D中有n-1条边

![image-20210807233014282](Data structure and algorithm.assets/image-20210807233014282.png)

```java
public class PrimAlgorithm {

    public static void main(String[] args) {
        //测试看看图是否创建ok
        char[] data = new char[]{'A','B','C','D','E','F','G'};
        int verxs = data.length;
        //邻接矩阵的关系使用二维数组表示,10000这个大数，表示两个点不联通
        int [][] weight=new int[][]{
                //ABCDEFG
                {10000,5,7,10000,10000,10000,2},        //A
                {5,10000,10000,9,10000,10000,3},        //B
                {7,10000,10000,10000,8,10000,10000},    //C
                {10000,9,10000,10000,10000,4,10000},
                {10000,10000,8,10000,10000,5,4},
                {10000,10000,10000,4,5,10000,6},
                {2,3,10000,10000,4,6,10000},
        };

        //创建MGraph对象和MiniTree对象
        MGraph graph = new MGraph(verxs);
        MinTree minTree = new MinTree();
        minTree.createGraph(graph, verxs, data, weight);
        //显示图的邻接矩阵
        minTree.showGraph(graph);

        //测试prim算法
        minTree.prime(graph, 0);
    }
}

//最小生成树--村庄的图
class MinTree {

    /**
     * Description: 编写prim算法，得到最小生成树
     * @param graph: 图
     * @param v: 表示从第几个顶点开始生成
     * @return void:
     */
    public void prime(MGraph graph, int v) {
        //标记顶点是否被访问过 0-为被访问过
        int[] visited = new int[graph.verxs];
        //把当前结点已访问
        visited[v] = 1;
        //用俩个数据记录两个顶点的下标
        int v1 = -1;
        int v2 = -1;
        //minWeight初始化
        int minWeight = 10000;
        //遍历后产生n-1条边
        for (int k = 1; k < graph.verxs; k++) {

            for (int i = 0; i < graph.verxs; i++) { //遍历已经访问过的结点
                for (int j = 0; j < graph.verxs; j++) { //遍历还没有访问的结点
                    //已经访问过的结点与还没有访问的结点的权值小于minWeight
                    if (visited[i] == 1 && visited[j] == 0 && graph.weight[i][j] < minWeight) {
                        //替换miniWeight，并记录i和j的通路(寻找权值最小的边)
                        minWeight = graph.weight[i][j];
                        v1 = i;
                        v2 = j;
                    }
                }
            }
            //找到一条边是当前权值最小的(v1-v2)
            System.out.println("边<" + graph.data[v1] + "," + graph.data[v2] + ">:权值" + minWeight);
            //将当前这个结点标记为已经访问
            visited[v2] = 1;
            //重新设置miniWeight
            minWeight = 10000;
        }


    }

    //根据数据创建图的邻接矩阵
    public void createGraph(MGraph graph, int verxs, char data[], int[][] weight) {
        int i, j;
        for(i = 0; i < verxs; i++) {//顶点
            graph.data[i] = data[i];
            for(j = 0; j < verxs; j++) {
                graph.weight[i][j] = weight[i][j];
            }
        }
    }

    //显示图的邻接矩阵
    public void showGraph(MGraph graph) {
        for(int[] link: graph.weight) {
            System.out.println(Arrays.toString(link));
        }
    }
}

//原始图
class MGraph {
    int verxs;   //图结点的个数
    char[] data;    //结点数据
    int[][] weight; //边，邻接矩阵

    public MGraph(int verxs) {
        this.verxs = verxs;
        data = new char[verxs];
        weight = new int[verxs][verxs];
    }
}
```



### 10.7 Kruskal算法

- 克鲁斯卡尔(Kruskal)算法，同Prim算法一样是用来求加权连通图的最小生成树的算法

- 应用场景-修路问题：

  - 各个站点的距离用边线表示(权) ，如何修路保证各个村庄都能连通，并且总的修建公路总里程最短?
  - <img src="Data structure and algorithm.assets/image-20210808203743814.png" alt="image-20210808203743814" style="zoom:50%;" />

- 基本思想：按照权值从小到大的顺序选择n-1条边，并保证在选择边时不构成回路

- 图解：

  ![微信截图_20210807233707](Data structure and algorithm.assets/微信截图_20210807233707.png)

- 保证在选择边时不构成回路：
  - 即在添加边时边的两个顶点不能指向同一个终点
  - 顶点的终点是指在**当前最小生成树中与其连通的最大顶点**，将顶点数组映射到【0,1,2...数组】，**在添加边时要存储ends[start]=end**（start>end），以便查找顶点的终点
  - ![image-20210807234900478](Data structure and algorithm.assets/image-20210807234900478.png)

```java
public class KruskalDemo {
    private int KedgeNum;   //图边的个数
    private char[] data;    //图的结点数据（包含结点个数信息）
    private int[][] weight; //图的邻接矩阵
    private Kedge[] Kedges;   //图的边数据
    private static final int INF = Integer.MAX_VALUE;

    public static void main(String[] args) {
        char[] data = new char[]{'A', 'B', 'C', 'D', 'E', 'F', 'G'};
        int weight[][] = {
                /*A*//*B*//*C*//*D*//*E*//*F*//*G*/
                /*A*/ {0, 12, INF, INF, INF, 16, 14},
                /*B*/ {12, 0, 10, INF, INF, 7, INF},
                /*C*/ {INF, 10, 0, 3, 5, 6, INF},
                /*D*/ {INF, INF, 3, 0, 4, INF, INF},
                /*E*/ {INF, INF, 5, 4, 0, 2, 8},
                /*F*/ {16, 7, 6, INF, 2, 0, 9},
                /*G*/ {14, INF, INF, INF, 8, 9, 0}
        };

        //根据上面的数据构建一个图
        KruskalDemo kruskalDemo = new KruskalDemo(data, weight);
        //打印图的邻接矩阵
        kruskalDemo.print();
        //输出图的所有边
        Kedge[] Kedges = kruskalDemo.getKedges();
        System.out.println(Arrays.toString(Kedges));

        //Kruskal算法得到最小生成树
        kruskalDemo.kruskal();
    }

    public void kruskal() {
        int index = 0;  //最后结果数组的索引
        int[] ends = new int[data.length];  //用于保存各顶点在已有最小生成树的终点
        //创建结果数组，保存最后的最小生成树
        Kedge[] result = new Kedge[data.length - 1];
        //获取当前的所有边的集合
        Kedge[] Kedges = getKedges();
        //遍历所有边，判断准备加入的边的两顶点是否形成了回路，没有则将边添加到最小生成树
        for (int i = 0; i < KedgeNum; i++) {
            //获取两顶点
            int p1 = getPosition(Kedges[i].start);
            int p2 = getPosition(Kedges[i].end);
            //获取两顶点的终点
            int m = getEnd(ends, p1);
            int n = getEnd(ends, p2);
            //是否构成回路
            if (m != n) {
                ends[m] = n;    //设置在“已有最小生成树”中的终点
                result[index++] = Kedges[i];  //加入最小生成树的结果数组
            }
        }
        //输出最小生成树
        System.out.println(Arrays.toString(result));
    }

    /**
     * Description: 返回ch顶点对应的下标，如果找不到，返回-1
     * @param ch: 顶点的值，比如'A','B'
     * @return int:
     */
    private int getPosition(char ch) {
        for(int i = 0; i < data.length; i++) {
            if(data[i] == ch) {//找到
                return i;
            }
        }
        //找不到,返回-1
        return -1;
    }

    /**
     * Description: 获取下标为i的顶点的终点，用于判断两个顶点的终点是否相同
     * @param ends: 在遍历过程中逐渐形成的
     * @param i: 该顶点在data[]中的下标
     * @return int:
     */
    public int getEnd(int[] ends, int i) {
        while (ends[i] != 0) {
            i = ends[i];
        }
        return i;
    }

    //图的构造器
    public KruskalDemo(char[] data, int[][] weight) {

        int vlen = data.length;
        //构建图的结点数据
        this.data = new char[vlen];
        for (int i = 0; i < vlen; i++) {
            this.data[i] = data[i];
        }
        //构建图的邻接矩阵
        this.weight = new int[vlen][vlen];
        ArrayList<Kedge> temp = new ArrayList<>();
        for (int i = 0; i < vlen; i++) {
            for (int j = 0; j < vlen; j++) {
                this.weight[i][j] = weight[i][j];
                //统计边的个数
                if (this.weight[i][j] != INF && this.weight[i][j] != 0) {
                    this.KedgeNum++;
                    temp.add(new Kedge(data[i], data[j], this.weight[i][j]));
                }
            }
        }
        //构建图的边的数据(并且按从小到大排序)
        Kedges = new Kedge[KedgeNum];
        for (int i = 0; i < temp.size(); i++) {
            Kedges[i] = temp.get(i);
        }
        Arrays.sort(Kedges);
    }

    //打印图的邻接矩阵
    public void print() {
        System.out.println("邻接矩阵为: \n");
        for (int i = 0; i < weight.length; i++) {
            for (int j = 0; j < weight.length; j++) {
                System.out.printf("%12d", weight[i][j]);
            }
            System.out.println();//换行
        }
    }

    public Kedge[] getKedges() {
        return Kedges;
    }
}

//创建一个类EData ，它的对象实例就表示一条边
class Kedge implements Comparable<Kedge> {

    @Override
    public int compareTo(Kedge o) {
        if (this.weight > o.weight) {
            return 1;
        } else if (this.weight < o.weight) {
            return -1;
        } else {
            return 0;
        }
    }

    char start; //边的一个点
    char end; //边的另外一个点
    int weight; //边的权值

    //构造器
    public Kedge(char start, char end, int weight) {
        this.start = start;
        this.end = end;
        this.weight = weight;
    }

    //重写toString, 便于输出边信息
    @Override
    public String toString() {
        return "Kedge [<" + start + ", " + end + ">= " + weight + "]";
    }
}
```



### 10.8 Dijkstra算法

- 迪杰斯特拉(Dijkstra)算法是**典型最短路径算法**，用于计算一个结点到其他结点的最短路径。 它的主要特点是以起始点为中心向外层层扩展(**广度优先**搜索思想+**贪心**思想)，直到扩展到终点为止。

- 应用场景-最短路径问题：
  - ![image-20210808154049427](Data structure and algorithm.assets/image-20210808154049427.png)
  - 各个村庄的距离用边线表示(权)，计算出D村庄到其它各个村庄的最短距离? 
- 算法思路：
  - 通过Dijkstra计算图G中的最短路径时，需要指定一个起点D(即从顶点D开始计算)。
  - 此外，引进三个数组already_arr、dis和pre_visited。already_arr的作用是记录顶点是否已遍历过（0-未遍历；1-已遍历），而dis则是记录顶点到起点D的距离（初始值为无穷大）， pre_visited[i] = j表示初始起点 D 到达顶点 i 要最后经过顶点 j
  - 初始时，数组already_arr中只有起点D = 1；遍历与起点D相邻的点，获取距离并更新dis数组
  - 然后，从数组already_arr中找出**目前路径最短（dis最小）**且**already_arr[K] = 0**的顶点K，遍历与顶点K相邻的点且还未遍历过的点，获取距离并更新dis数组【条件是(出发顶点->index + index->i) < 目前出发顶点到 i 结点的距离】，全驱结点数组 pre_visited[i] = index，并且使already_arr[K] = 1
  - 重复第4步操作，直到遍历完所有顶点。
- 算法思路图解：

![无标题](Data structure and algorithm.assets/无标题.png)

```java
public class DijkstraDemo {
    public static void main(String[] args) {
        char[] vertex = {'A', 'B', 'C', 'D', 'E', 'F', 'G'};
        int[][] weight = new int[vertex.length][vertex.length];
        final int N = 65536;
        weight[0] = new int[]{N, 12, N, N, N, 16, 14};
        weight[1] = new int[]{12, N, 10, N, N, 7, N};
        weight[2] = new int[]{N, N, 10, 3, 5, 6, N};
        weight[3] = new int[]{N, N, 3, N, 4, N, N};
        weight[4] = new int[]{N, N, 5, 4, N, 2, 8};
        weight[5] = new int[]{16, 7, 6, N, 2, N, 9};
        weight[6] = new int[]{14, N, N, N, 8, 9, N};

        //创建图对象并显示
        KGraph kGraph = new KGraph(vertex, weight);
        kGraph.showKGraph();
        //dijkstra算法求D点到其他点的最小距离
        kGraph.dijkstra(3);
        kGraph.show();
    }
}


class KGraph {
    private char[] vertex;  //顶点数组
    private int[][] weight; //邻接矩阵
    // 记录各个顶点是否访问过 1表示访问过,0未访问,会动态更新
    public int[] already_arr;
    // 每个下标对应的值为前一个顶点下标, 会动态更新
    public int[] pre_visited;
    // 记录出发顶点到其他所有顶点的距离,比如G为出发顶点，就会记录G到其它顶点的距离，会动态更新，求的最短距离就会存放到dis
    public int[] dis;

    //迪杰斯特拉算法实现
    public void dijkstra(int index) {
        //初始化dis数组
        Arrays.fill(dis, 65536);
        this.dis[index] = 0;    //设置出发顶点的访问距离为0
        //初始化前驱结点
        Arrays.fill(pre_visited, -1);
        //设置出发顶点已经被访问
        this.already_arr[index] = 1;
        update(index);

        for (int j = 1; j < vertex.length; j++) {
            int min = 65536;
            //更新新的访问顶点(注意不是出发顶点)
            for(int i = 0; i < already_arr.length; i++) {
                if(already_arr[i] == 0 && dis[i] < min ) {
                    min = dis[i];
                    index = i;
                }
            }
            //更新 index 顶点被访问过
            already_arr[index] = 1;
            update(index);     //更新出发顶点到index下标周围顶点的距离和周围顶点的前驱顶点
        }
    }

    //更新出发顶点到index下标周围顶点的距离和周围顶点的前驱顶点
    public void update(int index) {
        int len;
        for (int i = 0; i < vertex.length; i++) {
            // 出发顶点到 index 顶点的距离+从 index 顶点到 i 顶点的距离
            len = dis[index] + weight[index][i];
            // 如果 i 结点没有被访问过，并且上述距离(出发顶点->index + index->i) < 目前出发顶点到 i 结点的距离
            if (already_arr[i] == 0 && len < dis[i]) {
                //更新 i 结点的前驱结点为index，并且更新出发顶点到 i 结点的距离
                pre_visited[i] = index;
                dis[i] = len;
            }
        }
    }

    //显示最后的结果
    public void show() {
        System.out.println(Arrays.toString(dis));
    }

    //显示图
    public void showKGraph() {
        for (int[] link : weight) {
            System.out.println(Arrays.toString(link));
        }
    }

    //构造器
    public KGraph(char[] vertex, int[][] weight) {
        this.vertex = vertex;
        this.weight = weight;
        this.already_arr = new int[vertex.length];
        this.pre_visited = new int[vertex.length];
        this.dis = new int[vertex.length];
    }
}
```



### 10.9 Floyd算法

- 迪杰斯特拉算法通过选定的被访问顶点，求出从出发访问顶点到其他顶点的最短路径；而**弗洛伊德算法**中每一个顶点都是出发访问点，所以需要将每一个顶点看做被访问顶点，**求出从每一个顶点到其他顶点的最短路径**。
- 应用场景-最短路径问题：
  - ![image-20210808204020088](Data structure and algorithm.assets/image-20210808204020088.png)
  - 各个村庄的距离用边线表示(权)，计算每个村庄到其它各个村庄的最短距离? 
- 算法思路和图解：
  - 顶点 vi 到顶点 vk 的最短路径已知为 Lik，顶点 vk 到 vj 的最短路径已知为 Lkj，顶点 vi 到 vj 的路径为 Lij，则 vi 到 vj 的最短路径为：min((Lik+Lkj),Lij)
  - **三层分别遍历**中间顶点，出发顶点和终点顶点{'A', 'B', 'C', 'D', 'E', 'F', 'G'...}，注意**出发顶点与中间顶点、中间顶点与终点顶点并不要求直连**
  - ![image-20210808204701749](Data structure and algorithm.assets/image-20210808204701749.png)
  - 把A作为中间顶点的所有情况都进行遍历后得到
  - ![image-20210808204754758](Data structure and algorithm.assets/image-20210808204754758.png)

```java
public class FloydDemo {
    public static void main(String[] args) {
        char[] vertex = {'A', 'B', 'C', 'D', 'E', 'F', 'G'};
        int[][] weight = new int[vertex.length][vertex.length];
        final int N = 65536;
        weight[0]=new int[]{0,5,7,N,N,N,2};
        weight[1]=new int[]{5,0,N,9,N,N,3};
        weight[2]=new int[]{7,N,0,N,8,N,N};
        weight[3]=new int[]{N,9,N,0,N,4,N};
        weight[4]=new int[]{N,N,8,N,0,5,4};
        weight[5]=new int[]{N,N,N,4,5,0,6};
        weight[6]=new int[]{2,3,N,N,4,6,0};

        //创建图对象
        FGraph fGraph = new FGraph(vertex, weight);
        //floyd算法处理并显示结果
        fGraph.floyd();
        fGraph.showFGraph();
    }
    
}

class FGraph {
    private char[] vertex;  //顶点数组
    //初始的邻接矩阵
    //同时记录出发顶点到其他所有顶点的距离,比如G为出发顶点，就会记录G到其它顶点的距离，会动态更新，求的最短距离就会存放到dis
    private int[][] dis;
    // 到达目顶点的前驱结点
    public int[][] pre;

    public void floyd() {
        int len;
        //遍历中间顶点
        for (int k = 0; k < vertex.length; k++) {
            //遍历出发顶点
            for (int i = 0; i < vertex.length; i++) {
                //遍历终点顶点
                for (int j = 0; j < vertex.length; j++) {
                    // 从i出发，经过k，最后到达j，i与k，k与j并不一定直连
                    len = dis[i][k] + dis[k][j];
                    if (len < dis[i][j]) {
                        // 满足条件更新距离和前驱结点
                        dis[i][j] = len;
                        // i与k，k与j并不一定直连，所以更新前驱结点也是pre[k][j](直连时=k)而不是k
                        pre[i][j] = pre[k][j];
                    }
                }
            }
        }
    }

    //显示dis数组和pre数组
    public void showFGraph() {
        System.out.println("dis");
        for (int[] link : dis) {
            System.out.println(Arrays.toString(link));
        }
        System.out.println("pre");
        for (int[] link : pre) {
            System.out.println(Arrays.toString(link));
        }
    }

    //构造器
    public FGraph(char[] vertex, int[][] dis) {
        this.vertex = vertex;
        this.dis = dis;
        this.pre = new int[vertex.length][vertex.length];
        //对pre进行初始化，前驱顶点的下标是自身的下标
        for (int i = 0; i < vertex.length; i++) {
            Arrays.fill(pre[i], i);
        }
    }
}
```



### 10.10 马踏棋盘算法

- 马踏棋盘：将马随机放在国际象棋的8×8棋盘Board[0～7][0～7]的某个方格中，马按走棋规则(马走日字)进行移动。要求每个方格只进入一次，走遍棋盘上全部64个方格

![image-20210808232049122](Data structure and algorithm.assets/image-20210808232049122.png)

- 马踏棋盘问题(骑士周游问题)实际上是图的深度优先搜索(DFS)的应用，可以用**回溯**（循环加递归，实质就是深度优先搜索）来解决，用**贪心算法**来进行优化

- 算法思路：

  1. 创建棋盘 chessBoard , 是一个二维数组

  2. 将当前位置设置为已经访问，然后根据当前位置，计算马儿还能走哪些位置，并放入到一个集合中(ArrayList), 最多有8个位置， 每走一步，就使用step+1

  3. 遍历ArrayList中存放的所有位置，看看哪个可以走通 , 如果走通，就继续，走不通，就**回溯**.

  4. 判断马儿是否完成了任务，使用  step 和**应该走的步数**比较 ， 如果没有达到数量，则表示没有完成任务，将整个棋盘置0
  5. 优化思路：在选择马儿下一步走的位置时，优先选择这些位置中的下一步可选择位置数少的位置

```java
public class HouseChessboard {
    private static int X = 8;   //棋盘列数
    private static int Y = 8;   //棋盘行数
    //创建一个数组，标记棋盘的各个位置是否被访问过
    private static boolean visited[][];
    //创建棋盘，记录step
    private static int chessboard[][];
    //所有位置被访问的标记
    private static boolean finished;

    public static void main(String[] args) {
        visited = new boolean[X][Y];
        chessboard = new int[X][Y];
        //测试耗时
        long start = System.currentTimeMillis();
        traverlChessboard(0, 0, 1);
        long end = System.currentTimeMillis();
        System.out.println("耗时：" + (end - start) + "ms");
        //输出棋盘最后情况
        for (int[] link : chessboard) {
            System.out.println(Arrays.toString(link));
        }

    }

    /**
     * Description: 完成马踏棋盘的算法
     * @param x: 马儿当前x坐标
     * @param y: 马儿当前y坐标
     * @param step: 马儿走的第几步，初始位置为第一步
     * @return void:
     */
    public static void traverlChessboard(int x, int y, int step) {
        chessboard[x][y] = step;
        //标记已访问
        visited[x][y] = true;
        //获取当前位置可以走的下一个位置的集合(行列与next方法内对应)
        ArrayList<Point> points = next(new Point(x, y));
        //对points进行排序
        sort(points);
        //遍历points
        while (!points.isEmpty()) {
            Point point = points.remove(0); //取出下一个可以走的位置
            if (!visited[point.x][point.y]) {   //说明还未被j
                traverlChessboard(point.x, point.y, step + 1);
            }
        }
        //回溯
        //判断马儿是否完成了任务，没有完成任务将整个棋盘置为0
        //step < X * Y:  1  棋盘还没有走完   2  棋盘处于一个回溯过程
        if (step < X * Y && !finished) {
            chessboard[x][y] = 0;
            visited[x][y] = false;
        } else {
            finished = true;
        }
    }

    /**
     * Description: 根据当前的位置（Point），计算马儿还能走哪些位置
     * @param curPoint: 当前的位置（Point）
     * @return java.util.ArrayList<java.awt.Point>: 可走位置的集合
     */
    public static ArrayList<Point> next(Point curPoint) {
        ArrayList<Point> points = new ArrayList<>();
        //创建一个Point
        Point p1 = new Point();
        //表示马儿可以走4这个位置
        if((p1.x = curPoint.x - 2) >= 0 && (p1.y = curPoint.y -1) >= 0) {
            points.add(new Point(p1));
        }
        //判断马儿可以走3这个位置
        if((p1.x = curPoint.x - 1) >=0 && (p1.y=curPoint.y-2)>=0) {
            points.add(new Point(p1));
        }
        //判断马儿可以走2这个位置
        if ((p1.x = curPoint.x + 1) < X && (p1.y = curPoint.y - 2) >= 0) {
            points.add(new Point(p1));
        }
        //判断马儿可以走1这个位置
        if ((p1.x = curPoint.x + 2) < X && (p1.y = curPoint.y - 1) >= 0) {
            points.add(new Point(p1));
        }
        //判断马儿可以走0这个位置
        if ((p1.x = curPoint.x + 2) < X && (p1.y = curPoint.y + 1) < Y) {
            points.add(new Point(p1));
        }
        //判断马儿可以走7这个位置
        if ((p1.x = curPoint.x + 1) < X && (p1.y = curPoint.y + 2) < Y) {
            points.add(new Point(p1));
        }
        //判断马儿可以走6这个位置
        if ((p1.x = curPoint.x - 1) >= 0 && (p1.y = curPoint.y + 2) < Y) {
            points.add(new Point(p1));
        }
        //判断马儿可以走5这个位置
        if ((p1.x = curPoint.x - 2) >= 0 && (p1.y = curPoint.y + 1) < Y) {
            points.add(new Point(p1));
        }

        return points;
    }

    //根据ArrayList<Point>的所有位置再求各位置的下一步位置个数，进行非递减排序
    public static void sort(ArrayList<Point> points) {
        points.sort(new Comparator<Point>() {
            @Override
            public int compare(Point o1, Point o2) {
                //先获取o1的下一步的所有位置个数
                int count1 = next(o1).size();
                int count2 = next(o2).size();
                if (count1 < count2) {
                    return -1;
                } else if (count1 == count2) {
                    return 0;
                } else {
                    return 1;
                }
            }
        });
    }
}
```

