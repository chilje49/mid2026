#1번 답안

나의 프람프트 : 
무지개 블록깨기 게임을 c언어로 작성하시오. 그래픽 raylib 최신 버전 라이브러리 활용.
비쥬얼 스튜디오 커뮤니티 최근 버전에서 nuget에서 설치해서 사용하고 싶어. 한글은 절대
사용하지 말것 반드시 작동이 되어야 하고, 5회 실패시 다시 시작하도록 해줘, 코드는 간결하게
```
#include "raylib.h"

#define SCREEN_WIDTH 800
#define SCREEN_HEIGHT 600

#define ROWS 7
#define COLS 10
#define BRICK_WIDTH 70
#define BRICK_HEIGHT 25
#define BRICK_PADDING 10
#define OFFSET_X 45
#define OFFSET_Y 60

typedef struct {
    Rectangle rect;
    bool active;
    Color color;
} Brick;

typedef struct {
    Vector2 pos;
    Vector2 speed;
    float radius;
} Ball;

typedef struct {
    Rectangle rect;
    float speed;
} Paddle;

void ResetGame(Ball* ball, Paddle* paddle, Brick bricks[ROWS][COLS]) {
    ball->pos = (Vector2){ SCREEN_WIDTH / 2.0f, SCREEN_HEIGHT / 2.0f };
    ball->speed = (Vector2){ 5.0f, -5.0f };
    ball->radius = 8.0f;

    paddle->rect = (Rectangle){ SCREEN_WIDTH / 2.0f - 50.0f, SCREEN_HEIGHT - 40.0f, 100.0f, 15.0f };
    paddle->speed = 8.0f;

    Color rainbow[ROWS] = {
        RED, ORANGE, YELLOW, GREEN, SKYBLUE, BLUE, PURPLE
    };

    for (int i = 0; i < ROWS; i++) {
        for (int j = 0; j < COLS; j++) {
            bricks[i][j].rect = (Rectangle){
                OFFSET_X + j * (BRICK_WIDTH + BRICK_PADDING),
                OFFSET_Y + i * (BRICK_HEIGHT + BRICK_PADDING),
                BRICK_WIDTH,
                BRICK_HEIGHT
            };
            bricks[i][j].active = true;
            bricks[i][j].color = rainbow[i];
        }
    }
}

int main(void) {
    InitWindow(SCREEN_WIDTH, SCREEN_HEIGHT, "Rainbow Brick Breaker");
    SetTargetFPS(60);

    Ball ball;
    Paddle paddle;
    Brick bricks[ROWS][COLS];

    int lives = 5;
    int score = 0;
    bool gameOver = false;

    ResetGame(&ball, &paddle, bricks);

    while (!WindowShouldClose()) {
        if (!gameOver) {
            // Paddle Movement
            if (IsKeyDown(KEY_LEFT) || IsKeyDown(KEY_A)) paddle.rect.x -= paddle.speed;
            if (IsKeyDown(KEY_RIGHT) || IsKeyDown(KEY_D)) paddle.rect.x += paddle.speed;

            if (paddle.rect.x < 0) paddle.rect.x = 0;
            if (paddle.rect.x + paddle.rect.width > SCREEN_WIDTH) paddle.rect.x = SCREEN_WIDTH - paddle.rect.width;

            // Ball Movement
            ball.pos.x += ball.speed.x;
            ball.pos.y += ball.speed.y;

            // Wall Collisions
            if (ball.pos.x - ball.radius < 0) { ball.pos.x = ball.radius; ball.speed.x *= -1; }
            if (ball.pos.x + ball.radius > SCREEN_WIDTH) { ball.pos.x = SCREEN_WIDTH - ball.radius; ball.speed.x *= -1; }
            if (ball.pos.y - ball.radius < 0) { ball.pos.y = ball.radius; ball.speed.y *= -1; }

            // Bottom Collision (Lose Life)
            if (ball.pos.y + ball.radius > SCREEN_HEIGHT) {
                lives--;
                if (lives <= 0) {
                    gameOver = true;
                } else {
                    ball.pos = (Vector2){ SCREEN_WIDTH / 2.0f, SCREEN_HEIGHT / 2.0f };
                    ball.speed = (Vector2){ 5.0f, -5.0f };
                }
            }

            // Paddle Collision
            if (CheckCollisionCircleRec(ball.pos, ball.radius, paddle.rect)) {
                ball.speed.y *= -1;
                float hitPoint = (ball.pos.x - (paddle.rect.x + paddle.rect.width / 2.0f)) / (paddle.rect.width / 2.0f);
                ball.speed.x = hitPoint * 6.0f;
            }

            // Brick Collisions
            bool levelClear = true;
            for (int i = 0; i < ROWS; i++) {
                for (int j = 0; j < COLS; j++) {
                    if (bricks[i][j].active) {
                        levelClear = false;
                        if (CheckCollisionCircleRec(ball.pos, ball.radius, bricks[i][j].rect)) {
                            bricks[i][j].active = false;
                            ball.speed.y *= -1;
                            score += 10;
                        }
                    }
                }
            }

            if (levelClear) {
                ResetGame(&ball, &paddle, bricks);
            }
        } else {
            // Restart on Space Key
            if (IsKeyPressed(KEY_SPACE)) {
                lives = 5;
                score = 0;
                gameOver = false;
                ResetGame(&ball, &paddle, bricks);
            }
        }

        // Draw
        BeginDrawing();
        ClearBackground(RAYWHITE);

        if (!gameOver) {
            // Draw Bricks
            for (int i = 0; i < ROWS; i++) {
                for (int j = 0; j < COLS; j++) {
                    if (bricks[i][j].active) {
                        DrawRectangleRec(bricks[i][j].rect, bricks[i][j].color);
                    }
                }
            }

            // Draw Paddle & Ball
            DrawRectangleRec(paddle.rect, DARKGRAY);
            DrawCircleV(ball.pos, ball.radius, MAROON);

            // Draw UI
            DrawText(TextFormat("Score: %d", score), 20, 20, 20, DARKGRAY);
            DrawText(TextFormat("Lives: %d", lives), SCREEN_WIDTH - 100, 20, 20, DARKGRAY);
        } else {
            DrawText("GAME OVER", SCREEN_WIDTH / 2.0f - MeasureText("GAME OVER", 40) / 2.0f, SCREEN_HEIGHT / 2.0f - 40, 40, RED);
            DrawText("Press SPACE to Restart", SCREEN_WIDTH / 2.0f - MeasureText("Press SPACE to Restart", 20) / 2.0f, SCREEN_HEIGHT / 2.0f + 20, 20, DARKGRAY);
        }

        EndDrawing();
    }
<img width="809" height="625" alt="스크린샷 2026-10-26 151021" src="https://github.com/user-attachments/assets/3513a5f9-c55d-43d8-91d0-068a33bcd0d4" />
    CloseWindow();
    return 0;
}
```
