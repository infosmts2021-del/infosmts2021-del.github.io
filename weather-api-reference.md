---
layout: page
title: Weather API Reference
permalink: /weather-api/
---

# Weather API Reference

## Open-Meteo API

Our weather dashboard uses the **Open-Meteo API**, a free and open-source weather API with global coverage.

### API Endpoint

```
https://api.open-meteo.com/v1/forecast
```

### Key Features

- ✅ **Free**: No API key required
- ✅ **Open Source**: Code available on GitHub
- ✅ **Global Coverage**: Weather data for any location worldwide
- ✅ **Privacy Focused**: Respects user privacy
- ✅ **No Rate Limits**: Generous usage limits
- ✅ **Accurate**: Data from official weather services

### Current Weather Parameters

```
current=temperature_2m,relative_humidity_2m,apparent_temperature,weather_code,wind_speed_10m,pressure_msl
```

| Parameter | Description |
|-----------|-------------|
| `temperature_2m` | Temperature at 2 meters above ground (°C) |
| `relative_humidity_2m` | Relative humidity (%) |
| `apparent_temperature` | "Feels like" temperature (°C) |
| `weather_code` | WMO weather code (0-99) |
| `wind_speed_10m` | Wind speed at 10 meters (km/h) |
| `pressure_msl` | Sea level pressure (hPa) |

### Daily Forecast Parameters

```
daily=weather_code,temperature_2m_max,temperature_2m_min
```

| Parameter | Description |
|-----------|-------------|
| `weather_code` | WMO weather code for the day |
| `temperature_2m_max` | Maximum daily temperature (°C) |
| `temperature_2m_min` | Minimum daily temperature (°C) |

### Request Example

```bash
curl "https://api.open-meteo.com/v1/forecast?latitude=37.7749&longitude=-122.4194&current=temperature_2m,weather_code&daily=weather_code,temperature_2m_max"
```

### Response Example

```json
{
  "latitude": 37.7749,
  "longitude": -122.4194,
  "generationtime_ms": 0.123,
  "utc_offset_seconds": -28800,
  "timezone": "America/Los_Angeles",
  "current": {
    "temperature_2m": 18.5,
    "relative_humidity_2m": 65,
    "weather_code": 0,
    "wind_speed_10m": 12.3
  },
  "daily": {
    "time": ["2024-01-20", "2024-01-21"],
    "temperature_2m_max": [20.1, 19.5],
    "temperature_2m_min": [15.2, 14.8]
  }
}
```

## WMO Weather Code Reference

### Clear and Cloudy (0-3)

| Code | Description |
|------|-------------|
| 0 | Clear sky |
| 1 | Mainly clear |
| 2 | Partly cloudy |
| 3 | Overcast |

### Fog (45, 48)

| Code | Description |
|------|-------------|
| 45 | Foggy |
| 48 | Depositing rime fog |

### Drizzle (51-57)

| Code | Description |
|------|-------------|
| 51 | Light drizzle |
| 53 | Moderate drizzle |
| 55 | Dense drizzle |

### Rain (61-67)

| Code | Description |
|------|-------------|
| 61 | Slight rain |
| 63 | Moderate rain |
| 65 | Heavy rain |

### Snow (71-77)

| Code | Description |
|------|-------------|
| 71 | Slight snow |
| 73 | Moderate snow |
| 75 | Heavy snow |
| 77 | Snow grains |

### Rain Showers (80-82)

| Code | Description |
|------|-------------|
| 80 | Slight rain showers |
| 81 | Moderate rain showers |
| 82 | Violent rain showers |

### Thunderstorm (95-99)

| Code | Description |
|------|-------------|
| 95 | Thunderstorm |
| 96 | Thunderstorm with slight hail |
| 99 | Thunderstorm with heavy hail |

## Usage Limits

- **Free Tier**: 10,000 requests/day
- **Paid Tier**: Up to 1,000,000 requests/day
- **Rate Limiting**: Not enforced on free tier

## Documentation

- **Official Docs**: [open-meteo.com/en/docs](https://open-meteo.com/en/docs)
- **GitHub Repo**: [github.com/open-meteo/open-meteo](https://github.com/open-meteo/open-meteo)
- **API Status**: [status.open-meteo.com](https://status.open-meteo.com)

## Temperature Conversion

### Celsius to Fahrenheit
```
°F = (°C × 9/5) + 32
```

### Fahrenheit to Celsius
```
°C = (°F - 32) × 5/9
```

## Best Practices

1. **Cache Results**: Store API responses for 10-15 minutes
2. **Error Handling**: Implement try-catch for network failures
3. **User Location**: Request user permission before using geolocation
4. **Timezone Aware**: Use the timezone field from API response
5. **Accessibility**: Provide text descriptions along with weather icons

## Common Issues and Solutions

### Issue: Getting 400 Bad Request
**Solution**: Verify latitude and longitude are within valid ranges:
- Latitude: -90 to 90
- Longitude: -180 to 180

### Issue: Slow Response Times
**Solution**: Implement caching or use the cache-buster prevention headers

### Issue: Inaccurate Forecasts
**Solution**: Data is updated multiple times daily; older forecasts are more accurate

---

**Happy weather tracking! 🌦️**
