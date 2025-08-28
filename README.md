<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Тренажёр для глаз</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        
        body {
            font-family: 'Arial', sans-serif;
            background: linear-gradient(135deg, #1a2a6c, #b21f1f, #fdbb2d);
            color: #fff;
            min-height: 100vh;
            overflow: hidden;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
        }
        
        .container {
            width: 100%;
            max-width: 1200px;
            text-align: center;
            position: relative;
        }
        
        h1 {
            margin-bottom: 20px;
            text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.5);
        }
        
        .exercise-area {
            width: 100%;
            height: 70vh;
            background-color: rgba(255, 255, 255, 0.1);
            border-radius: 15px;
            position: relative;
            overflow: hidden;
            margin-bottom: 20px;
            box-shadow: 0 8px 32px rgba(0, 0, 0, 0.3);
            backdrop-filter: blur(5px);
            border: 1px solid rgba(255, 255, 255, 0.2);
        }
        
        .target {
            position: absolute;
            border-radius: 50%;
            background: radial-gradient(circle, #ff8a00, #ff2070);
            box-shadow: 0 0 25px rgba(255, 138, 0, 0.7);
            transition: transform 0.2s;
        }
        
        .controls {
            margin: 20px 0;
        }
        
        button {
            background: rgba(255, 255, 255, 0.2);
            border: none;
            color: white;
            padding: 12px 24px;
            margin: 0 10px;
            border-radius: 50px;
            cursor: pointer;
            font-size: 16px;
            transition: all 0.3s ease;
            backdrop-filter: blur(5px);
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.2);
        }
        
        button:hover {
            background: rgba(255, 255, 255, 0.3);
            transform: translateY(-2px);
        }
        
        .settings-icon {
            position: fixed;
            top: 20px;
            right: 20px;
            background: rgba(255, 255, 255, 0.2);
            width: 50px;
            height: 50px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            z-index: 100;
            backdrop-filter: blur(5px);
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.2);
            transition: all 0.3s ease;
        }
        
        .settings-icon:hover {
            transform: rotate(45deg);
            background: rgba(255, 255, 255, 0.3);
        }
        
        .settings-icon svg {
            width: 24px;
            height: 24px;
            fill: white;
        }
        
        .settings-menu {
            position: fixed;
            top: 80px;
            right: 20px;
            background: rgba(30, 30, 50, 0.95);
            border-radius: 15px;
            padding: 20px;
            width: 300px;
            box-shadow: 0 8px 32px rgba(0, 0, 0, 0.3);
            backdrop-filter: blur(10px);
            border: 1px solid rgba(255, 255, 255, 0.2);
            z-index: 99;
            transform: translateX(400px);
            transition: transform 0.4s ease;
        }
        
        .settings-menu.active {
            transform: translateX(0);
        }
        
        .settings-menu h2 {
            margin-bottom: 20px;
            text-align: center;
            color: #ff8a00;
        }
        
        .setting-group {
            margin-bottom: 15px;
        }
        
        .setting-group label {
            display: block;
            margin-bottom: 5px;
            font-weight: bold;
        }
        
        .setting-group input[type="range"] {
            width: 100%;
            height: 8px;
            -webkit-appearance: none;
            background: rgba(255, 255, 255, 0.2);
            border-radius: 4px;
            outline: none;
        }
        
        .setting-group input[type="range"]::-webkit-slider-thumb {
            -webkit-appearance: none;
            width: 20px;
            height: 20px;
            border-radius: 50%;
            background: #ff8a00;
            cursor: pointer;
        }
        
        .setting-group input[type="color"] {
            width: 100%;
            height: 40px;
            border: none;
            border-radius: 5px;
            background: none;
            cursor: pointer;
        }
        
        .setting-group select {
            width: 100%;
            padding: 10px;
            border-radius: 5px;
            background: rgba(255, 255, 255, 0.1);
            border: 1px solid rgba(255, 255, 255, 0.2);
            color: white;
        }
        
        .instructions {
            background: rgba(255, 255, 255, 0.1);
            padding: 20px;
            border-radius: 15px;
            margin-top: 20px;
            max-width: 600px;
            backdrop-filter: blur(5px);
            border: 1px solid rgba(255, 255, 255, 0.2);
        }
        
        .instructions h2 {
            margin-bottom: 10px;
            color: #ff8a00;
        }
        
        .instructions p {
            margin-bottom: 15px;
            line-height: 1.5;
        }
        
        @media (max-width: 768px) {
            .settings-menu {
                width: 80%;
                right: 10%;
                left: 10%;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Тренажёр для глаз</h1>
        
        <div class="exercise-area" id="exerciseArea">
            <div class="target" id="target"></div>
        </div>
        
        <div class="controls">
            <button id="startBtn">Старт</button>
            <button id="pauseBtn">Пауза</button>
            <button id="resetBtn">Сброс</button>
        </div>
        
        <div class="instructions">
            <h2>Инструкция</h2>
            <p>Следите за движущимся объектом, не двигая головой. Это упражнение помогает расслабить глазные мышцы и улучшить кровообращение.</p>
            <p>Рекомендуется выполнять упражнение в течение 2-3 минут, затем сделать перерыв.</p>
        </div>
    </div>
    
    <div class="settings-icon" id="settingsIcon">
        <svg viewBox="0 0 24 24">
            <path d="M12 15.5A3.5 3.5 0 0 1 8.5 12 3.5 3.5 0 0 1 12 8.5a3.5 3.5 0 0 1 3.5 3.5 3.5 3.5 0 0 1-3.5 3.5m7.43-2.53c.04.32.07.64.07.97 0 .33-.03.66-.07.97l2.44 1.92c.18.14.23.41.12.62l-2.3 3.98c-.12.21-.37.31-.59.22l-2.88-1.18c-.61.48-1.29.87-2.02 1.15l-.44 3.06c-.04.24-.24.42-.49.42h-4.6c-.25 0-.45-.18-.49-.42l-.44-3.06c-.73-.28-1.41-.67-2.02-1.15l-2.88 1.18c-.22.09-.47 0-.59-.22l-2.3-3.98c-.12-.21-.08-.47.12-.62l2.44-1.92c-.04-.31-.07-.64-.07-.97 0-.33.03-.66.07-.97l-2.44-1.92c-.18-.14-.23-.41-.12-.62l2.3-3.98c.12-.21.37-.31.59-.22l2.88 1.18c.61-.48 1.29-.87 2.02-1.15l.44-3.06c.04-.24.24-.42.49-.42h4.6c.25 0 .45.18.49.42l.44 3.06c.73.28 1.41.67 2.02 1.15l2.88-1.18c.22-.09.47 0 .59.22l2.3 3.98c.12.21.08.47-.12.62l-2.44 1.92z"/>
        </svg>
    </div>
    
    <div class="settings-menu" id="settingsMenu">
        <h2>Настройки тренажёра</h2>
        
        <div class="setting-group">
            <label for="speed">Скорость движения</label>
            <input type="range" id="speed" min="1" max="10" value="5">
        </div>
        
        <div class="setting-group">
            <label for="size">Размер объекта</label>
            <input type="range" id="size" min="10" max="100" value="40">
        </div>
        
        <div class="setting-group">
            <label for="color">Цвет объекта</label>
            <input type="color" id="color" value="#ff8a00">
        </div>
        
        <div class="setting-group">
            <label for="trail">Эффект шлейфа</label>
            <input type="range" id="trail" min="0" max="10" value="3">
        </div>
        
        <div class="setting-group">
            <label for="pattern">Траектория движения</label>
            <select id="pattern">
                <option value="random">Случайная</option>
                <option value="circular">Круговая</option>
                <option value="horizontal">Горизонтальная</option>
                <option value="vertical">Вертикальная</option>
                <option value="figure8">Восьмёрка</option>
            </select>
        </div>
        
        <div class="setting-group">
            <label for="bgColor">Цвет фона</label>
            <input type="color" id="bgColor" value="#1a2a6c">
        </div>
        
        <button id="saveSettings">Сохранить настройки</button>
    </div>

    <script>
        document.addEventListener('DOMContentLoaded', function() {
            // Элементы DOM
            const exerciseArea = document.getElementById('exerciseArea');
            const target = document.getElementById('target');
            const startBtn = document.getElementById('startBtn');
            const pauseBtn = document.getElementById('pauseBtn');
            const resetBtn = document.getElementById('resetBtn');
            const settingsIcon = document.getElementById('settingsIcon');
            const settingsMenu = document.getElementById('settingsMenu');
            const saveSettingsBtn = document.getElementById('saveSettings');
            
            // Настройки по умолчанию
            let settings = {
                speed: 5,
                size: 40,
                color: '#ff8a00',
                trail: 3,
                pattern: 'random',
                bgColor: '#1a2a6c'
            };
            
            // Состояние тренажёра
            let isRunning = false;
            let animationId = null;
            let x = 0;
            let y = 0;
            let dx = 2;
            let dy = 2;
            
            // Загрузка сохранённых настроек
            function loadSettings() {
                const savedSettings = localStorage.getItem('eyeTrainerSettings');
                if (savedSettings) {
                    settings = JSON.parse(savedSettings);
                    
                    // Применяем настройки к элементам управления
                    document.getElementById('speed').value = settings.speed;
                    document.getElementById('size').value = settings.size;
                    document.getElementById('color').value = settings.color;
                    document.getElementById('trail').value = settings.trail;
                    document.getElementById('pattern').value = settings.pattern;
                    document.getElementById('bgColor').value = settings.bgColor;
                    
                    // Применяем настройки к тренажёру
                    applySettings();
                }
            }
            
            // Сохранение настроек
            function saveSettings() {
                settings.speed = parseInt(document.getElementById('speed').value);
                settings.size = parseInt(document.getElementById('size').value);
                settings.color = document.getElementById('color').value;
                settings.trail = parseInt(document.getElementById('trail').value);
                settings.pattern = document.getElementById('pattern').value;
                settings.bgColor = document.getElementById('bgColor').value;
                
                localStorage.setItem('eyeTrainerSettings', JSON.stringify(settings));
                applySettings();
                
                // Скрываем меню настроек
                settingsMenu.classList.remove('active');
            }
            
            // Применение настроек
            function applySettings() {
                // Размер цели
                target.style.width = `${settings.size}px`;
                target.style.height = `${settings.size}px`;
                
                // Цвет цели
                target.style.background = `radial-gradient(circle, ${settings.color}, ${adjustColor(settings.color, -50)})`;
                
                // Цвет фона
                document.body.style.background = `linear-gradient(135deg, ${settings.bgColor}, ${adjustColor(settings.bgColor, 40)}, ${adjustColor(settings.bgColor, 80)})`;
            }
            
            // Вспомогательная функция для корректировки цвета
            function adjustColor(hex, percent) {
                // Упрощённая реализация для демонстрации
                // В реальном приложении нужно преобразовать hex в RGB, изменить и вернуть обратно в hex
                return hex; // Здесь должна быть реальная реализация
            }
            
            // Функция анимации
            function animate() {
                if (!isRunning) return;
                
                const areaWidth = exerciseArea.offsetWidth;
                const areaHeight = exerciseArea.offsetHeight;
                const targetSize = settings.size;
                
                // Движение в зависимости от выбранного паттерна
                switch(settings.pattern) {
                    case 'random':
                        x += dx * settings.speed / 5;
                        y += dy * settings.speed / 5;
                        
                        if (x + targetSize > areaWidth || x < 0) {
                            dx = -dx;
                        }
                        
                        if (y + targetSize > areaHeight || y < 0) {
                            dy = -dy;
                        }
                        break;
                        
                    case 'circular':
                        // Круговое движение
                        const time = new Date().getTime() * 0.002;
                        const radius = Math.min(areaWidth, areaHeight) * 0.3;
                        x = Math.cos(time) * radius + areaWidth / 2 - targetSize / 2;
                        y = Math.sin(time) * radius + areaHeight / 2 - targetSize / 2;
                        break;
                        
                    case 'horizontal':
                        // Горизонтальное движение
                        x += dx * settings.speed / 5;
                        y = areaHeight / 2 - targetSize / 2;
                        
                        if (x + targetSize > areaWidth || x < 0) {
                            dx = -dx;
                        }
                        break;
                        
                    case 'vertical':
                        // Вертикальное движение
                        x = areaWidth / 2 - targetSize / 2;
                        y += dy * settings.speed / 5;
                        
                        if (y + targetSize > areaHeight || y < 0) {
                            dy = -dy;
                        }
                        break;
                        
                    case 'figure8':
                        // Движение по траектории восьмёрки
                        const t = new Date().getTime() * 0.001 * settings.speed / 5;
                        const a = Math.min(areaWidth, areaHeight) * 0.25;
                        x = a * Math.sin(t) + areaWidth / 2 - targetSize / 2;
                        y = a * Math.sin(t) * Math.cos(t) + areaHeight / 2 - targetSize / 2;
                        break;
                }
                
                // Ограничение координат
                x = Math.max(0, Math.min(areaWidth - targetSize, x));
                y = Math.max(0, Math.min(areaHeight - targetSize, y));
                
                // Применение позиции
                target.style.left = `${x}px`;
                target.style.top = `${y}px`;
                
                // Эффект шлейфа
                if (settings.trail > 0) {
                    const trailElement = target.cloneNode();
                    trailElement.style.opacity = '0.7';
                    trailElement.style.transform = 'scale(1.05)';
                    exerciseArea.appendChild(trailElement);
                    
                    setTimeout(() => {
                        if (exerciseArea.contains(trailElement)) {
                            exerciseArea.removeChild(trailElement);
                        }
                    }, settings.trail * 100);
                }
                
                animationId = requestAnimationFrame(animate);
            }
            
            // Управление тренажёром
            startBtn.addEventListener('click', () => {
                if (!isRunning) {
                    isRunning = true;
                    animate();
                }
            });
            
            pauseBtn.addEventListener('click', () => {
                isRunning = false;
                if (animationId) {
                    cancelAnimationFrame(animationId);
                }
            });
            
            resetBtn.addEventListener('click', () => {
                isRunning = false;
                if (animationId) {
                    cancelAnimationFrame(animationId);
                }
                
                // Сброс позиции
                x = exerciseArea.offsetWidth / 2 - settings.size / 2;
                y = exerciseArea.offsetHeight / 2 - settings.size / 2;
                target.style.left = `${x}px`;
                target.style.top = `${y}px`;
                
                // Очистка шлейфа
                const trails = document.querySelectorAll('.target:not(#target)');
                trails.forEach(trail => trail.remove());
            });
            
            // Управление настройками
            settingsIcon.addEventListener('click', () => {
                settingsMenu.classList.toggle('active');
            });
            
            saveSettingsBtn.addEventListener('click', saveSettings);
            
            // Инициализация
            loadSettings();
            resetBtn.click(); // Сброс для начальной позиции
        });
    </script>
</body>
</html>
