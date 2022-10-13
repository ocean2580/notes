# 节点

```c
 typedef struct node{
 	int data;
 	struct node* next;
 }Node, *Link;
```



# 数组转链表

```C
Link ArrToLL_H(Link head, int a[],int n){
 	head =(Link)calloc(1,sizeof(Node));
 	
 	for(int i=0;i<n;i++){
 		Link node = (Link)calloc(1,sizeof(Node));//temp!!!
 		node->data = a[i];//data
 		node->next = head->next;//next
 		head->next = node;//node
	 }
	 return head;
 }
 
 Link ArrToLL_R(Link head, int a[],int n){
 	head =(Link)calloc(1,sizeof(Node));
 	Link p = head;
 	
 	for(int i=0;i<n;i++){
 		Link node = (Link)calloc(1,sizeof(Node));//temp!!!
 		node->data = a[i];//data
 		node->next = p->next;//next
 		p->next = node;//node
 		
 		p = node;//(move p) p = p->next;
	 }
	 return head;
 }
 
```

# 遍历

```c
void Traverse(Link head){
 	Link p = head->next;

	while(p){
		printf("%d\t",p->data);
		p = p->next;//(move) 存储空间不一定连续，不可 p++ 后移。		 
	}
 }
 
```

# 清除释放

```c
void ClearLL(Link head){
 	 Link p;
 	 
	 while(head->next){
	 	p = head;
	 	head = head->next;
	 	free(p);
	 }
	 free(head);
 }
```

# 长度

```C
 int Length(Link head){
 	Link p = head;
 	int count = 0;
 	
 	while(p){
 		p = p->next;
		 count ++;
	 }
	 
	 return (count-1);//除去head 
 }
```

# 查询

```C
void Query(Link head,int x){
 	Link p = head->next;
 	int flag = 0;//存在性
 	int count = 0;
 	int i;
	  
 	while(p){
 		count ++;
 		
 		if(p->data == x){
 			i = count;
 			flag = 1;
            break;
		 }
		 
 		p = p->next;
	 }
	 
	  if(flag){
	  	printf("\n存在，位于第%d个节点。",i);
	  } else{
	  	printf("\n不存在。");
	  }
 }
```

# 增加

```c
 void Insert(Link head,int i,int x){
 	Link p = head;
 	int count = 0;
 	int flag = 0;
 	
 	while((p != NULL) && (count < i- 1)){// i~ 第几个节点 
 		p = p->next;
 		count ++;
	 }
	 
	 if(p == NULL) flag = 0;
	 else{
	 	Link node =(Link)calloc(1,sizeof(Node));
	 	node->data = x;
	 	node->next = p->next;
	 	p->next = node;
	 	
	 	flag = 1;
	 } 
	 
	 if(flag) printf("\n插入指定位置成功！\n");
	 else  printf("\n插入指定位置失败。\n");
 	
 }

 void add(Link head,int e){ 
 	Link p = head;
 	Link q = (Link)malloc(sizeof(Node));
 	while(p->next){ //move to rear
 		p = p->next;
	 }
	 
 	q->data = e;
 	q->next = p->next;
 	p->next = q;
 	
 }
```

# 输入数据处理成链表

```C
Link NewLL_H(Link head,Link node){
 	head = (Link)calloc(1,sizeof(Node));
 	int flag = 1;
 	int x;
 	 
 	printf("请输入：\n");
 	while(flag){
 		node = (Link)calloc(1,sizeof(Node));
 		
 		scanf("%d",&x);//data
 		node->data = x;
 		node->next = head->next;
 		head->next = node;
 		printf("还要继续输入吗？（按0停止输入)   ");
 		scanf("%d",&flag);
	 }
	 return head;
 }
 
int main(){
 	Link node;
 	Link head;
    head = NewLL_H(head,node); 
    
    ......
        
    return 0;
}
```









