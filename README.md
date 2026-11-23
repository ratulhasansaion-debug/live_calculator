# Rathul Weather Advisor (Spring Boot)

Enter the temperature, humidity and weather, and get advice on what to wear, what to eat,
how long to sleep, how long to use your phone and how long to study.

## How to run
Requires Java 17+ and Maven.

    mvn spring-boot:run

Open http://localhost:8081 in your browser (the calculator uses 8080, so both can run together).

Run the tests:

    mvn test

## REST API
POST /api/advice

    { "temperature": 32, "humidity": 80, "condition": "RAINY", "unit": "CELSIUS" }

- condition: SUNNY, CLOUDY, RAINY, STORMY, FOGGY, SNOWY
- unit: CELSIUS (default) or FAHRENHEIT
- humidity: 0-100, defaults to 60
