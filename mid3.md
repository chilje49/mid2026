#3번 답안

내가 작성한 프롬포트
c언어를 활용하여 숫자야구 판정 게임을 작성해줘. 숫자 3개와 사용자가 입력한 숫자 3개가
주어질 때, 볼(Ball)과 스트라이크(Strike) 개수를 계산하여 출력하는 프로그램을 작성해줘.
조건:
숫자와 위치가 모두 맞으면 Strike, 숫자는 맞지만 위치가 다르면 Ball입니다.
3 Strike일 경우 "HOMERUN"을 추가로 출력합니다.
입력 예시: 1 2 3 (정답) / 1 3 5 (입력)
출력 예시: 1 Strike, 1 Ball 
```
#include <stdio.h>

int main(void) {
    int secret[3];
    int user[3];
    int strikes = 0;
    int balls = 0;

    // Input 3 secret numbers
    printf("Enter 3 secret numbers: ");
    for (int i = 0; i < 3; i++) {
        scanf("%d", &secret[i]);
    }

    // Input 3 user numbers
    printf("Enter 3 user numbers: ");
    for (int i = 0; i < 3; i++) {
        scanf("%d", &user[i]);
    }

    // Calculate Strikes and Balls
    for (int i = 0; i < 3; i++) {
        for (int j = 0; j < 3; j++) {
            if (secret[i] == user[j]) {
                if (i == j) {
                    strikes++;
                } else {
                    balls++;
                }
            }
        }
    }

    // Print result
    printf("%d Strike, %d Ball\n", strikes, balls);

    // Check HOMERUN condition
    if (strikes == 3) {
        printf("HOMERUN\n");
    }

    return 0;
}
<img width="1210" height="529" alt="스크린샷 2026-10-26 152800" src="https://github.com/user-attachments/assets/5a5122ca-9727-4161-a20c-768cfeecee70" />
