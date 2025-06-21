
## Controle para o Jogo do Google Chrome (dinossauro) com a Franzininho

Este projeto demonstra como criar um controle físico para o jogo do dinossauro do Google Chrome usando a placa Franzininho.  
Ao pressionar um botão conectado à Franzininho, um comando de teclado (tecla espaço) é enviado ao computador via USB, fazendo o dinossauro pular.  

Com este exemplo, você aprende a:

- Trabalhar com entradas digitais (botões);
- Enviar comandos de teclado com a Franzininho;
- Integrar hardware com um jogo real e conhecido;
- Utilizar a Arduino IDE com placas compatíveis com Digispark.

💡 **Antes de começar**, siga o [tutorial de gravação do bootloader Micronucleus](https://github.com/Franzininho/franzininho-docs/tree/master/02-Franzininho-DIY/Grava%C3%A7%C3%A3o%20do%20bootloader/Micronucleus) caso ainda não tenha feito isso.

É um projeto educativo, simples e divertido — ideal para iniciantes.

---

## Circuito

Para este projeto, você irá precisar apenas de uma Franzininho e uma chave tátil:

![Circuito com botão](./circuito.png)

O botão deve ser conectado entre o pino digital 2 e o GND da Franzininho.

---

## Configuração da IDE Arduino

Vamos usar os exemplos da Digispark para USB. Para isso, você precisa instalar o pacote de suporte da Digispark.

1. Acesse o menu **Arquivo → Preferências** e cole a URL abaixo no campo "URLs Adicionais para Gerenciadores de Placas":

```
http://digistump.com/package_digistump_index.json
```

2. Vá em **Ferramentas → Placa → Gerenciador de Placas**, digite "digi" e instale o pacote “Digistump AVR Boards”.

3. Depois, selecione a placa: **Ferramentas → Placa → Digispark (Default - 16.5 MHz)**.

4. Vá em **Ferramentas → Programador** e escolha **Micronucleus**.

---

## Sketch

Para o funcionamento da Franzininho como controle do jogo do Google Chrome (dinossauro), carregue o seguinte sketch:

```c++
#include "DigiKeyboard.h"  // biblioteca da Digispark para teclado

const int btPin = 2;       // pino ao qual a tecla está conectada

int estadoAnteriorBotao = 0;  // armazena o estado anterior do botão

void setup() {
  pinMode(btPin, INPUT_PULLUP);  // configura o pino do botão como entrada com pull-up habilitado
}

void loop() {
  int estadoAtualBT = digitalRead(btPin);  // lê o estado do botão

  if ((estadoAtualBT != estadoAnteriorBotao) && (estadoAtualBT == LOW)) {
    // Se o botão foi pressionado e seu estado mudou

    // Envia comando da tecla espaço para o computador
    DigiKeyboard.sendKeyStroke(0);
    DigiKeyboard.println("  ");
  }

  estadoAnteriorBotao = estadoAtualBT;  // salva o estado atual para comparar na próxima leitura
}
```

---

## Funcionamento

Confira o vídeo de funcionamento clicando na imagem abaixo:

[![Video](http://img.youtube.com/vi/aMfCYi9xhcA/0.jpg)](http://www.youtube.com/watch?v=aMfCYi9xhcA "Controle para o Jogo do Google Chrome (Dino) com a Franzininho")

---

## Desafio

Você percebeu que o jogo precisa de mais um comando para o dinossauro abaixar?  
Tente inserir esse comando adicionando uma nova tecla!

---

## Crie novos projetos

O exemplo apresentado pode ser adaptado para controlar outros jogos que usam teclas como entrada.  
Verifique quais jogos você pode controlar e adicione mais botões e funcionalidades.

Boa diversão!
