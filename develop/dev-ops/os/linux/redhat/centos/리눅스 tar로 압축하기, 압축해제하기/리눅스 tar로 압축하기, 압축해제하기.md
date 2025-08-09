# 압축관련 주요 플래그

- c : 파일 압축
- x : 파일 압축해제
- v : 압축과정 출력
- f : 지정한 파일명으로 압축
- z : tar 압축 후 gzip으로 압축
- p : 소유권 등 퍼미션 유지
- -T : 파일에 적힌 목록들만 압축

# 압축하기

tar -플래그 압축대상디렉토리혹은파일명

- tar -cvzf result.tgz

# 압축풀기

tar -플래그 결과파일명.tgz 압축대상디렉토리혹은파일명

- tar -xvzf result.tgz target
- tar -xvzf result.tgz target.txt