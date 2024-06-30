# Estilofone no Arduino
*segundo trabalho da disciplina Eletronica para Computação (SSC0180)*

# Sobre o projeto
Nesse segundo trabalho, buscamos construir um estilofone formado por fios e resistores com funcionamento capacitado pelo arduino. Nele, tocando um fio de 5V em um fio de entrada, uma nota será emitida pelo computador conectado ao arduino.

# Funcionamento
Para o funcionamento do projeto, usamos fios que poderão ser tocados conectados a resistores e estes ao arduino, que, "percebendo" a ddp vinda do respectivo fio, printa a letra correspondente à nota em um simulador de piano virtual; os resistores associados aos fios foram usados na tentativa de reduzir a quantidade de ruído criado na análise da tensão. Exemplificando a ideia, ao tocar o primeiro fio com um vcc (fio que sai com 5V do arduino), a ddp presente é transmitida pelo fio que entra na primeira entrada analógica do arduíno, que, por sua vez, verifica isso e printa a tecla "u" no computador, correspondente à nota dó no simulador de piano. O instrumento possui 12 notas no total (uma oitva inteira, contando semitons; no simulador, emitem as notas de C4 à B4) e, logicamente, utiliza 12 entradas analógicas do arduíno. Para possibilitar a análise da voltagem e a transmissão dos dados analisados do arduino ao computador, foi usado também um outro programa que simula as teclas do computador quando aberto em um site de piano virtual.

# Componentes
Para o projeto, usamos unicamente jumpers, uma protoboard, resistores de 3k ohm, fita isolante, um pedaço de papelão e um arduíno mega. Resumidamente, os gastos foram de aproximadamente R$20,00 (quase que completamente em jumpers), tendo sido possível reutilizar grande parte dos componentes e usar o arduino emprestado (obrigado professor!).

# Fotos e vídeo
![Captura de tela 2024-06-28 152427](https://github.com/danieljmanzano/piano-no-arduino/assets/162331747/b48b6213-b2db-4bb4-8922-e110c85e97d9)
- *imagem do tinkercad*
- *observação: a entrada de 12 pinos representa os fios de entrada, o fio da saída de 5V do arduíno é usado para transmitir a ddp para a respectiva entrada de interesse; no caso da foto acima, a nota pressionada seria a primeira*

![Imagem do WhatsApp de 2024-06-30 à(s) 11 30 42_14e3eda5](https://github.com/danieljmanzano/estilofone-no-arduino/assets/162331747/ad2aef66-3a03-4b74-a2d9-79dbe753e7a6)
- *imagem do projeto final*

https://github.com/danieljmanzano/estilofone-no-arduino/assets/162331747/422b1f6b-7235-4e37-bb71-30dd99a4bfc2
- *vídeo explicando brevemente o projeto*


# Códigos
- Código do arduino
```
const char teclas[] = {'u', '8', 'i', '9', 'o', 'p', 'a', 'z', 's', 'x', 'd', 'c'};
int j = 1;
int arrayLeituraAntigo[12];

void setup() {
  pinMode(A0, INPUT);
  pinMode(A1, INPUT);
  pinMode(A2, INPUT);
  pinMode(A3, INPUT);
  pinMode(A4, INPUT);
  pinMode(A5, INPUT);
  pinMode(A6, INPUT);
  pinMode(A7, INPUT);
  pinMode(A8, INPUT);
  pinMode(A9, INPUT);
  pinMode(A10, INPUT);
  pinMode(A11, INPUT);
  Serial.begin(9600);
}

void loop() {
  int leituraA0 = analogRead(A0);
  //leituraA0 = map(leituraA0, 0, 1023, 0, 5);
  int leituraA1 = analogRead(A1);
  //leituraA1 = map(leituraA1, 0, 1023, 0, 5);
  int leituraA2 = analogRead(A2);
  //leituraA2 = map(leituraA2, 0, 1023, 0, 5);
  int leituraA3 = analogRead(A3);
  //leituraA3 = map(leituraA3, 0, 1023, 0, 5);
  int leituraA4 = analogRead(A4);
  //leituraA4 = map(leituraA4, 0, 1023, 0, 5);
  int leituraA5 = analogRead(A5);
  //leituraA5 = map(leituraA5, 0, 1023, 0, 5);
  int leituraA6 = analogRead(A6);
  //leituraA6 = map(leituraA6, 0, 1023, 0, 5);
  int leituraA7 = analogRead(A7);
  //leituraA7 = map(leituraA7, 0, 1023, 0, 5);
  int leituraA8 = analogRead(A8);
  //leituraA8 = map(leituraA8, 0, 1023, 0, 5);
  int leituraA9 = analogRead(A9);
  //leituraA9 = map(leituraA9, 0, 1023, 0, 5);
  int leituraA10 = analogRead(A10);
  //leituraA10 = map(leituraA10, 0, 1023, 0, 5);
  int leituraA11 = analogRead(A11);
  //leituraA11 = map(leituraA11, 0, 1023, 0, 5);
  int arrayLeitura[] = {leituraA0, leituraA1, leituraA2, leituraA3, leituraA4, leituraA5, leituraA6, leituraA7, leituraA8, leituraA9, leituraA10, leituraA11};
  if(j == 1) {
    for(int i = 0; i < 12; i++){
      arrayLeituraAntigo[i] = arrayLeitura[i];
    }
    j = 0;
  }
  for(int i = 0; i < 12; i++){
    /*Serial.print("Tecla ");
    Serial.println(i);
    Serial.println(arrayLeitura[i]);*/
    
    if (arrayLeitura[i] > 1000 && arrayLeitura[i] > arrayLeituraAntigo[i]){
      Serial.print(teclas[i]);
    }
  }
  if(j == 0) {
    for(int i = 0; i < 12; i++){
      arrayLeituraAntigo[i] = arrayLeitura[i];
    }
  }
  delay(200);
}
```
- Programa que recebe os dados do arduino e simula as teclas do computador (feito em Python)
```
import serial
import keyboard

serial_port = '/dev/ttyACM1'
baud_rate = 9600
t = 1
ser = serial.Serial(serial_port, baud_rate)

print(f"Conectando {serial_port}...")

try:
    while True:
        if ser.in_waiting > 0:
            char = ser.read().decode('utf-8')
            keyboard.write(char)
            print(f"lido: {char}")
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


