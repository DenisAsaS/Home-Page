<!--Широков Денис-->
<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Умный дом</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            text-align: center;
            background: linear-gradient(to right, #141e30, #243b55);
            color: white;
        }
        .tabs {
            display: flex;
            justify-content: center;
            background: rgba(0, 0, 0, 0.8);
            padding: 15px;
            border-bottom: 3px solid #00c6ff;
        }
        .tab {
            padding: 12px 24px;
            color: white;
            cursor: pointer;
            transition: 0.3s;
            font-weight: bold;
            border-radius: 10px;
        }
        .tab:hover {
            color: #00c6ff;
            transform: scale(1.1);
            background: rgba(255, 255, 255, 0.1);
        }
        .content {
            display: none;
            padding: 20px;
            background: rgba(255, 255, 255, 0.1);
            margin: 20px auto;
            border-radius: 15px;
            width: 80%;
            max-width: 600px;
            box-shadow: 0 0 20px rgba(0, 198, 255, 0.3);
            transition: 0.5s;
        }
        .active {
            display: block;
        }
        h2 {
            color: #00c6ff;
            text-shadow: 2px 2px 10px rgba(0, 198, 255, 0.5);
        }
        button {
            background: linear-gradient(45deg, #00c6ff, #0072ff);
            border: none;
            padding: 12px 24px;
            color: white;
            font-size: 18px;
            border-radius: 12px;
            cursor: pointer;
            transition: 0.3s;
            margin: 10px;
            box-shadow: 0 4px 10px rgba(0, 198, 255, 0.3);
        }
        button:hover {
            background: linear-gradient(45deg, #0072ff, #00c6ff);
            transform: scale(1.1);
        }
    </style>
</head>
<body>
    <div class="tabs">
        <div class="tab" onclick="showTab('home')">🏠 Главная</div>
        <div class="tab" onclick="showTab('lighting')">💡 Освещение</div>
        <div class="tab" onclick="showTab('climate')">🌡️ Климат</div>
        <div class="tab" onclick="showTab('security')">🔒 Безопасность</div>
        <div class="tab" onclick="showTab('settings')">⚙️ Настройки</div>
    </div>
    
    <div id="home" class="content active" onload="getWeather('Щучинск')">
            <div class="container">
			<body onload="getWeather('Щучинск')">
			<div id="weather" class="weather-card"></div>
            <input type="text" id="city" placeholder="Введите город" value="Щучинск">
            <button onclick="getWeather()">Изменить город</button>
    </div>
    <div id="lighting" class="content">
        <h2>💡 Освещение</h2>
        <p>Управление светом в доме.</p>
        <button onclick="toggleLight(true)">🔆 Включить свет</button>
        <button onclick="toggleLight(false)">💡 Выключить свет</button>
    </div>
    <div id="climate" class="content">
        <h2>🌡️ Климат</h2>
        <p>Температура: <span id="temperature">Загрузка...</span>°C 🌞</p>
        <p>Влажность: <span id="humidity">Загрузка...</span>% 💧</p>
    </div>
    <div id="security" class="content">
        <h2>🔒 Безопасность</h2>
        <p>Контроль камер, замков.</p>
    </div>
    <div id="settings" class="content">
        <h2>⚙️ Настройки</h2>
        <p>Wi-Fi, параметры системы.</p>
    </div>
    
    <script>
        function showTab(tabId) {
            document.querySelectorAll('.content').forEach(el => el.classList.remove('active'));
            document.getElementById(tabId).classList.add('active');
        }
        async function getWeather(defaultCity) {
            const apiKey = '96dc0cfa62f12fa2d223de62d3cbac83';
            const cityInput = document.getElementById('city');
            const city = defaultCity || cityInput.value;
            const url = `https://api.openweathermap.org/data/2.5/weather?q=${city}&appid=${apiKey}&units=metric&lang=ru`;
            
            try {
                const response = await fetch(url);
                const data = await response.json();
                if (data.cod === 200) {
                    document.getElementById('weather').innerHTML = `
                        <p>В городе <strong>${data.name}, ${data.sys.country}</strong></p>
                        <p>${data.weather[0].description}</p>
                        <p>🌡️ Температура: <span class='seven-segment'>${data.main.temp}°C</span></p>
                        <p>🌡️ Ощущается как: <span class='seven-segment'>${data.main.feels_like}°C</span></p>
                        <p>💧 Влажность: ${data.main.humidity}%</p>
                        <p>🌬️ Ветер: ${data.wind.speed} м/с</p>
                    `;
                } else {
                    document.getElementById('weather').innerHTML = `<p>Ошибка: ${data.message}</p>`;
                }
            } catch (error) {
                document.getElementById('weather').innerHTML = '<p>Не удалось получить данные.</p>';
            }
        }
    </script>
</body>
</html>
