## Wprowadzenie do Basha
  
1. Pomoc systemowa
   *  `man [sekcja] command`
   *  `info command` <- może nie być zainstalowane
   *  `apropos slowo_klucz`
   *  `command --help` lub `command -h`
   *  `which executable` wskazuje lokalizację danego pliku wykonywalnego
    
2. System plików
   *  `cd sciezka_bezwzględna` (zaczyna się od `/`) lub `cd sciezka_wzgledna` (względem bieżącego katalogu) - zmiana katalogu
   *  `ls` - wyświetlanie zawartości katalogu:
      *  `ls -a` - wyświetla pliki ukryte
      *  `ls -l` - *długi listing* (dokładniejsze informacje)
      *  **filtrowanie i ekspansja** [[więcej tutaj]](http://linuxcommand.org/lc3_lts0080.php):
         *  `ls nazwa` - wyświetli jeden plik `nazwa`
         *  `ls nazw?` - wyświetli wszystkie pliki zawierające w nazwie `nazw` oraz jeden znak
         *  `ls naz*` - wyświetli wszystkie pliki o nazwie zaczynającej się od `naz`
   *  `touch plik` - tworzy pusty plik
   *  `cp` - kopiowanie (`cp -r` do kopii katalogu)
   *  `mv` - przenoszenie / zmiana nazwy
   *  `rm` - usuwanie pliku (`rm -r` do usuwania folderu)
   *  `mkdir` - tworzenie katalogu
   *  `rmdir` - usuwanie pustego katalogu
   *  `stat` - informacje o statusie pliku
   *  `file` - informacja o typie pliku
  
3. Drukowanie w konsoli oraz filtry
   *  `cat`, `more`, `less` - wyświetlanie zawartości plików
   *  `tail` - wyświetla *ogon* pliku (końcówka)
   *  `head` - wyświetla *głowę* pliku (początek)
   *  `sort` - sortuje wejście
   *  `uniq` - usuwa duplikaty
   *  `grep` - szuka wzorców (sekwencji znaków) w wejściu
   *  `fmt` - formatuje tekst
   *  `echo` - zwraca tekst jako wyjście standardowe

4. Monitorowanie i zarządzanie procesami  
   *  `ps` - lista procesów
   *  `top` - wyświetla informacje o procesach w czasie rzeczywistym
   *  `kill` - wysyła sygnał do programu (patrz manual)

5.  Media
   *  `mount` - montowanie urządzeń
   *  `umount` - odmontowanie urządzeń
   *  `df -h` - uzyskanie informacji o wolnym miejscu na dysku
   *  `du` - obszar pamięci zajęty przez katalog
   
6. Przekierowanie I/O
   *  `command > file` powoduje przekierowanie wyjścia standardowego `command_1` i nadpisanie pliku `file` (zamiast wyświetlać na ekranie)
   *  `command >> file` jw., lecz dopisuje na końcu
   *  `command < file` powoduje przekierowanie pliku do `command` jako standardowe wejście
   *  `2>`, `2>>` - wyjście standardowe dla błędów
   *  `2>&1` przekierowuje standardowe wyjście błędów na wejście standardowe
   *  `1>&2` przekierowuje wyjście standardowe na standardowe wyjście błędów
   *  `command > /dev/null` - przekierowanie wyjścia standardowego do wirtualnego urządzenia (usunięcie)
   *  `command_1 | command_2` - pipeline (przekierowanie wyjścia `command_1` jako wejście `command_2`
   *  `command_2 $(command_1)` - powoduje wywołanie polecenia `command_1` i podanie go jako argumentu polecenia `command_2`
   *  alternatywnie można użyć ``command_2 `command_1` ``
   *  do anulowania działania znaków specjalnych używamy cudzysłowia `" "`, apostrofu `' '` lub ukośnika wstecznego `\`
   
7. Uprawnienia w Linuksie [[link]](http://linuxcommand.org/lc3_lts0090.php)
   *  `chmod` - zmiana uprawnień danego pliku
   *  `su` - przejście do konta superużytkownika (*superuser*)
   *  `sudo` - wykonanie jednej komendy jako superużytkownik
   *  `chown` - zmiana własności pliku/katalogu
   *  `chgrp` - zmiana własności pliku/katalogu dla grup
   
8. Zmienne
   *  `variable=1` - zmienna lokalna
   *  `export VARIABLE=1` - zmienna globalna (widoczna w podprocesach)
   * do tworzenia zmiennych można wykorzystać przekierowanie I/O oraz metody wywoływania poleceń (`$()`, `` ` ` ``)
   * aby użyć zmiennej dodajemy znak `$` (np. `echo $var`)
   
----
