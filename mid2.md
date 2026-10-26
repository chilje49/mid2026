#2번 문제

내가 입력한 프롬포트 :
중간시험 코딩 문항인데. 이해가 잘 안되. c언어, SQLITE로 해야 한다고 해, 나는 정말 난감해.
이해하기 쉽도록 답을 낼 수 있도록 해줘. SQlite와 c언어를 활용하여 간단한 DB를 설계하고 싶어.
```
#include <stdio.h>
#include <stdlib.h>
#include <sqlite3.h>

// 데이터 조회(SELECT) 시 결과를 한 줄씩 처리해 주는 콜백 함수
static int callback(void *NotUsed, int argc, char **argv, char **azColName) {
    for (int i = 0; i < argc; i++) {
        printf("%s = %s\n", azColName[i], argv[i] ? argv[i] : "NULL");
    }
    printf("--------------------\n");
    return 0;
}

int main(void) {
    sqlite3 *db;
    char *errMsg = 0;
    int rc;

    // 1. 데이터베이스 열기 (school.db 파일 생성)
    rc = sqlite3_open("school.db", &db);
    if (rc) {
        fprintf(stderr, "데이터베이스를 열 수 없습니다: %s\n", sqlite3_errmsg(db));
        return (0);
    } else {
        fprintf(stdout, "데이터베이스 연결 성공!\n");
    }

    // 2. 테이블 생성 (STUDENT 테이블: 학번, 이름, 점수)
    const char *sql_create = "CREATE TABLE IF NOT EXISTS STUDENT("  \
                             "ID INT PRIMARY KEY     NOT NULL," \
                             "NAME           TEXT    NOT NULL," \
                             "SCORE          INT     NOT NULL);";

    rc = sqlite3_exec(db, sql_create, callback, 0, &errMsg);
    if (rc != SQLITE_OK) {
        fprintf(stderr, "테이블 생성 실패: %s\n", errMsg);
        sqlite3_free(errMsg);
    } else {
        fprintf(stdout, "테이블 생성 완료!\n");
    }

    // 3. 데이터 삽입 (INSERT)
    const char *sql_insert = "INSERT INTO STUDENT (ID, NAME, SCORE) VALUES (1, 'Alice', 85); " \
                             "INSERT INTO STUDENT (ID, NAME, SCORE) VALUES (2, 'Bob', 92);";

    rc = sqlite3_exec(db, sql_insert, callback, 0, &errMsg);
    if (rc != SQLITE_OK) {
        fprintf(stderr, "데이터 삽입 실패: %s\n", errMsg);
        sqlite3_free(errMsg);
    } else {
        fprintf(stdout, "데이터 삽입 완료!\n");
    }

    // 4. 데이터 조회 (SELECT) - 결과는 callback 함수가 처리함
    const char *sql_select = "SELECT * FROM STUDENT;";
    
    fprintf(stdout, "\n--- 학생 목록 조회 결과 ---\n");
    rc = sqlite3_exec(db, sql_select, callback, 0, &errMsg);
    if (rc != SQLITE_OK) {
        fprintf(stderr, "데이터 조회 실패: %s\n", errMsg);
        sqlite3_free(errMsg);
    }

    // 5. 데이터베이스 닫기
    sqlite3_close(db);
    return 0;
}
```
<img width="1226" height="444" alt="스크린샷 2026-10-26 152411" src="https://github.com/user-attachments/assets/ae5ef229-0ec1-4ec9-917e-6f0d73602582" />
