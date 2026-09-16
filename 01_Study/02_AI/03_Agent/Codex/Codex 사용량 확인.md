월 GPT 20달러를 구독하게 되면 일반 사용 한도는 5시간 사용한도와 주간 사용 한도가 있다.
### CLI에서 /usage
토큰 사용 내역을 보여준다. 
```
/usage #사용량 메뉴 열기
/usage daily #일간 토큰 활동 바로 보기
/usage weekly #주간
/usage cumulative #누적
```

### CLI에서 잘못된 경로를 trust 경로로 지정했을 경우
~/.codex/config.toml 파일을 연다.
trust 문자열로 검색하다 보면, 내가 신뢰할 수 있는 경로로 지정한 리스트를 볼 수 있다.
잘못된 경로가 있으면 삭제 후 저장한다.

![[Pasted image 20260916183029.png]]