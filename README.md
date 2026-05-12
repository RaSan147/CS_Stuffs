# CS_Stuffs


```cpp
// prims
 #include <bits/stdc++.h>
using namespace std;
int main()
{
    int n, m;
    cout << "Enter Vertex and edges ";
    cin >> n >> m;
    map<char, int> mp;
    int ind = 0;
    int cost[100][100];
    for (int i = 0; i < 100; i++)
    {
        for (int j = 0; j < 100; j++)
        {
            cost[i][j] = 1e9;
        }
    }
    for (int i = 0; i < m; i++)
    {
        string edge;
        int w;
        cin >> edge >> w;
        if (mp.find(edge[0]) == mp.end())
            mp[edge[0]] = ind++;

        if (mp.find(edge[2]) == mp.end())
            mp[edge[2]] = ind++;
        int u = mp[edge[0]];
        int v = mp[edge[2]];
        cost[u][v] = w;
        cost[v][u] = w;
    }

    bool visited[100] = {false};
    visited[0] = true;
    int totalCost = 0;
    for (int k = 0; k < n - 1; k++)
    {
        int minCost = 0;
        int v = -1;
        for (int i = 0; i < n; i++)
        {
            if (visited[i])
            {
                for (int j = 0; j < n; j++)
                {
                    if (!visited[j] && cost[i][j] < minCost)
                    {
                        minCost = cost[i][j];
                        v = j;
                    }
                }
            }
        }
        if (v != -1)
        {
            totalCost += minCost;
            visited[v] = true;
        }
    }
    cout << "MST" << totalCost << endl;
}
/* prims
S-A 7
A-B 6
S-C 8
C-B 4
A-B 6
C-D 3
B-D 2
D-T 2
B-T 5
*/

// MST - 17
```


```cpp
// krushkal
#include<bits/stdc++.h>
using namespace std;
int parent[100];
int findparent(int x){
    if(parent[x]==x)
    return x;
return parent[x]= findparent(parent[x]);    
}
void unitset(int a, int b){
    a = findparent(a);
    b = findparent(b);
    if (b !=a){
        parent[b] =a;
    }

}
int main()
{
    int n, m;
    cout << "enter vertix and edges : ";
    cin >> n >> m;
    
    map <char,int> mp;
    int ind =0;

    vector<tuple<int , int , int>> edges;
    cout << "enter Edges: \n";
    for(int i=0; i<m; i++){
        string edge;
        int w;
        cin >> edge >> w;
        if(mp.find(edge[0])==mp.end())
            mp[edge[0]] = ind++;
         if(mp.find(edge[2])==mp.end())
            mp[edge[2]] = ind++; 
            
        int u = mp[edge[0]];    
        int v = mp[edge[2]];
        
        edges.push_back({w,u,v});

    }
    for(int i=0; i<n;i++){
        parent[i] =i;
    }
    sort(edges.begin(),edges.end());
    int totalcost =0;

    for(auto e : edges){
        int w = get<0>(e);
        int u = get<1>(e);
        int v = get<2>(e);

        if(findparent(u) != findparent(v)){
            unitset(u,v);
            totalcost+=w;
        }

    }
    cout << "MST:" << totalcost << endl;

}

/*krushkal
C D 2
D F 3
C E 3
A D 4
A B 5
A E 7
A C 8
B F 2
MST - 14
*/
```

```cpp
//lcs
#include<bits/stdc++.h>
using namespace std;
int main()
{
    string x,y;
    cout << "Enter first string: ";
    cin >> x;
    cout << "Enter second string: ";
    cin >> y;

    int m = x.length();
    int n = y.length();

    vector<vector<int>> dp(m+1,vector<int>(n+1,0));
    for(int i=1;i<=m;i++){
        for(int j=1;j<=n;j++){
            if(tolower(x[i-1]) == tolower(y[j-1])){
                dp[i][j] = 1+dp[i-1][j-1];
            }
            else{
                dp[i][j] = max(dp[i-1][j],dp[i][j-1]);
            }
        }
    }
    string lcs ="";
    int i=m, j=n;
    while (i>0 && j>0){
        if(tolower(x[i-1]) == tolower(y[j-1])){
            lcs+= x[i-1];
            i--;
            j--;
        }
        else if(dp[i-1][j]>dp[i][j-1]){
            i--;
        }
        else{
            j--;
        }

    }
    reverse(lcs.begin(),lcs.end());
   cout << "\nlength" << dp[m][n] << endl;
   cout << "LCS string" << lcs << endl;
}
```

