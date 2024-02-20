#include <LiquidCrystalRus.h>
#define Kn 45 

#define kn1 54
#define kn2 55
#define kn3 56
#define kn4 57
#define kn5 58
#define kn6 59

#define svet1 31
#define svet2 33
#define svet3 35
#define svet4 37
#define svet5 39
#define svet6 41

void setup() {

  pinMode(kn1, INPUT_PULLUP);
  pinMode(kn2, INPUT_PULLUP);
  pinMode(kn3, INPUT_PULLUP);
  pinMode(kn4, INPUT_PULLUP);
  pinMode(kn5, INPUT_PULLUP);
  pinMode(kn6, INPUT_PULLUP);

  pinMode(svet1, OUTPUT);
  pinMode(svet2, OUTPUT);
  pinMode(svet3, OUTPUT);
  pinMode(svet4, OUTPUT);
  pinMode(svet5, OUTPUT);
  pinMode(svet6, OUTPUT);

  pinMode(Kn, INPUT_PULLUP);

  Serial.begin(9600);
}

int mass_kn[6] = { 0, 0, 0, 0, 0, 0 };  // Массив последовательности кнопок
// Примечание: массив mass_kn глобальный, потому что он будет работать для всех левлов, то есть при считывание кнопок на разных левлов будет использоватся один и тот же массив
// Если что массив mass_kn можно сделать локальный в функции в нужном левле, если будет какая-либо ошибка
int mass_level1[6] = { 1, 2, 3, 4, 5, 6 };
int mass_level2[6] = {3, 5, 1, 2, 4, 6};
int mass_level3[6] = {6, 3, 4, 1, 5, 2};
int mass_level4[6] = {2, 5, 4, 6, 3, 1};
int mass_level5[6] = {4, 1, 3, 6, 5, 2};
 // Массив правильной последовательности для 1 левла

bool confirm = 0, button_status = false; //confirm - состояние выбираемого левла игры (0 - невыбранно, 1 - выбранно), button_status - варианты состояние кнопки (1 раз нажата, зажата, ненажата)
uint32_t t_button_status = 0; //t_button_status - время для изменение состояние
int level = 1; //level - вариант левла игры

constexpr uint8_t PIN_RS = 22;
constexpr uint8_t PIN_EN = 24;
constexpr uint8_t PIN_DB4 = 26;
constexpr uint8_t PIN_DB5 = 28;
constexpr uint8_t PIN_DB6 = 30;
constexpr uint8_t PIN_DB7 = 32;

LiquidCrystalRus lcd(PIN_RS, PIN_EN, PIN_DB4, PIN_DB5, PIN_DB6, PIN_DB7);

