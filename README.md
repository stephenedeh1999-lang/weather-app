<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Weather App</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        
        body {
            font-family: Arial, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
        }
        
        .container {
            background: white;
            padding: 40px;
            border-radius: 20px;
            box-shadow: 0 20px 40px rgba(0,0,0,0.2);
            width: 90%;
            max-width: 400px;
            text-align: center;
        }
        
        h1 {
            color: #333;
            margin-bottom: 20px;
            font-size: 28px;
        }
        
        .search-box {
            display: flex;
            gap: 10px;
            margin-bottom: 20px;
        }
        
        input {
            flex: 1;
            padding: 12px;
            border: 2px solid #ddd;
            border-radius: 10px;
            font-size: 16px;
            outline: none;
        }
        
        input:focus {
            border-color: #667eea;
        }
        
        button {
            padding: 12px 24px;
            background: #667eea;
            color: white;
            border: none;
            border-radius: 10px;
            cursor: pointer;
            font-size: 16px;
            transition: background 0.3s;
        }
        
        button:hover {
            background: #764ba2;
        }
        
        .weather-info {
            display: none;
            margin-top: 20px;
            padding: 20px;
            background: #f8f9fa;
            border-radius: 15px;
        }
        
        .weather-info.show {
            display: block;
        }
        
        .city-name {
            font-size: 24px;
            font-weight: bold;
            color: #333;
        }
        
        .temperature {
            font-size: 52px;
            font-weight: bold;
            color: #667eea;
            margin: 10px 0;
        }
        
        .description {
            font-size: 18px;
            color: #666;
            text-transform: capitalize;
        }
        
        .weather-icon {
            font-size: 64px;
            margin: 10px 0;
        }
        
        .error {
            color: #dc3545;
            margin-top: 10px;
            font-weight: bold;
        }
        
        .loading {
            display: none;
            margin-top: 10px;
            color: #667eea;
        }
        
        .loading.show {
            display: block;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>🌤️ Weather App</h1>
        
        <div class="search-box">
            <input type="text" id="cityInput" placeholder="Enter city name..." />
            <button onclick="getWeather()">Search</button>
        </div>
        
        <div class="loading" id="loading">Loading...</div>
        
        <div id="weatherInfo" class="weather-info">
            <div class="weather-icon" id="weatherIcon">☀️</div>
            <div class="city-name" id="cityName"></div>
            <div class="temperature" id="temperature"></div>
            <div class="description" id="description"></div>
        </div>
        
        <div id="errorMessage" class="error"></div>
    </div>

    <script>
        // 🔑 REPLACE WITH YOUR NEW API KEY
        const API_KEY = "478f991a7843ff7045079737b2c268f0";
        
        async function getWeather() {
            const city = document.getElementById('cityInput').value;
            const weatherInfo = document.getElementById('weatherInfo');
            const errorMessage = document.getElementById('errorMessage');
            const loading = document.getElementById('loading');
            
            weatherInfo.classList.remove('show');
            errorMessage.textContent = '';
            loading.classList.remove('show');
            
            if (!city) {
                errorMessage.textContent = '⚠️ Please enter a city name';
                return;
            }
            
            loading.classList.add('show');
            
            try {
                const url = `https://api.openweathermap.org/data/2.5/weather?q=${city}&appid=${API_KEY}&units=metric`;
                const response = await fetch(url);
                const data = await response.json();
                
                loading.classList.remove('show');
                
                if (data.cod === 200) {
                    const iconCode = data.weather[0].icon;
                    document.getElementById('cityName').textContent = `${data.name}, ${data.sys.country}`;
                    document.getElementById('temperature').textContent = `${Math.round(data.main.temp)}°C`;
                    document.getElementById('description').textContent = data.weather[0].description;
                    document.getElementById('weatherIcon').innerHTML = `<img src="https://openweathermap.org/img/wn/${iconCode}@2x.png" alt="Weather icon" width="80" height="80" />`;
                    weatherInfo.classList.add('show');
                } else {
                    errorMessage.textContent = '❌ City not found. Please try again.';
                }
            } catch (error) {
                loading.classList.remove('show');
                errorMessage.textContent = '❌ Something went wrong. Please try again.';
            }
        }
        
        document.getElementById('cityInput').addEventListener('keypress', function(e) {
            if (e.key === 'Enter') {
                getWeather();
            }
        });
    </script>
</body>
</html>
