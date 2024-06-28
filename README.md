# Piano no Arduino
*segundo trabalho da disciplina Eletronica para Computação (SSC0180)*

# Sobre o projeto
Nesse segundo trabalho, buscamos construir um piano/teclado feito de teclas de alumínio e que usa touch capacitivo com base no arduino. 

# Funcionamento
Para o funcionamento do projeto, usamos teclas feitas de papel alumínio capazes de transmitir tensão quando pressionadas diretamente para o arduíno, este que, "percebendo" a ddp vinda da respectiva tecla, printa a letra correspondente à nota em um simulador de piano virtual; além disso, usamos resistores associados aos fios na tentativa de reduzir a quantidade de ruído criado na análise da tensão. Por exemplo, ao pressionar a primeira tecla de alumínio, a ddp presente é transmitida pelo fio que entra na primeira entrada digital (no caso, a primeira seria a entrada 2) do arduíno, que, por sua vez, verifica isso e printa a tecla "u" no computador, correspondente à nota dó no simulador de piano. O piano possui 12 teclas no total (uma oitva inteira, contando semitons; no simulador, emitem as notas de C4 à B4) e, logicamente, utiliza 12 entradas digitais do arduíno. Para criar a diferença de potencial, usamos a saída de 5V do arduíno para tocar nas teclas (uma mão segura o fio, a outra toca no piano).

# Componentes
Para o projeto, usamos unicamente jumpers, resistores de 3k ohm e o arduíno (emprestado pelo professor). Como "extra", confeccionamos a superfície do teclado manualmente usando uma folha de alumínio e um pedaço de papelão, além de fita isolante para prender os fios.

# Fotos e vídeo
![Captura de tela 2024-06-28 152427](https://github.com/danieljmanzano/piano-no-arduino/assets/162331747/b48b6213-b2db-4bb4-8922-e110c85e97d9)

*observação: a entrada de 12 pinos de que saem os fios representa as teclas, o fio da saída de 5V do arduíno é usado para transmitir a ddp para a respectiva entrada (tecla que liga, pelo fio, ao arduíno) de interesse; no caso da foto acima, a tecla pressionada seria a primeira*


![Imagem do WhatsApp de 2024-06-27 à(s) 20 28 46_5ebbcad2](https://github.com/danieljmanzano/piano-no-arduino/assets/162331747/89168468-4477-4bbb-94bd-7698d7bc7360)
*foto do teclado* 


*aqui entra um vídeo*
# Códigos
- Código do arduino
```
// Definição dos pinos dos sensores de toque
const int touchPins[] = {52, 46, 40, 36, 32, 28, 22, 2, 5, 8, 11, 12}; // As posições dos pinos foram escolhidas de modo que os fios pudessem ficar mais espaçados
// Definição dos pinos das notas musicais correspondentes
const char teclas[] = {'u', '8', 'i', '9', 'o', 'p', 'a', 'z', 's', 'x', 'd', 'c'}; // Esses caracteres correspondem à oitava principal de um simulador de piano que usamos de base


void setup() {
  // Configuração dos pinos dos sensores como entrada
  for (int i = 0; i < 12; i++) {
    pinMode(touchPins[i], INPUT);
  }
  
  Serial.begin(9600);
}

void loop() {
  for (int i = 0; i < 12; i++) {
    // Verificação se o sensor de toque foi ativado
    if (digitalRead(touchPins[i]) == HIGH) {
      // Tocar a nota correspondente
      Serial.print(teclas[i]);
      delay(400);
    }
  }
}
```
- Programa que recebe os dados do arduino e simula as teclas do computador (feito em Python)
```
import serial
import keyboard

serial_port = '/dev/ttyACM1' //detalhe: o código foi usado em linux, a entrada usada muda em windows
baud_rate = 9600  

ser = serial.Serial(serial_port, baud_rate)

print(f"Conectando {serial_port}...")

try:
    while True:
        if ser.in_waiting > 0:
            char = ser.read().decode('utf-8')
            print(f"lido: {char}")
            keyboard.write(char)
except KeyboardInterrupt:
    print("\ncabo.")
finally:
    ser.close()
```

# Agradecimentos
Agradecemos ao professor Simões pelo auxílio que nos ofereceu ao dar ideias e soluções para problemas que enfrentamos no desenvolvimento do projeto, tornando o resultado final o melhor possível. Além disso, também agradecemos ao colega Fernando Valentim Torres pela ajuda que nos ofertou, fazendo funcionar a transmissão de dados do arduino ao computador com o segundo código e apoiando o grupo.

# Integrantes
- Daniel Jorge Manzano - 154468661
- Matheus Muzza Pires Ferreira - 15479468
- Rodrigo Silva de Almeida - 15645380
- Vinicius Morotti - 15491876


