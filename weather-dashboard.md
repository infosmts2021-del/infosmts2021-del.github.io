---
layout: page
title: Weather Dashboard
permalink: /weather/
---

# Weather Dashboard

<div id="weather-container" style="padding: 20px;">
  <div id="loading" style="text-align: center; display: none;">
    <p>Loading weather data...</p>
  </div>
  
  <div id="error" style="display: none; color: red; padding: 10px; background-color: #ffe6e6; border-radius: 5px; margin-bottom: 20px;">
  </div>
  
  <div id="weather-content" style="display: none;">
    <div style="display: flex; gap: 20px; flex-wrap: wrap;">
      <div id="current-weather" style="flex: 1; min-width: 300px; background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); color: white; padding: 20px; border-radius: 10px; box-shadow: 0 4px 6px rgba(0,0,0,0.1);">
        <h2 id="city-name"></h2>
        <div style="font-size: 48px; margin: 20px 0;" id="temperature"></div>
        <div id="description" style="font-size: 18px; margin-bottom: 20px;"></div>
        <div style="display: grid; grid-template-columns: 1fr 1fr; gap: 15px;">
          <div>
            <p style="margin: 0; opacity: 0.8;">Feels Like</p>
            <p id="feels-like" style="margin: 5px 0; font-size: 20px;"></p>
          </div>
          <div>
            <p style="margin: 0; opacity: 0.8;">Humidity</p>
            <p id="humidity" style="margin: 5px 0; font-size: 20px;"></p>
          </div>
          <div>
            <p style="margin: 0; opacity: 0.8;">Wind Speed</p>
            <p id="wind-speed" style="margin: 5px 0; font-size: 20px;"></p>
          </div>
          <div>
            <p style="margin: 0; opacity: 0.8;">Pressure</p>
            <p id="pressure" style="margin: 5px 0; font-size: 20px;"></p>
          </div>
        </div>
      </div>
      
      <div id="forecast-container" style="flex: 1; min-width: 300px;">
        <h3>5-Day Forecast</h3>
        <div id="forecast" style="display: grid; grid-template-columns: repeat(auto-fit, minmax(120px, 1fr)); gap: 10px;"></div>
      </div>
    </div>
  </div>
</div>

<style>
  .forecast-card {
    background: white;
    border: 1px solid #ddd;
    border-radius: 8px;
    padding: 15px;
    text-align: center;
    box-shadow: 0 2px 4px rgba(0,0,0,0.1);
    transition: transform 0.2s;
  }
  
  .forecast-card:hover {
    transform: translateY(-5px);
    box-shadow: 0 4px 8px rgba(0,0,0,0.15);
  }
  
  .forecast-card .date {
    font-weight: bold;
    color: #667eea;
    margin-bottom: 10px;
  }
  
  .forecast-card .temp {
    font-size: 18px;
    margin: 10px 0;
  }
  
  .forecast-card .high {
    color: #ff6b6b;
  }
  
  .forecast-card .low {
    color: #4ecdc4;
  }
  
  .forecast-card .condition {
    font-size: 12px;
    color: #666;
    margin-top: 10px;
  }
</style>

