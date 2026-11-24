# Temat projektu: Kryzys pamięciowy, czyli konfiguracja na pokładzie rakiety
Ten projekt jest modułem przechowywania zmiennych konfiguracyjnych w pamięci Flash mikrokontrolera ESP32. Pozwala na zapisywanie, odczytywanie, modyfikowanie i usuwanie zmiennych. Moduł obsługuje się poprzez interfejs CLI. 

## Interfejs CLI
Po uruchomieniu użytkownik zobaczy znak zachęty `space>`, dostępne komendy:

| Komenda | Składnia | Opis |
| :--- | :--- | :--- |
| `set` | `set <nazwa> <wartosc>` | Zapisuje nową zmienną lub aktualizuje istniejącą |
| `get` | `get <nazwa>` | Odczytuje i wyświetla wartość podanej zmiennej |
| `del` | `del <nazwa>` | Usuwa podaną zmienną |
| `list` | `list` | Wypisuje wszystkie zapisane zmienne wraz z ich typem i wartością |
| `help` | `help` | Wyświetla listę dostępnych komend i ich opis |


## Struktura projektu
```text
Zadanie_1/
├── CMakeLists.txt          # Główny plik konfiguracyjny projektu ESP-IDF
├── diagram.json            # Schemat połączeń płytki ESP32 dla symulatora Wokwi
├── wokwi.toml              # Konfiguracja pliku binarnego dla symulatora Wokwi
├── README.md               # Dokumentacja projektu
└── main/
    ├── CMakeLists.txt      # Rejestracja komponentu głównego
    └── Zadanie_1.c         # Implementacja logiki NVS, inicjalizacji domyślnej oraz komend CLI
```
## Jak uruchomić

### Wymagania
* Zainstalowany framework ESP-IDF oraz narzędzie idf.py
* (Opcjonalnie) Visual Studio Code z rozszerzeniem Wokwi Simulator

#### Opcja 1 - Symulacja
1. Otwórz folder projektu w VS Code i zbuduj program w terminalu ESP-IDF:
```bash
idf.py set-target esp32
idf.py build
```
2. Wciśnij F1 i wybierz Wokwi: Start Simulator

#### Opcja 2 - Fizyczna płytka ESP32
1. Podłącz płytkę ESP32 przez USB i sprawdź numer portu szeregowego
2. Zbuduj projekt, wgraj go do pamięci Flash i uruchom monitor portu szeregowego:
```bash
idf.py set-target esp32
idf.py -p <PORT> flash monitor
```

## Dodatkowe informacje
* Program sam rozpoznaje typ podawanych zmiennych (`bool`, `int`, `float`, `string`).
* Ze względów na ograniczenia pamięci Flash zastosowałem partycję NVS w celu wyrównania zużycia sektorów pamięci oraz uniknięcia kasowania całych sektorów przy każdej zmianie wartości. Ponadto przed zapisywaniem zmiennych sprawdzam czy podawana zmienna nie jest już zapisana w pamięci z taką samą wartością, aby ograniczyć niepotrzebne zapisy do pamięci.
* W przypadku braku zapisanej konfiguracji program wczytuje wartości domyślne.