Przeprowadzanie kompresji danych. Wykonywanie kopii zapasowej i odzyskiwanie danych. Harmonogramowanie operacji
===============================================================================================================

> Uwaga: przy wykonaniu zdań mogą powodować nieodwracalne zmiany w systemie, polecam wykonanie kopii zapasowej dysku wirtualnego (plik `*.vdi` w VirtualBox).

## Zadania:

1. Utwórz nowe konto `user`. Korzystając z materiałów na wykładzie zainstaluj narzędzie `quota`. Ustaw w pliku `/etc/fstab` niezbędne opcje, które umożliwią uruchomienie `quota` dla partycji systemowej (partycji *root*, zamontowanej w `/`). Uruchom ponownie system (maszynę wirtualną) i włącz program `quota` (`quotacheck`, `quotaon`).
   -  sprawdź aktualnie ustawiony limit dla użytkownika `user`, sprawdź ile limitu aktualnie wykorzystuje.
   -  ustaw dla użytkownika `user` limit twardy 30 MB oraz limit miękki 20 MB.
   -  używając polecenia `base64 /dev/urandom | head -c 1M > file.txt` utwórz kilka plików (`1M` oznacza, że każdy z nich ma rozmiar 1 MB) tak, aby przekroczyć limit miękki. Sprawdź co się stanie, gdy `user` przekroczy limit twardy.
   -  analogicznie sprawdź limit maksymalnej liczby plików (`inodes`).

2. Utwórz nowe konta: `quotauser1` i `quotauser2` w grupie `qoutagroup`. Powtórz ćwiczenia z zadania 1 dla grupy `quotagroup`. Sprawdź jak zachowa się system dla użytkownika `quotauser2`, gdy `quotauser1` wykorzysta limit grupy.

3. W katalogu domowym twojego użytkownika utwórz katalog `compress`. Wewnątrz, utwórz 4 pliki tekstowe o rozmiarze 1 MB (używając polecenia z zadania 1). Utwórz archiwum zawierające wszystkie utworzone pliki wykorzystując `tar`. Jaki jest rozmiar archiwum? Dlaczego?

4. Pobierz dowolny tekst z Internetu i zapisz go do pliku txt. (może to być np. artykuł z Wikipedii). Skompresuj plik za pomocą `bzip2` oraz `gzip`. Uwaga: zwróć uwagę, by kompresja nie spowodowała usunięcia pliku. Jakich poleceń użyjesz? Pokaż rozmiary oryginalne pliku oraz rozmiary po kompresji (możesz użyć polecenia `ls` z flagą `-lha`).

5. Zarchiwizuj całą strukturę katalogu domowego twojego użytkownika. Używając polecenia `tar` przeprowadź archiwizację używając kolejno algorytmu `bzip2`, `gzip` i `zip` z najwyższym oraz najniższym stopniem kompresji. Wyświetl rozmiary uzyskanych archiwów (polecenie `ls -lha`) oraz podaj wykorzystane komendy.

6. W katalogu domowym utwórz używając `tar` archiwum z wszystkich plików z rozszerzeniem `.conf`, które znajdują się w katalogu `/etc/systemd`. Rozpakuj to archiwum w katalogu domowym. Sprawdź kto jest właścicielem rozpakowanych plików, porównaj to z uprawnieniami i właścicielem oryginalnych plików. Rozpakuj utworzone archiwum w taki sposób, aby zachować tego samego właściciela pliku.

7. Z powyższego archiwum wypakuj tylko jeden wybrany przez siebie plik. Podaj użytą komendę.

8. Zainstaluj program `at` (`apt-get install at`).
   - Korzystając z polecenia `at` zaplanuj utworzenie pustego pliku `created_by_at` w katalogu `~/at_testing`.
   - Zaplanuj w ten sposób dodatkowe 2 zadania (podając inne nazwy plików).
   - Sprawdź kolejkę zaplanowanych zadań. Usuń jedno z nich i sprawdź kolejkę ponownie, czy zostało ono usunięte.
   - Zaplanuj utworzenie pliku `dobranocka.txt` na najbliższy wtorek o 19.05 (7.05 PM).
   - Zaplanuj zadanie, które zostanie wykonane za 10 minut od chwili, w której wydasz polecenie jego wykonania.

9. Używając `cron` stwórz prywatny harmonogram, który codziennie o północy wykona kopię zapasową folderu `~/Documents` w folderze `~/backup`, wykorzystując polecenie `rsync`. Zaplanuj drugą akcję, która co godzinę doda do pliku `"~/test.txt"` tekst "Wykonano!". Pokaż wyjście wykonania `crontab -l`. Jeśli wywołanie obu zadań zapiszesz w postaci skryptów, pokaż ich zawartość.

10. Używając katalogów systemowych (`/etc/cron.[...]`) zaplanuj jako administrator zadanie, które co godzinę wykona kopię zapasową folderu `/etc/systemd/` używając `tar -r` i zapisze plik w katalogu `/backup`. Pokaż zawartość zmodyfikowanego pliku.


## Literatura:
 * **`man`**: 
   *  `quota`, `edquota`, `setquota`, `repquota`, `warnquota`, `quotatool`,
   *  `gzip`, `gunzip`,
   *  `bzip2`, `bunzip2`,
   *  `zip`, `unzip`,
   *  `tar`,
   *  `cpio`,
   *  `dump`, `restore`,
   *  `rsync`,
   *  `sleep`, `at`, `batch`, `atq`, `atrm`,
   *  `cron`, `crontab`, `anacron`, `systemctl`
 * slajdy z wykładu nr 5
 * [The Linux Command Line](http://linuxcommand.org/) - [link do książki](https://sourceforge.net/projects/linuxcommand/files/TLCL/19.01/TLCL-19.01.pdf/download):
   *  rozdział 18 - archiwizacja i kopie zapasowe
 *  quota: [link1](https://margib.blogspot.com/2012/10/quota-zarzadzaniesystemem-plikow-i.html) [link2](https://linuxhint.com/disk_quota_ubuntu/)
 *  [cron](https://www.computerhope.com/unix/ucrontab.htm)
 *  [`cron` vs `anacron`](https://www.tecmint.com/cron-vs-anacron-schedule-jobs-using-anacron-on-linux/)
 *  [przykłady - `cron`](https://www.cyberciti.biz/faq/how-do-i-add-jobs-to-cron-under-linux-or-unix-oses/)
