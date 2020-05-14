---
title: "알고리즘 개념6 - 위상정렬"
excerpt: "위상정렬 (Topological Sort)"

categories:
 - Algorithm
tags:
 - Algorithm
 - 위상정렬

layout: single
---
- 여러 일들에 순서가 정해져 있을 때 순서에 맞게 나열하는 알고리즘

# 위상 정렬의 과정

1. 진입차수가 0인 정점과 이와 연결된 모든 간선을 지운다.
2. 남아 있는 정점의 진입차수를 갱신한다.
3. 그래프에 모든 정점이 없어질 때까지 1과 2를 반복한다.



[위상정렬을 이해할 수 있는 문제 : 백준 줄세우기](https://www.acmicpc.net/problem/2252)

[문제 정답 코드](https://soohyun0907.github.io/algorithm/BOJ-2252/)

