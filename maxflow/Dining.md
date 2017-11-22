描述
Cows are such finicky eaters. Each cow has a preference for certain foods and drinks, and she will consume no others.

Farmer John has cooked fabulous meals for his cows, but he forgot to check his menu against their preferences. Although he might not be able to stuff everybody, he wants to give a complete meal of both food and drink to as many cows as possible.

Farmer John has cooked F (1 ≤ F ≤ 100) types of foods and prepared D (1 ≤ D ≤ 100) types of drinks. Each of his N (1 ≤ N ≤ 100) cows has decided whether she is willing to eat a particular food or drink a particular drink. Farmer John must assign a food type and a drink type to each cow to maximize the number of cows who get both.

Each dish or drink can only be consumed by one cow (i.e., once food type 2 is assigned to a cow, no other cow can be assigned food type 2).

输入
Line 1: Three space-separated integers: N, F, and D
Lines 2..N+1: Each line i starts with a two integers Fi and Di, the number of dishes that cow i likes and the number of drinks that cow i likes. The next Fi integers denote the dishes that cow i will eat, and the Di integers following that denote the drinks that cow i will drink.
输出
Line 1: A single integer that is the maximum number of cows that can be fed both food and drink that conform to their wishes
样例输入
4 3 3
2 2 1 2 3 1
2 2 2 3 1 2
2 2 1 3 1 2
2 1 1 3 3
样例输出
3

’
#include<iostream>
#include<stdio.h>
#include<memory.h>
#include<vector>
#include<queue>
#define INF 0x3f3f3f3f
#define Max 10005
using namespace std;
int CurMax = 0;
struct edge{
	int to;
	int value;
	int recv;
};
vector<edge> Graph[Max];
int level[Max];

bool bfs(){
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
	if (level[CurMax])
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

int GetMaxFlow( ){
	int result = 0;
	while(bfs()){
		int flow = getflow(0,CurMax,INF);
			result = result +flow;
	}
	return result ;
}

void BuildGraph(int start,int end,int flag){
	edge e ;
	
	e.to = end;e.value = 1;e.recv = Graph[end].size();
	Graph[start].push_back(e);

	e.to = start;e.value = flag;e.recv = Graph[start].size()-1;

	Graph[end].push_back(e);
	return ;
}

int main(){

	int n,f,d;
	cin >> n >> f >>d;
	CurMax = 2*n+d+f+1;

	
	//src to food 
	for (int i = 1;i<=f;++i)
		BuildGraph(0,i,0);



	for (int i = 1;i<=n;++i){
		int nf,nd;
		cin >> nf>>nd;
		// food to cows 
		for (int j = 0;j<nf;++j)
		{
			int x;
			cin >>x;
			BuildGraph(x,i+f,0);
		}

		// drink to cows
		for (int j = 0;j<nd;++j){
			int x;
			cin >>x;
			BuildGraph(f+n+i,x+f+2*n,0);
		}
		// cows to cows
		BuildGraph(f+i,n+f+i,0);
	}

	//drink to dst 
	for (int i = 1;i<=d;++i)
		BuildGraph(f+2*n+i,CurMax,0);

	int Flow = GetMaxFlow();
	cout <<Flow<<endl;


	return 0;
}
’