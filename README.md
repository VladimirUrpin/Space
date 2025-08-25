<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Виртуальная Солнечная система</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        
        body {
            overflow: hidden;
            font-family: 'Arial', sans-serif;
            color: white;
            background: #000;
        }
        
        #canvas-container {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
        }
        
        #ui-container {
            position: absolute;
            bottom: 20px;
            left: 0;
            width: 100%;
            display: flex;
            justify-content: center;
            z-index: 100;
        }
        
        .planet-selector {
            display: flex;
            flex-wrap: wrap;
            justify-content: center;
            background: rgba(0, 30, 60, 0.7);
            padding: 10px;
            border-radius: 20px;
            backdrop-filter: blur(10px);
            max-width: 90%;
        }
        
        .planet-btn {
            width: 40px;
            height: 40px;
            margin: 5px;
            border-radius: 50%;
            border: 2px solid rgba(255, 255, 255, 0.5);
            background-size: cover;
            cursor: pointer;
            transition: all 0.3s ease;
            position: relative;
        }
        
        .planet-btn:hover {
            transform: scale(1.1);
            border-color: white;
        }
        
        .planet-btn:hover::after {
            content: attr(data-name);
            position: absolute;
            bottom: -30px;
            left: 50%;
            transform: translateX(-50%);
            background: rgba(0, 0, 0, 0.8);
            padding: 5px 10px;
            border-radius: 5px;
            white-space: nowrap;
            z-index: 101;
        }
        
        #sun { background: radial-gradient(circle, #ffd700, #ff8c00); }
        #mercury { background: radial-gradient(circle, #a9a9a9, #696969); }
        #venus { background: radial-gradient(circle, #daa520, #b8860b); }
        #earth { background: radial-gradient(circle, #1e90ff, #006400); }
        #mars { background: radial-gradient(circle, #ff4500, #8b0000); }
        #jupiter { background: radial-gradient(circle, #ffa500, #cd853f); }
        #saturn { background: radial-gradient(circle, #f4a460, #d2691e); }
        #uranus { background: radial-gradient(circle, #87ceeb, #4682b4); }
        #neptune { background: radial-gradient(circle, #0000cd, #191970); }
        #pluto { background: radial-gradient(circle, #bc8f8f, #a0522d); }
        
        #info-panel {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            width: 400px;
            height: 300px;
            background: rgba(0, 30, 60, 0.9);
            padding: 15px;
            border-radius: 10px;
            backdrop-filter: blur(10px);
            z-index: 200;
            resize: both;
            overflow: auto;
            display: none;
            box-shadow: 0 0 20px rgba(0, 100, 255, 0.5);
            border: 1px solid #1e90ff;
            min-width: 300px;
            min-height: 200px;
        }
        
        #info-panel.visible {
            display: block;
        }
        
        #close-info {
            position: absolute;
            top: 5px;
            right: 10px;
            background: none;
            border: none;
            color: white;
            font-size: 20px;
            cursor: pointer;
        }
        
        .panel-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 15px;
            padding-bottom: 10px;
            border-bottom: 1px solid #1e90ff;
        }
        
        .panel-content {
            height: calc(100% - 60px);
            overflow-y: auto;
        }
        
        .controls {
            position: absolute;
            bottom: 90px;
            left: 0;
            width: 100%;
            text-align: center;
            color: #ccc;
            font-size: 14px;
            padding: 0 10px;
        }
        
        h2 {
            margin-bottom: 10px;
            color: #63ace5;
        }
        
        #settings-btn {
            position: absolute;
            bottom: 20px;
            right: 20px;
            width: 40px;
            height: 40px;
            background: rgba(0, 30, 60, 0.7);
            border-radius: 50%;
            display: flex;
            justify-content: center;
            align-items: center;
            cursor: pointer;
            z-index: 100;
            backdrop-filter: blur(10px);
            border: 1px solid rgba(255, 255, 255, 0.3);
        }
        
        #settings-btn:hover {
            background: rgba(0, 50, 100, 0.9);
        }
        
        #settings-panel {
            position: absolute;
            bottom: 70px;
            right: 20px;
            background: rgba(0, 30, 60, 0.9);
            padding: 15px;
            border-radius: 10px;
            backdrop-filter: blur(10px);
            z-index: 99;
            width: 250px;
            display: none;
            box-shadow: 0 0 15px rgba(0, 0, 0, 0.5);
            border: 1px solid #1e90ff;
        }
        
        #settings-panel.visible {
            display: block;
        }
        
        .settings-group {
            margin-bottom: 15px;
        }
        
        .settings-group h3 {
            margin-bottom: 8px;
            font-size: 16px;
            color: #63ace5;
        }
        
        .slider-container {
            display: flex;
            align-items: center;
        }
        
        .slider-container span {
            margin-left: 10px;
            min-width: 40px;
            text-align: center;
        }
        
        input[type="range"] {
            width: 100%;
        }
        
        select {
            width: 100%;
            padding: 5px;
            background: rgba(0, 20, 40, 0.8);
            border: 1px solid #1e90ff;
            color: white;
            border-radius: 5px;
        }
        
        @media (max-width: 768px) {
            .planet-btn {
                width: 35px;
                height: 35px;
            }
            
            #info-panel {
                width: 90%;
                height: 50%;
            }
            
            .controls {
                bottom: 120px;
                font-size: 12px;
            }
        }
        
        @media (max-width: 480px) {
            .planet-btn {
                width: 30px;
                height: 30px;
                margin: 3px;
            }
            
            .planet-selector {
                padding: 8px;
                border-radius: 15px;
            }
            
            .controls {
                bottom: 130px;
            }
        }
    </style>
