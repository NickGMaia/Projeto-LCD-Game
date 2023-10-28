# **[Trabalho de Microcontroladores e IOT](<https://youtu.be/SKS9lwS5mfg>)**

<https://youtu.be/SKS9lwS5mfg>


## Introdução

Estamos aqui para apresentar um projeto que realizamos com dedicação para a matéria de Microcontroladores e IOT, utilizando o conhecido Arduino UNO R3.

A ideia principal foi fazer um jogo que utilizasse poucos recursos, que seja divertido, e para conhecermos mais sobre o funcionamento dos componentes e adquirir experiência com sistema embarcado.

Onde trabalhando em sinergia, unimos uma multiplicidade de ideias e inspirações em trabalhos anteriores para o processo deste projeto.

### Projeto

O projeto foi idealizado com a finalidade de criar um sistema de jogos portáteis. Optamos pelo Arduino Uno devido à sua acessibilidade, facilidade de uso e custo acessível. Além disso, o Arduino Uno é acompanhado por um software simples que permite a fácil implementação de comandos desejados.

O jogo desenvolvido para o sistema é um side-scroller, com uma visão lateral, onde o jogador controla um personagem que avança continuamente. Este estilo de jogo é exemplificado por clássicos como Mario, Mega Man e Sonic, entre outros. No nosso jogo, os jogadores acumulam pontos à medida que sobrevivem e desviam de obstáculos, permitindo competições amigáveis para determinar quem é o melhor.

## Componentes

Para o nosso projeto, optamos por utilizar componentes de baixo custo e facilmente acessíveis, garantindo uma abordagem econômica e eficiente.
As peças que utilizamos no projeto foram:

