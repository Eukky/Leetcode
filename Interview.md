# 面试知识点总结

## C/C++

- C和C++的特点与区别

  - C

    1. 作为一种面向过程的结构化语言，易于调试和维护。
    2. 表现能力和处理能力强，可以直接访问内存的物理地址。
    3. 实现了对硬件的编程操作，也适合用于应用软件的开发。
    4. 效率高，可移植性强。

  - C++

    1. 在C语言的基础上进行扩充和完善，使C++兼容了C语言面向过程特点，又成为了一种面向对象的程序设计语言。
    2. 可以使用抽象数据类型进行基于对象的编程。
    3. 可以使用多继承，多态进行面向对象编程。
    4. 可以负担起以模版为特征的范型化编程。

- C++是否为面向对象语言，面向对象的的三个特性  
  
  C++是一门面向对象语言，面向对象的三个特性分别为封装，继承，多态。  
  封装体现为C++当中具有对象和类的概念，在类当中可以对一系列数据和方法进行封装，并且控制其他类对这些数据方法的访问权限。  
  继承可以使一个新的类使用现有类的所有功能，并实现功能的扩展。
  多态可以简单概括为一个接口，多种方法。具体表现为在不同继承关系的对象中去调用同一个函数，产生了不同的结果。这种机制主要通过虚函数来实现。

- 类的内存分布
  
  如果一个类带有虚函数，在这个类的开始位置会插入一个虚函数指针，如果这个类有继承的父类，父类会放在该派生类的前面。

- 虚函数是什么  
  
  虚函数通过vitual关键字进行声明，在进行虚函数的声明之后，一个父类指针能够去调用派生类中的相应函数。在没有虚函数时，基类指针只能够访问派生类的成员变量而不能访问派生类的成员函数。  
  例子：假如有一个英雄类，英雄类有两个派生类分别为战士类和法师类，英雄类中有一个攻击的虚函数，法师类和战士类分别对攻击函数进行了不同的实现，法师的攻击是火球术，战士的攻击是劈砍，这时初始化一个英雄类指针，使它分别指向法师和战士，并且调用攻击函数，因为虚函数的关系这个指针指向法师类时调用攻击函数会使用火球术，指向战士类时调用攻击函数会使用劈砍。  
  虚函数通过虚函数表进行实现，虚函数表中包含了所有虚函数的地址，虚函数按照声明顺序放在虚函数表中，如何在派生类中对父类的函数进行了重载，覆盖的函数将会放在虚函数表中原本父类虚函数的位置，这样在调用该函数时就会调用子类中重新实现的函数。当一个子类有多个父类时，每一个父类将会拥有一个不同的虚函数表。在子类的虚函数没有覆盖父类虚函数的场合，子类的虚函数将会放在第一个被声明的父类的虚函数表后面。

- 构造函数能否为虚函数

  不可以，虚函数需要使用虚表指针调用虚表来实现，在调用构造函数之前还未生成对象，因此构造函数不能为虚函数。

- 如果析构函数为虚函数会发生什么  
  
  C++中默认的析构函数不是虚函数，如果将默认的析构函数声明为虚函数，因为需要额外的虚函数指针以及虚函数表，将会占用内存。但是当该类成为父类时，析构函数需要被设置成虚函数。在将析构函数设置成虚函数之后，在释放指向派生类父类指针时，会调用派生类的析构函数。如果不设置为虚函数，在释放指针时派生类的析构函数无法被调用，则会造成内存泄漏。

- 重写和重载有什么区别  
  
  范围区别：重写和被重写的函数在不同的类中，重载和被重载的函数在同一类中。  
  参数区别：重写与被重写的函数参数列表一定相同，重载和被重载的函数参数列表一定不同。  
  virtual的区别：重写的基类必须要有virtual修饰，重载函数和被重载函数可以被virtual修饰，也可以没有。