<script>
  // Using Open-Meteo API (free, no API key needed)
  const WEATHER_API = 'https://api.open-meteo.com/v1/forecast';
  
  // Default location: San Francisco
  const DEFAULT_LAT = 37.7749;
  const DEFAULT_LON = -122.4194;
  const DEFAULT_CITY = 'San Francisco';
  
  async function fetchWeather(lat = DEFAULT_LAT, lon = DEFAULT_LON, cityName = DEFAULT_CITY) {
    const loadingEl = document.getElementById('loading');
    const errorEl = document.getElementById('error');
    const contentEl = document.getElementById('weather-content');
    
    try {
      loadingEl.style.display = 'block';
      errorEl.style.display = 'none';
      contentEl.style.display = 'none';
      
      // Fetch current and forecast weather
      const response = await fetch(
        `${WEATHER_API}?latitude=${lat}&longitude=${lon}&current=temperature_2m,relative_humidity_2m,apparent_temperature,weather_code,wind_speed_10m,pressure_msl&daily=weather_code,temperature_2m_max,temperature_2m_min&timezone=auto`
      );
      
      if (!response.ok) {
        throw new Error('Failed to fetch weather data');
      }
      
      const data = await response.json();
      
      // Display current weather
      displayCurrentWeather(data, cityName);
      
      // Display forecast
      displayForecast(data);
      
      loadingEl.style.display = 'none';
      contentEl.style.display = 'block';
      
    } catch (error) {
      console.error('Weather API Error:', error);
      errorEl.textContent = '⚠️ Error loading weather data. Please try again later.';
      errorEl.style.display = 'block';
      loadingEl.style.display = 'none';
    }
  }
  
  function getWeatherDescription(code) {
    const weatherCodes = {
      0: 'Clear Sky',
      1: 'Mainly Clear',
      2: 'Partly Cloudy',
      3: 'Overcast',
      45: 'Foggy',
      48: 'Foggy with Rime',
      51: 'Light Drizzle',
      53: 'Moderate Drizzle',
      55: 'Heavy Drizzle',
      61: 'Slight Rain',
      63: 'Moderate Rain',
      65: 'Heavy Rain',
      71: 'Slight Snow',
      73: 'Moderate Snow',
      75: 'Heavy Snow',
      77: 'Snow Grains',
      80: 'Slight Rain Showers',
      81: 'Moderate Rain Showers',
      82: 'Violent Rain Showers',
      85: 'Slight Snow Showers',
      86: 'Heavy Snow Showers',
      95: 'Thunderstorm',
      96: 'Thunderstorm with Hail',
      99: 'Thunderstorm with Heavy Hail'
    };
    return weatherCodes[code] || 'Unknown';
  }
  
  function getWeatherEmoji(code) {
    if (code === 0) return '☀️';
    if (code === 1 || code === 2) return '⛅';
    if (code === 3) return '☁️';
    if (code === 45 || code === 48) return '🌫️';
    if (code >= 51 && code <= 55) return '🌧️';
    if (code >= 61 && code <= 65) return '🌧️';
    if (code >= 71 && code <= 77) return '❄️';
    if (code >= 80 && code <= 82) return '🌧️';
    if (code >= 85 && code <= 86) return '🌨️';
    if (code >= 95 && code <= 99) return '⛈️';
    return '🌤️';
  }
  
  function displayCurrentWeather(data, cityName) {
    const current = data.current;
    const timezone = data.timezone;
    
    document.getElementById('city-name').textContent = `${cityName} ${getWeatherEmoji(current.weather_code)}`;
    document.getElementById('temperature').textContent = `${Math.round(current.temperature_2m)}°C`;
    document.getElementById('description').textContent = getWeatherDescription(current.weather_code);
    document.getElementById('feels-like').textContent = `${Math.round(current.apparent_temperature)}°C`;
    document.getElementById('humidity').textContent = `${current.relative_humidity_2m}%`;
    document.getElementById('wind-speed').textContent = `${Math.round(current.wind_speed_10m)} km/h`;
    document.getElementById('pressure').textContent = `${current.pressure_msl} hPa`;
  }
  
  function displayForecast(data) {
    const daily = data.daily;
    const forecastEl = document.getElementById('forecast');
    forecastEl.innerHTML = '';
    
    for (let i = 1; i < 6 && i < daily.time.length; i++) {
      const date = new Date(daily.time[i]);
      const dayName = date.toLocaleDateString('en-US', { weekday: 'short' });
      const code = daily.weather_code[i];
      
      const card = document.createElement('div');
      card.className = 'forecast-card';
      card.innerHTML = `
        <div class="date">${dayName}</div>
        <div>${getWeatherEmoji(code)}</div>
        <div class="temp">
          <span class="high">${Math.round(daily.temperature_2m_max[i])}°</span> / 
          <span class="low">${Math.round(daily.temperature_2m_min[i])}°</span>
        </div>
        <div class="condition">${getWeatherDescription(code)}</div>
      `;
      
      forecastEl.appendChild(card);
    }
  }
  
  // Load weather on page load
  window.addEventListener('load', () => {
    fetchWeather();
  });
</script>
