#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<time.h>

struct Transaction
{
	char username[100];
	time_t timestamp;
	int type;//0-取款，1-存款
	int amount;
	
	struct Transaction * next;
};
typedef struct Transaction Transaction;

//交易记录信息链表 
Transaction * tHead=NULL;
Transaction * tTail=NULL;
Transaction * curTransaction=NULL;

struct Account
{
	char name[100];
	char password[7];
	char username[19];
	char idCard[19];
	char tel[12];
	float money;
	struct Account* next;
};
typedef struct Account Account;
int language;//1-中文,2-English
Account* head=NULL;//指向头结点的指针
Account* tail=NULL;//指向尾结点的指针
Account *curAccount=NULL;//记录当前账户
Account * otherAccount=NULL;//记录对方账户  
int findAccount(Account a)
{
	Account *curp=head;
	while(curp!=NULL)
	{
		if((strcmp(curp->username,a.username)==0)&&(strcmp(curp->password,a.password)==0))
		{
			curAccount=curp;
			return 1; 
		}
		curp=curp->next;
	}
	return 0;
}

void drawMoney()
{
	if(language==1)
	{
		printf("请输入取款金额：");
	    int money;
	    scanf("%d",&money);
	
	    //验证金额
	    if(curAccount->money>=money)
	    {
		    printf("取款成功！\n");
		    curAccount->money-=money;
		
		    //创建结点
		    Transaction * newNode=(Transaction*)malloc(sizeof(Transaction)); 
		
		    //结点初始化
		    newNode->next=NULL;
		    strcpy(newNode->username,curAccount->username);
		    newNode->timestamp=time(NULL);
		    newNode->type=0;
		    newNode->amount=money;
		
	    	//添加结点到链表
		    if(tHead==NULL)
		    {
			    tHead=newNode;
			    tTail=newNode;
		    }
		    else
		    {
			    tTail->next=newNode;
			    tTail=newNode;
		    }
	    } 
	    else
	    {
		    printf("余额不足！\n");
	    }
	}
	else if(language==2)
	{
		printf("enter your draw money：");
	    int money;
	    scanf("%d",&money);
	
	    //验证金额
	    if(curAccount->money>=money)
	    {
		    printf("withdrawal succeeded！\n");
		    curAccount->money-=money;
		
		    //产生交易记录...	
		
		    //创建结点
		    Transaction * newNode=(Transaction*)malloc(sizeof(Transaction)); 
		
		    //结点初始化
		    newNode->next=NULL;
		    strcpy(newNode->username,curAccount->username);
		    newNode->timestamp=time(NULL);
		    newNode->type=0;
		    newNode->amount=money;
		
		    //添加结点到链表
		    if(tHead==NULL)
		    {
			    tHead=newNode;
			    tTail=newNode;
		    }
		    else
		    {
			    tTail->next=newNode;
			    tTail=newNode;
		    }
	    } 
	    else
	    {
		    printf("sorry,your credit is running low！\n");
	    }
	}
}

void saveMoney()
{
	if(language==1)
	{
		printf("请输入存款金额：");
	    int money;
	    scanf("%d",&money);
	    printf("存款成功！\n");
	    curAccount->money+=money;
	}
	else if(language==2)
	{
		printf("enter your save money：");
	    int money;
	    scanf("%d",&money);
	    printf("deposit succeeded！\n");
	    curAccount->money+=money;
	}
}