- 智能指针是什么
  
  - 智能指针：智能指针是一个类，这个类的构造函数中传入一个普通指针，析构函数中释放传入的指针。智能指针的类都是栈上的对象，所以当函数结束时会自动被释放，方便管理堆内存。智能指针利用RAII（资源获取即初始化）技术堆普通指针进行封装，智能指针的实质是一个对象，行为表现像一个指针。

  - shared_ptr：基于引用计数的智能指针，可随意赋值，支持多个指针指向相同对象。每次拷贝都会使对象的引用计数加1，赋值使愿对象引用计数减1，新对象引用计数加1，当计数为0时，自动释放内存。

  - unique_ptr：唯一拥有所指对象。同一时刻智能有一个unique_ptr指向指定对象，禁止拷贝语义，只有移动语义。在出现异常的情况下，动态资源能得到释放。  
  
  - weak_ptr：若引用，只引用不计数，没有重载operator*和->，对shared_ptr资源观测。针对shared_ptr的循环引用，两个指针内存都无法释放的问题需要手动打破循环引用或使用weak_ptr。  
  
- 强制类型转换

  static_cast：静态类型转换，在编译时决定转换的类型，用于代替旧的C风格类型转换。  
  dynamic_cast：子类和父类之间的多态类型转换，可以用于将父类指针转换为子类指针。  
  reinterpreter_cast：重新解释类型转换。用于不同类型之间的指针类型转换，不同于static_cast，static_cast用于基本类型之间的转换。  
  const_cast：用于去掉const属性转换。

- static关键字的作用

  1. 限制符号的作用域只在本程序文件内使用。
  2. 指定变量的存储位置。对于函数内的变量都是存放在栈内存区，函数结束时自动释放。static变量都是存放在数据区，只有程序结束时才会被释放，且只存有一份。不管函数被调用多少次，static语句只在第一次调用时执行。后面的调用都不执行也不初始化。
  3. 类的静态成员变量。属于类而不是类的某个实例对象，在类中只是声明，并不是定义，因此不分配内存，对类用sizeof求大小也不会将static变量得大小加入。必须在类声明的外部，以及main()函数的外部，也就是全部变量区域对类的static成员变量再次定义（定以后才分配唯一内存，此时该类静态成员变量相当于是全局的静态变量了，只是调用的时候要使用类名加::）。若只定义不赋值初始化，则默认初始化为0。
  4. 类的静态成员函数。只能调用本类的静态成员变量或函数，不能调用本类的非静态成员函数和变量。因为非静态成员函数和变量在类成员函数中调用时，都是由形参中隐含一个指向当前实例对象的this指针来调用。然而静态成员函数没有这个this形参。

- malloc和new的区别

  1. malloc/free是标准库函数，new/delete是C++运算符。
  2. malloc失败返回空，new失败抛出异常。
  3. new/delete会调用构造，析构函数，malloc/free不会，所以无法满足动态对象的要求。
  4. new返回有类型的指针，malloc返回无类型的指针。
  5. malloc从堆上动态分配内存，new是从自由存储区为对象动态分配内存。自由存储区的位置取决于operator new的实现，不仅可以为堆，还可以是静态存储区。这些都看operator new在哪里为对象分配内存。
  6. new/delete可以被重载，malloc/free不可以。

- 有了malloc/free为什么还要new/delete

  1. new运算不需要进行强制类型转换，使用方便。
  2. new运算通过调用构造函数初始化动态创建对象，执行效率高。
  3. 使用new能够进行异常处理，使用更安全。

- STL内存机制实现

  1. x大于128byte，用第一级配置器直接向系统malloc，至于不成功的处理，过程仿照new，通过set_new_handler来处理，直到成功返回相应大小的内存或者是抛出异常或者是干脆结束运行；
  2. x小于128byte，用第二级配置器向内存池相应的free_list要内存，如果该freelist上面没有空闲块了，从内存池里面要内存，差不多要的是<=20个相应freelist大小的块，如果要不到，从系统的heap里面要内存给到内存池，换算的标准是bytes_to_get=2*total_bytes+ROUND_UP(heap_size>>4)，这时使用的是系统的malloc，如果要不到，从比较大的freelist那里要内存到内存池，如果还是要不到，从系统的heap里面要内存给到内存池，换算标准跟原来一样，但是这时候使用的是第一级配置器的allocate，主要是看看能不能通过out_of_memory机制得到一点内存。所以，freelist总是从内存池里要内存的，而内存池可能从freelist也可能从系统heap那里要内存。

  - sgi stl的alloc的主要开销就在于管理这些小内存，管理这些小内存的主要开销就在于，每次freelist上的内存块用完了，需要重新要空间，然后建立起这个list来。freelist上的内存，会一直保留着直到程序退出才还给系统。但这不会产生内存泄漏，一来是管理的都是小内存，二来是，占用的内存只会是整个程序运行过程中小内存占用量最大的那一刻所占用的内存。