</head>
<body>
    <div id="canvas-container">
        <!-- 3D сцена будет отрисована здесь -->
    </div>
    
    <div id="ui-container">
        <div class="planet-selector">
            <div class="planet-btn" id="sun" data-name="Солнце" title="Солнце"></div>
            <div class="planet-btn" id="mercury" data-name="Меркурий" title="Меркурий"></div>
            <div class="planet-btn" id="venus" data-name="Венера" title="Венера"></div>
            <div class="planet-btn" id="earth" data-name="Земля" title="Земля"></div>
            <div class="planet-btn" id="mars" data-name="Марс" title="Марс"></div>
            <div class="planet-btn" id="jupiter" data-name="Юпитер" title="Юпитер"></div>
            <div class="planet-btn" id="saturn" data-name="Сатурн" title="Сатурн"></div>
            <div class="planet-btn" id="uranus" data-name="Уран" title="Уран"></div>
            <div class="planet-btn" id="neptune" data-name="Нептун" title="Нептун"></div>
            <div class="planet-btn" id="pluto" data-name="Плутон" title="Плутон"></div>
        </div>
    </div>
    
    <div class="controls">
        <p>Используйте колесо мыши для приближения/отдаления | Зажмите левую кнопку мыши для вращения</p>
    </div>
    
    <div id="info-panel">
        <div class="panel-header">
            <h2 id="planet-name"></h2>
            <button id="close-info">×</button>
        </div>
        <div class="panel-content">
            <p id="planet-description"></p>
        </div>
    </div>

    <div id="settings-btn">
        ⚙️
    </div>
    
    <div id="settings-panel">
        <h3>Настройки системы</h3>
        
        <div class="settings-group">
            <h3>Скорость вращения</h3>
            <div class="slider-container">
                <input type="range" id="rotation-speed" min="0.1" max="2" step="0.1" value="1">
                <span id="rotation-value">1x</span>
            </div>
        </div>
        
        <div class="settings-group">
            <h3>Детализация планет</h3>
            <div class="slider-container">
                <input type="range" id="detail-level" min="16" max="64" step="16" value="32">
                <span id="detail-value">32</span>
            </div>
        </div>
        
        <div class="settings-group">
            <h3>Центральная планета</h3>
            <select id="center-planet">
                <option value="sun">Солнце</option>
                <option value="mercury">Меркурий</option>
                <option value="venus">Венера</option>
                <option value="earth">Земля</option>
                <option value="mars">Марс</option>
                <option value="jupiter">Юпитер</option>
                <option value="saturn">Сатурн</option>
                <option value="uranus">Уран</option>
                <option value="neptune">Нептун</option>
                <option value="pluto">Плутон</option>
            </select>
        </div>
    </div>

    <!-- Подключаем Three.js -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/three@0.128.0/examples/js/controls/OrbitControls.min.js"></script>
    
    <script>
        // Основные переменные
        let scene, camera, renderer, controls;
        let planets = {};
        let planetGroups = {};
        let rotationSpeedMultiplier = 1;
        let detailLevel = 32;
        let planetOrbitSpeeds = {};
        
        // Инициализация сцены
        function init() {
            // Создаем сцену
            scene = new THREE.Scene();
            
            // Создаем камеру
            camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
            camera.position.z = 50;
            
            // Создаем рендерер
            renderer = new THREE.WebGLRenderer({ antialias: true });
            renderer.setSize(window.innerWidth, window.innerHeight);
            renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));
            document.getElementById('canvas-container').appendChild(renderer.domElement);
            
            // Добавляем управление камерой
            controls = new THREE.OrbitControls(camera, renderer.domElement);
            controls.enableDamping = true;
            controls.dampingFactor = 0.05;
            controls.minDistance = 5;
            controls.maxDistance = 200;
            
            // Создаем освещение
            const ambientLight = new THREE.AmbientLight(0x333333);
            scene.add(ambientLight);
            
            const sunLight = new THREE.PointLight(0xffffff, 1.5);
            scene.add(sunLight);
            
            // Создаем звездное небо
            createStarField();
            
            // Создаем солнечную систему
            createSolarSystem();
            
            // Обработка событий
            setupEventListeners();
            
            // Запускаем анимацию
            animate();
            
            // Обработка изменения размера окна
            window.addEventListener('resize', onWindowResize);
        }
        
        // Создание звездного неба
        function createStarField() {
            const starGeometry = new THREE.BufferGeometry();
            const starMaterial = new THREE.PointsMaterial({
                color: 0xffffff,
                size: 0.1,
            });
            
            const starVertices = [];
            for (let i = 0; i < 5000; i++) {
                const x = (Math.random() - 0.5) * 2000;
                const y = (Math.random() - 0.5) * 2000;
                const z = (Math.random() - 0.5) * 2000;
                starVertices.push(x, y, z);
            }
            
            starGeometry.setAttribute('position', new THREE.Float32BufferAttribute(starVertices, 3));
            const starField = new THREE.Points(starGeometry, starMaterial);
            scene.add(starField);
        }
        
        // Создание солнечной системы
        function createSolarSystem() {
            // Создаем Солнце
            const sunGeometry = new THREE.SphereGeometry(5, detailLevel, detailLevel);
            const sunMaterial = new THREE.MeshBasicMaterial({ 
                color: 0xffff00,
                emissive: 0xffff00,
                emissiveIntensity: 0.5
            });
            const sun = new THREE.Mesh(sunGeometry, sunMaterial);
            scene.add(sun);
            planets.sun = sun;
            
            // Сохраняем базовые скорости вращения для каждой планеты
            planetOrbitSpeeds = {
                mercury: 0.01,
                venus: 0.007,
                earth: 0.005,
                mars: 0.004,
                jupiter: 0.003,
                saturn: 0.002,
                uranus: 0.0015,
                neptune: 0.001,
                pluto: 0.0007
            };
            
            // Создаем орбиты и планеты
            createPlanet('mercury', 1, 0.4, 7, 0xa9a9a9);
            createPlanet('venus', 2, 0.6, 10, 0xdaa520);
            
            // Земля с луной
            const earthGroup = new THREE.Group();
            scene.add(earthGroup);
            
            const earth = createPlanet('earth', 3, 0.6, 13, 0x1e90ff, earthGroup);
            
            // Луна
            const moonGeometry = new THREE.SphereGeometry(0.2, detailLevel/2, detailLevel/2);
            const moonMaterial = new THREE.MeshStandardMaterial({ color: 0xcccccc });
            const moon = new THREE.Mesh(moonGeometry, moonMaterial);
            moon.position.set(1, 0, 0);
            earth.add(moon);
            
            // Другие планеты
            createPlanet('mars', 4, 0.5, 16, 0xff4500);
            createPlanet('jupiter', 6, 1.2, 22, 0xffa500);
            
            // Сатурн с кольцами
            const saturnGroup = new THREE.Group();
            scene.add(saturnGroup);
            
            const saturn = createPlanet('saturn', 7, 1.0, 28, 0xf4a460, saturnGroup);
            
            // Кольца Сатурна
            const ringGeometry = new THREE.RingGeometry(1.2, 2, 32);
            const ringMaterial = new THREE.MeshBasicMaterial({ 
                color: 0xf0e68c, 
                side: THREE.DoubleSide,
                transparent: true,
                opacity: 0.8
            });
            const rings = new THREE.Mesh(ringGeometry, ringMaterial);
            rings.rotation.x = Math.PI / 2;
            saturn.add(rings);
            
            // Остальные планеты
            createPlanet('uranus', 5, 0.8, 34, 0x87ceeb);
            createPlanet('neptune', 5, 0.8, 40, 0x0000cd);
            createPlanet('pluto', 2, 0.3, 45, 0xbc8f8f);
        }
        
        // Функция для создания планет
        function createPlanet(name, orbitRadius, size, orbitSpeed, color, parent = scene) {
            // Группа для планеты и её орбиты
            const group = new THREE.Group();
            parent.add(group);
            
            // Орбита
            const orbit = new THREE.RingGeometry(orbitRadius, orbitRadius + 0.01, 64);
            const orbitMaterial = new THREE.MeshBasicMaterial({ 
                color: 0x444444, 
                side: THREE.DoubleSide,
                transparent: true,
                opacity: 0.3
            });
            const orbitMesh = new THREE.Mesh(orbit, orbitMaterial);
            orbitMesh.rotation.x = Math.PI / 2;
            group.add(orbitMesh);
            
            // Планета
            const geometry = new THREE.SphereGeometry(size, detailLevel, detailLevel);
            const material = new THREE.MeshStandardMaterial({ color: color });
            const planet = new THREE.Mesh(geometry, material);
            
            // Позиционируем планету на орбите
            planet.position.set(orbitRadius, 0, 0);
            group.add(planet);
            
            // Сохраняем параметры для анимации
            planet.userData = {
                orbitRadius: orbitRadius,
                orbitSpeed: planetOrbitSpeeds[name] || 0.005,
                rotationSpeed: 0.005,
                angle: Math.random() * Math.PI * 2
            };
            
            // Добавляем в коллекцию планет
            planets[name] = planet;
            planetGroups[name] = group;
            
            return planet;
        }
        
        // Анимация
        function animate() {
            requestAnimationFrame(animate);
            
            // Анимация планет
            for (const name in planets) {
                const planet = planets[name];
                if (planet.userData) {
                    // Движение по орбите
                    planet.userData.angle += planet.userData.orbitSpeed * rotationSpeedMultiplier;
                    planet.position.x = Math.cos(planet.userData.angle) * planet.userData.orbitRadius;
                    planet.position.z = Math.sin(planet.userData.angle) * planet.userData.orbitRadius;
                    
                    // Вращение вокруг своей оси
                    planet.rotation.y += planet.userData.rotationSpeed * rotationSpeedMultiplier;
                }
            }
            
            controls.update();
            renderer.render(scene, camera);
        }
        
        // Обработка событий
        function setupEventListeners() {
            // Кнопки выбора планет
            const planetButtons = document.querySelectorAll('.planet-btn');
            planetButtons.forEach(button => {
                button.addEventListener('click', () => {
                    const planetId = button.id;
                    focusOnPlanet(planetId);
                    showPlanetInfo(planetId);
                });
            });
            
            // Кнопка закрытия информации
            document.getElementById('close-info').addEventListener('click', () => {
                document.getElementById('info-panel').classList.remove('visible');
            });
            
            // Кнопка настроек
            document.getElementById('settings-btn').addEventListener('click', () => {
                document.getElementById('settings-panel').classList.toggle('visible');
            });
            
            // Настройки скорости вращения
            const rotationSlider = document.getElementById('rotation-speed');
            const rotationValue = document.getElementById('rotation-value');
            
            rotationSlider.addEventListener('input', () => {
                rotationSpeedMultiplier = parseFloat(rotationSlider.value);
                rotationValue.textContent = rotationSpeedMultiplier.toFixed(1) + 'x';
            });
            
            // Настройки детализации
            const detailSlider = document.getElementById('detail-level');
            const detailValue = document.getElementById('detail-value');
            
            detailSlider.addEventListener('input', () => {
                detailLevel = parseInt(detailSlider.value);
                detailValue.textContent = detailLevel;
                
                // Пересоздаем солнечную систему с новым уровнем детализации
                resetSolarSystem();
            });
            
            // Выбор центральной планеты
            document.getElementById('center-planet').addEventListener('change', (e) => {
                focusOnPlanet(e.target.value);
            });
        }
        
        // Пересоздание солнечной системы
        function resetSolarSystem() {
            // Удаляем все планеты
            for (const name in planets) {
                if (name !== 'sun') {
                    scene.remove(planetGroups[name]);
                }
            }
            
            // Очищаем коллекции
            planets = { sun: planets.sun };
            planetGroups = {};
            
            // Создаем планеты заново
            createSolarSystem();
        }
        
        // Фокусировка на планете
        function focusOnPlanet(planetId) {
            const planet = planets[planetId];
            if (planet) {
                // Определяем позицию для камеры
                let targetPosition = planet.position.clone();
                
                // Для внешних планет учитываем смещение группы
                if (planet.parent !== scene && planet.parent.parent === scene) {
                    targetPosition.add(planet.parent.position);
                }
                
                // Плавное перемещение камеры
                const focusDistance = planetId === 'sun' ? 10 : 5;
                const direction = new THREE.Vector3().subVectors(camera.position, targetPosition).normalize();
                const newPosition = targetPosition.clone().add(direction.multiplyScalar(focusDistance));
                
                // Анимация перемещения камеры
                animateCamera(camera.position.clone(), newPosition, targetPosition);
            }
        }
        
        // Анимация камеры
        function animateCamera(from, to, target) {
            const start = { x: from.x, y: from.y, z: from.z };
            const end = { x: to.x, y: to.y, z: to.z };
            const startTime = Date.now();
            const duration = 1000;
            
            function update() {
                const elapsed = Date.now() - startTime;
                const progress = Math.min(elapsed / duration, 1);
                
                // Используем easeOutCubic для плавности
                const ease = 1 - Math.pow(1 - progress, 3);
                
                camera.position.x = start.x + (end.x - start.x) * ease;
                camera.position.y = start.y + (end.y - start.y) * ease;
                camera.position.z = start.z + (end.z - start.z) * ease;
                
                controls.target.x = target.x;
                controls.target.y = target.y;
                controls.target.z = target.z;
                
                if (progress < 1) {
                    requestAnimationFrame(update);
                }
            }
            
            update();
        }
        
        // Показ информации о планете
        function showPlanetInfo(planetId) {
            const infoPanel = document.getElementById('info-panel');
            const planetName = document.getElementById('planet-name');
            const planetDescription = document.getElementById('planet-description');
            
            const planetInfo = {
                sun: {
                    name: "Солнце",
                    description: "Звезда в центре Солнечной системы. Состоит в основном из водорода и гелия. Диаметр: 1 391 000 км. Температура ядра: около 15 миллионов °C. Солнце содержит 99,86% массы всей Солнечной системы. Является источником энергии для всех процессов на Земле и других планетах."
                },
                mercury: {
                    name: "Меркурий",
                    description: "Ближайшая к Солнцу планета. Не имеет естественных спутников. Температура поверхности от -180°C до +430°C. Год длится всего 88 земных дней. Меркурий имеет очень разреженную атмосферу, которая состоит из атомов, выбитых с его поверхности солнечным ветром."
                },
                venus: {
                    name: "Венера",
                    description: "Вторая планета от Солнца. Имеет плотную атмосферу из углекислого газа. Температура поверхности около 460°C. Вращается в обратном направлении compared to other planets. Атмосферное давление на поверхности Венеры в 92 раза больше, чем на Земле. Облака Венеры состоят из серной кислоты."
                },
                earth: {
                    name: "Земля",
                    description: "Третья планета от Солнца. Единственное известное тело во Вселенной, населенное живыми организмами. Имеет один естественный спутник — Луну. 71% поверхности покрыт водой. Земля имеет мощное магнитное поле, защищающее ее от солнечного ветра. Период обращения вокруг Солнца: 365.25 дней."
                },
                mars: {
                    name: "Марс",
                    description: "Четвертая планета от Солнца. Имеет два естественных спутника — Фобос и Деймос. Известен как 'Красная планета' из-за оксида железа на поверхности. На Марсе находится самая высокая гора в Солнечной системе — Olympus Mons (21 км). Температура на поверхности варьируется от -153°C до +35°C."
                },
                jupiter: {
                    name: "Юпитер",
                    description: "Пятая и крупнейшая планета Солнечной системы. Газовый гигант. Имеет 79 известных спутников. Знаменит своим Большим Красным Пятном - гигантским штормом, который длится более 350 лет. Масса Юпитера в 2.5 раза превышает массу всех остальных планет вместе взятых. Имееет слабую систему колец."
                },
                saturn: {
                    name: "Сатурн",
                    description: "Шестая планета от Солнца. Известна своими кольцами, состоящими из частичек льда и камня. Имеет 82 подтвержденных спутника. Самая низкая плотность среди планет Солнечной системы. Скорость ветра в атмосфере Сатурна может достигать 1800 км/ч. Кольца Сатурна простираются на расстояние до 282 000 км от планеты."
                },
                uranus: {
                    name: "Уран",
                    description: "Седьмая планета от Солнца. Первая планета, обнаруженная с помощью телескопа. Имеет 27 известных спутников. Ось вращения наклонена на 98 градусов, поэтому он вращается 'лежа на боку'. Атмосфера Урана состоит в основном из водорода, гелия и метана, который придает планете сине-зеленый цвет."
                },
                neptune: {
                    name: "Нептун",
                    description: "Восьмая и самая дальняя от Солнца планета. Открыта путем математических расчетов. Имеет 14 известных спутников. Самые сильные ветры в Солнечной системе - до 2100 км/ч. Нептун имеет активную атмосферу с большими штормами, самым известным из которых является Большое Темное Пятно."
                },
                pluto: {
                    name: "Плутон",
                    description: "Карликовая планета в поясе Койпера. Ранее считался девятой планетой. Имеет 5 известных спутников, крупнейший — Харон. Состоит в основном из льда и rock. Плутон меньше Луны примерно в 6 раз. Температура на поверхности составляет около -229°C. Орбита Плутона сильно вытянута и наклонена относительно плоскости эклиптики."
                }
            };
            
            if (planetInfo[planetId]) {
                planetName.textContent = planetInfo[planetId].name;
                planetDescription.textContent = planetInfo[planetId].description;
                infoPanel.classList.add('visible');
            }
        }
        
        // Обработка изменения размера окна
        function onWindowResize() {
            camera.aspect = window.innerWidth / window.innerHeight;
            camera.updateProjectionMatrix();
            renderer.setSize(window.innerWidth, window.innerHeight);
        }
        
        // Запускаем инициализацию после загрузки страницы
        window.addEventListener('load', init);
    </script>
</body>
</html>
