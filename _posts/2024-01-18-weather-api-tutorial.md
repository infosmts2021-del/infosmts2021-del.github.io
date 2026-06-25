---
layout: post
title: "Building a Weather App: A Developer's Guide"
date: 2024-01-18
categories: tutorials development weather
---

# Building a Weather App: A Developer's Guide

Interested in creating your own weather application? This guide will walk you through the basics using the Open-Meteo API.

## Why Open-Meteo?

We chose Open-Meteo for our weather dashboard because:

- **No API Key Required**: Zero friction to get started
- **Free Forever**: Generous free tier
- **Accurate Data**: Backed by official weather services
- **Simple API**: Easy to understand and implement
- **Global Coverage**: Weather for anywhere on Earth

## Getting Started

### Step 1: Basic API Request

```javascript
const latitude = 37.7749;  // San Francisco
const longitude = -122.4194;

fetch(`https://api.open-meteo.com/v1/forecast?latitude=${latitude}&longitude=${longitude}&current=temperature_2m`)
  .then(response => response.json())
  .then(data => console.log(data.current.temperature_2m));
```

### Step 2: Parse Weather Data

```javascript
const current = data.current;
const temperature = current.temperature_2m;
const humidity = current.relative_humidity_2m;
const windSpeed = current.wind_speed_10m;
```

### Step 3: Display Results

```javascript
document.getElementById('temp').textContent = `${Math.round(temperature)}°C`;
document.getElementById('humidity').textContent = `${humidity}%`;
document.getElementById('wind').textContent = `${Math.round(windSpeed)} km/h`;
```

## Understanding Weather Codes

The API returns WMO weather codes. Here's how to interpret them:

```javascript
function getWeatherDescription(code) {
  const descriptions = {
    0: 'Clear Sky',
    1: 'Mainly Clear',
    2: 'Partly Cloudy',
    3: 'Overcast',
    61: 'Rain',
    71: 'Snow',
    95: 'Thunderstorm'
  };
  return descriptions[code] || 'Unknown';
}
```

## Building a 5-Day Forecast

```javascript
const daily = data.daily;

for (let i = 0; i < daily.time.length; i++) {
  const date = daily.time[i];
  const maxTemp = daily.temperature_2m_max[i];
  const minTemp = daily.temperature_2m_min[i];
  const code = daily.weather_code[i];
  
  console.log(`${date}: ${minTemp}°C - ${maxTemp}°C, ${getWeatherDescription(code)}`);
}
```

## Adding Error Handling

```javascript
async function getWeather(lat, lon) {
  try {
    const response = await fetch(
      `https://api.open-meteo.com/v1/forecast?latitude=${lat}&longitude=${lon}&current=temperature_2m`
    );
    
    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }
    
    const data = await response.json();
    return data.current;
    
  } catch (error) {
    console.error('Failed to fetch weather:', error);
    return null;
  }
}
```

## Performance Tips

### 1. Implement Caching

```javascript
const cache = new Map();

function getCachedWeather(lat, lon) {
  const key = `${lat},${lon}`;
  const cached = cache.get(key);
  
  if (cached && Date.now() - cached.timestamp < 900000) {
    return cached.data;
  }
  
  return null;
}
```

### 2. Debounce Requests

```javascript
function debounce(func, delay) {
  let timeout;
  return function(...args) {
    clearTimeout(timeout);
    timeout = setTimeout(() => func(...args), delay);
  };
}

const debouncedSearch = debounce(searchLocation, 500);
```

## Next Steps

1. **Add Geolocation**: Use browser's Geolocation API
2. **Location Search**: Implement city search functionality
3. **Styling**: Create beautiful weather visualizations
4. **Alerts**: Notify users of severe weather
5. **History**: Store and display historical weather data

## Resources

- [Open-Meteo Documentation](https://open-meteo.com/en/docs)
- [WMO Weather Codes](https://www.wmo.int/pages/prog/wcp/WDM/Guidance/International_Cloud_atlases.php)
- [JavaScript Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)

---

**Start building your weather app today! 🌤️**
