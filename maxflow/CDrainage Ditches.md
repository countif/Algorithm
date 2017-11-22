√Ë ˆ
Every time it rains on Farmer John's fields, a pond forms over Bessie's favorite clover patch. This means that the clover is covered by water for awhile and takes quite a long time to regrow. Thus, Farmer John has built a set of drainage ditches so that Bessie's clover patch is never covered in water. Instead, the water is drained to a nearby stream. Being an ace engineer, Farmer John has also installed regulators at the beginning of each ditch, so he can control at what rate water flows into that ditch. 
Farmer John knows not only how many gallons of water each ditch can transport per minute but also the exact layout of the ditches, which feed out of the pond and into each other and stream in a potentially complex network. 
Given all this information, determine the maximum rate at which water can be transported out of the pond and into the stream. For any given ditch, water flows in only one direction, but there might be a way that water can flow in a circle. 
 ‰»Î
The input includes several cases. For each case, the first line contains two space-separated integers, N (0 <= N <= 200) and M (2 <= M <= 200). N is the number of ditches that Farmer John has dug. M is the number of intersections points for those ditches. Intersection 1 is the pond. Intersection point M is the stream. Each of the following N lines contains three integers, Si, Ei, and Ci. Si and Ei (1 <= Si, Ei <= M) designate the intersections between which this ditch flows. Water will flow through this ditch from Si to Ei. Ci (0 <= Ci <= 10,000,000) is the maximum rate at which water will flow through the ditch.
 ‰≥ˆ
For each case, output a single integer, the maximum rate at which water may emptied from the pond.

#include<iostream>
#include<memory.h>
#include<stdio.h>
#include<vector>
#define Max 205
#define INF 0x3f3f3f3f
using namespace std;
struct  edge{
	int recv;
	int to;
	int value;
};

vector<edge>  Graph[Max];

int M; 
bool used[Max];
int GetRoute(int s,int e,int cur){
	if (s == e)
		return cur;
	used[s]= true;
	for(int i = 0;i<Graph[s].size();++i)
	{
		int curflow = min(Graph[s][i].value,cur);
		edge & Edg = Graph[s][i];
		if (curflow != 0 && !used[Edg.to]){
			int resultflow = GetRoute(Edg.to,e,curflow);
			if (resultflow != 0)
			{
				Edg.value -= resultflow;
				Graph[Edg.to][Edg.recv].value += resultflow;
				return resultflow;
			}
		}
	}
	return 0;
}

int getflow(){
	int result = 0;
	while(true){
		memset(used,0,sizeof(used));
		int flow = GetRoute(0,M-1,INF);
		if (flow != 0)
			result += flow;
		else
			break;
	}
	return result;
}

int main(){
	int n;
	while(cin >> n >> M){
		memset(Graph,0,sizeof(Graph));

		for (int i = 0;i<n;++i){
			int x,y;
			cin >>x>>y;
			int value;
			edge e;cin >> value;
			e.to = y-1;e.value = value;e.recv = Graph[y-1].size();
			Graph[x-1].push_back(e);
			e.to =x-1;e.value = 0;e.recv = Graph[x-1].size()-1;
			Graph[y-1].push_back(e);
		}
		int flow = getflow();
		cout << flow <<endl;
	}
	return 0;
}