sqlite: case sensitive
mysql: case insensitive
따라서 sqlitedump.sh와 같은 쉘 스크립트로 sql 구문들을 뽑아내서 다른 db로 이전할때, unique constraint와 같은 경우에서 에러가 생길 수 있다.