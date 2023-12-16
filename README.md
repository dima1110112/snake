<!DOCTYPE html>
<html>
<head>
    <title>Игра Змейка на JavaScript</title>
    <style>
        canvas {
            border: 1px solid black;
        }
    </style>
</head>
<body>
    <canvas id="canvas" width="400" height="400"></canvas>
    <script>
        // Получаем контекст холста
        var canvas = document.getElementById("canvas");
        var ctx = canvas.getContext("2d");

        // Задаем начальные координаты и скорость змейки
        var snake = [];
        snake[0] = {x: 200, y: 200};
        var dx = 10;
        var dy = 0;

        // Размер клетки и количество клеток на поле
        var cellSize = 10;
        var cells = canvas.width / cellSize;

        // Задаем начальные координаты еды
        var food = {
            x: Math.floor(Math.random() * cells) * cellSize,
            y: Math.floor(Math.random() * cells) * cellSize
        };

        // Рисуем змейку
        function drawSnake() {
            for(var i = 0; i < snake.length; i++) {
                ctx.fillStyle = (i == 0) ? "#0095DD" : "#00FF00";
                ctx.fillRect(snake[i].x, snake[i].y, cellSize, cellSize);
            }
        }

        // Рисуем еду
        function drawFood() {
            ctx.fillStyle = "#FF0000";
            ctx.fillRect(food.x, food.y, cellSize, cellSize);
        }

        // Обработка нажатий клавиш
        document.addEventListener("keydown", function(event) {
            if(event.keyCode == 37 && dx != 10) {
                dx = -10;
                dy = 0;
            }
            else if(event.keyCode == 38 && dy != 10) {
                dx = 0;
                dy = -10;
            }
            else if(event.keyCode == 39 && dx != -10) {
                dx = 10;
                dy = 0;
            }
            else if(event.keyCode == 40 && dy != -10) {
                dx = 0;
                dy = 10;
            }
        });

        // Проверка на столкновение со стенками и самой змейкой
        function checkCollision() {
            if(snake[0].x < 0 || snake[0].x >= canvas.width || snake[0].y < 0 || snake[0].y >= canvas.height) {
                return true;
            }
            for(var i = 1; i < snake.length; i++) {
                if(snake[0].x == snake[i].x && snake[0].y == snake[i].y) {
                    return true;
                }
            }
            return false;
        }

        // Обновление игры
        function update() {
            // Движение змейки
            var head = {x: snake[0].x + dx, y: snake[0].y + dy};
            snake.unshift(head);

            // Проверка на съедание еды
            if(snake[0].x == food.x && snake[0].y == food.y) {
                food = {
                    x: Math.floor(Math.random() * cells) * cellSize,
                    y: Math.floor(Math.random() * cells) * cellSize
                };
            }
            else {
                snake.pop();
            }

            // Проверка на столкновение
            if(checkCollision()) {
                clearInterval(gameInterval);
                alert("Игра окончена!");
            }

            // Отрисовка игровых объектов
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            drawSnake();
            drawFood();
        }

        // Запускаем игру
        var gameInterval = setInterval(update, 100);
    </script>
</body>
</html>
