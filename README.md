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
        
        #loader {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: #000;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            z-index: 1000;
        }
        
        .loader-planet {
            width: 60px;
            height: 60px;
            border-radius: 50%;
            background: conic-gradient(#4b86b4, #63ace5, #4b86b4);
            margin-bottom: 20px;
            animation: rotate 2s linear infinite;
        }
        
        @keyframes rotate {
            from { transform: rotate(0deg); }
            to { transform: rotate(360deg); }
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
        }
        
        .planet-btn:hover {
            transform: scale(1.1);
            border-color: white;
        }
        
        #sun { background-image: url('https://solar-textures.s3.us-east-2.amazonaws.com/sun.jpg'); }
        #mercury { background-image: url('https://solar-textures.s3.us-east-2.amazonaws.com/mercury.jpg'); }
        #venus { background-image: url('https://solar-textures.s3.us-east-2.amazonaws.com/venus.jpg'); }
        #earth { background-image: url('https://solar-textures.s3.us-east-2.amazonaws.com/earth.jpg'); }
        #mars { background-image: url('https://solar-textures.s3.us-east-2.amazonaws.com/mars.jpg'); }
        #jupiter { background-image: url('https://solar-textures.s3.us-east-2.amazonaws.com/jupiter.jpg'); }
        #saturn { background-image: url('https://solar-textures.s3.us-east-2.amazonaws.com/saturn.jpg'); }
        #uranus { background-image: url('https://solar-textures.s3.us-east-2.amazonaws.com/uranus.jpg'); }
        #neptune { background-image: url('https://solar-textures.s3.us-east-2.amazonaws.com/neptune.jpg'); }
        #pluto { background-image: url('https://solar-textures.s3.us-east-2.amazonaws.com/pluto.jpg'); }
        
        #info-panel {
            position: absolute;
            top: 20px;
            right: 20px;
            background: rgba(0, 30, 60, 0.7);
            padding: 15px;
            border-radius: 10px;
            max-width: 300px;
            backdrop-filter: blur(10px);
            transform: translateX(calc(100% + 30px));
            transition: transform 0.5s ease;
            max-height: 80vh;
            overflow-y: auto;
        }
        
        #info-panel.visible {
            transform: translateX(0);
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
        
        @media (max-width: 768px) {
            .planet-btn {
                width: 35px;
                height: 35px;
            }
            
            #info-panel {
                top: 10px;
                right: 10px;
                max-width: 80%;
                font-size: 14px;
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
    <div id="loader">
        <div class="loader-planet"></div>
        <p>Загрузка солнечной системы...</p>
    </div>
    
    <div id="canvas-container">
        <!-- 3D сцена будет отрисована здесь -->
    </div>
    
    <div id="ui-container">
        <div class="planet-selector">
            <div class="planet-btn" id="sun" title="Солнце"></div>
            <div class="planet-btn" id="mercury" title="Меркурий"></div>
            <div class="planet-btn" id="venus" title="Венера"></div>
            <div class="planet-btn" id="earth" title="Земля"></div>
            <div class="planet-btn" id="mars" title="Марс"></div>
            <div class="planet-btn" id="jupiter" title="Юпитер"></div>
            <div class="planet-btn" id="saturn" title="Сатурн"></div>
            <div class="planet-btn" id="uranus" title="Уран"></div>
            <div class="planet-btn" id="neptune" title="Нептун"></div>
            <div class="planet-btn" id="pluto" title="Плутон"></div>
        </div>
    </div>
    
    <div class="controls">
        <p>Используйте колесо мыши для приближения/отдаления | Зажмите левую кнопку мыши для вращения</p>
    </div>
    
    <div id="info-panel">
        <button id="close-info">×</button>
        <h2 id="planet-name"></h2>
        <p id="planet-description"></p>
    </div>

    <!-- Подключаем Three.js -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/three@0.128.0/examples/js/controls/OrbitControls.min.js"></script>
    
    <script>
        // Основные переменные
        let scene, camera, renderer, controls;
        let planets = {};
        let texturesLoaded = 0;
        let totalTextures = 10; // Количество текстур для загрузки
        
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
            renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2)); // Ограничиваем pixel ratio для производительности
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
            // Текстуры для планет (используем надежные источники)
            const textureLoader = new THREE.TextureLoader();
            
            // Создаем Солнце
            textureLoader.load('https://solar-textures.s3.us-east-2.amazonaws.com/sun.jpg', (texture) => {
                const sunGeometry = new THREE.SphereGeometry(5, 32, 32);
                const sunMaterial = new THREE.MeshBasicMaterial({ 
                    map: texture,
                    emissive: 0xffff00,
                    emissiveIntensity: 0.5
                });
                const sun = new THREE.Mesh(sunGeometry, sunMaterial);
                scene.add(sun);
                planets.sun = sun;
                textureLoaded();
            });
            
            // Создаем орбиты и планеты
            createPlanet('mercury', 1, 0.4, 7, 0.01, 'https://solar-textures.s3.us-east-2.amazonaws.com/mercury.jpg');
            createPlanet('venus', 2, 0.6, 10, 0.007, 'https://solar-textures.s3.us-east-2.amazonaws.com/venus.jpg');
            
            // Земля с луной
            const earthGroup = new THREE.Group();
            scene.add(earthGroup);
            
            createPlanet('earth', 3, 0.6, 13, 0.005, 'https://solar-textures.s3.us-east-2.amazonaws.com/earth.jpg', earthGroup, () => {
                // Луна
                textureLoader.load('https://solar-textures.s3.us-east-2.amazonaws.com/moon.jpg', (moonTexture) => {
                    const moonGeometry = new THREE.SphereGeometry(0.2, 32, 32);
                    const moonMaterial = new THREE.MeshStandardMaterial({ map: moonTexture });
                    const moon = new THREE.Mesh(moonGeometry, moonMaterial);
                    moon.position.set(1, 0, 0);
                    planets.earth.add(moon);
                    textureLoaded();
                });
            });
            
            // Другие планеты
            createPlanet('mars', 4, 0.5, 16, 0.004, 'https://solar-textures.s3.us-east-2.amazonaws.com/mars.jpg');
            createPlanet('jupiter', 6, 1.2, 22, 0.003, 'https://solar-textures.s3.us-east-2.amazonaws.com/jupiter.jpg');
            
            // Сатурн с кольцами
            const saturnGroup = new THREE.Group();
            scene.add(saturnGroup);
            
            createPlanet('saturn', 7, 1.0, 28, 0.002, 'https://solar-textures.s3.us-east-2.amazonaws.com/saturn.jpg', saturnGroup, () => {
                // Кольца Сатурна
                textureLoader.load('https://solar-textures.s3.us-east-2.amazonaws.com/saturn_rings.png', (ringTexture) => {
                    const ringGeometry = new THREE.RingGeometry(1.2, 2, 32);
                    const ringMaterial = new THREE.MeshBasicMaterial({ 
                        map: ringTexture, 
                        side: THREE.DoubleSide,
                        transparent: true
                    });
                    const rings = new THREE.Mesh(ringGeometry, ringMaterial);
                    rings.rotation.x = Math.PI / 2;
                    planets.saturn.add(rings);
                    textureLoaded();
                });
            });
            
            // Остальные планеты
            createPlanet('uranus', 5, 0.8, 34, 0.0015, 'https://solar-textures.s3.us-east-2.amazonaws.com/uranus.jpg');
            createPlanet('neptune', 5, 0.8, 40, 0.001, 'https://solar-textures.s3.us-east-2.amazonaws.com/neptune.jpg');
            createPlanet('pluto', 2, 0.3, 45, 0.0007, 'https://solar-textures.s3.us-east-2.amazonaws.com/pluto.jpg');
        }
        
        // Функция для создания планет
        function createPlanet(name, orbitRadius, size, orbitSpeed, rotationSpeed, textureUrl, parent = scene, onLoadCallback = null) {
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
            
            // Загрузка текстуры и создание планеты
            const textureLoader = new THREE.TextureLoader();
            textureLoader.load(textureUrl, (texture) => {
                const geometry = new THREE.SphereGeometry(size, 32, 32);
                const material = new THREE.MeshStandardMaterial({ map: texture });
                const planet = new THREE.Mesh(geometry, material);
                
                // Позиционируем планету на орбите
                planet.position.set(orbitRadius, 0, 0);
                group.add(planet);
                
                // Сохраняем параметры для анимации
                planet.userData = {
                    orbitRadius: orbitRadius,
                    orbitSpeed: orbitSpeed,
                    rotationSpeed: rotationSpeed,
                    angle: Math.random() * Math.PI * 2
                };
                
                // Добавляем в коллекцию планет
                planets[name] = planet;
                
                textureLoaded();
                
                if (onLoadCallback) onLoadCallback();
            });
        }
        
        // Отслеживание загрузки текстур
        function textureLoaded() {
            texturesLoaded++;
            
            // Если все текстуры загружены, скрываем лоадер
            if (texturesLoaded >= totalTextures) {
                setTimeout(() => {
                    document.getElementById('loader').style.opacity = 0;
                    setTimeout(() => {
                        document.getElementById('loader').style.display = 'none';
                    }, 500);
                }, 1000);
            }
        }
        
        // Анимация
        function animate() {
            requestAnimationFrame(animate);
            
            // Анимация планет
            for (const name in planets) {
                const planet = planets[name];
                if (planet.userData) {
                    // Движение по орбите
                    planet.userData.angle += planet.userData.orbitSpeed;
                    planet.position.x = Math.cos(planet.userData.angle) * planet.userData.orbitRadius;
                    planet.position.z = Math.sin(planet.userData.angle) * planet.userData.orbitRadius;
                    
                    // Вращение вокруг своей оси
                    planet.rotation.y += planet.userData.rotationSpeed;
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
            const duration = 1000; // 1 секунда
            
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
                    description: "Звезда в центре Солнечной системы. Состоит в основном из водорода и гелия. Диаметр: 1 391 000 км. Температура ядра: около 15 миллионов °C."
                },
                mercury: {
                    name: "Меркурий",
                    description: "Ближайшая к Солнцу планета. Не имеет естественных спутников. Температура поверхности от -180°C до +430°C. Год длится всего 88 земных дней."
                },
                venus: {
                    name: "Венера",
                    description: "Вторая планета от Солнца. Имеет плотную атмосферу из углекислого газа. Температура поверхности около 460°C. Вращается в обратном направлении compared to other planets."
                },
                earth: {
                    name: "Земля",
                    description: "Третья планета от Солнца. Единственное известное тело во Вселенной, населенное живыми организмами. Имеет один естественный спутник — Луну. 71% поверхности покрыт водой."
                },
                mars: {
                    name: "Марс",
                    description: "Четвертая планета от Солнца. Имеет два естественных спутника — Фобос и Деймос. Известен как 'Красная планета' из-за оксида железа на поверхности."
                },
                jupiter: {
                    name: "Юпитер",
                    description: "Пятая и крупнейшая планета Солнечной системы. Газовый гигант. Имеет 79 известных спутников. Знаменит своим Большим Красным Пятном - гигантским штормом."
                },
                saturn: {
                    name: "Сатурн",
                    description: "Шестая планета от Солнца. Известна своими кольцами, состоящими из частичек льда и камня. Имеет 82 подтвержденных спутника. Самая низкая плотность среди планет Солнечной системы."
                },
                uranus: {
                    name: "Уран",
                    description: "Седьмая планета от Солнца. Первая планета, обнаруженная с помощью телескопа. Имеет 27 известных спутников. Ось вращения наклонена на 98 градусов, поэтому он вращается 'лежа на боку'."
                },
                neptune: {
                    name: "Нептун",
                    description: "Восьмая и самая дальняя от Солнца планета. Открыта путем математических расчетов. Имеет 14 известных спутников. Самые сильные ветры в Солнечной системе - до 2100 км/ч."
                },
                pluto: {
                    name: "Плутон",
                    description: "Карликовая планета в поясе Койпера. Ранее считался девятой планетой. Имеет 5 известных спутников, крупнейший — Харон. Состоит в основном из льда и rock."
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
