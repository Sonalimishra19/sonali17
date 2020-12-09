BFS:
#include <stdio.h>
#include <stdlib.h>
#include <stdbool.h>

#define NOV 8

struct vertex
{
   char label;
   bool visited;
};

//queue variables

int queue [NOV];
int rear = -1;
int front = 0;
int queueVertexCount = 0;

struct vertex* vertices[NOV];
int adjMatrix[NOV][NOV];
int vertexCount = 0;

void enqueue(int data) 
{
   queue[++rear] = data;
   queueVertexCount++;
}

int dequeue() 
{
   queueVertexCount--;
   return queue[front++]; 
}

bool isQueueEmpty() 
{
   return queueVertexCount == 0;
}

void addVertex(char label) 
{
   struct vertex* ver = (struct vertex*) malloc(sizeof(struct vertex));
   ver->label = label;  
   ver->visited = false;     
   vertices[vertexCount++] = ver;
}

void addEdge(int start,int end) 
{
   adjMatrix[start][end] = adjMatrix[end][start] = 1;
}

void displayVertex(int vertexIndex) 
{
   printf("%c ",vertices[vertexIndex]->label);
} 

int getAdjUnvisitedVertex(int vertexIndex) 
{
   int i;
	
   for(i = 0; i<vertexCount; i++) 
   {
      if(adjMatrix[vertexIndex][i] == 1 && vertices[i]->visited == false)
         return i;
   }
	
   return -1;
}

void BFS() 
{
   int i;

   vertices[0]->visited = true;

   displayVertex(0);

   enqueue(0);
   int unvisitedVertex;

   while(!isQueueEmpty()) 
   {
      //get the unvisited vertex of vertex which is at front of the queue
      int tempVertex = dequeue();   

      //no adjacent vertex found
      while((unvisitedVertex = getAdjUnvisitedVertex(tempVertex)) != -1) 
      {    
         vertices[unvisitedVertex]->visited = true;
         displayVertex(unvisitedVertex);
         enqueue(unvisitedVertex);               
      }		
   }

   //queue is empty, search is complete, reset the visited flag        
   for(i = 0;i<vertexCount;i++) 
   {
      vertices[i]->visited = false;
   }
}

int main()
{
  int i, j;

  for(i = 0; i<NOV; i++)
  {
    for(j = 0; j<NOV; j++)
     adjMatrix[i][j] = 0;
  }

  addVertex('S');   
  addVertex('A');   
  addVertex('B');   
  addVertex('C');   
  addVertex('D');   
  addVertex('E');   
  addVertex('F');
  addVertex('G');

  addEdge(0, 1);
  addEdge(0, 2);
  addEdge(0, 3);
  addEdge(1, 4);
  addEdge(4, 7);
  addEdge(2, 5);
  addEdge(5, 7);
  addEdge(3, 6);
  addEdge(6, 7);
  
  printf("\nBreadth First Search: ");
  BFS();

  return 0;
}
