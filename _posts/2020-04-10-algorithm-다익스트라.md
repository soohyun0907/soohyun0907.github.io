---
title: "알고리즘 개념2 - 그래프"
excerpt: "다익스트라(Dijkstra) 알고리즘"

categories:
 - Algorithm
tags:
 - Algorithm
 - Dijkstra
layout: single
---

# 다익스트라 알고리즘 (Dijkstra Algorithm)

- 시작 정점에서 거리가 최소인 정점을 선택해 나가면서 최단 경로를 구하는 방식
  - 탐욕 기법을 사용한 알고리즘으로 MST의 Prim 알고리즘과 유사하다.
- 시작 정점(S)에서 끝 정점(T) 까지의 최단 경로에 정점 X가 존재한다. 이때, 최단 경로는 S에서 X까지의 최단 경로와 X에서 T까지의 최단 경로로 구성된다.
- 음의 가중치가 있는 경우에는 사용할 수 없다.

## 동작 과정

1. 시작 정점을 입력 받는다.
2. 거리를 저장할 D배열을 무한으로 초기화 한 후 시작점에서 갈 수 있는 곳의 값은 바꿔 놓는다.
3. 아직 방문하지 않은 점들이 가지고 있는 거리 값과 현재 정점에서 방문하지 않은 정점까지의 가중치의 합이 작다면 변경하여 적는다.
4. 모든 정점을 방문할 때까지 반복

### Dijkstra.java

```java
import java.util.Arrays;
import java.util.Scanner;

public class Dijkstra {

	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		int V = sc.nextInt();
		int E = sc.nextInt();
		int[][] adj = new int[V][V];
		for (int i = 0; i < E; i++) {
			adj[sc.nextInt()][sc.nextInt()] = sc.nextInt();
		}
		int[] D = new int[V];
		Arrays.fill(D, Integer.MAX_VALUE);
		boolean[] check = new boolean[V];
		// dijkstra 시작점이 a정점이라면 D[a]=0
		D[0] = 0;
		for (int i = 0; i < V - 1; i++) {
			int min = Integer.MAX_VALUE;
			int index = -1;
			for (int j = 0; j < V; j++) {
				if (!check[j] && min > D[j]) {
					min = D[j];
					index = j;
				}
			}

			// 새로운 친구로부터 갈 수 있는 경로들을 업데이트
			for (int j = 0; j < V; j++) {
				// 아직 처리하지 않았으면서, 경로가 존재하고, index까지의 거리 + index부터 j까지의 거리가 D[j]보다 작으면
				if (!check[j] && adj[index][j] != 0 && D[index] + adj[index][j] < D[j]) {
					D[j] = D[index] + adj[index][j];
				}
			}
			// 처리된 것으로 체크
			check[index] = true;
		}

		System.out.println(Arrays.toString(D));
		sc.close();
	}

}
```

### Dijkstra_PQ.java

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;
import java.util.PriorityQueue;
import java.util.Scanner;

public class Dijkstra_PQ {

	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		int V = sc.nextInt();
		int E = sc.nextInt();
		List<Edge>[] adj = new ArrayList[V];

		for (int i = 0; i < V; i++)
			adj[i] = new ArrayList<>();

		for (int i = 0; i < E; i++) {
			// 첫번째가 출발지, 두번째가 도착지, 세번째가 가중치
			adj[sc.nextInt()].add(new Edge(sc.nextInt(), sc.nextInt()));
		}

		// dijkstra
		PriorityQueue<Edge> pq = new PriorityQueue<>();
		boolean[] check = new boolean[V];
		Edge[] D = new Edge[V];
		// 0번에서 출발하는 걸로
		for (int i = 0; i < V; i++) {
			if (i == 0) {
				D[i] = new Edge(i, 0);
			} else {
				D[i] = new Edge(i, Integer.MAX_VALUE);
			}
			pq.add(D[i]);
		}
		while (!pq.isEmpty()) {
			Edge edge = pq.poll();

			for (Edge next : adj[edge.v]) {
				// check되지 않았으면서, D[next.v]가 D[edge.v] + next.weight 보다 더 크다면 갱신
				if (!check[next.v] && D[next.v].weight > D[edge.v].weight + next.weight) {
					D[next.v].weight = D[edge.v].weight + next.weight;
					pq.remove(D[next.v]);
					pq.add(D[next.v]);
				}
			}
			check[edge.v] = true;
		}

		System.out.println(Arrays.toString(D));
		sc.close();
	}

	static class Edge implements Comparable<Edge> {
		int v, weight;

		public Edge(int v, int weight) {
			this.v = v;
			this.weight = weight;
		}

		@Override
		public int compareTo(Edge o) {
			return Integer.compare(this.weight, o.weight);
		}

	}

}
```



# 벨만 포드 알고리즘 (Bellman-Ford Algorithm)

- 음의 가중치를 포함하는 그래프에서 최단 경로를 구할 수 있음
  - 가중치의 합이 음인 사이클은 허용하지 않음
  - Dijkstra로 최단 경로를 구할 수 있다면 벨만-포드로도 구할 수 있음
- 출발점에서 각 정점까지 간선 하나로 구성된 경로 고려, 최단 경로 구함
- 최대 간선 n-1개까지 고려한 경로들에서 최단 경로 구함
- Dijkstra 알고리즘에 비해 시간이 많이 소요됨
- 시간복잡도 O(VE)