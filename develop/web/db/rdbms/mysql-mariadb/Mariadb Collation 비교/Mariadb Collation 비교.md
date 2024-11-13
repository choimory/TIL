# utf8_* | euckr_* | ascii_* / latin_* 등

- 앞의 접두사는 문자 인코딩 방식을 이야기 한다. 
- 아스키코드, 라틴, euckr, utf8, utf16, utf32, cp850 등 다양한 방식을 지원한다.
- 글로벌 서비스등의 이유가 아니라면, 한글 저장시 문제가 없으면서도 다양한 언어를 소화할 수 있는 utf8이 가장 일반적인 선택이 된다.

# utf8mb3 | utf8mb4

- mb3와 mb4의 가장 큰 차이는 이모지 저장 가능여부이다.
- utf8은 최대 3bytes인데, 이모지는 4bytes이므로 문자가 깨져버리고 만다. 때문에 4bytes를 지원하는 utf8mb4로 처리한다.

# utf8mb4_bin | utf8mb4_general_ci

- bin과 general_ci의 차이는 대소문자 구분 여부이다.
  - e.g id값이 ABC인 컬럼에 `where id = 'abc'`했을시, general_ci는 조건에 충족하며 bin은 충족되지 않는다. 
- general_ci가 현재 utf8mb4 기본 collation이다.