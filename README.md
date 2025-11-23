<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Weather App</title>

    <style>
        body {
            margin: 0;
            height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            background: linear-gradient(to bottom, #80b7ff, #c9e5ff);
            font-family: Arial, sans-serif;
        }

        .card {
            width: 420px;
            padding: 40px;
            text-align: center;
            background: rgba(255, 255, 255, 0.40);
            backdrop-filter: blur(20px);
            border-radius: 20px;
            box-shadow: 0 20px 40px rgba(0,0,0,0.15);
        }

        h2 {
            margin-top: 0;
            font-size: 26px;
            font-weight: 700;
        }

        .search-box {
            display: flex;
            gap: 10px;
            margin-top: 20px;
            margin-bottom: 30px;
        }

        .search-box input {
            flex: 1;
            padding: 12px;
            border-radius: 8px;
            border: none;
            outline: none;
            font-size: 15px;
        }

        .search-box button {
            padding: 12px 20px;
            border: none;
            border-radius: 8px;
            background: #1d74ff;
            color: white;
            cursor: pointer;
            font-size: 15px;
            font-weight: bold;
        }

        .search-box button:hover {
            background: #0053c7;
        }

        .icon {
            width: 110px;
            margin: 10px auto;
        }

        .temp {
            font-size: 48px;
            font-weight: bold;
        }

        .condition {
            font-size: 20px;
            margin-bottom: 20px;
        }

        .details {
            font-size: 16px;
            color: #333;
            line-height: 1.6;
        }

        .error {
            display: none;
            padding: 10px;
            margin-bottom: 10px;
            background: #ffb5b5;
            color: #b10000;
            border-radius: 8px;
        }
    </style>
</head>
<body>

<div class="card">
    <h2>Weather App</h2>

    <div id="error" class="error">City not found</div>

    <div class="search-box">
        <input type="text" id="city" placeholder="Enter city name">
        <button onclick="getWeather()">Search</button>
    </div>

    <img id="icon" class="icon" src="https://cdn.weatherapi.com/weather/64x64/day/113.png">
    
    <div class="temp" id="temp">--°C</div>
    <div class="condition" id="condition">---</div>

    <div class="details">
        <div id="humidity">Humidity: --%</div>
        <div id="wind">Wind: -- km/h</div>
    </div>
</div>

<script>
    const apiKey = "39484e2f3e894821821160606252311"; // Your API key

    async function getWeather() {
        const city = document.getElementById("city").value;
        const errorBox = document.getElementById("error");

        if (city === "") return;

        try {
            const url = `https://api.weatherapi.com/v1/current.json?key=${apiKey}&q=${city}&aqi=no`;
            const response = await fetch(url);

            if (!response.ok) {
                throw new Error();
            }

            const data = await response.json();
            errorBox.style.display = "none";

            document.getElementById("temp").innerText = data.current.temp_c + "°C";
            document.getElementById("condition").innerText = data.current.condition.text;
            document.getElementById("humidity").innerText = "Humidity: " + data.current.humidity + "%";
            document.getElementById("wind").innerText = "Wind: " + data.current.wind_kph + " km/h";
            document.getElementById("icon").src = "https:" + data.current.condition.icon;

        } catch {
            errorBox.style.display = "block";
            document.getElementById("temp").innerText = "--°C";
            document.getElementById("condition").innerText = "---";
        }
    }
</script>

</body>
</html>
