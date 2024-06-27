# Piano no Arduino
*segundo trabalho da disciplina Eletronica para Computação (SSC0180)*

# Sobre o projeto
Nesse segundo trabalho, buscamos construir um piano/teclado feito de teclas de alumínio e que usa touch capacitivo com base no arduino. 

# Funcionamento
Para o funcionamento do projeto, usamos teclas feitas de papel alumínio capazes de transmitir tensão quando pressionadas diretamente para o arduíno, este que, "percebendo" a ddp vinda da respectiva tecla, toca o buzzer associado ao circuito com a frequência específica da nota pressionada.

# Componentes
Para o projeto, usamos unicamente jumpers, um buzzer e o arduíno (emprestado pelo professor). Como "extra", confeccionamos a superfície do teclado manualmente usando uma folha de alumínio e um pedaço de papelão, além de fita para prender os fios.

# Fotos e vídeo
![Captura de tela 2024-06-27 175931](https://github.com/danieljmanzano/piano-no-arduino/assets/162331747/79e49f3c-95bb-4f1a-88e5-8c035ae55d0d)
*observação: a entrada de 12 pinos de que saem os fios representa as teclas, o fio da saída de 5V do arduíno é usado para transmitir a ddp para a respectiva entrada (tecla que liga, pelo fio, ao arduíno) de interesse; no caso da foto acima, a tecla pressionada seria a primeira*


![Imagem do WhatsApp de 2024-06-27 à(s) 20 28 46_5ebbcad2](https://github.com/danieljmanzano/piano-no-arduino/assets/162331747/f3fa80a2-2791-41b4-9fe9-9010bc8e0862)
*foto do projeto final*


*aqui entra um vídeo*

# Código
```
// ------ Código do projeto ------
// Definição dos pinos dos sensores de toque
const int touchPins[] = {52, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12}; // Obs.: o primeiro pino sendo o 52 se deve ao fato de os pinos 1 e 13 do arduíno não estarem em bom funcionamento
// Definição dos pinos das notas musicais correspondentes
const int notes[] = {264, 278, 294, 312, 330, 350, 370, 392, 416, 440, 467, 494}; // Toca uma oitava inteira (dó a sí), incluindo semitons

// Pino do buzzer
const int buzzerPin = A0;

void setup() {
  // Configuração dos pinos dos sensores como entrada
  for (int i = 0; i < 12; i++) {
    pinMode(touchPins[i], INPUT);
  }
  // Configuração do pino do buzzer como saída
  pinMode(buzzerPin, OUTPUT);
}

void loop() {
  for (int i = 0; i < 12; i++) {
    // Verificação se o sensor de toque foi ativado
    if (digitalRead(touchPins[i]) == HIGH) {
      // Tocar a nota correspondente
      tone(buzzerPin, notes[i]);
    }
  }
  // Parar o som se nenhum sensor estiver sendo tocado
  if (noTouchDetected()) {
    noTone(buzzerPin);
  }
}

bool noTouchDetected() {
  for (int i = 0; i < 12; i++) {
    if (digitalRead(touchPins[i]) == HIGH) {
      return false;
    }
  }
  return true;
}
```

# Integrantes
- Daniel Jorge Manzano - 154468661
- Matheus Muzza Pires Ferreira - 15479468
- Rodrigo Silva de Almeida - 15645380
- Vinicius Morotti - 15491876

