## 题目

1. 编写一个程序，开启3个线程，这3个线程的ID分别为A、B、C，每个线程将自己的ID在屏幕上打印10遍，要求输出结果必须按ABC的顺序显示；如：ABCABC….依次递推。

   ``` c++
   std::mutex m;
   std::condition_variable cv;
   char g_char = 0;
   void print_fun(char ch)
   {
       char ch_ = ch - 'A';
       for (int i = 0; i < 10; ++i) {
           std::unique_lock lk(m);
           cv.wait(lk, [=]{return ch_ == g_char;});
           std::cout<<ch<<std::endl;
           g_char = (ch_+1)%3;
           cv.notify_all();
       }
   }
   
   int main() {
       std::vector<std::thread> threads{};
       threads.push_back(std::thread(print_fun, 'A'));
       threads.push_back(std::thread(print_fun, 'B'));
       threads.push_back(std::thread(print_fun, 'C'));
       std::for_each(threads.begin(), threads.end(), std::mem_fn(&std::thread::join));
       return 0;
   }
   ```

   

2. 子线程循环 10 次，接着主线程循环 100 次，接着又回到子线程循环 10 次，接着再回到主线程又循环 100 次，如此循环50次，试写出代码

   ``` c++
   std::mutex m;
   std::condition_variable cv;
   int flag = 10;
   
   void print_fun(int count)
   {
       for (int j = 0; j <50; ++j) {
           std::unique_lock lk(m);
           cv.wait(lk, [=]{return flag==count;});
           for (int i = 0; i <count; ++i) {
               std::cout<<i;
           }
           std::cout<<"\n----------"<<std::endl;
           flag = (count == 10)?50:10;
           cv.notify_one();
       }
   }
   
   int main() {
       std::thread f(print_fun, 10);
       print_fun(50);
       f.join();
       return 0;
   }
   ```

3. 有四个线程1、2、3、4。线程1的功能就是输出1，线程2的功能就是输出2，以此类推.........现在有四个文件ABCD。初始都为空。现要让四个文件呈如下格式：

   A：1 2 3 4 1 2....

   B：2 3 4 1 2 3....

   C：3 4 1 2 3 4....

   D：4 1 2 3 4 1....

   ``` c++
   std::mutex m;
   std::condition_variable cv;
   int flag=0;
   
   void print_fun(int num)
   {
       for (int j = 0; j <50; ++j) {
           std::unique_lock lk(m);
           cv.wait(lk, [=]{return flag==num;});
           std::cout<<"thread "<< num+1<<std::endl;
           flag = (flag + 1) % 4;
           cv.notify_all();
       }
   }
   
   int main() {
       std::vector<std::thread> v;
       v.push_back(std::thread(print_fun, 0));
       v.push_back(std::thread(print_fun, 1));
       v.push_back(std::thread(print_fun, 2));
       v.push_back(std::thread(print_fun, 3));
       for(auto& i : v)
           i.join();
       return 0;
   }
   ```

4. 线程安全stack

   ``` c++
   #include <exception>
   #include <memory>
   #include <mutex>
   #include <stack>
   
   struct emptyStack : std::exception
   {
       const char* what() const noexcept
       {
           return "empty stack!";
       }
   };
   
   template<typename T>
   class A {
       std::stack<T> s;
       mutable std::mutex m;
   public:
       A() : s(std::stack<T>()) {}
   
       A(const A& rhs)
       {
           std::lock_guard<std::mutex> l(rhs.m);
           s = rhs.s;
       }
   
       A& operator=(const A&) = delete;
   
       void push(T n)
       {
           std::lock_guard<std::mutex> l(m);
           s.push(std::move(n));
       }
   
       std::shared_ptr<T> pop() // 返回一个指向栈顶元素的指针
       {
           std::lock_guard<std::mutex> l(m);
           if (s.empty()) throw emptyStack();
           const std::shared_ptr<T> res(std::make_shared<T>(std::move(s.top())));
           s.pop();
           return res;
       }
   
       void pop(T& n) // 传引用获取结果
       {
           std::lock_guard<std::mutex> l(m);
           if (s.empty()) throw emptyStack();
           n = std::move(s.top());
           s.pop();
       }
   
       bool empty() const
       {
           std::lock_guard<std::mutex> l(m);
           return s.empty();
       }
   };
   ```

5. 线程安全queue

   ``` c++
   #include <memory>
   #include <mutex>
   #include <condition_variable>
   #include <queue>
   
   template<typename T>
   class A {
       mutable std::mutex m;
       std::queue<std::shared_ptr<T>> q; // 之前为std::queue<T> q
       std::condition_variable cv;
   public:
       A() {}
       A(const A& rhs)
       {
           std::lock_guard<std::mutex> l(rhs.m);
           q = rhs.q;
       }
   
       void push(T x)
       {
           std::shared_ptr<T> data(std::make_shared<T>(std::move(x)));
           // 上面的构造在锁外完成，之前只能在pop中且持有锁时完成
           // 内存分配操作开销很大，这种做法减少了mutex的持有时间，提升了性能
           std::lock_guard<std::mutex> l(m);
           q.push(data); // 之前为q.push(std::move(x))
           cv.notify_one();
       }
   
       void wait_and_pop(T& x)
       {
           std::unique_lock<std::mutex> l(m);
           cv.wait(l, [this] { return !q.empty(); });
           x = std::move(*q.front()); // 之前为std::move(q.front())
           q.pop();
       }
   
       std::shared_ptr<T> wait_and_pop()
       {
           std::unique_lock<std::mutex> l(m);
           cv.wait(l, [this] { return !q.empty(); });
           std::shared_ptr<T> res = q.front();
           // 之前为std::make_shared<T>(std::move(q.front()))
           // 现在构造转移到了push中
           q.pop();
           return res;
       }
   
       bool try_pop(T& x)
       {
           std::lock_guard<std::mutex> l(m);
           if (q.empty()) return false;
           x = std::move(*q.front()); // 之前为std::move(q.front())
           q.pop();
           return true;
       }
   
       std::shared_ptr<T> try_pop()
       {
           std::lock_guard<std::mutex> l(m);
           if (q.empty()) return std::shared_ptr<T>();
           std::shared_ptr<T> res = q.front();
           // 之前为std::make_shared<T>(std::move(q.front()))
           q.pop();
           return res;
       }
   
       bool empty() const
       {
           std::lock_guard<std::mutex> l(m);
           return q.empty();
       }
   };
   ```

   

