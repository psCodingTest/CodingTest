# 문제 링크

https://school.programmers.co.kr/learn/courses/30/lessons/17683#

---

## 문제 설명

네오가 기억하는 음표 m이 존재하며 해당 방송국에서 재생한 음악배열 N 이 존재합니다

네오가 기억하는 m과 일치하는 곡을 찾는 문제 입니다.

---


## 문제 접근 과정

1. 처음에는 일치하는 문자열을 찾는 문제라고 하여 Trie 를 고려하였지만 
    해당음악이 연속적으로 반복된다는 조건이 존재하였으며 문자열의 길이와 배열의 길이가 그리 길지 않았기 떄문에 완탐으로 하였습니다 
2. 배열의 최대갯수가 100이며 해당 네오가 기억하는 음표의 길이가 m이기 떄문에 N * M^2 이기때문에 완탐으로도 충분히 가능하다고 생각했습니다
---


## 시간복잡도

시간복잡도는 해당 배열의 길이가 N 네오가 기억하는 문자열의 길이가 M

즉 O(N * M^2) 이기 때문에 수치로 환산하면 207,072,100 입니다

---

## 코드
```java
import java.util.*;

// 1시간 반정도

class Solution {
   	public static String solution(String m, String[] musicinfos) {
		List<Music> musicList = new ArrayList<>();

		for (String musician : musicinfos) { // N 번만큼 반복
			int order = 1;
			String[] arr = musician.split(",");
			String[] startTimeSplit = arr[0].split(":");
			String[] endTimeSplit = arr[1].split(":");
			Integer startTime =
				(Integer.parseInt(startTimeSplit[0]) * 60) + Integer.parseInt(startTimeSplit[1]);
			Integer endTime =
				(Integer.parseInt(endTimeSplit[0]) * 60) + Integer.parseInt(endTimeSplit[1]);
			String author = arr[2];
			String[] songSplit = replace(arr[3]).split("");
			// 전처리 단계
			int size = m.length();
			int total = endTime - startTime;

			for (int i = 0; i < total; i++) { // M번 만큼 반복

				boolean isFind = false;
				StringBuilder str = new StringBuilder();
				int j = i;
				int count = 0;

				while (true) { // 모듈러 연산을 통해 최대 네오가 기억하는 길이만큼 문자열 연산을 해줌
					str.append(songSplit[j++ % songSplit.length]);
					if (str.toString().equals(replace(m))) { // 만약 해당 곡의 정보가 네오가 기억하는 곡의 정보와 같을경우 더해줌
						musicList.add(new Music(total, author, order));
						order++;
						isFind = true;
						break;
					}
					if (++count == total || count == size) break;
					// 1. 만약 음악 재생시간 보다 작을경우 break
				}
				if (isFind) break;
				// 곡이 존재한다면 해당 N번째 반복문을 종료
			}
		}
		Collections.sort(musicList);

		if (musicList.isEmpty()) {
			return "(None)"; // 네오가 기억하는 곡이 없을 경우
		} else {
			return musicList.get(0).title; // 존재할 경우
		}
	}

	public static String replace(String melody) {
		return melody.replace("C#", "c")
			.replace("D#", "d")
			.replace("F#", "f")
			.replace("G#", "g")
			.replace("A#", "a");
		/*
		"ABC", "C#DEFGAB" 이 경우 ABC# 이기 떄문에 해당 #을 포함한 문자를 소문자로 변환하여 처리
		 */
	}

	public static class Music implements Comparable<Music> {
		int songSize;
		String title;
		Integer order;

		public Music(int songSize, String title, Integer order) {
			this.songSize = songSize;
			this.title = title;
			this.order = order;
		}

		@Override
		public int compareTo(Music o) {
			if (o.songSize == this.songSize) {
				return this.order - o.order;
			}
			return o.songSize - this.songSize;
		}
	}
	/**
	 1. 네오가 기억한 음표 m은 1439 이하의 문자열로 제공
	 2. musicInfo 는 100 개 이하의 곡정보를 담은배열로 {시작시간, 끝시간, 제목, 음표정보} 가 담김



	 1.C, C#, D, D#, E, F, F#, G, G#, A, A#, B 12개의 음이 사용됌
	 2.음악의 시간보다 길게 재생된경우 반복해서 재생되는거임 -> 슬라이딩 윈도우?
	 3.00:00을 넘겨서 재생되는 음악은 없다
	 4.같은 음이 존재하는경우 음악의 길이가 가장 긴 음악을 반환
	 5.없을 경우 None 을 반환

	 제한사항
	 1. 곡의 길이만큼만 재생
	 i.ABCD 3분 일 경우 ABC 만큼 재생
	 2. 만약 일치하는 곡이 여러개 존재하는 경우 해당 곡의 길이기준으로 내림차순 정렬
	 i. 같을 경우 먼저 입력된 음악의 순서로 정렬


	 */
}
```


##  느낀점
- 카카오 문제는 늘 그렇듯이 생각보다 제약조건이 많아서 엣지 케이스를 찾는데 시간이 걸렸습니다.
- 코드를 막상 다 작성하고 보니 좀 정돈이 안되었다는 느낌이 듭니다 항상 정돈되어있게 코테를 하는 습관을 길러야할것 같습니다
