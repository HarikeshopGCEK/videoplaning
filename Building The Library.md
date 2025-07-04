# Splitting the Work & Work flow

***Building an Arduino library for school students which controls a obstacle avoidance bot with two Ultra Sonic Sensors at Left and Right. The project is a simple one and will improve the knowledge about OOP programming in C++ language. The project was in my (Harikesh) mind for a long time and figured it will be good my fellow team mates to know those stuff too. So I gave each task taking their knowledge in the field as criteria ***

| Name           | Task                                                                        |
| -------------- | --------------------------------------------------------------------------- |
| Harikesh       | Gave everyone the initial idea and initialize the Project and help everyone |
| Anamika PP     | Create the function defenitions                                             |
| Adithya Raj KK | Documentation and README.md file                                            |
# About the project
*The file system will look like something below*

```plaintext
📁 obstacle_avoider_library
 ┣ 📁 examples
	┗ 📄 example.cpp
 ┣ 📁 test
 ┣ 📁 src
	┣ 📄 obstacle.cpp
	┗ 📄 obstalce.h
 ┣ 📄 library.json
 ┣ 📄 LICENSE
 ┗ 📄 README.md
```

1. The example folder contains the example code which utilizes the library
2. `obstacle.h` file contain the header file for the library
3. And the `obstacle.cpp` contain the function definitions
4. The created library will be published to platformio registry 
## Code files:
### `obstacle.cpp`

```cpp
#include <Arduino.h>
#include "obstacle.h"

// Constructor
ObstacleAvoider::ObstacleAvoider(int trigPin, int echoPin, float maxDistance)
    : _trigPin(trigPin), _echoPin(echoPin), _maxDistance(maxDistance)
{
}

// Initialize sensor pins
void ObstacleAvoider::begin()
{
    pinMode(_trigPin, OUTPUT);
    pinMode(_echoPin, INPUT);
    digitalWrite(_trigPin, LOW);
    delayMicroseconds(30);
}

// Measure distance
float ObstacleAvoider::getDistance()
{
    digitalWrite(_trigPin, LOW);
    delayMicroseconds(2);
    digitalWrite(_trigPin, HIGH);
    delayMicroseconds(10);
    digitalWrite(_trigPin, LOW);

    _duration = pulseIn(_echoPin, HIGH, 30000);  // Timeout 30ms

    if (_duration == 0) {
        return _maxDistance + 1;
    }

    _distance = (_duration * 0.0343) / 2.0;
    return _distance;
}

// Detect obstacle
bool ObstacleAvoider::isObstacleDetected()
{
    return (getDistance() <= _maxDistance);
}

// Run logic with Serial output instead of driver control
void ObstacleAvoider::run()
{
    if (isObstacleDetected()) {
        Serial.println("Obstacle Detected!");
        Serial.println("Action: STOP");
        delay(200);
        Serial.println("Action: TURN LEFT");
        delay(400);
    } else {
        Serial.println("Path Clear");
        Serial.println("Action: MOVE FORWARD");
    }
}
```

## `obstacle.h`

```c
#ifndef _OBSTACLE_H_
#define _OBSTACLE_H_

#include <Arduino.h>
#include <driver.h>
class ObstacleAvoider
{
public:
    ObstacleAvoider(int trigPin, int echoPin, float maxDistance = 20.0);
    void begin();
    void run();
    bool isObstacleDetected();
    float getDistance();

private:
    int _trigPin;
    int _echoPin;
    long _duration;
    float _distance;
    float _maxDistance;
};

#endif
```

# Contributions of Members :

