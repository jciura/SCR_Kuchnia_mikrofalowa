

# Kuchnia mikrofalowa

---

## Dane studenta

- **Imię:** Jakub
- **Nazwisko:** Ciura
- **E-mail:** jciura@student.agh.edu.pl

---

## Opis modelowanego systemu

### Opis ogólny
Modelowany system to **kuchenka mikrofalowa** – urządzenie przeznaczone do podgrzewania oraz przygotowywania posiłków.

Główne funkcje kuchenki mikrofalowej obejmują:
- Podgrzewanie i rozmrażanie żywności,
- Odmierzanie czasu gotowania,
- Monitorowanie temperatury oraz obecności drzwi zamkniętych,
- Informowanie użytkownika o stanie pracy (np. dźwiękowe powiadomienia, wyświetlacz)

### Opis dla użytkownika

Z punktu widzenia użytkownika, obsługa kuchenki mikrofalowej jest intuicyjna i składa się z kilku kroków:

1. **Umieszczenie żywności:** Użytkownik otwiera drzwi i umieszcza jedzenie na talerzu obrotowym wewnątrz komory.
2. **Zamknięcie drzwi:** Drzwi muszą być zamknięte, aby możliwe było uruchomienie mikrofal.
3. **Wybór programu:** Użytkownik wybiera czas podgrzewania i/lub moc (np. za pomocą przycisków lub pokrętła).
4. **Start:** Po naciśnięciu przycisku "Start" rozpoczyna się proces podgrzewania. System weryfikuje warunki bezpieczeństwa (np. czy drzwi są zamknięte).
5. **Podgrzewanie:** W trakcie pracy użytkownik może zatrzymać działanie (pauza) lub anulować je całkowicie.
6. **Zakończenie:** Po zakończeniu podgrzewania rozlega się sygnał dźwiękowy, a urządzenie zatrzymuje generowanie mikrofal.
---

## Spis komponentów AADL z komentarzem
   
###  Pakiet: `MicrowaveSystem`

Zawiera wszystkie komponenty opisujące system mikrofalówki.

---

###  Common_Types
Zawiera stworzone typy danych, magistrale i pamięć


| Nazwa                  | Opis                                                                 |
|------------------------|----------------------------------------------------------------------|
| `HeatPower`            | Zakres mocy grzania od 0 do 100.                                     |
| `OperationMode`        | Tryby pracy: `OFF`, `HEATING`, `DEFROST`, `GRILL`.       |
| `Status`| Status pracy; wartości: START, STOP, PAUSE. 
| `UserCommand` | Dane wejściowe: status, tryb, moc, czas.                |
| `BuzzerVar`         		 | Sterowanie sygnałem dźwiękowym.	

---

### Devices

| Nazwa         | Opis                                                                 |
|---------------|----------------------------------------------------------------------|
| `Door` | Czujnik drzwi – status drzwi i sygnał awaryjnego zatrzymania.        |
`Button` | Wejście użytkownika - czas, moc, tryb.        |
| `Heater`      | Urządzenie grzewcze – przyjmuje moc i zwraca faktyczną moc.         |
| `Screen`          | Wyświetlanie statusu. |            
| `Buzzer`       | Sygnał dźwiękowy przy zakończeniu pracy.                |                     

---


###  Wątki

| Nazwa     | Opis                                                                  |
|-----------|-----------------------------------------------------------------------|
| `tTimer`  | Zarządza czasem pracy grzania.                                       |
| `tHeater` | Ustawia i odczytuje faktyczną moc grzania.                           |
| `tBuzzer` | Odbiera od kontrolera informacje czy włączyć sygnał dźwiękowy.                        |
| `tCtl`     |Kontroler, który zarządza wszystkim.            |

---

### Procesy

| Nazwa             | Opis                                                                 |
|-------------------|----------------------------------------------------------------------|
| `Controller`      | Główny kontroler – przetwarza polecenia i zarządza komponentami.     |

---

###  Zasoby systemowe

| Nazwa    | Opis                                                                    |
|----------|-------------------------------------------------------------------------|
| `corei5` | Procesor wykonujący wątki.  |
| `RAM` | Pamięć  |
| `ethernet` | Magistrala |

---

### System

| Nazwa                  | Opis                                                                  |
|------------------------|-----------------------------------------------------------------------|
| `MicrowaveSystem`      | Główna deklaracja systemu. Implementacja systemu – komponenty, połączenia, przypisania zasobów.                                            |


## Model - rysunek
![Schemat mikrofalówki](schemat.png)

---

## Wyniki przeprowadzonych analiz
### 1. Analiza czasowa
![Analiza czasowa](analizaczasowa.png)

| Wątek| Period| Deadline| Execution Time|
|-----------|-------------------------------------------------------|------------------------------------|---------------|
| `tCtl`  |20s | 15s | 2-4 ms|
| `tHeater` | 50ms           | -             | 2-5 ms|
| `tTimer` | 1s| -| 1-2 ms|


Całkowita wymagana moc obliczeniowa nie przekracza specyfikacji procesora (3 MIPS)
    

### 2. Binding Constraints
![Analiza bindingów](bindingconstraints.png)
    

### 3. Analiza wag
![Analiza wag](wagi.png)

Wagi urządzeń nie przekraczają limitu 10 kg

### 4. Not Bound Resource Budget
![Analiza not bound](notbound.png)


### 5. Bound Resource Budget
![Analiza bound](bound.png)

### 6. Weryfikacja bezpieczeństwa
    
-   Mechanizm zatrzymania przy otwartych drzwiach
    
-   Separacja wątków sterujących

---

## Wnioski

1. Model poprawnie odwzorowuje działanie kuchenki mikrofalowej
        
2. System spełnia założenia dotyczące poboru mocy i wagi
    
3. Architektura zapewnia odpowiedni poziom bezpieczeństwa
   

