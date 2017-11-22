# √Ë ˆ

Farmer John completed his new barn just last week, complete with all the latest milking technology. Unfortunately, due to engineering problems, all the stalls in the new barn are different. For the first week, Farmer John randomly assigned cows to stalls, but it quickly became clear that any given cow was only willing to produce milk in certain stalls. For the last week, Farmer John has been collecting data on which cows are willing to produce milk in which stalls. A stall may be only assigned to one cow, and, of course, a cow may be only assigned to one stall. 
Given the preferences of the cows, compute the maximum number of milk-producing assignments of cows to stalls that is possible. 

#  ‰»Î

The input includes several cases. For each case, the first line contains two integers, N (0 <= N <= 200) and M (0 <= M <= 200). N is the number of cows that Farmer John has and M is the number of stalls in the new barn. Each of the following N lines corresponds to a single cow. The first integer (Si) on the line is the number of stalls that the cow is willing to produce milk in (0 <= Si <= M). The subsequent Si integers on that line are the stalls in which that cow is willing to produce milk. The stall numbers will be integers in the range (1..M), and no stall will be listed twice for a given cow.

#  ‰≥ˆ
For each case, output a single line with a single integer, the maximum number of milk-producing stall assignments that can be made.

	#include<iostream>
	#include<stdio.h>
	#include<vector>
	#include<memory.h>
	#define Max 505
	using namespace std;

	struct edge{
		int value;
		int to;
		int recv;
	};

	int Number[Max];

	vector<edge>Graph[Max];
	bool used[Max];

	void BuildGraph(int cur,int IndexNum,int n){
		edge e ;
		for (int i = 0;i<IndexNum;++i){
			e.to = Number[i]+n; e.value = 1;e.recv = Graph[Number[i]+n].size();
			Graph[cur].push_back(e);
			e.to = cur;e.value = 0; e.recv = Graph[cur].size()-1;
			Graph[Number[i]+n].push_back(e);
		}
		return ;
	} 


	int getflow(int s,int end,int curflow){
		used[s] = true;
		if (s == end)
			return curflow;
		for (int i = 0;i<Graph[s].size();++i){
			edge &e = Graph[s][i];
			if (!used[e.to] ){
				int minflow = min(e.value,curflow);

				if (minflow != 0){
					int flow = getflow(e.to,end,minflow);
					if (flow != 0){

						e.value -= flow;
						Graph[e.to][e.recv].value += flow;
						return flow;
					}

				}	
			}
		
		}
		return 0;

	}

	int ComputeMaxFlow(int n){
		int result = 0;
		while(true){
			memset(used,0,sizeof(used));
			int curresult = getflow(0,2*n+1,Max);
			if(curresult == 0)
				break;
			else 
				result += curresult;
		}

		return result ;
		
	}

	int main(){
		int n,m;


		while(cin >> n >> m){
			int number ;
			for (int i = 0;i<Max;++i)
				Graph[i].clear();
			for (int i = 1;i<=n;++i){
				cin>>number;
				for (int i = 0;i<number;++i)
					cin >> Number[i];
				BuildGraph(i,number,n);
			}

			for (int i = 1;i<=n;++i){
				edge e;
				e.to = i; e.value = 1; e.recv = Graph[i].size();
				Graph[0].push_back(e);
				e.to = 0; e.value = 0; e.recv = Graph[0].size()-1;
				Graph[i].push_back(e);
			}
			for (int i = 1;i<=n;++i){
				edge e;
				e.to = 2*n+1; e.value = 1;e.recv  = Graph[2*n+1].size();
				Graph[n+i].push_back(e);
				e.to = 0;e.value = 0;e.recv = Graph[n+i].size()-1;
				Graph[2*n+1].push_back(e);
			}

			int Flow  =  ComputeMaxFlow(n);
			cout <<Flow<<endl;
		}

		return 0;
	}