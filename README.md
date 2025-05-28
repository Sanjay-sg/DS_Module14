# DS_Module14

## AIM: 
~~~
To Implement the program for applications of Queue using c program.
Applications of Queue are :
1) First come First Serve scheduling Algorithm
2) Shortest Job First Scheduling Algorithm
3)Round Robin Algorithm
4) Breadth First search
~~~
## PROGRAMS:
### First come First Serve scheduling Algorithm
```
#include <stdio.h>
// Function to find the waiting time for all processes
int waitingtime(int proc[], int n,
int burst_time[], int wait_time[]) {
   // waiting time for first process is 0
   wait_time[0] = 0;
   // calculating waiting time
   for (int i = 1; i < n ; i++ )
   wait_time[i] = burst_time[i-1] + wait_time[i-1] ;
   return 0;
}
// Function to calculate turn around time
int turnaroundtime( int proc[], int n,
int burst_time[], int wait_time[], int tat[]) {
   // calculating turnaround time by adding
   // burst_time[i] + wait_time[i]
   int i;
   for ( i = 0; i < n ; i++)
   tat[i] = burst_time[i] + wait_time[i];
   return 0;
}
//Function to calculate average time
int avgtime( int proc[], int n, int burst_time[]) {
   int wait_time[n], tat[n], total_wt = 0, total_tat = 0;
   int i;
   //Function to find waiting time of all processes
   waitingtime(proc, n, burst_time, wait_time);
   //Function to find turn around time for all processes
   turnaroundtime(proc, n, burst_time, wait_time, tat);
   //Display processes along with all details
   printf("Processes  Burst   Waiting  Turn around \n");
   // Calculate total waiting time and total turn
   // around time
   for ( i=0; i<n; i++) {
      total_wt = total_wt + wait_time[i];
      total_tat = total_tat + tat[i];
      printf(" %d\t\t   %d\t\t    %d\t\t   %d\n", i+1, burst_time[i], wait_time[i], tat[i]);
   }
   printf("Average waiting time = %f\n", (float)total_wt / (float)n);
   printf("Average turn around time = %f\n", (float)total_tat / (float)n);
   return 0;
}
// main function
int main() {
    int k,m;
    int proc[10];
    int burst_time[10]; 
   //process id's
   scanf("%d",&m);
   for(k=0;k<m;k++)
   {
     scanf("%d",&proc[k]);  
   }
   ;
   for(k=0;k<m;k++)
   {
     scanf("%d",&burst_time[k]);  
   }
 
   avgtime(proc, m, burst_time);
   return 0;
}

```
### Shortest Job First Scheduling Algorithm
```
#include<stdio.h>
 
int main()
{
    int bt[20],p[20],wt[20],tat[20],i,j,n,total=0,temp;//pos,temp;
    float avg_wt,avg_tat;
    //printf("Enter number of process:");
    scanf("%d",&n);
 
   // printf("\nEnter Burst Time:\n");
    for(i=0;i<n;i++)
    {
      //  printf("p%d:",i+1);
        scanf("%d",&bt[i]);
        p[i]=i+1;           //contains process number
    }
 
    //sorting burst time in ascending order using selection sort
   //type your code here....
   for(i=0;i<n-1;i++)
   {
      for(j=i+1;j<n;j++)
      {
         if(bt[i]>bt[j])
         {
            //swapting bt
            temp=bt[i];
            bt[i]=bt[j];
            bt[j]=temp;
            // swaping process
            temp=p[i];
            p[i]=p[j];
            p[j]=temp;
            
            
            
         }
      }
   }
 
    wt[0]=0;            //waiting time for first process will be zero
 
    //calculate waiting time
    for(i=1;i<n;i++)
    {
        wt[i]=0;
        for(j=0;j<i;j++)
            wt[i]+=bt[j];
 
        total+=wt[i];
    }
 
    avg_wt=(float)total/n;      //average waiting time
    total=0;
 
    printf("Process    Burst Time    Waiting Time  Turnaround Time\n");
    for(i=0;i<n;i++)
    {
        tat[i]=bt[i]+wt[i];     //calculate turnaround time
        total+=tat[i];
        //printf("\n");
        printf("p%d          %d               %d             %d\n",p[i],bt[i],wt[i],tat[i]);
    }
 
    avg_tat=(float)total/n;     //average turnaround time
   // printf("\n");
    printf("Average Waiting Time=%f\n",avg_wt);
  //  printf("\n");
    printf("Average Turnaround Time=%f\n",avg_tat);
    return 0;
}

```
### Round Robin Algorithm
```
#include<stdio.h>  

int main()  
{  
    // initlialize the variable name  
    int i, NOP, sum=0,count=0, y, quant, wt=0, tat=0, at[10], bt[10], temp[10];  
    float avg_wt, avg_tat;  
   // printf(" Total number of process in the system: ");  
    scanf("%d", &NOP);  
    y = NOP; // Assign the number of process to variable y  
  
// Use for loop to enter the details of the process like Arrival time and the Burst Time  
for(i=0; i<NOP; i++)  
{  
//printf("\n Enter the Arrival and Burst time of the Process[%d]\n", i+1);  
//printf(" Arrival time is: \t");  // Accept arrival time  
scanf("%d", &at[i]);  
//printf(" \nBurst time is: \t"); // Accept the Burst time  
scanf("%d", &bt[i]);  
temp[i] = bt[i]; // store the burst time in temp array  
}  
// Accept the Time quantum  
//printf("Enter the Time Quantum for the process: \t");  
scanf("%d", &quant);  
// Display the process No, burst time, Turn Around Time and the waiting time  
printf("  Process No     Burst Time     TAT       Waiting Time \n");  
for(sum=0, i = 0; y!=0; )  
{  
if(temp[i] <= quant && temp[i] > 0) // define the conditions   
{  
    sum = sum + temp[i];  
    temp[i] = 0;  
    count=1;  
    }     
    else if(temp[i] > 0)  
    {  
        temp[i] = temp[i] - quant;  
        sum = sum + quant;    
    }  
    if(temp[i]==0 && count==1)  
    {  
        y--; //decrement the process no.  
        printf("  Process No[%d]    %d    %d    %d\n", i+1, bt[i], sum-at[i], sum-at[i]-bt[i]);  
        wt = wt+sum-at[i]-bt[i];  
        tat = tat+sum-at[i];  
        count =0;     
    }  
    if(i==NOP-1)  
    {  
        i=0;  
    }  
    else if(at[i+1]<=sum)  
    {  
        i++;  
    }  
    else  
    {  
        i=0;  
    }  
}  
// represents the average waiting time and Turn Around time  
avg_wt = wt * 1.0/NOP;  
avg_tat = tat * 1.0/NOP;  
printf("\n");
printf(" Average Turn Around Time:  %f\n", avg_wt);  
printf(" Average Waiting Time:  %f\n", avg_tat);  
return 0;
}  

```
### Breadth First Search Algorithm 
```
#include <stdio.h>
#include <stdlib.h>
#define SIZE 40
 
struct queue {
  int items[SIZE];
  int front;
  int rear;
};
 
struct queue* createQueue();
void enqueue(struct queue* q, int);
int dequeue(struct queue* q);
void display(struct queue* q);
int isEmpty(struct queue* q);
void printQueue(struct queue* q);
 
struct node {
  int vertex;
  struct node* next;
};
 
struct node* createNode(int);
 
struct Graph {
  int numVertices;
  struct node** adjLists;
  int* visited;
};
void enqueue(struct queue* q, int value) {
  if (q->rear == SIZE - 1)
    printf("Queue is Full!!");
  else {
    if (q->front == -1)
      q->front = 0;
    q->rear++;
    q->items[q->rear] = value;
  }
} 
```
## OUPUT 
### FIRST COME FIRST SERVE SCHEDULING 
![image](https://github.com/user-attachments/assets/f963ffed-5d2c-4a83-82ca-2aad3bcc9af8)
###  Shortest Job First Scheduling Algorithm
![image](https://github.com/user-attachments/assets/9e6697f0-9e74-4ecd-ad54-3197a8942d52)

### ROUND ROBIN ALGORITHM 
![image](https://github.com/user-attachments/assets/fc92164e-1341-47bb-88f0-c0ee9d097333)

### BREADtH FIRST SEARCH 
![image](https://github.com/user-attachments/assets/4da59c50-ed99-4302-ab96-95aa02c9ff69)



