---
title: "Algorithm_Graph"
excerpt: "Graph"

categories:
 - Algorithm
tags:
 - Algorithm 
 - Graph
layout: single
---
# 그래프

- 사물과 사물의 연결관계를 표현하는 수단
- 정점과 정점간의 연결관계인 간선을 모아 놓은 자료 구조
  - 그래프로 표현하고자 하는 사물을 정점 (Vertex)
  - 정점간의 관계를 표현한 것이 간선 (Edge)
- 선형자료구조나 트리자료구조로 표현하기 어려운 M:N관계를 표현

## 그래프의 표현

- 간선의 배열 : 정점과 정점의 연결정보인 간선을 모아놓음
  - 가장 간단하게 그래프의 연결정보를 표현할 수 있는 방법
- 인접 리스트 : 각 정점별로 연결된 정점의 정보를 모아놓음
- 인접 행렬 : 2차원 행렬에 각 정점과 정점간의 연결정보를 표시해둠

## 그래프의 종류

- 유향 그래프와 무향 그래프
- 가중치 그래프
- 순환 그래프와 비순환 그래프
- 연결 그래프와 비연결 그래프
- 트리 (두 개의 정점사이 1개의 경로만이 존재, 비순환 연결)
  - 간선의 개수 = 정점의 개수 - 1
  - 신장트리, 최소신장 트리
- DAG (Directed Acyclic Graph : 유향 + 비순환)

### 신장 트리

- 그래프의 모든 정점과 간선의 부분집합으로 구성되는 트리
- 사이클이 안생기는 조건 안에서 간선을 정점-1개 고르는 것.

# 최소 신장 트리(MST)

- 신장 트리 중에서 사용된 간선들의 가중치 합이 최소인 트리
- 특징
  - 무방향 가중치 그래프
  - 그래프의 가중치의 합이 최소여야 한다.
  - N개의 정점을 가지는 그래프에 대해 반드시 (N-1)개의 간선을 사용해야 한다.
  - 사이클을 포함해서는 안된다.
- 사용하는 이유?
  - 도로망, 통신망, 유통망 등등 여러 분야에서 비용을 최소로 해야 그만큼의 이익을 볼 수 있다.
- 대표적인 방법
  - Kruskal
  - Prim

## Kruskal Algorithm

- 탐욕적인 방법을 이용하여 가중 그래프(가중치를 간선에 할당한 그래프)의 모든 정점을 최소 비용으로 연결하는 최적 해를 구하는 것
- 간선을 하나씩 선택해서 MST(최소신장트리)를 찾는 알고리즘

### 동작과정

1. 그래프의 간선들을 가중치의 오름차순으로 정렬한다.

2. 정렬된 간선 리스트에서 순서대로 사이클을 형성하지 않는 간선을 선택한다.

   a. 가중치가 가장 낮은 간선을 선택한다.

   b. 사이클이 형성되면 다음 간선을 선택한다.

3. 해당 간선을 MST 집합에 추가한다.

4. (N-1)개의 간선이 선택될 때까지 반복. (N은 정점의 수)

### Kruskal.java

```java
import java.util.Arrays;
import java.util.Comparator;
import java.util.Scanner;

public class Kruskal {

	static int[] parents;
	static int[] rank;

	// 입력은 첫줄에 정점의 갯수와 간선의 갯수가 들어오고
	// 그 다음줄부터 간선의 정보가 정점1 정점2 가중치로 간선의 갯수만큼 들어옴
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		int V = sc.nextInt();
		int E = sc.nextInt();
		int[][] edges = new int[E][3];
		for (int i = 0; i < E; i++) {
			edges[i][0] = sc.nextInt(); // 정점1의 정보
			edges[i][1] = sc.nextInt(); // 정점2의 정보
			edges[i][2] = sc.nextInt(); // 가중치의 정보
		}
		// 간선들을 가중치 오름차순으로 정렬
		Arrays.sort(edges, new Comparator<int[]>() {

			@Override
			public int compare(int[] o1, int[] o2) {
				return Integer.compare(o1[2], o2[2]);
			}

		});
		// 각 정점에 대해 unionFind 연산 준비
		for (int i = 0; i < V; i++) {
			makeSet(i);
		}
		// 정점의 갯수-1번 반복하면서
		int cnt = 0, result = 0;
		for (int i = 0; i < V - 1; i++) {
			int a = findSet(edges[i][0]);
			int b = findSet(edges[i][1]);
			// 같은 팀이라면 패스
			if (a == b)
				continue;
			// 간선이 연결하는 두 정점이 같은 팀이 아니라면 한팀으로 합쳐주고 간선선택
			union(a, b);
			result += edges[i][2];
			cnt++;
			if (cnt == V - 1)
				break;
		}
		System.out.println(result);
	}

	static void makeSet(int x) {
		parents[x] = x;
	}

	static int findSet(int x) {
		if (x == parents[x])
			return x;
		else {
			parents[x] = findSet(parents[x]);
			return parents[x];
		}
	}

	static void union(int x, int y) {
		int px = findSet(x);
		int py = findSet(y);
		if (rank[px] > rank[py]) {
			parents[py] = px;
		} else {
			parents[px] = py;
			if (rank[px] == rank[py]) {
				rank[py]++;
			}
		}
	}
}
```

