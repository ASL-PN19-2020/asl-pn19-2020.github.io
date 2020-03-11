## Wprowadzenie do Basha
  
1.  Pomoc systemowa
  
  *  `man [sekcja] command`
  *  `info command` <- może nie być zainstalowane
  *  `apropos slowo_klucz`
  *  `command --help` lub `command -h`
    
1.  System plików

  *  `cd sciezka_bezwzględna` (zaczyna się od `/`) lub `cd sciezka_wzgledna` (względem bieżącego katalogu) - zmiana katalogu
  *  `ls` - wyświetlanie zawartości katalogu:
    *  `ls -a` - wyświetla pliki ukryte
    *  `ls -l` - *długi listing* (dokładniejsze informacje)
    *  **filtrowanie**:
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
  
1.  Drukowanie w konsoli

  *  `cat`, `more`, `less` - wyświetlanie zawartości plików
  *  `tail` - wyświetla *ogon* pliku (końcówka)
  *  `head` - wyświetla *głowę* pliku (początek)