void level_menu(int num){ //num - нужное количество вараинтов левлов игры
  long time, t_press; //time - время для измененния номера левла игры
  bool btnState = !digitalRead(Kn); // начальное состояние кнопки (0 - ненажата, 1 - зажата)
  if (btnState && button_status && millis() - t_button_status > 700) {
    t_button_status = millis();
    confirm = 1;
  }
  if (btnState && !button_status && millis() - t_button_status > 70) {
    button_status = true;
    t_button_status = millis();
  }
  if (!btnState && button_status && millis() - t_button_status > 70) {
    button_status = false;
    t_button_status = millis();
    if (confirm == 0) {
      if (level < num) {
        if (millis() - time > 100) {
          level++;
          time = millis();
        }
      } else if (level == num) {
        if (millis() - time > 100) {
          level = 1;
          time = millis();
        }
      }
    }
  }

   
}
void level1(int *mass_answer) {
  bool teg1 = 0, teg2 = 0, teg3 = 0, teg4 = 0, teg5 = 0, teg6 = 0, answer_key = 1;  // переменная teg, (как флаг, только немного с другим алгоритмом). Данная перменная отвечает за считывания нажатия на всех 6-и кнопках
  int k_but = 0;                                                //Переменная k_but отвечает за считывание нажатия с каждой кнопки, данная переменаая нужна для того чтобы пользователь нажал на любые кнопки ровно 6 раз
  long time1 = 0, time2 = 0, time3 = 0, time4 = 0, time5 = 0, time6 = 0;              // Это переменный времени для каждой кнопки

  digitalWrite(svet1, LOW);
  digitalWrite(svet2, LOW);
  digitalWrite(svet3, LOW);
  digitalWrite(svet4, LOW);
  digitalWrite(svet5, LOW);
  digitalWrite(svet6, LOW);

  lcd.clear();
  lcd.setCursor(5, 0);
  lcd.print("Запомни");
  delay(500);   

  // Ниже включается последовательность светодиодов
  if (mass_answer == mass_level1){
    digitalWrite(svet1, HIGH);
    delay(500);
    digitalWrite(svet2, HIGH);
    delay(500);
    digitalWrite(svet3, HIGH);
    delay(500);
    digitalWrite(svet4, HIGH);
    delay(500);
    digitalWrite(svet5, HIGH);
    delay(500);
    digitalWrite(svet6, HIGH);
    delay(500);
  }
  if (mass_answer == mass_level3){
    digitalWrite(svet6, HIGH);
    delay(450);
    digitalWrite(svet3, HIGH);
    delay(450);
    digitalWrite(svet4, HIGH);
    delay(450);
    digitalWrite(svet1, HIGH);
    delay(450);
    digitalWrite(svet5, HIGH);
    delay(450);
    digitalWrite(svet2, HIGH);
    delay(450);
  }
  if(mass_answer == mass_level2){
    digitalWrite(svet3, HIGH);
    delay(600);
    digitalWrite(svet5, HIGH);
    delay(600);
    digitalWrite(svet1, HIGH);
    delay(600);
    digitalWrite(svet2, HIGH);
    delay(600);
    digitalWrite(svet4, HIGH);
    delay(600);
    digitalWrite(svet6, HIGH);
    delay(600);
  }
  if(mass_answer == mass_level4){
    digitalWrite(svet2, HIGH);
    delay(300);
    digitalWrite(svet5, HIGH);
    delay(300);
    digitalWrite(svet4, HIGH);
    delay(300);
    digitalWrite(svet6, HIGH);
    delay(300);
    digitalWrite(svet3, HIGH);
    delay(300);
    digitalWrite(svet1, HIGH);
    delay(300);
  }
  if(mass_answer == mass_level5){
    digitalWrite(svet4, HIGH);
    delay(200);
    digitalWrite(svet1, HIGH);
    delay(200);
    digitalWrite(svet3, HIGH);
    delay(200);
    digitalWrite(svet6, HIGH);
    delay(200);
    digitalWrite(svet5, HIGH);
    delay(200);
    digitalWrite(svet2, HIGH);
    delay(300);
  }
  digitalWrite(svet1, LOW);
  digitalWrite(svet2, LOW);
  digitalWrite(svet3, LOW);
  digitalWrite(svet4, LOW);
  digitalWrite(svet5, LOW);
  digitalWrite(svet6, LOW);
   
  lcd.clear();
  lcd.setCursor(4, 0);
  lcd.print("Повтори,");
  lcd.setCursor(0.3, 1);
  lcd.print("используя кнопки");  

  while (k_but != 6) {                     // В данной комбинации у нас 6 последовательных нажатий, по этому while будет идти до тех пор пока k_but не считает 6 нажатий
    if (!digitalRead(kn1) == 1) teg1 = 1;  // Этот блок ифов и елс ифов нужен для считывания нажатие с 1 кнопки
    else if (!digitalRead(kn1) == 0 && teg1 == 1) {
      if (millis() - time1 > 70) {
        mass_kn[k_but] = 1;
        k_but += 1;
        teg1 = 0;
        time1 = millis();
      }
    }
    if (!digitalRead(kn2) == 1) teg2 = 1;  // Этот блок ифов и елс ифов нужен для считывания нажатие с 2 кнопки
    else if (!digitalRead(kn2) == 0 && teg2 == 1) {
      if (millis() - time2 > 70) {
        mass_kn[k_but] = 2;
        k_but += 1;
        teg2 = 0;
        time2 = millis();
      }
    }
    if (!digitalRead(kn3) == 1) teg3 = 1;  // Этот блок ифов и елс ифов нужен для считывания нажатие с 3 кнопки
    else if (!digitalRead(kn3) == 0 && teg3 == 1) {
      if (millis() - time3 > 70) {
        mass_kn[k_but] = 3;
        k_but += 1;
        teg3 = 0;
        time3 = millis();
      }
    }
    if (!digitalRead(kn4) == 1) teg4 = 1;  // Этот блок ифов и елс ифов нужен для считывания нажатие с 4 кнопки
    else if (!digitalRead(kn4) == 0 && teg4 == 1) {
      if (millis() - time4 > 70) {
        mass_kn[k_but] = 4;
        k_but += 1;
        teg4 = 0;
        time4 = millis();
      }
    }
    if (!digitalRead(kn5) == 1) teg5 = 1;  // Этот блок ифов и елс ифов нужен для считывания нажатие с 5 кнопки
    else if (!digitalRead(kn5) == 0 && teg5 == 1) {
      if (millis() - time5 > 70) {
        mass_kn[k_but] = 5;
        k_but += 1;
        teg5 = 0;
        time5 = millis();
      }
    }
    if (!digitalRead(kn6) == 1) teg6 = 1;  // Этот блок ифов и елс ифов нужен для считывания нажатие с 6 кнопки
    else if (!digitalRead(kn6) == 0 && teg6 == 1) {
      if (millis() - time6 > 70) {
        mass_kn[k_but] = 6;
        k_but += 1;
        teg6 = 0;
        time6 = millis();
      }
    }
    delay(10);
    // Ниже закоменнтированный код - это вывод проверки того что при нажатие переменная увеличивается на 1
    // То есть переменная k_but служит для фиксации нажатие с любой кнопки
    //Serial.println(k_but);
  }

  // Ниже закоменнтированный код - это вывод всего списка значений кнопок после того как ты записал их
  // for (int i = 0; i < 3; i++) {
  //   Serial.print(mass_svet[i]);
  //   Serial.print(" ");
  // }
  // Serial.println(mass_svet[3]);
  // delay(3000);

  //Ниже for отвечает за проверку правильности комбинации кнопок
  for (int i = 0; i < 6; i++) {
    if (mass_kn[i] != mass_answer[i]) {
      answer_key = 0;
      break;
    }
  }

  // Дальше внизу выводится правильная или не правильная комбинация ввода с кнопок
  if (answer_key == 1){
    lcd.clear();
    lcd.setCursor(3, 0);
    lcd.print("Правильно!");
    lcd.setCursor(0.5, 1);
    lcd.print("Уровень пройден!");
  }           
  else if (answer_key == 0){
    lcd.clear();
    lcd.setCursor(4, 0);
    lcd.print("Неверно!");
    lcd.setCursor(0.5, 1);
    lcd.print("Попробуй заново!");
  }  
  delay(3000);

  // Ниже в функции у меня обнуляются все нужные мне переменные чтоб код работал бесконечно
  // Бесконечно в том смысле, если мы ещё раз вызовем этот левл, чтоб он ещё раз заработал
  k_but = 0;
  for (int i = 0; i < 6; i++) mass_kn[i] = 0;
  answer_key = 1;
  confirm = 0; 
}

