# codeflux

#define TRIG_PIN 9
#define ECHO_PIN 10

// Motor driver pins
#define IN1 2
#define IN2 3
#define IN3 4
#define IN4 5

long duration;
int distance;

int getDistance()
{
  digitalWrite(TRIG_PIN, LOW);
  delayMicroseconds(2);

  digitalWrite(TRIG_PIN, HIGH);
  delayMicroseconds(10);
  digitalWrite(TRIG_PIN, LOW);

  duration = pulseIn(ECHO_PIN, HIGH, 30000);

  if (duration == 0)
    return 999;

  distance = duration * 0.034 / 2;
  return distance;
}

void forward()
{
  digitalWrite(IN1, HIGH);
  digitalWrite(IN2, LOW);
  digitalWrite(IN3, HIGH);
  digitalWrite(IN4, LOW);
}

void backward()
{
  digitalWrite(IN1, LOW);
  digitalWrite(IN2, HIGH);
  digitalWrite(IN3, LOW);
  digitalWrite(IN4, HIGH);
}

void stopCar()
{
  digitalWrite(IN1, LOW);
  digitalWrite(IN2, LOW);
  digitalWrite(IN3, LOW);
  digitalWrite(IN4, LOW);
}

void turnRight()
{
  // Left motor forward, right motor backward
  digitalWrite(IN1, HIGH);
  digitalWrite(IN2, LOW);
  digitalWrite(IN3, LOW);
  digitalWrite(IN4, HIGH);
}

void setup()
{
  pinMode(TRIG_PIN, OUTPUT);
  pinMode(ECHO_PIN, INPUT);

  pinMode(IN1, OUTPUT);
  pinMode(IN2, OUTPUT);
  pinMode(IN3, OUTPUT);
  pinMode(IN4, OUTPUT);

  Serial.begin(9600);
  stopCar();
}

void loop()
{
  int d = getDistance();

  Serial.print("Distance: ");
  Serial.print(d);
  Serial.println(" cm");

  if (d > 20)
  {
    forward();
  }
  else
  {
    stopCar();
    delay(300);

    backward();
    delay(400);

    stopCar();
    delay(200);

    turnRight();
    delay(500);

    stopCar();
    delay(200);
  }

  delay(50);
}

