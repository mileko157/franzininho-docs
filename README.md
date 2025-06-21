## 🎮 Jogo de Reflexo com a Franzininho (Modo Multiplayer com LCD, Buzzer e Ranking)

Este projeto evoluído permite dois jogadores testarem seus reflexos com feedback visual em **display LCD**, som de vitória via **buzzer**, e registro de placares com **melhor de 3 partidas**. Tudo isso usando a **Franzininho DIY**.

---

## 🧰 Materiais necessários

- 1 placa Franzininho DIY  
- 2 LEDs (um para cada jogador)  
- 2 resistores de 220Ω  
- 2 botões (push-button)  
- 1 buzzer ativo  
- 1 display LCD 16x2 (com módulo I2C) ou OLED  
- Jumpers, protoboard  
- Bibliotecas: `LiquidCrystal_I2C`, `EEPROM` (ambas na IDE Arduino)

---

## 🔌 Ligações principais

- LED1 no pino D2  
- LED2 no pino D3  
- Botão1 no pino D4  
- Botão2 no pino D5  
- Buzzer no pino D6  
- Display LCD via I2C nos pinos A4 (SDA) e A5 (SCL)

---

## 💻 Código-fonte com recursos extras

```cpp
#include <LiquidCrystal_I2C.h>
#include <EEPROM.h>

#define LED1 2
#define LED2 3
#define BOTAO1 4
#define BOTAO2 5
#define BUZZER 6

LiquidCrystal_I2C lcd(0x27, 16, 2);

int placar1 = 0, placar2 = 0;
bool esperando = false;
unsigned long tempoLED;

void setup() {
  pinMode(LED1, OUTPUT);
  pinMode(LED2, OUTPUT);
  pinMode(BOTAO1, INPUT_PULLUP);
  pinMode(BOTAO2, INPUT_PULLUP);
  pinMode(BUZZER, OUTPUT);
  lcd.init();
  lcd.backlight();
  Serial.begin(9600);
  delay(1000);
  lcd.setCursor(0, 0);
  lcd.print("Reflexo Multiplayer");
  delay(2000);
  lcd.clear();
}

void loop() {
  if (!esperando) {
    lcd.setCursor(0, 0);
    lcd.print("Preparar...");
    delay(random(2000, 5000));
    digitalWrite(LED1, HIGH);
    digitalWrite(LED2, HIGH);
    lcd.setCursor(0, 0);
    lcd.print("Vai!           ");
    tempoLED = millis();
    esperando = true;
  }

  if (esperando) {
    if (digitalRead(BOTAO1) == LOW) {
      digitalWrite(LED1, LOW);
      digitalWrite(LED2, LOW);
      unsigned long tempo = millis() - tempoLED;
      placar1++;
      lcd.clear();
      lcd.setCursor(0, 0);
      lcd.print("J1: ");
      lcd.print(tempo);
      lcd.print(" ms");
      lcd.setCursor(0, 1);
      lcd.print("Placar J1:");
      lcd.print(placar1);
      somVitoria();
      checarVitoria();
      esperando = false;
      delay(2000);
    } else if (digitalRead(BOTAO2) == LOW) {
      digitalWrite(LED1, LOW);
      digitalWrite(LED2, LOW);
      unsigned long tempo = millis() - tempoLED;
      placar2++;
      lcd.clear();
      lcd.setCursor(0, 0);
      lcd.print("J2: ");
      lcd.print(tempo);
      lcd.print(" ms");
      lcd.setCursor(0, 1);
      lcd.print("Placar J2:");
      lcd.print(placar2);
      somVitoria();
      checarVitoria();
      esperando = false;
      delay(2000);
    }
  }
}

void somVitoria() {
  tone(BUZZER, 1000, 300);
  delay(300);
  noTone(BUZZER);
}

void checarVitoria() {
  if (placar1 >= 3 || placar2 >= 3) {
    lcd.clear();
    if (placar1 > placar2) {
      lcd.print("Jogador 1 Venceu!");
      EEPROM.write(0, 1); // salvar vencedor
    } else {
      lcd.print("Jogador 2 Venceu!");
      EEPROM.write(0, 2);
    }
    delay(3000);
    lcd.clear();
    lcd.print("Reiniciando...");
    delay(2000);
    placar1 = 0;
    placar2 = 0;
    lcd.clear();
  }
}
```

---

## 🧠 Conceitos aplicados

- Uso de displays LCD via I2C  
- Leitura de botões com `INPUT_PULLUP`  
- Temporização com `millis()`  
- Áudio com buzzer (tom de vitória)  
- Armazenamento de dados com `EEPROM.write()`  
- Placar com vitória melhor de 3

---

## 📚 Tutoriais complementares

- [LCD I2C com Arduino](https://www.arduino.cc/en/Tutorial/HelloWorld)  
- [EEPROM Arduino](https://www.arduino.cc/en/Reference/EEPROM)  
- [Buzzer Arduino básico](https://www.arduino.cc/en/Tutorial/toneMelody)  

---

**Feito com ❤️ pela comunidade Franzininho.**
