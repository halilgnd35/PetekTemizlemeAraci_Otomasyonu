# PetekTemizlemeAraci_Otomasyonu
Hali hazırda bir tuşla çalışan ve bu tuşa belirli aralıklarda belirli sayıda tekrar tekrar basmayı gerektiren kombi petek temizleme cihazına kendi yaptığım arduino nano, röle, buzzer, led gibi componentlerden oluşan bir kartın yardımıyla can veren kodlar aşağıdadır.Burada can vermekten kasıt gerekli bağlantılar yapıldıktan sonra cihazın gerekli sürede gerekli tekrarda kendi kendine çalışan yani otomatik bir tuş görevi görmesidir.İşini bitirdikten sonra ise buzzer ve led yardımıyla geri bildirim vermektedir.
```javascript
------------------------------Kodlar------------------------------
//Author:Halil Gundogdu
#define dongu 6
#define role_pin 3
#define buton_pin 5
#define buzzer_pin 7
long int count;
int buz_say;
bool dongukir=false;
bool buz=true;
void setup()
{
  pinMode(role_pin, OUTPUT);
  pinMode(buton_pin, INPUT);
  pinMode(buzzer_pin, OUTPUT);
}

void loop()
{
  if (digitalRead(buton_pin))
{
  delay(500);
  for (int i = 1; i <= dongu; i++)
    {
      buz=false;
      count = 0;
      while (1)
      {
        if (digitalRead(buton_pin))
        {
          digitalWrite(role_pin,LOW);
          dongukir=true;
          buz=true;
          delay(500);
          break;
        }
        if (count <= 20000)
          digitalWrite(role_pin, HIGH);
        else if ((count <= 120000) && (i != dongu))
          digitalWrite(role_pin, LOW);
        else
        {
          digitalWrite(role_pin, LOW);
          break;
        }
        delay(1);
        count += 1;
      }
      if(dongukir==true)
      {
        dongukir=false;
        break;
      }
    }
  }
  if(buz==false)
  {
    buz=true;
    buz_say=0;
    while(1)
    {
      buz_say+=1;
      if(buz_say<=500)
        digitalWrite(buzzer_pin,HIGH);
      else if(buz_say<=1000)
        digitalWrite(buzzer_pin,LOW);
      else
        buz_say=0;
      delay(1);
      if (digitalRead(buton_pin))
      {
        digitalWrite(buzzer_pin,LOW);
        delay(500);
        break;
      }
    }
  }
}
```
