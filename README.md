# LeetCode 1319. Number of Operations to Make Network Connected

There are n computers numbered from 0 to n - 1 connected by ethernet cables connections forming a network where connections[i] = [ai, bi] represents a connection between computers ai and bi. Any computer can reach any other computer directly or indirectly through the network.

You are given an initial computer network connections. You can extract certain cables between two directly connected computers, and place them between any pair of disconnected computers to make them directly connected.

Return the minimum number of times you need to do this in order to make all the computers connected. If it is not possible, return -1.

## Code

```java
class disjointSet{
    int[] set;
    int[] rank;
    disjointSet(int N){
        set=new int[N];
        rank=new int[N];
        for(int i=0;i<N;i++){
            set[i]=i;
        }
    }
    int find(int x){
        if(set[x]==x){return x;}
        return find(set[x]);
    }
    void union(int a,int b){
        int x=find(a);
        int y=find(b);
        if(x==y){return;}
        if(rank[x]<rank[y]){set[x]=y;}
        else if(rank[x]>rank[y]){set[y]=x;}
        else{
            set[y]=x;
            rank[x]+=1;
        }
        
    }
}
class Solution {
    public int makeConnected(int n, int[][] connections) {
       int spare=0;
       int maxJoinedNode=Integer.MIN_VALUE; 
       int selected=0;
       disjointSet set=new disjointSet(n);
       for(int i=0;i<connections.length;i++){
           int first=connections[i][0];
           int second=connections[i][1];
           if(set.find(first)!=set.find(second)){
              set.union(first,second);
              selected++;
              maxJoinedNode=Math.max(maxJoinedNode,Math.max(first,second));
           }
       } 
       int nodesLeft=n-maxJoinedNode-1;
       spare=connections.length-selected;
       int sepSets=0;
       for(int i=0;i<set.set.length;i++){if(set.set[i]==i){sepSets++;}}
       if(nodesLeft==0 && sepSets==spare){return 0;}
       if(sepSets!=0){if(spare>=sepSets){return sepSets-1;}}
       if(spare>=nodesLeft){return nodesLeft;}
       return -1;
    }
}
```

## Explanation
This code is solving the "Number of Operations to Make Network Connected" problem, which asks us to find the minimum number of connections required to make all nodes connected in a given network.

The idea behind the solution is to use a disjoint set data structure to keep track of which nodes are connected to each other. Initially, each node is its own set. We iterate over all connections, and for each connection, we check if the two nodes are already connected or not. If they are not connected, we merge the sets to which they belong.

While iterating over the connections, we also keep track of the maximum node index that is a part of any connected component. We do this because any node with an index greater than this value will not be a part of any connected component.

After merging all the sets, we count the number of spare connections. These are the connections that were not used during the merging process. We can use these spare connections to connect disconnected components. We also count the number of separate sets using the disjoint set data structure.

If there are enough spare connections to connect all separate sets, we return the number of separate sets - 1. Otherwise, if there are enough spare connections to connect all nodes left after merging the sets, we return that number. Otherwise, it is not possible to connect all nodes, so we return -1.

That's a brief explanation of the approach! 





## User

[LeetCode](https://leetcode.com/problems/number-of-operations-to-make-network-connected/)
