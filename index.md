# Biometric Pulse Sensor
A device whose basic function is to take the BPM of the person who puts their finger on the pulse sensor. The device tells you your BPM via the LCD display, as well as if your BPM is too high (during which a fan will turn on to cool you down) and if your BPM is just right (in which case an LED heart will light up and the LCD will tell you that you're a winner). Additionally, the device can connect to a website that keeps track of your BPM history.

<!--- You should comment out all portions of your portfolio that you have not completed yet, as well as any instructions: -->
```HTML 
<!--- This is an HTML comment in Markdown -->
<!--- Anything between these symbols will not render on the published site -->
```

| **Engineer** | **School** | **Area of Interest** | **Grade** |
|:--:|:--:|:--:|:--:|
| Enakshi D | Amador Valley High School | Mechanical Engineering | Incoming Senior

<!--- **Replace the BlueStamp logo below with an image of yourself and your completed project. Follow the guide [here](https://tomcam.github.io/least-github-pages/adding-images-github-pages-site.html) if you need help.** -->

![Headstone Image](logo.svg)
  
# Final Milestone

**Don't forget to replace the text below with the embedding for your milestone video. Go to Youtube, click Share -> Embed, and copy and paste the code to replace what's below.**

<iframe width="560" height="315" src="https://www.youtube.com/embed/F7M7imOVGug" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

For your final milestone, explain the outcome of your project. Key details to include are:
- What you've accomplished since your previous milestone
- What your biggest challenges and triumphs were at BSE
- A summary of key topics you learned about
- What you hope to learn in the future after everything you've learned at BSE

When I finished my second milestone, I was having trouble trying to debug both my LCD & my fan. Now, I have finished both debugging my 2nd milestone and also establishing my 3rd milestone. My 3rd milestone was initially supposed to be a website that could automatically receive data from my pulse sensor and store it in a table, however due to the fact that I would have to learn Javascript to do that, and I was running out of time, I created a website that could store data that is manually inputted via code. My biggest challenges during this experience were finding a proper threshold value that would allow the pulse sensor to function properly and establishing a proper connection with the ESP8266 Wifi Module. However, once those two things were resolved, I was very relieved. Overall, in this program I learned about 

# Second Milestone

**Don't forget to replace the text below with the embedding for your milestone video. Go to Youtube, click Share -> Embed, and copy and paste the code to replace what's below.**

<iframe width="912" height="514" src="https://www.youtube.com/embed/g_gqeUIlyaw" title="Enakshi D  Milestone 2" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

For my second milestone, I finished my first modification and made significant progress on the second modification. The idea for my first modification was to take advantage of the fact that the pulse sensor was consistently inaccurate and turn it into a game of sorts. If you were able to get the BPM below 100, LEDs on the breadboard (in the shape of a heart!) would light up and the LCD would display a message saying "You're a winner!". There were some issues with the LCD not clearing properly, but I was able to debug that, and the idea worked wonderfully. I then moved on to my second modification, in which I would have a fan run if the BPM was higher than 180. The fan works great, however the LCD has had clearing issues. So before I complete my final milestone, the code needs to be debugged so that the LCD functions properly. Additionally, my final milestone needs to include my 3rd modification, a website connected to my pulse sensor that tracks the history of my BPM.

# First Milestone

<iframe width="560" height="315" src="https://www.youtube.com/embed/MIPMGA8jAr0?si=bzfyHiUqIGdHUFmP" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

I completed my base project for my first milestone. I have an LCD display which shows my BP as well as my pulse sensor. Both of these components are attached to my breadboard through female to male & male to male wires. I coded my pulse sensor first by bringing in its library then producing a code that would allow the sensor to sense the amount of greenlight reflecting off of my blood, which then turns into a reading for my BPM. Then, I translated all of this data from the Serial Monitor to the LCD, utilizing a potentiometer in the process to make sure that my LCD text was actually visible. The major issue that I had during this milestone was trying to get the sensor to read my BPM correctly. It was producing values all the way up to 900 and rarely went under 100, so I messed around with the Threshold value in my code. Now, the values are still slightly wacky, but I am able to get consistent values on occasion. Once that was finished, I began to think of my modifications for the second milestone.

# Schematics 
Here's where you'll put images of your schematics. [Tinkercad](https://www.tinkercad.com/blog/official-guide-to-tinkercad-circuits) and [Fritzing](https://fritzing.org/learning/) are both great resoruces to create professional schematic diagrams, though BSE recommends Tinkercad becuase it can be done easily and for free in the browser. 

# Code
Here's where you'll put your code. The syntax below places it into a block of code. Follow the guide [here]([url](https://www.markdownguide.org/extended-syntax/)) to learn how to customize it to your project needs. 

The code for my Breadboard set-up:

```c++
#include <PulseSensorPlayground.h>
#include <LiquidCrystal.h>

PulseSensorPlayground pulseSensor;
LiquidCrystal lcd(12, 11, 4, 5, 6, 7);

const int PULSE_SENSOR_PIN = 0;
const int THRESHOLD = 510;
const int pinLED = 9;
const int brightness = 300;
int currentBPM = 0;
int motorPin = 3;

void setLEDbrightness(int brightness){
  analogWrite(pinLED, brightness);
}

void setup(){
  lcd.begin(16, 2);
  lcd.clear();
 
  pulseSensor.analogInput(PULSE_SENSOR_PIN);
  pulseSensor.setThreshold(THRESHOLD);

  pinMode(pinLED, OUTPUT);
  pinMode(motorPin, OUTPUT);

  if(pulseSensor.begin()){
    lcd.print("Pulse Sensor ON");
    delay(1000);
    lcd.clear();
    }
}

void loop() {
  if (pulseSensor.sawStartOfBeat()) {
    currentBPM = pulseSensor.getBeatsPerMinute();

    setLEDbrightness(0);
    lcd.setCursor(0, 0);
    lcd.print("BPM = ");
    lcd.print(currentBPM);
    lcd.print("   ");

    lcd.setCursor(0, 1);

    if (currentBPM < 100) {
      digitalWrite(motorPin, LOW);
      digitalWrite(9, HIGH);
      lcd.print("You're a winner! ");
    } else if (currentBPM > 180) {
      analogWrite(motorPin, 100);
      lcd.print("BPM too high!    ");
    } else {
      analogWrite(motorPin, 0);
      lcd.print("Heartbeat > 100  ");
    }
    delay(500);
  }
}
```

The code for my Wifi module:

```c++
#include <ESP8266WiFi.h>
#include <ESP8266WebServer.h>

ESP8266WebServer server(80);

const char *ssid = "ESP8266";
const char  *password = "password";

const char MAIN_page[] PROGMEM = R"=====(
<!doctype html>
<html>
<head>
  <title>Biometric Pulse Sensor History</title>
</head>

<style>
  table, th, td {
    border:3px solid black;
    border-collapse:collapse;
    font-family:verdana;
    table-layout:fixed;
    width:100%;
    text-align:center;
  }
</style>

<body style="background-color:mistyrose;">
  <h1 style="color:crimson; font-family: 'Trebuchet MS', sans-serif; text-align:center; font-size:200%;">Biometric Pulse Sensor History</h1>
  <h3 style="color:black; font-family:verdana; text-align:center; font-size:50%">The Biometric Pulse Sensor reads the BPM of the person who places their finger on the sensor. This table keeps a record of the history of the device's readings.</h3>
  <h3 style="color:black; font-family:verdana; text-align:center; font-size:50%">Leave your finger in the pulse sensor for at least 2 minutes and wait for a stable reading. Then record it in the table with the date & time.</h3>
  <h3 style="color:crimson; font-family:verdana; text-align:center; font-size:50%;">IMPORTANT: If the sensor gives you crazy values like 200+ and you haven't been exercising, try readjusting your finger. DON'T RECORD THAT VALUE.</h3>

  <table id="dataTable;">
    <tr>
      <th>Date</th>
      <th>Time</th>
      <th>BPM</th>
    </tr>
    <tr>
      <td>7/2/2025</td>
      <td>11:50 AM</td>
      <td>83</td>
    </tr>
  </table>

</body>
</html>
)=====";

void handleRoot() {
  server.send(200, "text/html", MAIN_page);
}

void setup() {
  Serial.begin(9600);
  WiFi.softAP(ssid, password);
  Serial.println("Access point started");
  Serial.print("AP IP address: ");
  Serial.println(WiFi.softAPIP());

  server.on("/", handleRoot);
  server.begin();
  Serial.println("http started");
}

void loop() {
  server.handleClient();
}
```

# Bill of Materials
Here's where you'll list the parts in your project. To add more rows, just copy and paste the example rows below.
Don't forget to place the link of where to buy each component inside the quotation marks in the corresponding row after href =. Follow the guide [here]([url](https://www.markdownguide.org/extended-syntax/)) to learn how to customize this to your project needs. 

| **Part** | **Note** | **Price** | **Link** |
|:--:|:--:|:--:|:--:|
| Item Name | What the item is used for | $Price | <a href="https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/"> Link </a> |
| Item Name | What the item is used for | $Price | <a href="https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/"> Link </a> |
| Item Name | What the item is used for | $Price | <a href="https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/"> Link </a> |

# Other Resources/Examples
One of the best parts about Github is that you can view how other people set up their own work. Here are some past BSE portfolios that are awesome examples. You can view how they set up their portfolio, and you can view their index.md files to understand how they implemented different portfolio components.
- [Example 1](https://trashytuber.github.io/YimingJiaBlueStamp/)
- [Example 2](https://sviatil0.github.io/Sviatoslav_BSE/)
- [Example 3](https://arneshkumar.github.io/arneshbluestamp/)

To watch the BSE tutorial on how to create a portfolio, click here.
