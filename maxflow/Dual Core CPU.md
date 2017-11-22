描述
As more and more computers are equipped with dual core CPU, SetagLilb, the Chief Technology Officer of TinySoft Corporation, decided to update their famous product - SWODNIW.

The routine consists of N modules, and each of them should run in a certain core. The costs for all the routines to execute on two cores has been estimated. Let's define them as Ai and Bi. Meanwhile, M pairs of modules need to do some data-exchange. If they are running on the same core, then the cost of this action can be ignored. Otherwise, some extra cost are needed. You should arrange wisely to minimize the total cost.

输入
There are two integers in the first line of input data, N and M (1 ≤ N ≤ 20000, 1 ≤ M ≤ 200000) .
The next N lines, each contains two integer, Ai and Bi.
In the following M lines, each contains three integers: a, b, w. The meaning is that if module a and module b don't execute on the same core, you should pay extra w dollars for the data-exchange between them.

输出
Output only one integer, the minimum total cost.

样例输入
3 1
1 10
2 10
10 3
2 3 1000
样例输出
13


#include<iostream>
#include<stdio.h>
#include<vector>
#include<queue>
#include<memory.h>
#define Max 30005
#define INF 0x3f3f3f3f
using namespace std;
struct edge{
	int value;
	int to;
	int recv;
};

int level[Max];
vector<edge> Graph[Max];
bool bfs(int n){
	memset(level,0,sizeof(level));
	level[0] = 1;
	queue<int> Q;
	Q.push(0);
	while(!Q.empty()){
		int t =	Q.front();
		Q.pop();
		for (int i = 0;i<Graph[t].size();++i){
			edge e = Graph[t][i];
			if (!level[e.to] &&e.value > 0){
				level[e.to] = level[t] + 1;
				Q.push(e.to);
			}
		}
	}
	if (level[n+1])
		return true;
	else
		return false;
}

int getflow(int s,int end ,int curflow){
	if (s == end)
		return curflow;

	int result = 0;

	for (int i =0;i<Graph[s].size()&&result<curflow;++i){
		edge &e = Graph[s][i];
		int minflow = min(e.value,curflow);
		if (minflow != 0&& level[e.to] == level[s] + 1){
			int flow = getflow(e.to,end,minflow);
			if (flow !=0){
				e.value -= flow;
				Graph[e.to][e.recv].value += flow;
				result += flow;
				curflow -= flow;
			}
		
		}

	}
	if (result ==0)
		level[s] = -1;

	return result;
}

int GetMaxFlow(int n ){
	int result = 0;
	while(bfs(n)){
		int flow = getflow(0,n+1,INF);
			result = result +flow;
	}
	return result ;
}

int main(){
	int n,m;
	while(cin >> n >> m){
		for (int i = 0;i<Max;++i)
			Graph[i].clear();

		for (int i = 1;i<=n;++i){
			edge e ;
			int value;
			cin >> value;
			e.to = i;e.value = value;e.recv = Graph[i].size();
			Graph[0].push_back(e);

			e.to = 0; e.value = 0; e.recv = Graph[0].size()-1;
			Graph[i].push_back(e);

			cin >> value;
			e.to = n+1; e.value = value; e.recv = Graph[n+1].size();
			Graph[i].push_back(e);

			e.to = i; e.value = 0; e.recv = Graph[i].size() - 1;
			Graph[n+1].push_back(e);
		}

		for (int i = 0;i<m;++i){
			int x,y,value;
			cin >> x>>y>>value;
			edge e;
			e.to = y; e.value = value; e.recv = Graph[y].size();
			Graph[x].push_back(e);
			e.to = x; e.value = value; e.recv = Graph[x].size()-1;
			Graph[y].push_back(e);
		}

		int Flow = GetMaxFlow(n);
		cout << Flow<<endl;
	}

return 0;
}