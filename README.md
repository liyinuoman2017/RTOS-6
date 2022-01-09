# RTOS-6
**嵌入式实时操作系统6——链表数据结构**

- **链表结构的作用**

在《RTOS系列5——就绪表》中描述了操作系统内核中的就绪表使用了链表结构，就绪表的框图如下：

![在这里插入图片描述](https://img-blog.csdnimg.cn/be52098457d84af7816f743afaf5058b.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAbGl5aW51bzIwMTc=,size_17,color_FFFFFF,t_70,g_se,x_16)

在操作系统内核中不仅仅是就绪表使用了链表结构，等待表和挂起表也都用到了链表结构。
链表数据结构有以下优点：

> **1、在保留原有物理顺序的情况下，插入和删除速度快，效率高。插入和删除只需要改变几个指针变量**。
> **2、链表中的表项数量没有上限。存储的表项上限只与内存空间大小有关，理论上如果内存无限大，链表中的表项可以动态增加到无限个。**
> **3、动态分配内存，需要用多少个表项，就分配几个表项，不需要预先分配内存，不存在内存浪费的情况。**

**分析：**

**1、插入和删除效率高**

![在这里插入图片描述](https://img-blog.csdnimg.cn/d4bc6e9b2f5241ffadd5055e99bee878.png)

如果要在元素“3”之后插入一个“8”，就需要移动元素“3”之后的“14179”5个元素。

![在这里插入图片描述](https://img-blog.csdnimg.cn/0639b38e4eb64d5b8ac44d0cc7210f59.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAbGl5aW51bzIwMTc=,size_17,color_FFFFFF,t_70,g_se,x_16)

使用链表结构时，只有改变4个指针的数据既可。如果元素有很多个时，链表的这种操作的将会带来极高的效率。

**2、链表中的表项数量没有上限**

![在这里插入图片描述](https://img-blog.csdnimg.cn/079022bbf69f44d9a2e879e3fec91f96.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAbGl5aW51bzIwMTc=,size_19,color_FFFFFF,t_70,g_se,x_16)

如果内存无限大，链表中的表项可以动态增加到无限个，不会出现**元素满**的情况。

**3、动态分配内存**

![在这里插入图片描述](https://img-blog.csdnimg.cn/5ce2cad4836b41238c666a7f80276410.png)

分配内存，不需要预先分配内存，不存在内存浪费的情况。

 - **链表数据结构**
 
链表分为单向链表和双向链表，这两种链表的结构如下：

![在这里插入图片描述](https://img-blog.csdnimg.cn/d50c4451356349b4bdf73aa0d75f8b81.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAbGl5aW51bzIwMTc=,size_19,color_FFFFFF,t_70,g_se,x_16)

双向链表中的每个对象中包含：关键字key和两个指针：next和prev，next是指向下一个后继元素的指针，prev是指向上一个前驱元素的指针，data为用户数据。
单向链表中的每个对象中包含：关键字key和一个指针：next，next是指向下一个后继元素的指针，data为用户数据。
关键字key用于查找对象和对象排序。
链表有多种形式，它可以是排序的，可以是未排序的，可以是循环的，可以是非循环的。
如果链表是排序的，链表的线性顺序和链表元素中的关键字key的线性顺序一致。在循环列表中，表头元素的prev指针指向表尾元素，表尾元素的next指针指向表头元素。
链表的基本操作是：
**1、查询一个元素
2、插入一个元素
3、删除一个元素**
操作系统内核中通常使用的链表为双向循环链表。双向循环链表的结构如下图：

![在这里插入图片描述](https://img-blog.csdnimg.cn/d62182ecbb544abba627b322c1e80746.png)

 - **链表实现**

链表有两种常见的实现方式：
**1、链表中包含用户数据。
2、用户数据中包含链表。**
C语言实现如下：

![在这里插入图片描述](https://img-blog.csdnimg.cn/dcabae382c714e9187ded582a95810c9.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAbGl5aW51bzIwMTc=,size_19,color_FFFFFF,t_70,g_se,x_16)

这两种实现的链表的结构框图如下：

![在这里插入图片描述](https://img-blog.csdnimg.cn/a9805147f68347b186be48bea30a0c56.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAbGl5aW51bzIwMTc=,size_19,color_FFFFFF,t_70,g_se,x_16)

链表中包含用户数据的方式最大的问题是：当用户数据改变时，整个链表结构也会改变，不同的用户数据需要定义不同的链表结构。
用户数据中包含链表的方式可以通用一种链表结构，用户数据可以不同，**操作系统内核种通常使用用户数据中包含链表的方式。**

 - **万能药**

上文提到用户数据中包含链表的方式可以通用一种链表结构，可以适应不同的用户数据。但是世界上没有万能药，这种链表形式同样存在一个问题：

![在这里插入图片描述](https://img-blog.csdnimg.cn/748483fd50b54cff81591bef8b1aa648.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAbGl5aW51bzIwMTc=,size_19,color_FFFFFF,t_70,g_se,x_16)

由于链表指针指向的是另外一个链表对象，此时我们将无法得到整个数据对象的地址。由上图可知我们通过next指针可以得到下一个list元素的地址，但是我们无法获取整个数据对象的地址。
办法总比困难多！我们有两种常用的方法解决“无法获取整个数据对象的地址”的问题：
**1、指针法。
2、地址反推法。**

**指针法**
指针法是在链表结构种增加一个void *  owner指针，用这个指针指向整个数据对象的地址,因为是void * 类型，owner指针可以指向任意类型的对象。C语言代码实现如下：

![在这里插入图片描述](https://img-blog.csdnimg.cn/b6518ff3055c48078ead65655605f9b5.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAbGl5aW51bzIwMTc=,size_19,color_FFFFFF,t_70,g_se,x_16)

这种链表的结构框图如下：

![在这里插入图片描述](https://img-blog.csdnimg.cn/49f0aa5cbe494f28b32d8065b277c7d0.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAbGl5aW51bzIwMTc=,size_19,color_FFFFFF,t_70,g_se,x_16)

在指针法中，使用链表结构中的owner指针可以定位到整个数据对象的地址。在小型的实时操作系统中通常使用这种方式。

**地址反推法**
地址反推法是**已知数据对象的类型和链表的地址**，可以反推得到数据对象的地址。C语言代码实现如下：

![在这里插入图片描述](https://img-blog.csdnimg.cn/2fb804f466cc46c7934bf4e83363a457.png)

container_of是可以根据结构体的成员变量获取所在结构体的首地址，这种方法的原理是：编译器记录了每一个结构体类型数据中的每一个元素的偏移地址，现在已知一个结构体类型，和该结构体内的一个元素地址（链表元素地址），就能反推出该结构体对象的地址。
**linux内核常用这种地址反推法定位数据对象的地址**。

 - **哨兵**

用代码演示一下向链表中插入对象：

![在这里插入图片描述](https://img-blog.csdnimg.cn/282dab2222fb4714a34cffa9e28ff57e.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAbGl5aW51bzIwMTc=,size_19,color_FFFFFF,t_70,g_se,x_16)

以上代码在一个演示了在一个空链表中插入了第一个对象。非空链表中插入对象是同样的操作吗？代码如下：

![在这里插入图片描述](https://img-blog.csdnimg.cn/0fbed06ae03e47898401c12e89ec424a.png)

由此可见链表在空状态和非空状态插入和删除对象的操作是不同的，这样就给提高了程序的复杂度，需要在插入和删除对象前判断链表是否为“空”。
**哨兵机制**（**表头机制**）可以解决这个问题。哨兵是一个哑对象，作用是简化边界条件处理。使用哨兵可以使得代码紧凑，使用哨兵机制的链表如下如：

![在这里插入图片描述](https://img-blog.csdnimg.cn/815e9e5f897d45a4878345e230e0010e.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAbGl5aW51bzIwMTc=,size_19,color_FFFFFF,t_70,g_se,x_16)

使用哨兵机制（表头机制）后链表在空状态和非空状态插入和删除对象的操作是相同的，这样可以使得代码紧凑，清晰。

 - **深入分析FREERTOS就绪表**

FreeRTOS就绪表的源码如下：

![在这里插入图片描述](https://img-blog.csdnimg.cn/32ee06cbff694004b6b15b41d4de85d3.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAbGl5aW51bzIwMTc=,size_19,color_FFFFFF,t_70,g_se,x_16)

FreeRTOS中的TCB结构如下：

![在这里插入图片描述](https://img-blog.csdnimg.cn/861cc20ac0304340b929954945e83b0b.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAbGl5aW51bzIwMTc=,size_19,color_FFFFFF,t_70,g_se,x_16)


> **FreeRTOS中的就绪表特点：**
>  **1、使用的是用户数据中包含链表的方式。**
>   **2、使用指针法定位对象地址。** 
>   **3、使用哨兵机制（表头机制）。**


**分析：**
**1、用户数据中包含链表**

![在这里插入图片描述](https://img-blog.csdnimg.cn/f55fe645bc564fe5aaef651a7b3772c4.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAbGl5aW51bzIwMTc=,size_19,color_FFFFFF,t_70,g_se,x_16)

FreeRTOS中在TCB数据结构中加入了链表对象ListItem_t ，这种使用方法是用户数据中包含链表的方式。

**2、指针法定位对象地址**

![在这里插入图片描述](https://img-blog.csdnimg.cn/f91cc5d5fa5545e0ab4677a7a9abdbd3.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAbGl5aW51bzIwMTc=,size_19,color_FFFFFF,t_70,g_se,x_16)

FreeRTOS中xLIST_ITEM中加入了void * pvOwner指针用于定位包含链表的对象的地址，这种使用方法是指针法定位对象地址。

**3、哨兵机制**

![在这里插入图片描述](https://img-blog.csdnimg.cn/a132ba159e5741cfae387baa9d38e609.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAbGl5aW51bzIwMTc=,size_19,color_FFFFFF,t_70,g_se,x_16)

FreeRTOS中MiniListItem_t就是哨兵（表头）。

 - **代码：**
 

```c

/* github: liyinuo2017								author:liwei */
/**********************链表中包含户数据形式 **********************/
struct list_contain_data_def						
{
		
	u32 		key;								/*键值 */
	struct		list_contain_data_def *  next;		/*指向下一个对象*/
	struct		list_contain_data_def *  previous;	/*指向上一个对象*/
	u8			user_data[100]; 					/*用户数据*/

}list_contain_data_t;


/**********************用户数据中包含链表形式 **********************/
struct list_def
{
	u32 		key;					/*键值 */ 	
	struct		list_def *	next;		/*指向下一个对象*/
	struct		list_def *	previous;	/*指向上一个对象*/	
	
}list;		/*链表 */
typedef struct	list	 list_t;  /*链表 */

struct data_contain_list_def
{
		
	u8			user_data[100]; 		/*用户数据*/
	
	list_t		user_list				/*链表*/

}data_contain_list_t;

struct list_def
{
	u32 		key;					/*键值 */ 	
	struct		list_def *	next;		/*指向下一个对象*/
	struct		list_def *	previous;	/*指向上一个对象*/	
	void *		owner;					/*指向链表拥有者 */
	
}list;		/*链表 */
typedef struct	list	 list_t;  /*链表 */

struct data_contain_list_def
{
		
	u8			user_data[100]; 		/*用户数据*/
	
	list_t		user_list				/*链表*/

}data_contain_list_t;


#define container_of(ptr, type, member) ({ \

const typeof( ((type *)0)->member ) *__mptr = (ptr); \

(type *)( (char *)__mptr - offsetof(type,member) );})
	

struct xLIST_ITEM
{
	listFIRST_LIST_ITEM_INTEGRITY_CHECK_VALUE			/*检查位*/
		
	configLIST_VOLATILE TickType_t xItemValue;			/*用于排序的值 */
	struct xLIST_ITEM * configLIST_VOLATILE pxNext; 	/*指向下一项*/
	struct xLIST_ITEM * configLIST_VOLATILE pxPrevious; /*指向上一项*/
	void * pvOwner; 									/*TCB指针 */
	struct xLIST * configLIST_VOLATILE pxContainer; 	/*指向放置此列表项的列表 */

	listSECOND_LIST_ITEM_INTEGRITY_CHECK_VALUE			/*检查位*/
};
typedef struct xLIST_ITEM ListItem_t;					/* 列表项 */

struct xMINI_LIST_ITEM
{
	listFIRST_LIST_ITEM_INTEGRITY_CHECK_VALUE			/*检查位 */
		
	configLIST_VOLATILE TickType_t xItemValue;			/*用于排序的值 */
	struct xLIST_ITEM * configLIST_VOLATILE pxNext; 	/*指向下一项*/
	struct xLIST_ITEM * configLIST_VOLATILE pxPrevious; /*指向上一项*/
};
typedef struct xMINI_LIST_ITEM MiniListItem_t;	   /* MINI列表项 */

typedef struct xLIST
{
	listFIRST_LIST_INTEGRITY_CHECK_VALUE				/*检查位*/
		
	volatile UBaseType_t uxNumberOfItems;				/*列表项总数量*/		
	ListItem_t * configLIST_VOLATILE pxIndex;			/*用于遍历列表*/
	MiniListItem_t xListEnd;							/*列表的最末项*/

	listSECOND_LIST_INTEGRITY_CHECK_VALUE				/*检查位*/
} List_t;												/* 列表 */

#define configMAX_PRIORITIES			( 10 )	

PRIVILEGED_DATA static List_t pxReadyTasksLists[ configMAX_PRIORITIES ];/*就绪表 */


typedef struct tskTaskControlBlock			
{
	volatile StackType_t	*pxTopOfStack;		/*栈指针*/
	ListItem_t				xStateListItem; 	/*任务的状态列表项*/
	ListItem_t				xEventListItem; 	/*< 任务的事件列表项 */
	UBaseType_t 			uxPriority; 		/*优先级*/
	StackType_t 			*pxStack;			/*栈起始地址 */
	char					pcTaskName[ configMAX_TASK_NAME_LEN ];/*任务名 */

	/*省略部分 */

} tskTCB;
```

> <font color=red>**未完待续…
  
实时操作系统系列将持续更新
  
创作不易希望朋友们点赞，转发，评论，关注。
  
您的点赞，转发，评论，关注将是我持续更新的动力
  
作者：李巍
  
Github：liyinuoman2017**