- Arduino Uno ![Arduino](https://github.com/NickGMaia/Projeto-LCD-Game/assets/127119412/76066f0f-c675-473a-86d4-b092a02c1ef7)

- LCD 16x2  ![Display](https://github.com/NickGMaia/Projeto-LCD-Game/assets/127119412/06167576-4e69-4b88-8bce-e878770d5340)

- Jumpers 15x ![Jumpers](https://github.com/NickGMaia/Projeto-LCD-Game/assets/127119412/b28177d8-c726-4a14-a120-f89b94dc07f3)

- botão ![Botão](https://github.com/NickGMaia/Projeto-LCD-Game/assets/127119412/2e150f3d-9e21-41e6-bb6a-669d0ed14a69)

- Protoboard ![Protoboard](https://github.com/NickGMaia/Projeto-LCD-Game/assets/127119412/919b19c4-9163-4a0f-a19b-43f5a0143eca)

- Cabo USB ![USB](https://github.com/NickGMaia/Projeto-LCD-Game/assets/127119412/f2268caf-023f-49a1-a9a6-f281fbadf546)

> ### Função de cada peça mencionada
>
> - Arduino Uno : O Arduino é a peça base, como o cérebro do sistema, com a sua função de intermediar todas as peças em conjunto, guardar os códigos no microcontrolador, fornecer pinos digitais e analógico, interfaces USB e de alimentação.
>
> - LCD 16x2: Serve para projetar o jogo na tela, fazendo conexões com o Arduino por meio de jumpers.
> - Jumpers: Estes cabos interligam uma peça com a outra, possibilitando a conexão elétrica dos componentes do circuito e a transferência de dados.
> - Botão: O botão é usado para controlar o jogo na tela LCD, conectando-se ao Arduino e à tela.
> - Protoboard: Sua função é fornecer um meio conveniente e temporário para montar e testar circuitos eletrônicos sem a necessidade de soldagem.
> - Cabo USB: Utilizado para programar o Arduino, permitindo o upload de dados do Arduino IDE para o chipset do Arduino.
>

## Linguagem

A Linguagem utilizada para a realização do projeto foi a C++, visto que a linguagem em que o Arduino se baseia é uma versão em C++. Isso acontece pois é uma linguagem bem compreendida por muitos programadores, especialmente para iniciantes na área de eletrônica e programação.

Além de oferecer eficiência e controle precisos sobre os recursos de hardware, características cruciais em sistemas embarcados como o Arduino, onde a otimização é essencial. Além de ser fácil se portar para outros sistemas.

### Código fonte

``` C++
#include <LiquidCrystal.h>

#define PIN_BUTTON 2
#define PIN_AUTOPLAY 1
#define PIN_READWRITE 10
#define PIN_CONTRAST 12

#define SPRITE_RUN1 1
#define SPRITE_RUN2 2
#define SPRITE_JUMP 3
#define SPRITE_JUMP_UPPER '.'         
#define SPRITE_JUMP_LOWER 4
#define SPRITE_TERRAIN_EMPTY ' '      
#define SPRITE_TERRAIN_SOLID 5
#define SPRITE_TERRAIN_SOLID_RIGHT 6
#define SPRITE_TERRAIN_SOLID_LEFT 7

#define HERO_HORIZONTAL_POSITION 1    

#define TERRAIN_WIDTH 16
#define TERRAIN_EMPTY 0
#define TERRAIN_LOWER_BLOCK 1
#define TERRAIN_UPPER_BLOCK 2

#define HERO_POSITION_OFF 0          
#define HERO_POSITION_RUN_LOWER_1 1  
#define HERO_POSITION_RUN_LOWER_2 2 

#define HERO_POSITION_JUMP_1 3       
#define HERO_POSITION_JUMP_2 4       
#define HERO_POSITION_JUMP_3 5       
#define HERO_POSITION_JUMP_4 6       
#define HERO_POSITION_JUMP_5 7       
#define HERO_POSITION_JUMP_6 8       
#define HERO_POSITION_JUMP_7 9       
#define HERO_POSITION_JUMP_8 10      

#define HERO_POSITION_RUN_UPPER_1 11 
#define HERO_POSITION_RUN_UPPER_2 12 

LiquidCrystal lcd(11, 9, 6, 5, 4, 3);
static char terrainUpper[TERRAIN_WIDTH + 1];
static char terrainLower[TERRAIN_WIDTH + 1];
static bool buttonPushed = false;

void initializeGraphics(){
  static byte graphics[] = {
    
    B01100,
    B01100,
    B00000,
    B01110,
    B11100,
    B01100,
    B11010,
    B10011,
    
    B01100,
    B01100,
    B00000,
    B01100,
    B01100,
    B01100,
    B01100,
    B01110,
    
    B01100,
    B01100,
    B00000,
    B11110,
    B01101,
    B11111,
    B10000,
    B00000,
    
    B11110,
    B01101,
    B11111,
    B10000,
    B00000,
    B00000,
    B00000,
    B00000,
    
    B11111,
    B11111,
    B11111,
    B11111,
    B11111,
    B11111,
    B11111,
    B11111,
    
    B00011,
    B00011,
    B00011,
    B00011,
    B00011,
    B00011,
    B00011,
    B00011,
   
    B11000,
    B11000,
    B11000,
    B11000,
    B11000,
    B11000,
    B11000,
    B11000,
  };
  int i;
 
  for (i = 0; i < 7; ++i) {
   lcd.createChar(i + 1, &graphics[i * 8]);
  }
  for (i = 0; i < TERRAIN_WIDTH; ++i) {
    terrainUpper[i] = SPRITE_TERRAIN_EMPTY;
    terrainLower[i] = SPRITE_TERRAIN_EMPTY;
  }
}


void advanceTerrain(char* terrain, byte newTerrain){
  for (int i = 0; i < TERRAIN_WIDTH; ++i) {
    char current = terrain[i];
    char next = (i == TERRAIN_WIDTH-1) ? newTerrain : terrain[i+1];
    switch (current){
      case SPRITE_TERRAIN_EMPTY:
        terrain[i] = (next == SPRITE_TERRAIN_SOLID) ? SPRITE_TERRAIN_SOLID_RIGHT : SPRITE_TERRAIN_EMPTY;
        break;
      case SPRITE_TERRAIN_SOLID:
        terrain[i] = (next == SPRITE_TERRAIN_EMPTY) ? SPRITE_TERRAIN_SOLID_LEFT : SPRITE_TERRAIN_SOLID;
        break;
      case SPRITE_TERRAIN_SOLID_RIGHT:
        terrain[i] = SPRITE_TERRAIN_SOLID;
        break;
      case SPRITE_TERRAIN_SOLID_LEFT:
        terrain[i] = SPRITE_TERRAIN_EMPTY;
        break;
    }
  }
}

bool drawHero(byte position, char* terrainUpper, char* terrainLower, unsigned int score) {
  bool collide = false;
  char upperSave = terrainUpper[HERO_HORIZONTAL_POSITION];
  char lowerSave = terrainLower[HERO_HORIZONTAL_POSITION];
  byte upper, lower;
  switch (position) {
    case HERO_POSITION_OFF:
      upper = lower = SPRITE_TERRAIN_EMPTY;
      break;
    case HERO_POSITION_RUN_LOWER_1:
      upper = SPRITE_TERRAIN_EMPTY;
      lower = SPRITE_RUN1;
      break;
    case HERO_POSITION_RUN_LOWER_2:
      upper = SPRITE_TERRAIN_EMPTY;
      lower = SPRITE_RUN2;
      break;
    case HERO_POSITION_JUMP_1:
    case HERO_POSITION_JUMP_8:
      upper = SPRITE_TERRAIN_EMPTY;
      lower = SPRITE_JUMP;
      break;
    case HERO_POSITION_JUMP_2:
    case HERO_POSITION_JUMP_7:
      upper = SPRITE_JUMP_UPPER;
      lower = SPRITE_JUMP_LOWER;
      break;
    case HERO_POSITION_JUMP_3:
    case HERO_POSITION_JUMP_4:
    case HERO_POSITION_JUMP_5:
    case HERO_POSITION_JUMP_6:
      upper = SPRITE_JUMP;
      lower = SPRITE_TERRAIN_EMPTY;
      break;
    case HERO_POSITION_RUN_UPPER_1:
      upper = SPRITE_RUN1;
      lower = SPRITE_TERRAIN_EMPTY;
      break;
    case HERO_POSITION_RUN_UPPER_2:
      upper = SPRITE_RUN2;
      lower = SPRITE_TERRAIN_EMPTY;
      break;
  }
  if (upper != ' ') {
    terrainUpper[HERO_HORIZONTAL_POSITION] = upper;
    collide = (upperSave == SPRITE_TERRAIN_EMPTY) ? false : true;
  }
  if (lower != ' ') {
    terrainLower[HERO_HORIZONTAL_POSITION] = lower;
    collide |= (lowerSave == SPRITE_TERRAIN_EMPTY) ? false : true;
  }
  
  byte digits = (score > 9999) ? 5 : (score > 999) ? 4 : (score > 99) ? 3 : (score > 9) ? 2 : 1;
  
  
  terrainUpper[TERRAIN_WIDTH] = '\0';
  terrainLower[TERRAIN_WIDTH] = '\0';
  char temp = terrainUpper[16-digits];
  terrainUpper[16-digits] = '\0';
  lcd.setCursor(0,0);
  lcd.print(terrainUpper);
  terrainUpper[16-digits] = temp;  
  lcd.setCursor(0,1);
  lcd.print(terrainLower);
  
  lcd.setCursor(16 - digits,0);
  lcd.print(score);

  terrainUpper[HERO_HORIZONTAL_POSITION] = upperSave;
  terrainLower[HERO_HORIZONTAL_POSITION] = lowerSave;
  return collide;
}


void buttonPush() {
  buttonPushed = true;
}

void setup(){
  pinMode(PIN_READWRITE, OUTPUT);
  digitalWrite(PIN_READWRITE, LOW);
  pinMode(PIN_CONTRAST, OUTPUT);
  digitalWrite(PIN_CONTRAST, LOW);
  pinMode(PIN_BUTTON, INPUT);
  digitalWrite(PIN_BUTTON, HIGH);
  pinMode(PIN_AUTOPLAY, OUTPUT);
  digitalWrite(PIN_AUTOPLAY, HIGH);
  
  
  attachInterrupt(0, buttonPush, FALLING);
  
  initializeGraphics();
  
  lcd.begin(16, 2);
}

void loop(){
  static byte heroPos = HERO_POSITION_RUN_LOWER_1;
  static byte newTerrainType = TERRAIN_EMPTY;
  static byte newTerrainDuration = 1;
  static bool playing = false;
  static bool blink = false;
  static unsigned int distance = 0;
  
  if (!playing) {
    drawHero((blink) ? HERO_POSITION_OFF : heroPos, terrainUpper, terrainLower, distance >> 3);
    if (blink) {
      lcd.setCursor(0,0);
      lcd.print("Press Start");
    }
    delay(250);
    blink = !blink;
    if (buttonPushed) {
      initializeGraphics();
      heroPos = HERO_POSITION_RUN_LOWER_1;
      playing = true;
      buttonPushed = false;
      distance = 0;
    }
    return;
  }

  
  advanceTerrain(terrainLower, newTerrainType == TERRAIN_LOWER_BLOCK ? SPRITE_TERRAIN_SOLID : SPRITE_TERRAIN_EMPTY);
  advanceTerrain(terrainUpper, newTerrainType == TERRAIN_UPPER_BLOCK ? SPRITE_TERRAIN_SOLID : SPRITE_TERRAIN_EMPTY);
  
  
  if (--newTerrainDuration == 0) {
    if (newTerrainType == TERRAIN_EMPTY) {
      newTerrainType = (random(3) == 0) ? TERRAIN_UPPER_BLOCK : TERRAIN_LOWER_BLOCK;
      newTerrainDuration = 2 + random(10);
    } else {
      newTerrainType = TERRAIN_EMPTY;
      newTerrainDuration = 10 + random(10);
    }
  }
    
  if (buttonPushed) {
    if (heroPos <= HERO_POSITION_RUN_LOWER_2) heroPos = HERO_POSITION_JUMP_1;
    buttonPushed = false;
  }  

  if (drawHero(heroPos, terrainUpper, terrainLower, distance >> 3)) {
    playing = false; 
  } else {
    if (heroPos == HERO_POSITION_RUN_LOWER_2 || heroPos == HERO_POSITION_JUMP_8) {
      heroPos = HERO_POSITION_RUN_LOWER_1;
    } else if ((heroPos >= HERO_POSITION_JUMP_3 && heroPos <= HERO_POSITION_JUMP_5) && terrainLower[HERO_HORIZONTAL_POSITION] != SPRITE_TERRAIN_EMPTY) {
      heroPos = HERO_POSITION_RUN_UPPER_1;
    } else if (heroPos >= HERO_POSITION_RUN_UPPER_1 && terrainLower[HERO_HORIZONTAL_POSITION] == SPRITE_TERRAIN_EMPTY) {
      heroPos = HERO_POSITION_JUMP_5;
    } else if (heroPos == HERO_POSITION_RUN_UPPER_2) {
      heroPos = HERO_POSITION_RUN_UPPER_1;
    } else {
      ++heroPos;
    }
    ++distance;
    
    digitalWrite(PIN_AUTOPLAY, terrainLower[HERO_HORIZONTAL_POSITION + 2] == SPRITE_TERRAIN_EMPTY ? HIGH : LOW);
  }
  delay(50);
}

```

## Authors

Nicolas Garcia Soto Maia

Adrian Dominick T. Y. Mateus

Beatriz Alves de Sousa

Ísis Sofia Gueiros Soares