void updataPassword()//修改密码 
{
	if(language==1)
	{
		printf("请输入旧密码:");
	    char oldpassword[100];
	    scanf("%s",oldpassword);
	    if(strcmp(oldpassword,curAccount->password)==0)
	    {
		    printf("请输入新密码:");
		    scanf("%s",curAccount->password);
		    if(strcmp(curAccount->password,oldpassword)==0)
		    {
			    for(int i=0;i<3;i++)
			    {
				    printf("新密码与旧密码相同，请重新输入:");
			        scanf("%s",curAccount->password);
			        if(strcmp(curAccount->password,oldpassword)!=0)
			        {
			    	    printf("修改成功!");
			    	    break;
				    }
			    }
		    }
		    else
		    {
			    printf("修改成功!\n");
		    }
	    }
	    else
	    {
		    printf("密码错误，不能修改!\n");
	    }
	}
	else if(language==2)
	{
		printf("enter your old code:");
	    char oldpassword[100];
	    scanf("%s",oldpassword);
	    if(strcmp(oldpassword,curAccount->password)==0)
	    {
		    printf("enter your new code:");
		    scanf("%s",curAccount->password);
		    if(strcmp(curAccount->password,oldpassword)==0)
		    {
			    for(int i=0;i<3;i++)
			    {
				    printf("the new code is the same as the old code,re-enter:");
			        scanf("%s",curAccount->password);
			        if(strcmp(curAccount->password,oldpassword)!=0)
			        {
			    	    printf("modified successfully!");
			    	    break;
				    }
			    }
		    }
		    else
		    {
			    printf("modified successfully!\n");
		    }
	    }
	    else
	    {
		    printf("the code is incorrect and cannot be modified!\n");
	    }
	}
}
 
void signIn()//登录
{
	if(language==1)
	{
		for(int i=0;i<3;i++)
	    {
		    Account a;
		    printf("请输入账号:");
		    scanf("%s",a.username);
		    printf("请输入密码:");
		    scanf("%s",a.password);
		    if(findAccount(a))
		    {
		    	printf("登陆成功!\n");
			    return;
		    }
		    else
		    {
			    printf("登陆失败!\n");
		    }
	    }
	}
	else if(language==2)
	{
		for(int i=0;i<3;i++)
	    {
		    Account a;
		    printf("enter your username:");
		    scanf("%s",a.username);
		    printf("enter your code:");
		    scanf("%s",a.password);
		    if(findAccount(a))
		    {
		    	printf("login successful!\n");
			    break;
		    }
		    else
		    {
			    printf("login failed!\n");
		    }
	    }
	}
}

void signUp()//注册
{
	//申请一块堆内存空间，将其地址赋值给指针newNode(创建节点）
	Account* newNode=(Account*)malloc(sizeof(Account));
	//结点赋值
	if(language==1)
	{
	    printf("请输入姓名:\n");
	    scanf("%s",newNode->name);
	    printf("请输入密码:\n");
	    scanf("%s",newNode->password);
	    printf("请输入账号:\n");
	    scanf("%s",newNode->username);
	    printf("请输入身份证:\n");
	    scanf("%s",newNode->idCard); 
	    printf("请输入电话:\n");
	    scanf("%s",newNode->tel);
    }
	else if(language==2)
	{
		printf("enter your name:\n");
	    scanf("%s",newNode->name);
	    printf("enter your code:\n");
	    scanf("%s",newNode->password);
	    printf("enter your account:\n");
	    scanf("%s",newNode->username);
	    printf("enter your idcard:\n");
	    scanf("%s",newNode->idCard); 
	    printf("enter your tel:\n");
	    scanf("%s",newNode->tel);
	}
	newNode->money=0.0f;
	newNode->next=NULL;
	//添加结点到链表
	if(head==NULL)
	{
		head=newNode;
		tail=newNode;
	}
	else
	{
		tail->next=newNode;
		tail=newNode;
	}
	printf("创建成功\n");
} 

//根据卡号查找对方账户，找到返回1，找不到返回0 
int findOtherAccount(char otherUsername[])
{
	Account * curp=head;
	while(curp!=NULL)
	{
		if(strcmp(curp->username,otherUsername)==0)	
		{
			otherAccount = curp; 
			return 1;
		}
		curp=curp->next;
	} 
	return 0;
}

