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
            overflow-x: hidden;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            padding: 20px;
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
            font-size: 2.5rem;
        }
        
        .exercise-area {
            width: 100%;
            height: 60vh;
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
            max-height: 80vh;
            overflow-y: auto;
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
            transition: all 0.3s ease;
            overflow: hidden;
            max-height: 200px;
        }
        
        .instructions.hidden {
            max-height: 0;
            padding: 0;
            margin: 0;
            opacity: 0;
        }
        
        .instructions h2 {
            margin-bottom: 10px;
            color: #ff8a00;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        
        .instructions h2 span {
            cursor: pointer;
            font-size: 18px;
        }
        
        .instructions p {
            margin-bottom: 15px;
            line-height: 1.5;
        }
        
        .trainer-types {
            display: flex;
            flex-wrap: wrap;
            gap: 10px;
            justify-content: center;
            margin-bottom: 20px;
        }
        
        .trainer-type-btn {
            background: rgba(255, 255, 255, 0.2);
            border: none;
            color: white;
            padding: 10px 20px;
            border-radius: 50px;
            cursor: pointer;
            transition: all 0.3s ease;
            backdrop-filter: blur(5px);
        }
        
        .trainer-type-btn.active {
            background: rgba(255, 138, 0, 0.5);
            box-shadow: 0 0 15px rgba(255, 138, 0, 0.5);
        }
        
        .timer {
            font-size: 1.2rem;
            margin-bottom: 15px;
            background: rgba(255, 255, 255, 0.1);
            padding: 10px 20px;
            border-radius: 50px;
            display: inline-block;
            backdrop-filter: blur(5px);
        }
        
        .sound-toggle {
            position: fixed;
            bottom: 20px;
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
        }
        
        .sound-toggle.muted svg {
            opacity: 0.5;
        }
        
        @media (max-width: 768px) {
            .settings-menu {
                width: 90%;
                right: 5%;
                left: 5%;
            }
            
            h1 {
                font-size: 2rem;
            }
            
            .exercise-area {
                height: 50vh;
            }
            
            body {
                overflow-y: auto;
                justify-content: flex-start;
                padding-top: 40px;
            }
            
            .trainer-types {
                flex-direction: column;
                align-items: center;
            }
            
            .trainer-type-btn {
                width: 100%;
                max-width: 250px;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Тренажёр для глаз</h1>
        
        <div class="trainer-types">
            <button class="trainer-type-btn active" data-type="movingObject">Движущийся объект</button>
            <button class="trainer-type-btn" data-type="focusChange">Смена фокуса</button>
            <button class="trainer-type-btn" data-type="palming">Пальминг</button>
            <button class="trainer-type-btn" data-type="figureEight">Восьмёрка</button>
        </div>
        
        <div class="timer" id="timer">00:00</div>
        
        <div class="exercise-area" id="exerciseArea">
            <div class="target" id="target"></div>
            <div id="specialContent"></div>
        </div>
    </div>
    
    <div class="settings-icon" id="settingsIcon">
        <svg viewBox="0 0 24 24">
            <path d="M12 15.5A3.5 3.5 0 0 1 8.5 12 3.5 3.5 0 0 1 12 8.5a3.5 3.5 0 0 1 3.5 3.5 3.5 3.5 0 0 1-3.5 3.5m7.43-2.53c.04.32.07.64.07.97 0 .33-.03.66-.07.97l2.44 1.92c.18.14.23.41.12.62l-2.3 3.98c-.12.21-.37.31-.59.22l-2.88-1.18c-.61.48-1.29.87-2.02 1.15l-.44 3.06c-.04.24-.24.42-.49.42h-4.6c-.25 0-.45-.18-.49-.42l-.44-3.06c-.73-.28-1.41-.67-2.02-1.15l-2.88 1.18c-.22.09-.47 0-.59-.22l-2.3-3.98c-.12-.21-.08-.47.12-.62l2.44-1.92c-.04-.31-.07-.64-.07-.97 0-.33.03-.66.07-.97l-2.44-1.92c-.18-.14-.23-.41-.12-.62l2.3-3.98c.12-.21.37-.31.59-.22l2.88 1.18c.61-.48 1.29-.87 2.02-1.15l.44-3.06c.04-.24.24-.42.49-.42h4.6c.25 0 .45.18.49.42l.44 3.06c.73.28 1.41.67 2.02 1.15l2.88-1.18c.22-.09.47 0 .59.22l2.3 3.98c.12.21.08.47-.12.62l-2.44 1.92z"/>
        </svg>
    </div>
    
    <div class="sound-toggle" id="soundToggle">
        <svg viewBox="0 0 24 24" width="24" height="24" fill="white">
            <path d="M14,3.23V5.29C16.89,6.15 19,8.83 19,12C19,15.17 16.89,17.84 14,18.7V20.77C18,19.86 21,16.28 21,12C21,7.72 18,4.14 14,3.23M16.5,12C16.5,10.23 15.5,8.71 14,7.97V16C15.5,15.29 16.5,13.76 16.5,12M3,9V15H7L12,20V4L7,9H3Z"/>
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
        
        <div class="setting-group">
            <label for="duration">Длительность сеанса (мин)</label>
            <input type="range" id="duration" min="1" max="10" value="3">
        </div>
        
        <button id="saveSettings" style="width:100%; padding:12px; margin-top:10px; background:rgba(255,138,0,0.7); border:none; border-radius:5px; color:white; cursor:pointer;">Сохранить настройки</button>
    </div>
    
    <div class="instructions" id="instructions">
        <h2>Инструкция <span id="toggleInstructions">−</span></h2>
        <div id="instructionsContent">
            <p>Следите за движущимся объектом, не двигая головой. Это упражнение помогает расслабить глазные мышцы и улучшить кровообращение.</p>
            <p>Рекомендуется выполнять упражнение в течение 2-3 минут, затем сделать перерыв.</p>
            <p>В режиме "Пальминг" закройте глаза и прикройте их ладонями, слушая звуковой отчет.</p>
        </div>
    </div>

    <script>
        document.addEventListener('DOMContentLoaded', function() {
            // Элементы DOM
            const exerciseArea = document.getElementById('exerciseArea');
            const target = document.getElementById('target');
            const settingsIcon = document.getElementById('settingsIcon');
            const settingsMenu = document.getElementById('settingsMenu');
            const saveSettingsBtn = document.getElementById('saveSettings');
            const instructions = document.getElementById('instructions');
            const toggleInstructions = document.getElementById('toggleInstructions');
            const timer = document.getElementById('timer');
            const specialContent = document.getElementById('specialContent');
            const trainerTypeButtons = document.querySelectorAll('.trainer-type-btn');
            const soundToggle = document.getElementById('soundToggle');
            
            // Настройки по умолчанию
            let settings = {
                speed: 5,
                size: 40,
                color: '#ff8a00',
                trail: 3,
                pattern: 'random',
                bgColor: '#1a2a6c',
                duration: 3,
                currentTrainer: 'movingObject',
                soundEnabled: true
            };
            
            // Состояние тренажёра
            let isRunning = true;
            let animationId = null;
            let x = 0;
            let y = 0;
            let dx = 2;
            let dy = 2;
            let startTime = null;
            let timerInterval = null;
            let palmTimer = null;
            let palmCounter = 30;
            let tickSound = null;
            
            // Инициализация звука
            function initSound() {
                try {
                    // Создаем звук тикающего маятника
                    const AudioContext = window.AudioContext || window.webkitAudioContext;
                    const audioContext = new AudioContext();
                    
                    // Создаем осциллятор для звука тика
                    tickSound = function() {
                        if (!settings.soundEnabled) return;
                        
                        const oscillator = audioContext.createOscillator();
                        const gainNode = audioContext.createGain();
                        
                        oscillator.type = 'sine';
                        oscillator.frequency.setValueAtTime(800, audioContext.currentTime);
                        gainNode.gain.setValueAtTime(0.1, audioContext.currentTime);
                        gainNode.gain.exponentialRampToValueAtTime(0.001, audioContext.currentTime + 0.1);
                        
                        oscillator.connect(gainNode);
                        gainNode.connect(audioContext.destination);
                        
                        oscillator.start();
                        oscillator.stop(audioContext.currentTime + 0.1);
                    };
                } catch (e) {
                    console.log('Аудио не поддерживается в этом браузере', e);
                }
            }
            
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
                    document.getElementById('duration').value = settings.duration;
                    
                    // Настройка звука
                    if (!settings.soundEnabled) {
                        soundToggle.classList.add('muted');
                    }
                    
                    // Активируем текущий тип тренажёра
                    document.querySelectorAll('.trainer-type-btn').forEach(btn => {
                        if (btn.dataset.type === settings.currentTrainer) {
                            btn.classList.add('active');
                        } else {
                            btn.classList.remove('active');
                        }
                    });
                    
                    // Применяем настройки к тренажёру
                    applySettings();
                    changeTrainerType(settings.currentTrainer);
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
                settings.duration = parseInt(document.getElementById('duration').value);
                
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
                const num = parseInt(hex.replace("#", ""), 16);
                const R = (num >> 16) + percent;
                const G = ((num >> 8) & 0x00FF) + percent;
                const B = (num & 0x0000FF) + percent;
                
                return `#${((1 << 24) + (R < 0 ? 0 : R > 255 ? 255 : R) * 65536 + 
                          (G < 0 ? 0 : G > 255 ? 255 : G) * 256 + 
                          (B < 0 ? 0 : B > 255 ? 255 : B)).toString(16).slice(1)}`;
            }
            
            // Функция анимации для движущегося объекта
            function animateMovingObject() {
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
                
                animationId = requestAnimationFrame(animateMovingObject);
            }
            
            // Анимация для смены фокуса
            function animateFocusChange() {
                if (!isRunning) return;
                
                const time = new Date().getTime() * 0.001;
                const scale = 0.7 + 0.3 * Math.sin(time);
                
                target.style.transform = `scale(${scale})`;
                target.style.left = '50%';
                target.style.top = '50%';
                target.style.marginLeft = `-${settings.size/2}px`;
                target.style.marginTop = `-${settings.size/2}px`;
                
                animationId = requestAnimationFrame(animateFocusChange);
            }
            
            // Анимация для пальминга
            function animatePalming() {
                if (!isRunning) return;
                
                exerciseArea.style.background = 'rgba(0, 0, 0, 0.8)';
                target.style.display = 'none';
                
                palmCounter = 30;
                
                specialContent.innerHTML = `
                    <div style="color: white; font-size: 24px; text-align: center; padding-top: 30%">
                        Закройте глаза и мягко прикройте их ладонями<br>
                        <div style="font-size: 36px; margin-top: 20px;" id="palmCounter">30</div>
                    </div>
                `;
                
                // Запускаем звуковой отчет
                if (palmTimer) clearInterval(palmTimer);
                
                palmTimer = setInterval(() => {
                    if (palmCounter > 0 && isRunning && settings.currentTrainer === 'palming') {
                        palmCounter--;
                        document.getElementById('palmCounter').textContent = palmCounter;
                        
                        // Воспроизводим звук тика
                        if (tickSound) {
                            tickSound();
                        }
                    } else if (palmCounter <= 0) {
                        clearInterval(palmTimer);
                        
                        // Перезапускаем через 2 секунды
                        setTimeout(() => {
                            if (isRunning && settings.currentTrainer === 'palming') {
                                exerciseArea.style.background = '';
                                target.style.display = 'block';
                                specialContent.innerHTML = '';
                                setTimeout(animatePalming, 1000);
                            }
                        }, 2000);
                    }
                }, 1000);
            }
            
            // Анимация для восьмёрки
            function animateFigureEight() {
                if (!isRunning) return;
                
                const time = new Date().getTime() * 0.001 * settings.speed / 5;
                const areaWidth = exerciseArea.offsetWidth;
                const areaHeight = exerciseArea.offsetHeight;
                const a = Math.min(areaWidth, areaHeight) * 0.2;
                
                // Сложная траектория в виде восьмёрки
                x = areaWidth/2 + a * Math.sin(time) * Math.cos(time);
                y = areaHeight/2 + a * Math.sin(time);
                
                target.style.left = `${x - settings.size/2}px`;
                target.style.top = `${y - settings.size/2}px`;
                
                animationId = requestAnimationFrame(animateFigureEight);
            }
            
            // Смена типа тренажёра
            function changeTrainerType(type) {
                settings.currentTrainer = type;
                
                // Останавливаем текущую анимацию
                if (animationId) {
                    cancelAnimationFrame(animationId);
                }
                
                // Останавливаем таймер пальминга
                if (palmTimer) {
                    clearInterval(palmTimer);
                    palmTimer = null;
                }
                
                // Сбрасываем специальный контент
                specialContent.innerHTML = '';
                target.style.display = 'block';
                exerciseArea.style.background = '';
                
                // Запускаем выбранную анимацию
                switch(type) {
                    case 'movingObject':
                        animateMovingObject();
                        break;
                    case 'focusChange':
                        animateFocusChange();
                        break;
                    case 'palming':
                        animatePalming();
                        break;
                    case 'figureEight':
                        animateFigureEight();
                        break;
                }
            }
            
            // Обновление таймера
            function updateTimer() {
                if (!startTime) {
                    startTime = new Date();
                }
                
                const now = new Date();
                const diff = now - startTime;
                const minutes = Math.floor(diff / 60000);
                const seconds = Math.floor((diff % 60000) / 1000);
                
                timer.textContent = `${minutes.toString().padStart(2, '0')}:${seconds.toString().padStart(2, '0')}`;
                
                // Проверяем, не истекло ли время сеанса
                if (minutes >= settings.duration) {
                    // Сбрасываем таймер
                    startTime = new Date();
                    
                    // Можно добавить уведомление о завершении сеанса
                }
            }
            
            // Инициализация
            initSound();
            loadSettings();
            
            // Запускаем анимацию сразу
            animateMovingObject();
            
            // Запускаем таймер
            timerInterval = setInterval(updateTimer, 1000);
            
            // Обработчики событий
            settingsIcon.addEventListener('click', () => {
                settingsMenu.classList.toggle('active');
            });
            
            saveSettingsBtn.addEventListener('click', saveSettings);
            
            toggleInstructions.addEventListener('click', () => {
                instructions.classList.toggle('hidden');
                toggleInstructions.textContent = instructions.classList.contains('hidden') ? '+' : '−';
            });
            
            // Обработчики для кнопок выбора типа тренажёра
            trainerTypeButtons.forEach(btn => {
                btn.addEventListener('click', () => {
                    trainerTypeButtons.forEach(b => b.classList.remove('active'));
                    btn.classList.add('active');
                    changeTrainerType(btn.dataset.type);
                });
            });
            
            // Переключение звука
            soundToggle.addEventListener('click', () => {
                settings.soundEnabled = !settings.soundEnabled;
                soundToggle.classList.toggle('muted');
                
                // Сохраняем настройки
                localStorage.setItem('eyeTrainerSettings', JSON.stringify(settings));
            });
            
            // Адаптация для мобильных устройств
            function checkMobile() {
                if (/Android|webOS|iPhone|iPad|iPod|BlackBerry|IEMobile|Opera Mini/i.test(navigator.userAgent)) {
                    // Убираем overflow: hidden для body чтобы разрешить прокрутку
                    document.body.style.overflow = 'auto';
                    document.body.style.justifyContent = 'flex-start';
                }
            }
            
            checkMobile();
        });
    </script>
</body>
</html>