```cpp
// 0/1 knapsack
#include<bits/stdc++.h>
using namespace std;
int knapsack(int W,int wt[],int val[],int n, bool selcted[]){
    if (n==0 || W==0)
        return 0;
    if(wt[n-1]>W)
        return knapsack(W,wt,val,n-1,selcted);
    int take = val[n-1] + knapsack(W-wt[n-1],wt,val,n-1,selcted);      
    int skip = knapsack(W,wt,val,n-1,selcted);   
    if(take > skip){
        selcted[n-1]= true;
        return take;
    }   
    return skip;


}
int main()
{
    int n,W;
    cout << "Enter number of items: ";
    cin >> n;

    int wt[n],val[n];
    bool selected[n] = {false};
    cout<< "Enter weights of items: ";
    for(int i=0; i<n;i++){
        cin >> wt[i];
    }
    cout<< "Enter value of items: ";
    for(int i=0; i<n;i++){
        cin >> val[i];
    }
    cout <<"Enter Capacity: ";
    cin >> W;
    cout << "Max Profit: " << knapsack(W,wt,val,n,selected);
    for(int i=0; i<n;i++){
        if(selected[i]){
            cout << i << " ";
        }
    }
    cout<< endl;

}


fractioal knapsack
#include<bits/stdc++.h>
using namespace std;
int main()
{
    int n,W;
    cout << "Enter number of items: ";
    cin >> n;
    vector<double> wt(n),val(n),ratio(n);
    cout << "Enter weights of items: ";
    for(int i=0;i <n;i++){
        cin >> wt[i];
    }
    cout << "Enter value of items: ";
    for(int i=0;i <n;i++){
        cin >> val[i];
        ratio[i] = val[i]/wt[i];
    }
    double capacity;
    cout << "Enter Capaacity: ";
    cin >> capacity;
    double totolvalue =0;
    for(int i=0;i<n-1;i++)
    {
        for(int j=i+1;j<n;j++){
            if(ratio[i]<ratio[j]){
                swap(ratio[i],ratio[j]);
                swap(val[i],val[j]);
                swap(wt[i],wt[j]);
            }
        }

    }
    int i;
    for(i=0; i<n ;i++){
        if(wt[i]<=capacity){
            totolvalue += val[i];
            capacity -= wt[i];
        }
        else{
            break;
        }
    }
    if(i<n){
        totolvalue += ratio[i]*capacity;
    }
    cout << "Max Profit: " << totolvalue << endl;
   
}
```


```cpp
//BFS
#include<bits/stdc++.h>
using namespace std;

vector<int> adj[100];   // adjacency list
bool visited[100];

void bfs(int start) {
    queue<int> q;
    q.push(start);
    visited[start] = true;

    while (!q.empty()) {
        int node = q.front();
        q.pop();
        cout << node << " ";

        for (int neighbor : adj[node]) {
            if (!visited[neighbor]) {
                visited[neighbor] = true;
                q.push(neighbor);
            }
        }
    }
}


int main() {
    int vertices, edges;
    
    cout << "Enter number of vertices: ";
    cin >> vertices;

    cout << "Enter number of edges: ";
    cin >> edges;

    cout << "Enter edges (u v):\n";
    for (int i = 0; i < edges; i++) {
        int u, v;
        cin >> u >> v;

        // Undirected graph
        adj[u].push_back(v);
        adj[v].push_back(u);
    }

    int start;
    cout << "Enter starting vertex: ";
    cin >> start;

    
    cout << "BFS Traversal: ";
    fill(begin(visited), end(visited), false);
    bfs(start);

    cout << endl;



    return 0;
}
/*
bfs
1 2
1 3
3 4
4 5
4 8
4 7
3 6
6 7
6 9
9 8
*/
```

