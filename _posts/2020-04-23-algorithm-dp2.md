---
title: "알고리즘 개념5 - 동적계획법2"
excerpt: "LIS (최장 증가 수열)"

categories:
 - Algorithm
tags:
 - Algorithm
 - DP
 - LIS

layout: single
---

- 분할 정복은 하향식 방법 DP는 상향식 방법
- 동적 계획법은 최적화 문제를 해결하는 알고리즘으로 탐욕기법 알고리즘과 흡사하다.

# DP 적용 접근 방법

- 최적해 구조의 특성을 파악하라
  - 문제를 부분 문제로 나눈다.
- 최적해의 값을 재귀적으로 정의하라
  - 부분 문제의 최적해 값에 기반하여 문제의 최적해 값을 정의한다.
- 상향식 방법으로 최적해의 값을 계산하라
  - 가장 작은 부분문제부터 해를 구한 뒤 테이블에 저장한다.
  - 테이블에 저장되어 있는 부분 문제의 해를 이용하여 점차적으로 상위 부분 문제의 최적해를 구한다. (상향식 방법)

# LIS (최장 증가 수열)

> [최장 증가 부분 수열:나무위키](https://namu.wiki/w/%EC%B5%9C%EC%9E%A5%20%EC%A6%9D%EA%B0%80%20%EB%B6%80%EB%B6%84%20%EC%88%98%EC%97%B4)

## O(N^2)

```java
import java.util.Arrays;

/**
 * @author soohyun 
 * DP 
 * O(N^2)
 */

public class LIS_1 {

	public static void main(String[] args) {
		int[] a = { 3, 2, 6, 4, 5, 1 };
		int[] LIS = new int[a.length]; // i번째 숫자를 마지막 글자로 사용할 경우의 최장 증가 수열의 길이
		int[] path = new int[a.length]; // 경로 역추적을 하기 위해
		
		for (int i = 0; i < LIS.length; i++) {
			LIS[i] = 1; // 초기값 (1개짜리 수열)
			path[i] = -1; // 나의 앞의 수열 숫자의 index
			for (int j = 0; j < i; j++) { // 내 앞의 숫자 중에 나보다 작은 숫자를 찾기
				if (a[j] < a[i] && LIS[i] < LIS[j] + 1) {
					LIS[i] = LIS[j] + 1;
					path[i] = j;
				}
			}
		}

		int maxLISIndex = 0;
		for (int i = 0; i < LIS.length; i++) {
			if (LIS[maxLISIndex] < LIS[i]) {
				maxLISIndex = i;
			}
		}
		
		System.out.println("최장 증가 수열의 길이 : " + LIS[maxLISIndex]);
		System.out.println(Arrays.toString(path));
		String result = "";
		for (int i = maxLISIndex; i != -1 ; i = path[i] ) {
			result = a[i] + " " + result;
		}
		System.out.println(result);
	} // end of main

}
```

## O(NlogN)

```java
import java.util.Arrays;

/**
 * @author soohyun 
 * LIS 알고리즘 > 이진탐색 이용
 * O[NlogN]
 */

public class LIS_2 {

	public static void main(String[] args) {
		int[] a = { 8, 2, 4, 3, 6, 11, 7, 10, 14, 5 };
		int[] C = new int[a.length]; // LIS로 사용 가능한 숫자를 저장
		int[] path = new int[a.length]; // 경로를 저장할 배열 역추적할 index를 저장
		int size = 0; // LIS 개수 관리할 변수

		path[size] = -1; // 첫번째 숫자라는 의미
		C[size++] = 0; // 첫번째 숫자의 index 반영
		for (int i = 1; i < a.length; i++) {
			// C배열의 마지막 숫자와 수열값을 비교
			if (a[C[size - 1]] < a[i]) {
				path[i] = C[size - 1];
				C[size++] = i; // 맨 뒤에 붙임
			} else { // LIS 마지막 숫자보다 크지 않으면 LIS의 값을 업데이트한다 (이진탐색)
//				int temp = Arrays.binarySearch(C, 0, size, a[i]); // 삽입할 위치
				int temp = binarySearch0(a, C, 0, size, a[i]); // 삽입할 위치
				if (temp < 0)
					temp = -temp - 1;
				path[i] = path[C[temp]]; // 덮어쓸 위치의 index를 내껄로 복사
				C[temp] = i; // 수열의 값을 LIS에 삽입할 위치에 덮어쓰기
			}
		}
		System.out.println("LIS 개수 : " + size);
		System.out.println("C : " + Arrays.toString(C));
		String result = "";
		for(int i = C[size-1];i != -1;i = path[i]) {
			result = a[i] + " " + result;
		}
		System.out.println("LIS 수열 : " + result);
	}

	private static int binarySearch0(int[] a, int[] C, int fromIndex, int toIndex, int key) {
		int low = fromIndex;
		int high = toIndex - 1;

		while (low <= high) {
			int mid = (low + high) >>> 1;
			int midVal = a[C[mid]];

			if (midVal < key)
				low = mid + 1;
			else if (midVal > key)
				high = mid - 1;
			else
				return mid; // key found
		}
		return -(low + 1); // key not found.
	}

}
```

