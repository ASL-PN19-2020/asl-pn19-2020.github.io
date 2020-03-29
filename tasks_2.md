Zarządzanie kontami użytkowników
================================

## Zadania:

1.  Zmień hasło obecnego użytkownika na `vdiff_p@ssw0rd`.

2. Utwórz dwóch użytkowników:
  * nazwa konta: twoje imię, hasło: `PWr2020` (użyj polecenia `useradd` oraz `passwd`)
  * nazwa konta: `developer`, hasło: `p@ssp@ss` (użyj polecenia `adduser`)
    Uwaga: uważaj, by wykorzystane hasło nie zapisało się w historii shella.
   
3.  Spróbuj przełączyć się na oba konta użytkownika (użyj `su` albo zaloguj się ponownie).

4.  Sprawdź zawartość plików `/etc/passwd` oraz `/etc/shadow`. Jaki algorytm został użyty do zaszyfrowania hasła w pliku `/etc/shadow`?

5.  Utwórz nową grupę `asl`. Dodaj obu użytkowników z zad. 2 do tej grupy.

6.  Sprawdź, co zostało dodane do pliku `/etc/group`.

7. Usuń konto `developer` z grupy `asl`. Sprawdź jak zmienił się plik `/etc/group`.

8.  Usuń utworzone konta i grupę. Porównaj, jak zmieniły się odpowiednie pliki w folderze `/etc`.

9.  Napisz skrypt, który pobierze nazwę użytkownika i hasło (jako argument lub pytając o nie użytkownika - polecenie `read`), a następnie utworzy użytkownika o danej nazwie. Dodatkowo, powinien on utworzyć w katalogu tego użytkownika foldery o następującej strukturze:

    ```
    .
    /home/<nazwa-użytkownika>
    │
    │───Desktop
    │
    │───Downloads
    │
    │───Pictures
    │
    │───Templates
    │
    │───Documents
    │
    │───Music
    │
    │───Public
    │
    └───Videos
    ```

    Użyj skryptu do utworzenia kont jak w zad. 2.
   
10.  Zastosuj polecenie `chage` dla konta `developer`, by wymusić zmianę hasła przy następnym logowaniu. Spróbuj zalogować się na to konto, używając `su - developer` by sprawdzić, czy wymuszona została zmiana hasła.

11. Utwórz skrypt, który pozwoli na dodanie grupy i użytkowników według pliku, jak w przykładzie:

    plik `example.txt`
    ```
    pz,pass_12
    ab,ad2020@pwr
    ccc,pwr_p@ss
    ```
    
    Wywołanie funkcji dla tego pliku spowoduje utworzenie grupy `example` oraz trzech użytkowników: `pz` z hasłem `pass_12`, `ab` z hasłem `ad2020@pwr` oraz `ccc` z hasłem `pwr_p@ss`, każdy należący do grupy `example`. Dla każdego konta powinien zostać utworzony katalog domowy jak w zad. 9.
    
    Utwórz dwa pliki spełniające powyższe wytyczne: `smallgroup.txt` z dwoma użytkownikami oraz `biggroup.txt` z trzema użytkownikami i wykorzystaj je w skrypcie.


## Literatura:
 * **`man`**: `useradd`, `adduser`, `usermod`, `passwd`, `userdel`, `deluser`, `groupadd`, `addgroup`, `groupmod`, `gpasswd`, `groupdel`, `delgroup`, 
 * slajdy z wykładu nr 3
 * [Ubuntu User Management](https://help.ubuntu.com/lts/serverguide/user-management.html)
 * [LinuxTechi](https://www.linuxtechi.com/linux-commands-to-manage-local-accounts/)
 * [Guru99](https://www.guru99.com/linux-admin.html)
 * [plik `/etc/shadow`](https://www.cyberciti.biz/faq/understanding-etcshadow-file/)
 * [plik `/etc/passwd`](https://www.cyberciti.biz/faq/understanding-etcpasswd-file-format/)