```cpp
//DFS
#include<bits/stdc++.h>
using namespace std;

vector<int> adj[100];   // adjacency list
bool visited[100];


void dfs(int node) {
    visited[node] = true;
    cout << node << " ";

    for (int neighbor : adj[node]) {
        if (!visited[neighbor]) {
            dfs(neighbor);
        }
    }
}

int main() {
    int vertices, edges;
    
    cout << "Enter number of vertices: ";
    cin >> vertices;

    cout << "Enter number of edges: ";
    cin >> edges;

    cout << "Enter edges (u v):\n";
    for (int i = 0; i < edges; i++) {
        int u, v;
        cin >> u >> v;

        // Undirected graph
        adj[u].push_back(v);
        adj[v].push_back(u);
    }

    int start;
    cout << "Enter starting vertex: ";
    cin >> start;

    cout << endl;

    // DFS
    cout << "DFS Traversal: ";
    fill(begin(visited), end(visited), false);
    dfs(start);

    return 0;
}
/*
DFS
1 4 
1 2 
4 3
2 3
2 5
3 10
3 9
2 7
2 8
5 7
8 7
5 6
*/
```


```cpp
//scc
#include <bits/stdc++.h>
using namespace std;

const int N = 1e5 + 5;

vector<int> adj[N];
vector<int> transpose[N];
bool visited[N];
stack<int> st;
vector<int> component;

int n, m;

void dfs1(int node)
{
    visited[node] = true;
    for (int next : adj[node])
    {
        if (!visited[next])
            dfs1(next);
    }
    st.push(node);
}

void dfs2(int node)
{
    visited[node] = true;
    component.push_back(node);
    for (int next : transpose[node])
    {
        if (!visited[next])
            dfs2(next);
    }
}

int main()
{
    cin >> n >> m;

    for (int i = 0; i < m; i++)
    {
        int u, v;
        cin >> u >> v;
        adj[u].push_back(v);
        transpose[v].push_back(u);
    }

    for (int i = 1; i <= n; i++)
    {
        if (!visited[i])
            dfs1(i);
    }

    memset(visited, false, sizeof(visited));

    int scc_count = 0;

    while (!st.empty())
    {
        int node = st.top();
        st.pop();

        if (!visited[node])
        {
            component.clear();
            dfs2(node);
            scc_count++;

            cout << "SCC" << scc_count << ": {";
            for (int i = 0; i < component.size(); i++) {
                cout << component[i];
                if (i < component.size() - 1)
                    cout << ",";
            }
            cout << "}\n";
        }
    }

    return 0;
}
/*scc
1 2
2 3
3 1
1 4
3 4
4 5
5 6
6 7
7 4
*/
```

```cpp
// articulation point
#include <bits/stdc++.h>
using namespace std;

const int N = 1e5 + 5;

vector<int> adj[N];
bool visited[N];
int disc[N];      
int low[N];      
int parent[N];
bool isArticulation[N];

int timer = 0;
int n, m;

void dfs(int u) {
    visited[u] = true;
    disc[u] = low[u] = ++timer;

    int children = 0; 

    for (int v : adj[u]) {

        if (!visited[v]) {
            children++;
            parent[v] = u;
            dfs(v);

           
            low[u] = min(low[u], low[v]);

            
            if (parent[u] == -1 && children > 1)
                isArticulation[u] = true;

            
            if (parent[u] != -1 && low[v] >= disc[u])
                isArticulation[u] = true;
        }

        else if (v != parent[u]) {
           
            low[u] = min(low[u], disc[v]);
        }
    }
}

int main() {
    cin >> n >> m;

    for (int i = 0; i < m; i++) {
        int u, v;
        cin >> u >> v;
        adj[u].push_back(v);
        adj[v].push_back(u);   
    }

    for (int i = 1; i <= n; i++) {
        parent[i] = -1;
        visited[i] = false;
        isArticulation[i] = false;
    }

    for (int i = 1; i <= n; i++) {
        if (!visited[i])
            dfs(i);
    }

    cout << "Articulation Points:\n";
    for (int i = 1; i <= n; i++) {
        if (isArticulation[i])
            cout << i << " ";
    }

    return 0;
}
/*articulation point
1 2
1 4
2 3
4 3
3 5
3 6
5 6
*/
```