# √Ë ˆ
Given a connected undirected graph, tell if its minimum spanning tree is unique.

Definition 1 (Spanning Tree): Consider a connected, undirected graph G = (V, E). A spanning tree of G is a subgraph of G, say T = (V', E'), with the following properties:
1. V' = V.
2. T is connected and acyclic.

Definition 2 (Minimum Spanning Tree): Consider an edge-weighted, connected, undirected graph G = (V, E). The minimum spanning tree T = (V, E') of G is the spanning tree that has the smallest total cost. The total cost of T means the sum of the weights on all the edges in E'.
#  ‰»Î
The first line contains a single integer t (1 <= t <= 20), the number of test cases. Each case represents a graph. It begins with a line containing two integers n and m (1 <= n <= 100), the number of nodes and edges. Each of the following m lines contains a triple (xi, yi, wi), indicating that xi and yi are connected by an edge with weight = wi. For any two nodes, there is at most one edge connecting them.
#  ‰≥ˆ
For each input, if the MST is unique, print the total cost of it, or otherwise print the string 'Not Unique!'.


	#include<iostream>
	#include<stdio.h>
	#include<memory.h>
	#define Max 150
	#define INF 99999999
	using namespace std;
	int map[Max][Max];

	bool visit[Max];
	int dist[Max];

	bool used[Max][Max];
	int MaxEdge[Max][Max];
	int preu[Max];


	int	prim(int n){
		int result = 0;
		memset(preu,0,sizeof(preu));
		memset(used,0,sizeof(used));
		memset(MaxEdge,0,sizeof(MaxEdge));
		memset(visit,0,sizeof(visit));
		dist[1] = 0;visit[1] =true;
		
		for (int i = 1;i<=n;++i){
			dist[i] = map[1][i];
			preu[i] = 1;	
		}


		for (int i = 0;i<n-1;++i){
			int Min = INF,Minv = -1;
			for (int j = 1;j<=n;++j)
			{
				if (!visit[j]&& dist[j]<Min )	
				{
					Minv = j;
					Min = dist[j];
				}
			}
			result  += Min;
			visit[Minv] = true;

			used[Minv][preu[Minv]] = used[preu[Minv]][Minv] = true;

			for (int j = 1;j<=n;++j){
				if (visit[j]&&j!=Minv)
					MaxEdge[j][Minv] = max(MaxEdge[j][preu[Minv]],dist[Minv]);
				if (!visit[j]&&dist[j]>map[Minv][j] )
				{
					dist[j] = map[Minv][j];
					preu[j] = Minv;
				}
			}
			
			
		}


		return result;

	}


	int main(){
		int t;
		cin >>t;
		while(t--){
			int n,m;
			cin >> n>>m;
			for (int i = 1;i<=n;++i){
				for (int j = 1;j<=n;++j){
					map[i][j] = INF;

				}
			}
			for (int i= 0;i<m;++i){
				int x,y,w;
				cin >>x>>y>>w;
				map[x][y] = w;
				map[y][x] = w;
			}
			int mst= prim(n);
			int Result = INF;
			for (int i = 1;i<=n;++i){
				for (int j = 1;j<=n;++j){
					if (!used[i][j]){
						Result = min(Result,mst+map[i][j]-MaxEdge[i][j]);
						
					}
				}
				
			}
			if (Result == mst)
				cout <<"Not Unique!"<<endl;
			else
				cout << mst<<endl;
		
		}



	return 0;
	}