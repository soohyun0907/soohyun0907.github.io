---
title: "알고리즘 개념2 - 문자열"
excerpt: "KMP 알고리즘"

categories:
 - Algorithm
tags:
 - Algorithm
layout: single
---

# KMP Algorithm

- Knuth, Morris, Pratt의 앞글자를 한글자씩 따서 KMP

- 불일치가 발생한 텍스트 문자열의 앞부분에 어떤 문자가 있는지 미리 알고 있으므로, 불일치가 발생한 앞 부분에 대하여 다시 비교하지 않고 매칭을 수행
- 시간복잡도 : O(N+M) // N : 문자열 길이, M : 패턴 길이

## KMP.java

```java

import java.util.Arrays;

//Knuth, Morris, Prett
public class KMP {
	// 모든 경우를 다 비교하지 않아도 부분 문자열을 찾을 수 있는 알고리즘
	// 접두사와 접미사의 개념을 활용하여 '반복되는 연산을 얼마나 줄일 수 있는지'를 판별하여
	// 매칭할 문자열을 빠르게 점프하는 기법.
	// 접두사와 접미사가 일치하는 최대길이가 필요(점프 해야되서)

	// 실패테이블
	static int[] getPi(String pattern) {
		// 접두사와 접미사가 일치하는 최대길이를 담을 배열 선언
		int[] pi = new int[pattern.length()];

		// idx
		int j = 0;
		// i,j가 가리키는 값이 일치하면 둘다 증가
		// 불일치하면 i만 증가시켜야 하므로 for문
		for (int i = 1; i < pattern.length(); i++) {

			// pattern 내에서 i와 j가 가리키는 값이 다를때
			//while문안에 넣는 이유는 중간단계를 건너뛰고 최대한으로 점프하려고
			while (j > 0 && pattern.charAt(i) != pattern.charAt(j)) {
				//j의 값을 한칸 뺀곳의 값으로 j를 바꿈
				j = pi[j - 1];
			}
			// pattern 내에서 i와 j가 가리키는 값이 같으면
			if (pattern.charAt(i) == pattern.charAt(j)) {
				// i번째의 최대길이는 ++j한 값
				pi[i] = ++j;
			}
		}

		return pi;
	}
	static void KMP(String parent, String pattern) {
		int[] table = getPi(pattern);
		
		int j = 0; 
		for(int i = 0 ; i< parent.length(); i++) {
			while(j >0 && parent.charAt(i) != pattern.charAt(j)) {
				j = table[j - 1];
			}
			//부모와 패턴이 일치한다면
			if(parent.charAt(i) == pattern.charAt(j)) {
				//j의 값이 패턴의 길이-1이라면 한번 다찾은거니까
				//찾아다고 처리
				if(j == pattern.length()-1) {
					System.out.println((i-pattern.length()+1) + "째 인덱스에서 찾음" );
					//패턴을 또 찾기 위해서
					j = table[j];
				}else {
					//다찾은건아니라면 계속 진행해야하므로 j값 증가
					j++;
				}
			}
		}
		
	}
	public static void main(String[] args) {
		String parent = "ababacabacaabacaaba";
		String pattern = "abacaaba";
		KMP(parent, pattern);

	}

}
```



# Rabin-Karp Algorithm

- 문자열 검색을 위해 해시 값 함수를 이용
- 패턴 내의 문자들을 일일이 비교하는 대신에 패턴의 해시 값과 본문 안에 있는 하위 문자열의 해시 값만을 비교
- 최악의 시간 복잡도는 O(MN)이지만 평균적으로는 선형에 가까운 빠른 속도를 가지는 알고리즘

# Booyer-Moore Algorithm

- 뒤에부터 검사하면서 불필요한 이동 줄임. (오른쪽에서 왼쪽으로 비교)
- 대부분의 상용 소프트웨어에서 채택하고 있는 알고리즘
- 보이어-무어 알고리즘은 패턴에 오른쪽 끝에 있는 문자가 불일치하고 이 문자가 패턴 내에 존재하지 않는 경우, 이동 거리는 무려 패턴의 길이 만큼이 된다.