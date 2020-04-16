---
title: "알고리즘 개념4 - 동적계획법"
excerpt: "DP (Dynamic Programming)"

categories:
 - Algorithm
tags:
 - Algorithm
 - DP

layout: single
---

# DP (Dynamic Programming)

- 동적 계획 알고리즘은 그리디 알고리즘과 같이 최적화 문제를 해결하는 알고리즘이다.
- 최적화 문제 : 최적(최대값이나 최소값 같은) 값을 구하는 문제 (유일한 값 X)
- 동적 계획 알고리즘은 먼저 작은 부분 문제들의 해들을 구하고 이들을 이용하여 보다 큰 크기의 부분 문제들을 해결하여, 최종적으로 원래 주어진 문제를 해결하는 알고리즘 설계 기법이다.

## 중복 부분문제 구조 (Overlapping Subproblem)

- 큰 문제를 이루는 작은 문제들을 먼저 해결하고 작은 문제들의 최적 해(Optimal Solution)를 이용하여 순환적으로 큰 문제를 해결한다.
- 일반적으로 수학적 도구인 점화식을 사용한다.
- 이미 해결된 작은 문제들의 해들을 어떤 저장 공간에 저장하게 된다.
- N번째 피보나치 수를 구하기 위해서 N-1번째, N-2번째  피보나치 수를 구하는 것

## 최적 부분문제 구조 (Optimal Substructure)

- 문제의 정답을 작은 문제의 정답에서 구할 수 있는 경우
- 최적화의 원칙 : 어떤 문제에 대한 해가 최적일 때 그 해를 구성하는 작은 문제들의 해 역시 최적이어야 한다.

## 동적 계획법을 푸는 두가지 방법

### 1. Top - Down

- 주로 재귀호출을 이용한다.

1. 문제를 작은 문제로 나눈다.
2. 작은 문제를 푼다
3. 작은 문제를 풀었으니, 이제 문제를 푼다.

### 2. Bottom - Up

- 주로 for 문을 이용한다.

1. 문제의 크기가 작은 문제부터 푼다.
2. 문제의 크기를 조금씩 크게 만들면서 문제를 점점 푼다.

# 플로이드 워셜 알고리즘 (Floyd-Warshall)

- 모든 점을 경유 가능한 점들로 고려된 모든 쌍 i와 j의 최단 경로의 거리를 찾는 방식

- 시간복잡도 : O(n^3)

```java
import java.util.Arrays;

public class 플로이드워셜 {
	
	public static void main(String[] args) {
		
		final int m = Integer.MAX_VALUE;
		int[][] d = { { 0, m, 2, 3 }, 
				{ 4, 0, 1, 8 }, 
				{ 2, 5, 0, m }, 
				{ m, 9, 6, 0 } };
		
		for (int k = 0; k < d.length; k++) {
			for (int i = 0; i < d.length; i++) {
				if (k == i) {
					continue;
				}
				for (int j = 0; j < d.length; j++) {
					if (k == j || i == j) {
						continue;
					}
					if (d[i][k] != m && d[k][j] != m && d[i][j] > d[i][k] + d[k][j]) {
						d[i][j] = d[i][k] + d[k][j];
					}
				}
			}
		}
		
		for (int i = 0; i < d.length; i++) {
			System.out.println(Arrays.toString(d[i]));
		}

	} // end of main
} // end of class
```

