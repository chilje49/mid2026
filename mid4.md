#4번 답안

내가 작성한 프롬포트 :
c언어로 아스키 피라미드 패턴 출력해줘. 높이 N을 입력받아 중앙 정렬된 피라미드
형태의 문자 패턴을 출력하도록 해. 홀수 번째 행은 *, 짝수 번째 행은 #으로 출력해.
```
#include <stdio.h>

int main(void) {
    int n;

    printf("Enter height N: ");
    if (scanf("%d", &n) != 1 || n <= 0) {
        return 0;
    }

    for (int i = 1; i <= n; i++) {
        // Print leading spaces for center alignment
        for (int j = 0; j < n - i; j++) {
            printf(" ");
        }

        // Determine symbol: '*' for odd rows, '#' for even rows
        char symbol = (i % 2 != 0) ? '*' : '#';

        // Print characters
        for (int j = 0; j < (2 * i - 1); j++) {
            printf("%c", symbol);
        }

        printf("\n");
    }

    return 0;
}
```
<img width="843" height="462" alt="스크린샷 2026-10-26 153244" src="https://github.com/user-attachments/assets/f1727a8a-4b21-44b2-985d-df666b857aa5" />
