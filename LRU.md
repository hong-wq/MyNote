# LRU

---

* Least Recently Used

  最近最少使用，（最近使用的是有用的，很久没访问的是无用的），会选择淘汰最近最少使用的数据

* 使用

  * get(key):得到对应key的值
  * put(key,val):若存在key，则修改key的值，若不存在则插入

* 设计

  1. cache 要求是有序的，以区分最近使用和未使用的，并且能快速删除元素

     ->使用双向链表

     c++能用list实现，因为list底层的实现就是双向链表，某个节点可以用list的迭代器来表示

  2. cache要求能够快速找到对应key值

     ->使用map

  3. 结合成新的数据结构哈希链表

  4. 为啥要双向呢？

     删除链表中的一个节点需要修改其前驱节点的指针，因为使用了hash来快速定位节点的位置，若使用单链表只知道该节点的位置，无法快速知道其前驱是谁(需要遍历)，而双链表有prev指针，能够快速定位并修改

* [LRU缓存](https://leetcode.cn/problems/lru-cache/submissions/)

  ```c++
  struct Node
  {				//C++声明结构体是可以不加struct
      Node* next;
      Node* prev;
      int key, val;
      Node():next(nullptr),prev(nullptr),key(0),val(0){}
      Node(int k, int v):prev(nullptr),next(nullptr),key(k),val(v){}
  };
  
  class DoubleLink
  {
  public:
      DoubleLink(){
          head = new Node();
          tail = new Node();
          head->next = tail;
          tail->prev = head;
          sz = 0;
      }
      ~DoubleLink(){
          destory();     
      }
      void delNode(Node* x){
          x->prev->next = x->next;
          x->next->prev = x->prev;
          --sz;
      }
      
      Node* pop_back(){
          Node* x = tail->prev;
          delNode(x);
          return x;
      }
      void push_front(Node* x){
          x->prev = head;
          x->next = head->next;
          head->next->prev = x;
          head->next = x;
          ++sz;
      }
      void destory(){
          Node* p = head;
          while(p){
              Node* tmp = p->next;
              delete p;
              p = tmp;
          }
      }
      const Node* getBack() const{return tail->prev;} 
      const size_t size() const{ return sz;}
  private:
      Node* head;
      Node* tail;
      int sz;
  };
  
  class LRUCache {
  public:
      LRUCache(int ca):capacity(ca) {}
      ~LRUCache(){}
      int get(int key) {
          if(!mp.count(key)) return -1;
          Node* tmp = mp[key];
          list.delNode(tmp);
          mp.erase(key);
          list.push_front(tmp);
          mp[key] = tmp;
          return tmp->val;
      }
      
      void put(int key, int value) {
          if(mp.count(key)){
              Node* tmp = mp[key];
              list.delNode(tmp);
              tmp->val = value;
              mp.erase(key);
              list.push_front(tmp);
              mp[key] = tmp;
          }else{
              Node* tmp = new Node(key,value);//注意啊,有new一定要delete
              if(mp.size() == capacity){
                  mp.erase(list.getBack()->key);
                  Node* tmp = list.pop_back();    
                  delete tmp;     //完全不要tmp了
              }
              list.push_front(tmp);
              mp[key] = tmp;
          }
      }
  private:
      DoubleLink list;
      unordered_map<int, Node*>mp;
      int capacity;
  };
  
  ```

  

* 运用

* 缺点

* 