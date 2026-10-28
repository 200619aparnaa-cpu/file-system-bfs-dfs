
#include <stdio.h>
#include <string.h>

#define MAX 20

char files[MAX][50];
int adj[MAX][MAX];
int visited[MAX];
int n;

// Breadth First Search
void BFS(int start, char target[])
{
    int queue[MAX], front = 0, rear = 0;
    int found = 0;

    for (int i = 0; i < n; i++)
        visited[i] = 0;

    visited[start] = 1;
    queue[rear++] = start;

    printf("\nBFS Traversal: ");

    while (front < rear)
    {
        int current = queue[front++];

        printf("%s ", files[current]);

        if (strcmp(files[current], target) == 0)
            found = 1;

        for (int i = 0; i < n; i++)
        {
            if (adj[current][i] && !visited[i])
            {
                visited[i] = 1;
                queue[rear++] = i;
            }
        }
    }

    if (found)
        printf("\nFile found using BFS.\n");
    else
        printf("\nFile not found using BFS.\n");
}

// Depth First Search
void DFSUtil(int current, char target[], int *found)
{
    visited[current] = 1;

    printf("%s ", files[current]);

    if (strcmp(files[current], target) == 0)
        *found = 1;

    for (int i = 0; i < n; i++)
    {
        if (adj[current][i] && !visited[i])
            DFSUtil(i, target, found);
    }
}

void DFS(int start, char target[])
{
    int found = 0;

    for (int i = 0; i < n; i++)
        visited[i] = 0;

    printf("\nDFS Traversal: ");

    DFSUtil(start, target, &found);

    if (found)
        printf("\nFile found using DFS.\n");
    else
        printf("\nFile not found using DFS.\n");
}

int main()
{
    int edges, u, v, start;
    char target[50];

    printf("FILE SYSTEM USING BFS AND DFS\n");

    printf("Enter number of files/directories (max %d): ", MAX);
    scanf("%d", &n);

    if (n < 1 || n > MAX)
    {
        printf("Invalid number of nodes!\n");
        return 1;
    }

    for (int i = 0; i < n; i++)
    {
        printf("Enter name of node %d: ", i);
        scanf(" %49[^\n]", files[i]);
    }

    printf("\nEnter number of connections: ");
    scanf("%d", &edges);

    if (edges < 0 || edges > n * (n - 1) / 2)
    {
        printf("Invalid number of connections!\n");
        return 1;
    }

    printf("\nEnter connections using node numbers (0 to %d):\n", n - 1);

    for (int i = 0; i < edges; i++)
    {
        printf("Connection %d: ", i + 1);
        scanf("%d %d", &u, &v);

        if (u < 0 || u >= n || v < 0 || v >= n || u == v)
        {
            printf("Invalid connection!\n");
            return 1;
        }

        adj[u][v] = 1;
        adj[v][u] = 1;
    }

    printf("\nAdjacency Matrix:\n");

    for (int i = 0; i < n; i++)
    {
        for (int j = 0; j < n; j++)
            printf("%d ", adj[i][j]);

        printf("\n");
    }

    printf("\nEnter starting node number: ");
    scanf("%d", &start);

    if (start < 0 || start >= n)
    {
        printf("Invalid starting node!\n");
        return 1;
    }

    printf("Enter file name to search: ");
    scanf(" %49[^\n]", target);

    BFS(start, target);
    DFS(start, target);

    return 0;
}
