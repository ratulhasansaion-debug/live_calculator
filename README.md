# Rathul Weather Advisor

A Spring Boot web app that turns today's weather into practical daily advice.
Enter the temperature, humidity and weather condition, and it tells you what to wear,
what to eat, how long to sleep, how long to use your phone and how long to study.

## Screenshots

| Home | Result |
|---|---|
| ![Home screen](docs/screenshots/home.png) | ![Advice result](docs/screenshots/result.png) |

## Features

- **Clothing advice** that adapts to temperature and weather (umbrella for rain, sunscreen for sun, layers for cold)
- **Food and water advice**, including local dishes such as khichuri, panta bhat and pitha
- **Recommended sleep, phone and study hours** with a short tip for each
- **"Feels like" temperature** that accounts for humidity, so 32°C at 80% humidity is treated as extreme heat
- **Celsius and Fahrenheit** input
- **Adaptive UI**: the page colour changes with how the weather feels, from icy blue to hot red
- Input validation with clear error messages
- REST API, so the logic can be reused by other apps

## Tech stack

- Java 17+
- Spring Boot 3.3 (Web, Validation)
- Plain HTML, CSS and JavaScript frontend (no build step)
- JUnit 5

## Getting started

### Prerequisites

- JDK 17 or newer
- Maven 3.9+ (or use IntelliJ IDEA, which bundles Maven)

### Run

```bash
git clone <https://github.com/ratulhasansaion-debug/live_calculator/edit/main/README.md>
cd weather-advisor
mvn spring-boot:run
```

Open <http://localhost:8081>.

In IntelliJ IDEA: open the folder that contains `pom.xml`, then run
`WeatherAdvisorApplication`.

### Run the tests

```bash
mvn test
```

## How to use

1. Enter the temperature and choose °C or °F.
2. Set the humidity with the slider.
3. Pick the weather: Sunny, Cloudy, Rainy, Stormy, Foggy or Snowy.
4. Press **Get advice**.

## How the advice is decided

The app first computes a "feels like" temperature (above 20°C, humidity makes it feel hotter),
then places it in one of seven levels:

| Feels like | Level |
|---|---|
| below 5°C | Freezing |
| 5 to 15°C | Cold |
| 16 to 21°C | Cool |
| 22 to 27°C | Mild |
| 28 to 33°C | Warm |
| 34 to 39°C | Hot |
| 40°C and above | Extreme heat |

Each level sets the base advice, and the weather condition (rain, fog, storm and so on) adds extra items.
The advice is general guidance, not medical advice.

## REST API

### `POST /api/advice`

Request:

```json
{
  "temperature": 25,
  "humidity": 50,
  "condition": "SUNNY",
  "unit": "CELSIUS"
}
```

| Field | Type | Notes |
|---|---|---|
| `temperature` | number | Required, between -60 and 60 °C (or the Fahrenheit equivalent) |
| `humidity` | integer | Optional, 0 to 100, defaults to 60 |
| `condition` | string | Required: `SUNNY`, `CLOUDY`, `RAINY`, `STORMY`, `FOGGY`, `SNOWY` |
| `unit` | string | Optional: `CELSIUS` (default) or `FAHRENHEIT` |

Response (shortened):

```json
{
  "temperatureC": 25.0,
  "feelsLikeC": 26.2,
  "level": "MILD",
  "levelLabel": "Mild",
  "summary": "It is 25°C and feels like 26.2°C. Conditions: Mild.",
  "dress": ["Cotton t-shirt or half-sleeve shirt", "..."],
  "food": ["A balanced meal: rice or roti, dal, vegetables, fish or meat", "..."],
  "waterLiters": 2.5,
  "sleepHours": 8.0,
  "sleepNote": "Be in bed by 10-11 pm so waking up in the morning is easy.",
  "phoneHours": 2.0,
  "phoneNote": "...",
  "studyHours": 6.0,
  "studyNote": "..."
}
```

Invalid input returns HTTP 400 with `{ "error": "..." }`.

## Project structure

```
weather-advisor
├── pom.xml
├── docs/screenshots/                 # README images
└── src
    ├── main
    │   ├── java/com/rathul/weather
    │   │   ├── WeatherAdvisorApplication.java
    │   │   ├── controller/AdvisorController.java
    │   │   ├── service/AdvisorService.java    # all advice rules live here
    │   │   ├── dto/                           # request and response records
    │   │   ├── model/                         # Condition, TempUnit, FeelLevel
    │   │   └── exception/
    │   └── resources
    │       ├── application.properties
    │       └── static/index.html              # the web UI
    └── test/java/com/rathul/weather/AdvisorServiceTest.java
```

## Configuration

The port is set in `src/main/resources/application.properties`:

```properties
server.port=8081
```

## Author

Made by Rathul.