void loop() {
  level_menu(5); //Входные данные функции это, как раз то самое количество левлов, которое у нас будет 
  delay(100);
  lcd.begin(16, 2);
  lcd.clear();
  lcd.setCursor(3, 0);
  lcd.print("Уровень ");
  lcd.print(level);
  lcd.setCursor(3, 1);
  if (confirm == 0) {
    lcd.print("Не выбран");

  } 
  else if (confirm == 1) {
     lcd.print("Выбран");
     delay(500);
     lcd.clear();
     lcd.setCursor(7, 0);
     lcd.print("3 ");
     delay(1000);
     lcd.clear();
     lcd.setCursor(7, 0);
     lcd.print("2 ");
     delay(1000);
     lcd.clear();
     lcd.setCursor(7, 0);
     lcd.print("1 ");
     delay(1000);
     lcd.clear();
     lcd.setCursor(5, 0);
     lcd.print("Старт");
     delay(500);
     if (level == 1){
      level1(mass_level1);
     }
     if (level == 2){
      level1(mass_level2);
     }
     if (level == 3){
      level1(mass_level3);
     }
     if (level == 4){
      level1(mass_level4);
     }
     if (level == 5){
      level1(mass_level5);
     }
  //Примечание, на ввод функции level1 подаётся список правильных ответов под названием mass_level1
}
}