void transfer()
{
	if(language==1)
	{
		printf("请输入对方账户：");
	    char otherUsername[100];
	    scanf("%s",otherUsername);
	
	    //判断账户合法性
	    if(findOtherAccount(otherUsername))
	    {
		    printf("请输入转账金额：");
		    int money;
		    scanf("%d",&money);
		
		    //判断金额的合法性
		    if(curAccount->money>=money)
		    {
			    //转账（当前账户余额减少，对方账户余额增多）
			    curAccount->money -= money;
			    otherAccount->money += money;
			    printf("转账成功!\n");
		    }
		    else
		    {
			    printf("余额不足！\n");
	        }
	    }
    	else
	    {
		    printf("账户不存在\n");
	    }
	}
	else if(language==2)
	{
		printf("enter opposite account：");
	    char otherUsername[100];
	    scanf("%s",otherUsername);
	
	    //判断账户合法性
	    if(findOtherAccount(otherUsername))
	    {
		    printf("enter transfer amount：");
		    int money;
		    scanf("%d",&money);
		
		    //判断金额的合法性
		    if(curAccount->money>=money)
		    {
			    //转账（当前账户余额减少，对方账户余额增多）
			    curAccount->money -= money;
			    otherAccount->money += money;
			    printf("transfer succeeded!\n");
		    }
		    else
		    {
			    printf("sorry,your credit is running low！\n");
	        }
	    }
    	else
	    {
		    printf("account dose not exist\n");
	    }
	}
}

void showMenu()
{
	while(1)
	{
		if(language==1)
		{
			system("cls");
			printf("登录，请按1\n");
			printf("开户，请按2\n");
			printf("修改密码，请按3\n");
			printf("取款，请按4\n");
			printf("存款，请按5\n");
			printf("转账，请按6\n"); 
			printf("退出，请按7\n");
		}
		else if(language==2)
		{
			system("cls");
			printf("sign in,press 1\n");
			printf("sign up,press 2\n");
			printf("change password,press 3\n");
			printf("draw money,press 4\n");
			printf("save money,press 5\n");
			printf("transfer accounts,press 6\n");
			printf("sign out,press 7\n");
			system("pause");
		}
		int n;
		scanf("%d",&n);
		if(n==1)
		{
			system("cls");
		 	signIn();
		 	system("pause");
		}
		else if(n==2)
	    {
	    	system("cls");
			signUp();
			system("pause");
		}
		else if(n==3)
		{
			system("cls");
			signIn();
			updataPassword();
			system("pause"); 
		}
		else if(n==4)
		{
			system("cls");
			signIn();
			drawMoney();
			system("pause");
		}
		else if(n==5)
		{
			system("cls");
			signIn();
			saveMoney();
			system("pause");
		}
		else if(n==6)
		{
			system("cls");
			signIn();
			transfer();
			system("pause");
		} 
		else if(n==7)
		{
			system("cls");
		 	return;
		 	system("pause");
		}
		
	}
}
void printLinkedList()
{
	Account* curP=head;
	while(curP!=NULL)
	{
		printf("%s\t%s\t%s\t%s\t%s\t%f\n",curP->name,curP->password,curP->username,curP->idCard,curP->tel,curP->money);
		curP=curP->next;
	}
}

void loadData()
{
	FILE* fp=fopen("D:/atm.txt","r"); 
	if(fp!=NULL)
	{
		while(!feof(fp))
		{
			//创建结点
			Account *newNode=(Account *)malloc(sizeof(Account));
			
			//结点初始化
			newNode->next=NULL;
			fscanf(fp,"%s\t%s\t%s\t%s\t%s\t%f\n",newNode->name,newNode->password,newNode->username,newNode->idCard,newNode->tel,&newNode->money);
			
			//添加结点到链表 
			if(head==NULL)
			{
				head=newNode;
				tail=newNode;
			}
			else
			{
				tail->next=newNode;
				tail=newNode;
			}
		}
		fclose(fp); 
	}
}

void saveData()
{
	FILE* fp=fopen("D:/atm.txt","w");
	if(fp!=NULL)
	{
		Account* curP=head;
	    while(curP!=NULL)
	    {
		    fprintf(fp,"%s\t%s\t%s\t%s\t%s\t%f\n",curP->name,curP->password,curP->username,curP->idCard,curP->tel,curP->money);
		    curP=curP->next;
	    }
	    fclose(fp);
	}
}

int main()
{
	loadData();//加载账户信息 
	printf("中文服务，请按1\n");
	printf("English Service,press 2\n");
	scanf("%d",&language);
	
	showMenu();
	
	printLinkedList();
	saveData();
	
	return 0;
}
 
