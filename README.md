<h1>Практическая работа №5. Введение в разработку игр с использованием C++ и raylib</h1>
<h2>1. Инициализация окна и переменных</h2>
    <code>const int screenWidth = 800;
const int screenHeight = 450;
InitWindow(screenWidth, screenHeight, "Arkanoid на raylib");
SetTargetFPS(60);</code><br><br>
    Данный код:
    <ul>
      <li>Создает окно размером 800x450 пикселей с заголовком "Arkanoid на raylib".</li>
      <li>Устанавливает частоту обновления экрана на 60 FPS.</li>
    </ul>
<h2>2. Создание игровых объектов</h2>
    <p><b>Мяч:</b> начальная позиция в центре экрана, скорость 5 по X и 4 по Y, радиус 20.</p>
    <code>Vector2 ballPosition = { (float)screenWidth / 2, (float)screenHeight / 2 };
Vector2 ballSpeed = { 5.0f, 4.0f };
int ballRadius = 20;</code>
    <p><b>Ракетка:</b> начальная позиция 50 пикселей от левого края, высота 100, ширина 20.</p>
    <code>Vector2 paddlePosition = { 50.0f, (float)screenHeight / 2 - 50.0f };
Vector2 paddleSize = { 20.0f, 100.0f };</code>
    <p><b>Счет:</b> Изначально 0.</p>
    <code>int score = 0;</code>
<h2>3. Движения мяча</h2>
    <p>Мяч движется по X и Y, обновляя свою позицию.</p>
    <code>ballPosition.x += ballSpeed.x;
ballPosition.y += ballSpeed.y;</code>
    <p>Если мяч касается <b>верхней</b> или <b>нижней</b> границы, он отскакивает (меняем знак скорости по Y).</p>
    <code>if ((ballPosition.y >= (screenHeight - ballRadius)) ||
    (ballPosition.y <= ballRadius))
{
    ballSpeed.y *= -1.0f;
}</code>
      <p>Если мяч касается <b>правой границы</b>, он отскакивает (меняем знак скорости по X).</p>
      <code>if (ballPosition.x >= (screenWidth - ballRadius))
{
    ballSpeed.x *= -1.0f;
}
</code>
<p>Если мяч уходит за <b>левую границу</b>, игра сбрасывает <b>позицию мяча</b>, <b>скорость</b> и <b>счет</b>.</p>
<code>if (ballPosition.x < 0)
{
    Vector2 ballPosition = { (float)screenWidth / 2, (float)screenHeight / 2 };
    Vector2 ballSpeed = { 5.0f, 4.0f };
    score = 0;
}
</code>
<h2>4. Движение ракетки (управление мышью)</h2>
<p>Ракетка следует за Y координатой <b>мыши</b>, ограничена рамками экрана.</p>
<code>paddlePosition.y = GetMouseY() - paddleSize.y / 2;
if (paddlePosition.y < 0) paddlePosition.y = 0;
if (paddlePosition.y > screenHeight - paddleSize.y) paddlePosition.y = screenHeight - paddleSize.y;
</code>
<h2>5. Обнаружение столкновений (мяч и ракетка)</h2>
<p>Для реализации <b>удара</b> ракеткой по мячу используется следующий код:</p>
<code>if (CheckCollisionCircleRec(ballPosition, ballRadius,
    (Rectangle) { paddlePosition.x, paddlePosition.y, paddleSize.x, paddleSize.y }))
{
    if (ballSpeed.x < 0)
    {
        ballSpeed.x *= -1.1f;
        score++;
    }
}
</code><br>
<p>В данном коде проверяется столкновение мяча с ракеткой. Если мяч летел влево, он отскакивает и ускоряется на 10%, а счет увеличивается.</p>
<h2>6. Отрисовка графики</h2>
<p>Для <b>графического представления</b> игры используется следющий код:</p>
<code>BeginDrawing();
ClearBackground(RAYWHITE);
DrawCircleV(ballPosition, ballRadius, MAROON);
DrawRectangleV(paddlePosition, paddleSize, BLACK);
DrawText(TextFormat("Score: %i", score), 10, 10, 20, DARKGRAY);
DrawText("Управляйте ракеткой с помощью мыши", screenWidth - 350, screenHeight - 30, 15, GRAY);
EndDrawing();
</code><br>
<p>Данный код:</p>
<ol>
    <li>Очищает экран перед отрисовкой (ClearBackground).</li>
    <li>Рисует мяч (DrawCircleV).</li>
    <li>Рисует ракетку (DrawRectangleV).</li>
    <li>Выводит счет (DrawText).</li>
    <li>Выводит инструкцию (DrawText).</li>
</ol>
<h2>7. Закрытие окна</h2>
<p>Для <b>завершения игры</b> используется следующий код</p>
<code>CloseWindow();
return 0;
</code>
<h2>8. Результат выполнения программы</h2>
<img width="650" src="https://github.com/user-attachments/assets/21e44945-00aa-4103-913a-2742666897be" />
<img width="650" src="https://github.com/user-attachments/assets/d239ee67-bf75-4cce-b4c2-2825706b2c60" />
<img width="650" src="https://github.com/user-attachments/assets/03be62a2-92e4-4278-af64-49311d141d86" />
