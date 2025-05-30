# MorseCode Transmitter – Soluție discretă în situații de criză

Dispozitiv inovator bazat pe Arduino care permite transmiterea discretă a unui mesaj în cod Morse folosind o tastatură 4x4, un LED și un buzzer. Ideal pentru persoane aflate în pericol sau în imposibilitatea de a comunica verbal.

## 📌 Despre proiect

Ideea acestui proiect a pornit dintr-o nevoie reală: aceea de a semnaliza o situație de urgență fără a atrage atenția. Inspirația a venit după vizionarea unui caz real în care un adolescent a fost salvat datorită codului Morse. Proiectul simulează o „jucărie” antistres pentru a masca intenția reală a dispozitivului, ceea ce îl face ideal pentru persoane cu ADHD, anxietate sau autism.

## 💡 Ce face acest proiect?

- Permite introducerea unui cuvânt printr-o tastatură 4x4 (matricială)
- Arduino citește fiecare caracter și îl convertește în cod Morse
- LED-ul clipește conform codului Morse
- Buzzer-ul emite bipuri corespunzătoare
- Transmite un mesaj fără a atrage atenția nedorită

## 🔧 Componente necesare

- Arduino Uno (sau Nano)
- Tastatură 4x4 matricială
- Buzzer activ
- LED + rezistor de 220 ohmi
- Fire jumper
- Breadboard
- Cablu USB

## 🔌 Schemă de conectare

- **Tastatura 4x4:** 8 pini conectați la pinii 2–9 ai plăcii Arduino
- **LED:**
  - Anod (pata lungă) → Pin 11 prin rezistor de 220 ohmi
  - Catod (pata scurtă) → GND
- **Buzzer activ:**
  - Plus → Pin 10
  - Minus → GND

## 💻 Instalare & Configurare

1. Deschide Arduino IDE
2. Instalează librăria `Keypad`:  
   `Sketch > Include Library > Manage Libraries > Search "Keypad"`
3. Conectează componentele conform schemei
4. Încarcă codul în placa Arduino

## 💻 Cod Arduino

#include <Keypad.h>

// Definirea tastaturii 4x4
const byte ROWS = 4;
const byte COLS = 4;
char keys[ROWS][COLS] = {
  {'1','2','3','A'},
  {'4','5','6','B'},
  {'7','8','9','C'},
  {'*','0','#','D'}
};
byte rowPins[ROWS] = {9, 8, 7, 6};
byte colPins[COLS] = {5, 4, 3, 2};
Keypad keypad = Keypad(makeKeymap(keys), rowPins, colPins, ROWS, COLS);

int ledPin = 11;
int buzzerPin = 10;

// Cod Morse pentru litere și cifre
String morseCode(char c) {
  switch (toupper(c)) {
    case 'A': return ".-";
    case 'B': return "-...";
    case 'C': return "-.-.";
    case 'D': return "-..";
    case 'E': return ".";
    case 'F': return "..-.";
    case 'G': return "--.";
    case 'H': return "....";
    case 'I': return "..";
    case 'J': return ".---";
    case 'K': return "-.-";
    case 'L': return ".-..";
    case 'M': return "--";
    case 'N': return "-.";
    case 'O': return "---";
    case 'P': return ".--.";
    case 'Q': return "--.-";
    case 'R': return ".-.";
    case 'S': return "...";
    case 'T': return "-";
    case 'U': return "..-";
    case 'V': return "...-";
    case 'W': return ".--";
    case 'X': return "-..-";
    case 'Y': return "-.--";
    case 'Z': return "--..";
    case '1': return ".----";
    case '2': return "..---";
    case '3': return "...--";
    case '4': return "....-";
    case '5': return ".....";
    case '6': return "-....";
    case '7': return "--...";
    case '8': return "---..";
    case '9': return "----.";
    case '0': return "-----";
    default: return "";
  }
}

void dot() {
  digitalWrite(ledPin, HIGH);
  digitalWrite(buzzerPin, HIGH);
  delay(200);
  digitalWrite(ledPin, LOW);
  digitalWrite(buzzerPin, LOW);
  delay(200);
}

void dash() {
  digitalWrite(ledPin, HIGH);
  digitalWrite(buzzerPin, HIGH);
  delay(600);
  digitalWrite(ledPin, LOW);
  digitalWrite(buzzerPin, LOW);
  delay(200);
}

void sendMorse(String code) {
  for (int i = 0; i < code.length(); i++) {
    if (code[i] == '.') dot();
    else if (code[i] == '-') dash();
  }
  delay(600); // pauză între caractere
}

void setup() {
  pinMode(ledPin, OUTPUT);
  pinMode(buzzerPin, OUTPUT);
  Serial.begin(9600);
}

void loop() {
  char key = keypad.getKey();

  if (key) {
    Serial.print("Tasta apăsată: ");
    Serial.println(key);
    String code = morseCode(key);
    if (code.length() > 0) {
      sendMorse(code);
    }
  }
}

## ▶️ Cum se folosește?

- Apasă litere și cifre pe tastatură
- Dispozitivul va emite lumini și sunete în cod Morse
- Se transmite un mesaj fără a părea suspect

## 🎯 Utilizări posibile

- Persoane în pericol care nu pot vorbi
- Situații de urgență în spații publice
- Comunicare discretă pentru persoane cu nevoi speciale

## 🧠 Idei de îmbunătățire

- Transmiterea codului și prin semnale RF sau Bluetooth
- Integrare cu aplicații mobile
- Detectare automată a cuvintelor-cheie (ex: SOS)

## 📸 Demo (în lucru)

Imagini și videoclipuri cu prototipul în funcțiune vor fi adăugate în curând.
