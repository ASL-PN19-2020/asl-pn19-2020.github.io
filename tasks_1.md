Konsola tekstowa: komendy i skrypty
===================================

## Zadania:

1. Napisz skrypt, który sprawdzi bieżącą godzinę (polecenie `date`) i, jeżeli wypada ona pomiędzy 15:00 a 17:00 wyświetli komunikat `"Tea time!"`.

2. Utwórz plik tekstowy zawierający 5 powitań w kolejnych linijkach. Następnie napisz skrypt, który wyświetli losowe powitanie z pliku tekstowego. Wykorzystaj polecenie `shuf` oraz `head` lub `tail`.

3. Utwórz skrypt, który utworzy w danym folderze następującą strukturę katalogów, wraz z pustymi plikami:
```
.
project
│   README.md
│   script.sh    
│
└───data
│   │   example1.txt
│   │   example2.txt
│   │
│   └───labels
│       │   labels_big.txt
│       │   labels_small.txt
│   
└───config
    │   db_config.ini
    │   sys_config.ini
```

4. Przgotuj skrypt, którego zadaniem będzie stworzenie katalogów według listy w pliku - np. w postaci:
```
katalog
folder
biblioteka
magazyn
nic
nieotwierac
```
Wykorzystaj dwie metody: przez przekierowanie STDOUT `cat` oraz podając nazwę pliku jako argument skryptu. 

5. Zmodyfikuj powyższy skrypt tak, aby w każdym pliku tworzył zadaną liczbę pustych plików o nazwie `text_n.txt` (`n` - identyfikator pliku, np. `text_1.txt`, `text_2.txt`). Liczba plików ma być podawana jako argument skryptu.

6. Napisz skrypt, który zapyta użytkownika o ścieżkę, a następnie sprawdzi, czy ścieżka prowadzi do katalogu, do zwykłego pliku czy do innego rodzaju pliku i wyświetli stosowny komunikat. Wyświetl informacje nt. obiektu pod ścieżką używając długiego listingu, a jeżeli jest to plik - polecenia `file`.

7. Napisz skrypt, który przejdzie do zadanego katalogu (podanego jako pierwszy argument), a następnie zamieni wszystkie rozszerzenie plików w tym katalogu o zadanym rozszerzeniu (drugi argument) na inne (trzeci argument). Np. wywołanie `./change_ext ~/Documents .txt .md` zamieni wszystkie pliki `.txt` w katalogu `~/Documents` na `.md`.



## Literatura:
  *  **The Linux Command**: [Rozdział 1 - polecenia w Bashu](http://linuxcommand.org/lc3_learning_the_shell.php)
     Pierwsze 6 podrozdziałów zawierają w większości polecenia przerobione w ramach poprzednich zajęć. Proszę zapoznać się z rozdziałami:
     *  [7. (przekierowanie I/O)](http://linuxcommand.org/lc3_lts0070.php),
     *  [8. (ekspansja zmiennych)](http://linuxcommand.org/lc3_lts0080.php),
     *  [9. (uprawnienia)](http://linuxcommand.org/lc3_lts0090.php),
     *  [10. (kontrola zadań)](http://linuxcommand.org/lc3_lts0100.php).
     
     Większość poleceń została zgromadzona we [Wprowadzeniu do Basha](3_bash_intro.md)
  
     **The Linux Command**: [Rozdział 4 - skrypty w Bashu](http://linuxcommand.org/lc3_writing_shell_scripts.php)
     *  [1. Tworzenie skryptów](http://linuxcommand.org/lc3_wss0010.php)
     *  [2. Środowisko. Funkcje](http://linuxcommand.org/lc3_wss0020.php)
     *  [3. Przykład skryptu w Bashu](http://linuxcommand.org/lc3_wss0030.php)
     *  [4. Zmienne](http://linuxcommand.org/lc3_wss0040.php)
     *  [5. Przypisywanie wyniku komend do zmiennej. Stałe](http://linuxcommand.org/lc3_wss0050.php)
     *  [8. Instrukcje warunkowe](http://linuxcommand.org/lc3_wss0080.php)
     *  [9. Jak debugować Basha?](http://linuxcommand.org/lc3_wss0090.php)
     *  [10. Wczytywanie danych z klawiatury](http://linuxcommand.org/lc3_wss0100.php)
     *  [11. `case`, pętle `while` i `until`](http://linuxcommand.org/lc3_wss0110.php)
     *  [12. Argumenty pozycyjne](http://linuxcommand.org/lc3_wss0120.php)
     *  [13. Pętla `for`](http://linuxcommand.org/lc3_wss0130.php)
     *  [14. Błędy i sygnały 1](http://linuxcommand.org/lc3_wss0140.php)
     *  [15. Błędy i sygnały 2](http://linuxcommand.org/lc3_wss0150.php)
 
 *  [Shell Scripting Tutorial](https://www.shellscript.sh/)
 *  [Bash Scritping Cheatsheet](https://devhints.io/bash)