- vector和list的优缺点

  - vector与数组类似，拥有一段连续的内存空间，并且起始地址不变。便于随机访问，时间复杂度为O（1），但因为内存空间是连续的，所以在进入插入和删除操作时，会造成内存块的拷贝，时间复杂度为O（n）。此外，当数组内存空间不足，会采取扩容，通过重新申请一块更大的内存空间进行内存拷贝。
  
  - list底层是由双向链表实现的，因此内存空间不是连续的。根据链表的实现原理，List查询效率较低，时间复杂度为O（n），但插入和删除效率较高。只需要在插入的地方更改指针的指向即可，不用移动数据。

- map和unordered_map的不同
  
  map内部由红黑树实现，具有自动排序功能。红黑树的每一个节点表示map的一个元素，因此对map中元素的操作其实就是对红黑树元素的操作。使用中序遍历可以将元素从小到大进行遍历。由于红黑树的实现，map由较高的空间占用率。
  unordered_map内部由哈希表实现，内部元素无序，通过把key值映射到哈希表中的一个位置来进行访问，查找的时间复杂度可以达到O(1)。

- 深拷贝与浅拷贝

  - 浅拷贝： 将原对象或原数组的引用直接赋给新对象，新数组，新对象只是原对象的一个引用（等号操作）。默认拷贝函数为浅拷贝。
  - 深拷贝： 创建一个新的对象和数组（在堆区动态申请内存），将原对象的各项属性的“值”（数组的所有元素）拷贝过来，使用深拷贝可以避免内存的重复释放。

## 排序算法

- 冒泡排序  
  
  比较相邻的元素。如果第一个比第二个大，就交换它们两个；  
  对每一对相邻元素作同样的工作，从开始第一对到结尾的最后一对，这样在最后的元素应该会是最大的数；  
  针对所有的元素重复以上的步骤，除了最后一个；  
  重复步骤1~3，直到排序完成。  