## Prim Algorithm

- 하나의 정점에서 연결된 간선들 중에 하나씩 선택하면서 MST를 만들어 가는 방식
- 정점을 하나씩 선택할 때 마다 간선을 추가하면서 트리 확장
- 두 종류의 상호배타집합 정보를 유지
  - 트리 정점들 : MST를 만들기 위해 선택된 정점들
  - 비트리 정점들 : 선택되지 않은 정점들

### 동작과정

1. 임의 정점을 하나 선택해서 시작
2. 선택한 정점과 인접하는 정점들 중의 최소 비용의 간선이 존재하는 정점을 선택
3. 모든 정점이 선택될 때까지 1,2 과정 반복

### Prim.java

```java
import java.util.Arrays;
import java.util.Scanner;

public class Prim {

	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		int V = sc.nextInt();
		int E = sc.nextInt();
		int[][] adj = new int[V][V];

		for (int i = 0; i < E; i++) {
			int a = sc.nextInt();
			int b = sc.nextInt();
			int c = sc.nextInt();
			adj[a][b] = adj[b][a] = c;
		}

		boolean[] check = new boolean[V];
		int[] key = new int[V]; // 현재 선택된 정점들로부터 도달할 수 있는 최소거리
		int[] p = new int[V]; // 최소신장트리의 구조를 저장할 배열

		// key의 초기값은 무한대
		Arrays.fill(key, Integer.MAX_VALUE);

		// 아무거나 하나 선택! 처음 선택되는 친구가 루트이므로 부모는 없는걸로, 거리는 0으로 설정
		p[0] = -1;
		key[0] = 0;

		// 이미 하나 골랐으니 나머지 V-1개를 골라보자.
		for (int i = 0; i < V - 1; i++) {
			int min = Integer.MAX_VALUE;
			// 안골라진 친구들 중에서 key가 가장 작은 친구들을 뽑자.
			int index = -1; // 찾으면 그 위치를 여기에 저장.
			for (int j = 0; j < V; j++) {
				// 일단, 안골라진지 검사, key의 최소값을 기억해야함.
				if (!check[j] && key[j] < min) {
					index = j;
					min = key[j];
				}
			}
			// index : 선택이 안된 정점 중 key가 제일 작은 친구가 들어있음.
			check[index] = true;
			// index 정점에서 출발하는 모든 간선에 대해서 key를 업데이트
			for (int j = 0; j < V; j++) {
				// check가 안되어있으면서, 간선은 존재하고, 그 간선이 key값보다 작다면, key값을 업데이트.
				if (!check[j] && adj[index][j] != 0 && key[j] > adj[index][j]) {
					p[j] = index;
					key[j] = adj[index][j];
				}
			}
		}
		int result = 0;
		for (int i = 0; i < V; i++) {
			result += key[i];
		}
		System.out.println(result);
		System.out.println(Arrays.toString(p));
		sc.close();
	}

}
```

### Prim2.java

```java
import java.util.PriorityQueue;
import java.util.Scanner;

public class Prim2 {

	static int N;
	static int[][] graph; // 가중치 그래프

	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		N = sc.nextInt();
	}

	private static double prim(int start) {

		// MST에 들어가지 않은 녀석들.
		PriorityQueue<Edge> pq = new PriorityQueue<>();
		// 모든 정점들을 다 관리.
		Edge[] dynamicGraph = new Edge[N];

		for (int n = 0; n < N; n++) {
			dynamicGraph[n] = new Edge(n, Long.MAX_VALUE);
			if (n == start) {
				dynamicGraph[n].cost = 0;
			}
			pq.add(dynamicGraph[n]);
		}
		long cost = 0;

		while (!pq.isEmpty()) {
			Edge front = pq.poll(); // MST에 포함되는 녀석.
			cost += front.cost;

			// 자식 탐색.
			for (int i = 0; i < N; i++) {
				Edge child = dynamicGraph[i];
				// pq : 비MST.
				if (pq.contains(child)) {
					long tempCost = graph[front.idx][child.idx];
					if (tempCost < child.cost) {
						child.cost = tempCost;
						pq.remove(child);
						pq.offer(child);
					}
				}
			}
		}

		return cost;
	}

	static class Edge implements Comparable<Edge> {
		int idx;
		long cost;

		public Edge(int idx, long cost) {
			super();
			this.idx = idx;
			this.cost = cost;
		}

		@Override
		public int compareTo(Edge o) {
			return Long.compare(this.cost, o.cost);
		}

	}

}
```