- 选择排序  
  
  初始状态：无序区为R[1..n]，有序区为空；
  第i趟排序(i=1,2,3…n-1)开始时，当前有序区和无序区分别为R[1..i-1]和R(i..n）。该趟排序从当前无序区中-选出关键字最小的记录 R[k]，将它与无序区的第1个记录R交换，使R[1..i]和R[i+1..n)分别变为记录个数增加1个的新有序区和记录个数减少1个的新无序区；n-1趟结束，数组有序化了。

- 插入排序  
  
  从第一个元素开始，该元素可以认为已经被排序；
  取出下一个元素，在已经排序的元素序列中从后向前扫描；
  如果该元素（已排序）大于新元素，将该元素移到下一位置；
  重复步骤3，直到找到已排序的元素小于或者等于新元素的位置；
  将新元素插入到该位置后；
  重复步骤2~5。

- 希尔排序  
  
  选择一个增量序列t1，t2，…，tk，其中ti>tj，tk=1；
  按增量序列个数k，对序列进行k 趟排序；
  每趟排序，根据对应的增量ti，将待排序列分割成若干长度为m 的子序列，分别对各子表进行直接插入排序。仅增量因子为1 时，整个序列作为一个表来处理，表长度即为整个序列的长度。

- 归并排序  
  
  把长度为n的输入序列分成两个长度为n/2的子序列；
  对这两个子序列分别采用归并排序；
  将两个排序好的子序列合并成一个最终的排序序列。

  ```cpp
    void Merge(vector<int>& array, vector<int>& left, vector<int>& right){
        int length1 = left.size();
        int length2 = right.size();

        int i = 0, j = 0;

        while(i < length1 || j < length2){
            if(i >= length1){
                array.push_back(right[j++]);
            }else if(j >= length2){
                array.push_back(left[i++]);
            }else if(left[i] < right[j]){
                array.push_back(left[i++]);
            }else{
                array.push_back(right[j++]);
            }
        }
    }

    void MergeSort(vector<int>& array){
    int length = array.size();
    if(length <= 1){
        return;
    }
    vector<int> left;
    vector<int> right;

    for(int i = 0; i < length; i++){
        if(i < length / 2){
            left.push_back(array[i]);
        }else{
            right.push_back(array[i]);
        }
    }

    MergeSort(left);
    MergeSort(right);
    array.clear();
    Merge(array, left, right);
    }
  ```

- 快速排序  
  
  从数列中挑出一个元素，称为 “基准”（pivot）
  重新排序数列，所有元素比基准值小的摆放在基准前面，所有元素比基准值大的摆在基准的后面（相同的数可以到任一边）。在这个分区退出之后，该基准就处于数列的中间位置。这个称为分区（partition）操作；
  递归地（recursive）把小于基准值元素的子数列和大于基准值元素的子数列排序。

  ```cpp
    void quick_sort(int s[], int l, int r)
    {
        if (l < r)
        {
            //Swap(s[l], s[(l + r) / 2]); //将中间的这个数和第一个数交换 参见注1
            int i = l, j = r, x = s[l];
            while (i < j)
            {
                while(i < j && s[j] >= x) // 从右向左找第一个小于x的数
                    j--;  
                if(i < j)
                    s[i++] = s[j];
                while(i < j && s[i] < x) // 从左向右找第一个大于等于x的数
                    i++;  
                if(i < j)
                    s[j--] = s[i];
            }
            s[i] = x;
            quick_sort(s, l, i - 1); // 递归调用
            quick_sort(s, i + 1, r);
        }
    }  
  ```

## 数据结构

- 链表

  - 如何判断链表成环

    - 使用追赶的方法：设定两个指针slow、first，从头指针开始，每次分别前进1步、2步。如存在环，则两者相遇；如不存在环，fast遇到NULL退出。  

    - 环的接入点：碰撞点p到连接点的距离=头指针到连接点的距离，因此，分别从碰撞点、头指针开始走，相遇的那个点就是连接点。

    ```cpp
    /**
     * Definition for singly-linked list.
     * struct ListNode {
     *     int val;
     *     ListNode *next;
     *     ListNode(int x) : val(x), next(NULL) {}
     * };
     */
    ListNode *detectCycle(ListNode *head) {
        ListNode* slow = head;
        ListNode* fast = head;
        int pos = 0;
        while(fast != NULL && fast -> next != NULL){
            slow = slow -> next;
            fast = fast -> next -> next;
            if(slow == fast){
                slow = head;
                while(slow != fast){
                    pos++;
                    slow = slow -> next;
                    fast = fast -> next;
                }
                return fast;
            }
        }
        return nullptr;
    }
    ```

  - 单链表反转

    ```cpp
    /*
    * Definition for singly-linked list.
    * struct ListNode {
    *     int val;
    *     ListNode *next;
    *     ListNode(int x) : val(x), next(NULL) {}
    * };
    */
    ListNode* ReverseList(ListNode* pHead) {
        ListNode *p,*q,*r;
        if(pHead==NULL || pHead->next==NULL){
            return pHead;
        }else{
            p=pHead;
            q=p->next;
            pHead->next=NULL;
            while(q!=NULL){
                r=q->next;
                q->next=p;
                p=q;
                q=r;
            }
            return p;
        }
    }
    ```

  - 每隔n个链表反转
  
    ```cpp
    /*
    * Definition for singly-linked list.
    * struct ListNode {
    *     int val;
    *     ListNode *next;
    *     ListNode(int x) : val(x), next(NULL) {}
    * };
    */
    class Solution {
    public:
        ListNode* reverseKGroup(ListNode* head, int k) {
            if(!head || !head -> next){
                return head;
            }
            ListNode* ptr, *nptr = NULL, *pptr = NULL;
            int size = 0;
            int count = 0;
            ptr = head;
            while(ptr){
                ptr = ptr -> next;
                size++;
            }
            if(size < k){
                return head;
            }
            ptr = head;
            while(ptr && count < k){
                nptr = ptr -> next;
                ptr -> next = pptr;
                pptr = ptr;
                ptr = nptr;
                count++;
            }
            if(nptr){
                head -> next = reverseKGroup(nptr, k);
            }
            return pptr;
        }
    };
    ```

- 树
- 图

## 图形学

- VAO，VBO分别是什么  
  
  VAO:顶点数组对象  
  VBO:顶点缓冲对象  
  VBO可以理解为一个缓冲容器，将需要渲染的顶点放在缓存中等待GPU去取用。VAO可以看作一个存放VBO的容器，VAO中存放着这次渲染需要使用的所有VBO的信息，VAO把VBO存放到特定位置，在需要时直接在这个位置取用VBO。  

- 渲染管线  
  
  3D坐标转换为2D坐标的处理过程。分为两个主要部分，第一部分为将3D坐标转变为2D坐标，第二部分为把2D坐标转变为实际有颜色的像素。  
  渲染管线一共分为六个阶段，顶点着色器（可自定义），图元装配，几何着色器（可自定义），光栅化，片段着色器（可自定义），测试与混合。  
  顶点着色器是将一个单独顶点作为输入，把3D坐标转换为另一种3D坐标，同时顶点着色器允许对顶点属性进行一些基本处理。  
  图元装配是将顶点着色器输出的所有顶点作为输入，并将所有的点装配成制定图元的形状。  
  几何着色器将图元装配的输出的一系列顶点的集合作为输入，可以通过产生新的顶点构造出新的图元来生成其他形状。  
  几何着色器的输出会被传入光栅化阶段，这里会把图元映射为最终屏幕上相应的像素，生成共片段着色器使用的片段，在片段着色器运行之前会进行裁切，将视图以外的所有像素丢弃，以此来提升效率。  
  片段着色器会计算一个像素最终颜色，通常包含3D场景数据（光照，阴影等），这些数据可以被用来计算最终像素的颜色。  
  Alpha测试与混合会检测片段对应的深度值，用来判断这个像素是在其他物体前面还是后面，决定是否应该丢弃。该阶段也会检查alpha值，并对物体进行混合。  

- 坐标变换  
  
  从一个坐标系变换到另一个坐标系，需要用到几个变换矩阵，分别为模型矩阵，观察矩阵，投影矩阵  
  顶点起始于局部坐标，会依次变为世界坐标，观察坐标，裁剪坐标，直到屏幕坐标结束  
  模型矩阵记载了物体的位移，缩放，旋转信息，局部坐标通过与模型矩阵相乘来转换为世界坐标  
  观察矩阵模拟了从摄像机视角观察到的空间，通常由一系列位移和旋转的组合来进行完成，用来将世界坐标变换到观察坐标
  投影矩阵指定了一个范围的坐标，会将指定范围内的坐标变换为标准化坐标的范围，所有在范围外的坐标不会被映射射到-1.0到1.0的范围之内，会被裁剪掉。在这一阶段之后，最终的坐标将会被映射到屏幕空间中，并被转换成片段。  

- 深度缓冲  

  深度缓冲就像颜色缓冲一样，在每个片段中储存了信息，并且通常和颜色缓冲由一样的宽度和高度，深度缓冲是有窗口系统自动创建的，通常会以12，24或32位的float来存储它的深度值，大部分系统中，深度缓冲精度为24位  
  当深度测试被启用的时候，opengl会将一个片段的深度值与深度缓冲内容进行对比，如果测试通过了，深度缓冲会更新深度值，如果测试失败了，片段会被丢弃。  

- 模版测试  

  模版测试根据模版缓冲来进行，一个模版缓冲中，通常每个模版值都是8位，每个像素或者片段能够拥有256种不同的模版值。我们可以通过设定不同的模版值来选择保留或者抛弃这个片段。
  写入模版缓冲的大体步骤如下：
  1. 启用模版缓冲写入  
  2. 渲染物体，更新模版缓冲内容  
  3. 禁用模版缓冲写入  
  4. 渲染物体，这次根据模版缓冲的内容丢弃特定片段  

## 操作系统

- 堆和栈的区别

  C语言的内存模型分为5个区：栈区、堆区、静态区、常量区、代码区。每个区存储的内容如下：
  1. 栈区：存放函数的参数值、局部变量等，由编译器自动分配和释放，通常在函数执行完后就释放了，其操作方式类似于数据结构中的栈。栈内存分配运算内置于CPU的指令集，效率很高，但是分配的内存量有限，比如iOS中栈区的大小是2M。
  2. 堆区：就是通过new、malloc、realloc分配的内存块，编译器不会负责它们的释放工作，需要用程序区释放。分配方式类似于数据结构中的链表。在iOS开发中所说的“内存泄漏”说的就是堆区的内存。
  3. 静态区：全局变量和静态变量（在iOS中就是用static修饰的局部变量或者是全局全局变量）的存储是放在一块的，初始化的全局变量和静态变量在一块区域，未初始化的全局变量和未初始化的静态变量在相邻的另一块区域。程序结束后，由系统释放。
  4. 常量区：常量存储在这里，不允许修改。
  5. 代码区：存放函数体的二进制代码。

  堆和栈的区别：

  1. 堆空间的内存是动态分配的，一般存放对象，并且需要手动释放内存。当然，iOS引入了ARC（自动引用计数管理技术）之后，程序员就不需要用代码管理对象的内存了，之前MRC（手动管理内存）的时候，程序员需要手动release对象。另外，ARC只是一种中间层的技术，虽然在ARC模式下，程序员不需要像之前那么麻烦管理内存，但是需要遵循ARC技术的规范操作，比如使用属性限定符weak、strong、assigen等。因此，如果程序员没有按ARC的规则并合理的使用这些属性限定符的话，同样是会造成内存泄漏的。
  2. 栈空间的内存是由系统自动分配，一般存放局部变量，比如对象的地址等值，不需要程序员对这块内存进行管理，比如，函数中的局部变量的作用范围（生命周期）就是在调完这个函数之后就结束了。这些在系统层面都已经限定住了，程序员只需要在这种约束下进行程序编程就好，根本就没有把这块内存的管理权给到程序员，肯定也就不存在让程序员管理一说。

- 协程是什么

  协程是一种用户态轻量级线程。拥有自己的寄存器上下文和栈，协程调度切换时，将寄存器上下文和栈保存到其他地方，在切换回来的时候回复先前保存的寄存器上下文和栈。因此协程能够保留上一次调用时的状态。协程无法利用多核资源，本质是个单线程，但是由于高扩展性和低成本，适合用于高并发处理。

- 进程与线程的区别

  - 进程是资源（CPU、内存等）分配的基本单位，它是程序执行时的一个实例。程序运行时系统就会创建一个进程，并为它分配资源，然后把该进程放入进程就绪队列，进程调度器选中它的时候就会为它分配CPU时间，程序开始真正运行。
  
  - 线程是程序执行时的最小单位，它是进程的一个执行流，是CPU调度和分派的基本单位，一个进程可以由很多个线程组成，线程间共享进程的所有资源，每个线程有自己的堆栈和局部变量。线程由CPU独立调度执行，在多CPU环境下就允许多个线程同时运行。同样多线程也可以实现并发操作，每个请求分配一个线程来处理。

- 进程间的通信方式

  1. 管道：管道是一种半双工的通信方式，数据只能单向流动，而且只能在具有亲缘关系的进程间使用。进程的亲缘关系通常是指父子进程关系。单进程中的管道几乎没有作用，通常调用pipe的进程之后接着调用fork，来创建父进程和子进程之间的IPC通道。
  2. 信号量：信号量是一个计数器，可以用来控制多个进程对共享资源的访问。它常作为一种锁机制，防止某进程正在访问共享资源时，其他进程也访问该资源。因此，主要作为进程间以及同一进程内不同线程之间的同步手段。不能用来传递复杂信息，若要传递数据需要结合共享内存。
  3. 消息队列：消息队列是由消息的链表，存放在内核中并由消息队列标识符标识。消息队列克服了信号传递信息少、管道只能承载无格式字节流以及缓冲区大小受限等缺点。但是在读的时候需要考虑上一次没有读完数据的问题。
  4. 共享内存：共享内存就是映射一段能被其他进程所访问的内存，这段共享内存由一个进程创建，但多个进程都可以访问。共享内存是最快的 IPC 方式，它是针对其他进程间通信方式运行效率低而专门设计的。它往往与其他通信机制，如信号量，配合使用，来实现进程间的同步和通信。
  5. 套接字：套解口也是一种进程间通信机制，与其他通信机制不同的是，它可用于不同的进程通信。

- 线程间通信方式

  1. 全局变量
  2. Messages消息机制
  3. CEvent对象（MFC中的一种线程通信对象，通过其触发状态的改变实现同步与通信）

- C++实现LRU缓存系统设计

  - 数据结构
    unordered_map + double linked list
    1. 维护一个双向链表，该链表将缓存中的数据块按访问时间从新到旧排列起来。
    2. 使用哈希表保证缓存中数据的访问速度，map中的一个元素包含键值key以及链表中键值为key的迭代器，通过key查找记录的地址，即可O(1)时间内访问链表中的记录。

  - 接口

    1. int get(int key)  
    在哈希表中查找键值为key的元素，如果不存在返回-1，如果存在返回该key对应的value值。  
    实现：首先将键值为key的记录与链表首元交换位置，接着更新哈希表中键值为key的迭代器。
    2. void put(int key, int value)  
    将key，value这条记录放入缓存，如果该记录已经在缓存中，更新该记录到缓存链表头部，如果不再缓存中且缓存未满，插入缓存链表头部，如果缓存满，则删除尾部数据。

## 网络

- TCP的3次握手与4次挥手
  
  - 握手

    第一次握手：客户端发送握手信号SYN=1和SEQ=x（随机产生的序列号）的数据包到服务器，服务器由SYN=1知道，客户端要求建立联机。  
    第二次握手：服务器收到请求后要求确认联机信息，向客户端发送SYN=1，ACK=x+1，以及随机产生的确认端序列号SEQ=y的包。  
    第三次握手：客户端收到后检查ACK是否正确，若正确，客户端会再发送ACK=y+1以及序列号SEQ=z，服务端收到后确认ACK值则连接建立成功。  
    完成三次握手之后客户端与服务器开始传送数据。

  - 为什么需要三次握手

    为了防止已失效的连接请求报文段突然又传送到了服务端，从而产生错误。  

  - 挥手

    由于TCP连接是全双工的，因此每个方向都必须单独进行关闭。  
    第一次挥手：TCP客户端发送一个FIN，用来关闭客户到服务器的数据传送。  
    第二次挥手：服务器收到这个FIN，发回一个ACK，确认序号为收到的序号加1。
    第三次挥手：服务器关闭客户端连接后，再发回一个FIN给客户端。
    第四次挥手：客户端收到服务器端的FIN后，发回ACK报文确认，并将确认序号设置为收到序号加1。

  - 为什么需要四次挥手

    当收到对方的FIN时，仅仅表示对方没有数据发送，但未必我放已经将全部数据发送给了对方，因此需要再次发送FIN来表示确认关闭连接。

- TCP与UDP的区别
  
  TCP是面向链接的协议，在发送数据前，必须和对方建立可靠的链接，一个TCP链接必须经过三次握手才能建立。  
  UDP是一个非链接的协议，传输数据之前源端和终端不建立链接，当需要传送时就简单的抓去来自应用程序的数据，并尽可能快的把它扔到网络上。在发送端，UDP传送数据的速度仅仅受应用程序生成数据的速度，计算机能力和传输带宽的限制，在接收端，UDP把每个消息段放在队列中，应用程序每次从队列中读取一个消息。
  TCP保证数据正确性，UDP可能丢包，TCP保证数据顺序，UDP不保证。

- http和https的区别

  https协议是由ssl+http协议构建的可进行加密传输，身份认证的网络协议，要比http更加安全。
  http是超文本传输协议，信息是明文传输，https则是具有安全性的ssl加密传输协议，两者使用完全不同的连接方式，分别使用80端口和443端口。

  - http协议的工作流程

    1. 连接：首先客户机与服务器需要建立连接，由TCP/IP握手连接实现。只要单击某个超级连接，HTTP工作开始。
    2. 建立连接后，客户机发送一个请求给服务器，请求方式的格式为：统一资源标志符（URL），协议版本号，后边是MIME信息包括请求修饰符，客户机信息和可能的内容。
    3. 应答：服务器接到请求后，给予相应的响应信息，其格式为一个状态行，包括信息的协议版本号，一个成功或错误的代码，后边是MIME信息包括服务器信息，实体信息和可能的内容，客户端接受服务器所返回的信息通过浏览器现实在用户的显示屏上。
    4. 关闭：当应答结束后，浏览器和服务器关闭连接，以保证其他浏览器可以与服务器进行连接。

  - https的工作流程

    1. 浏览器请求连接
    2. 服务器返回证书，证书包含网站地址，加密公钥以及证书的颁发机构等信息。
    3. 浏览器收到证书后开始验证证书合法性，生成随机密码，去除证书中提供的公钥对应随机密码加密，将之前生成的加密随机密码等信息发送给网站。
    4. 服务器收到信息后使用自己的私钥揭秘浏览器用公钥加密的信息，并验证hash是否与浏览器发来的一致，再使用加密的随机对称密码加密一段消息，发送给浏览器。
    5. 浏览器揭秘并计算握手消息的hash，如果与服务器端发来的一致，则握手过程结束，之后所有的通信数据将由之前浏览器生成的随机密码利用对称加密算法加密。
