Projekt zaliczeniowy
--------------------

Wykonaj poniższe zadania oraz przygotuj raport zgodnie z poleceniami. Zapisz raport w formacie PDF i prześlij przez serwer Discord. **Nieprzekraczalny termin przesłania rozwiązania**: 14.06.2020 r. godzina 24:00. Raporty przesłane po tym terminie nie będą uznawane.

Zachowaj zmiany w maszynach wirtualnych - będą one sprawdzane podczas indywidualnych prezentacji projektów na zajęciach 15.06.2020 r. (max 5 minut prezentacji + 2 pytania).

## Wymagania wstępne:

   Projekt wymaga wykorzystania Twojej laboratoryjnej maszyny z Debianem (z GUI) oraz dwóch maszyn do routingu (VM1 oraz VM2). Poniżej przedstawiona jest wymagana konfiguracja:

   * Twoja maszyna:

     - wybierz adapter sieciowy NAT, aby maszyna miała dostęp do Internetu

     - firewall powinien być wyłączony

     > W przypadku problemów z Twoją maszyną możesz skorzystać z [przygotowanego obrazu systemu Debian](https://drive.google.com/file/d/1OrX9p3IeBHA9xLjKyeCJnvOacWzhkjvy/view?usp=sharing) (~2,7 GB). Aby zainstalować wybierz w oknie VirtualBox `Plik` -> `Importuj urządzenie wirtualne`, następnie wybierz pobrany plik `Debian_ASL.ova`, kliknij `Dalej` i następnie `Importuj`. Login `debian`, hasło `password`, konto `debian` może korzystać z `sudo`. W maszynie może być konieczne dopasowanie skalowania (`Widok` -> `Ekran X` -> wybierz odpowiednią wartość).

   * maszyny z laboratorium z routingu (login `debian`, hasło `password`, przejście na konto roota `sudo -i`):

     - Skasuj konfigurację maszyn używając skryptu `cook-vms`.

     - Zaktualizuj konfigurację maszyny VM2 aby uzyskała ona dostęp do Internetu:

       ```
       vboxmanage modifyvm vm2 --nic3 nat
       ```

     - Uruchom maszyny używając skryptu start-vms.

     - Udostępnij Internet przez maszynę VM2: na maszynie VM2 zidentyfikuj kartę sieciową podłączoną do Internetu, a następnie skonfiguruj opcję translacji adresów (*masquerading*) w usłudze `iptables` (zastąp kartę `enp0s9` nazwą twojej karty sieciowej, karta sieciowa zwykle ma nazwę `enp0s9` lub `eth2`):

       ```console
       $ sudo -i
       # ip ad
       # iptables -t nat -A POSTROUTING -o enp0s9 -j MASQUERADE
       ```

       Na maszynie VM1 powinno poprawnie działać polecenie:

       ```console
       $ ping 8.8.8.8
       ```

     - Na maszynie VM1 skonfiguruj usługę DNS:

       ```console
       $ echo "nameserver 8.8.8.8" | sudo tee /etc/resolv.conf 
       ```

       Wówczas, na maszynie VM1 powinno działać polecenie:

       ```console
       $ ping dns.google.com
       ```

## Zadania **(20 + 2 pkt.)**

> zadania z gwiazdką (`*`) to zadania dodatkowe (max 2 pkt.)

1. ***Skrypt tworzenia użytkowników*** (dowolna maszyna) **(5 pkt.)**

   Napisz skrypt, który jako argument przyjmuje ścieżkę do pliku tekstowego i tworzy konta użytkowników na podstawie jej treści. Plik tekstowy powinien mieć następującą postać:

   ```
   username,password,groupname
   <username_1>,<password_1>,<groupname_1>
   ...
   <username_n>,<password_n>,<groupname_n>
   ```

   Uwzględnij fakt, że grupa może nie istnieć dla danego wiersza. W tym przypadku sprawdź, czy grupa istnieje i stwórz ją w razie potrzeby. **(2 pkt.)**

   Dodatkowo rozszerz skrypt o następujące funkcjonalności:

   * utworzy użytkownika `administrator` **(0,5 pkt.)**
   * dla każdej grupy utworzy odpowiedni folder o tej samej nazwie (co grupa) w folderze `/Materials`; zawartość takiego folderu może być modyfikowana tylko przez użytkownika `administrator`, natomiast pozostali użytkownicy mogą tylko odczytywać zawartość tych folderów **(2 pkt.)**
   * wyświetli po wykonaniu liczbę utworzonych użytkowników **(0,5 pkt.)**
   
   Wywołaj skrypt dla pliku tekstowego przesłanego na indywidualnym kanale na Discordzie. W raporcie umieść:
   * wywołanie `cat /etc/groups` oraz `cat /etc/passwd`
   * kod przygotowanego skryptu
   * rezultat wywołania `ls -lha /Materials`


2. ***Harmonogram kopii zapasowej*** (dowolna maszyna) **(4 + 1 pkt.)**
   
   Utwórz folder `/home/Projects`. Stwórz w nim następujące katalogi (zastąp wyrażenia `<>` odpowiednio Twoim numerem indeksu, imieniem oraz nazwiskiem), a w nich umieść wymienione pliki o dowolnej zawartości:

   ```
   .
   |
   project_<nr indeksu>
   │   |   README.md
   │   |   script.sh    
   │
   data_<imie>
   │   |   example1.txt
   │   |   example2.txt
   │   
   config_<nazwisko>
       |   db_config.ini
       |   sys_config.ini
   ```

   Stwórz prywatny harmonogram, który co 4 godziny będzie wykonywał kopię zapasową (`rsync` lub `tar -r`) folderu `/home/Projects` do folderu `/backups`. **(2 pkt.)**

   Dodatkowo:
   * ustaw, by harmonogram wykonywał się tylko od 8:00 do 22:00 **(1 pkt.)**
   * ustaw, by harmonogram wykonywał się tylko
   w piątki, soboty i niedziele **(1 pkt.)**
   * (`*`) każda kopia zapasowa powinna być nazwana według formatu `<dzień>-<miesiąc>-<rok>-<godzina>-<minuta>`. **(1 pkt.)**

   W raporcie umieść:
   * wyjście wykonania `crontab -l`
   * jeśli wywołanie synchronizacji zapiszesz w postaci skryptu, pokaż również zawartość tego skryptu

3. ***Serwer plików*** (maszyny z laboratorium z routingu) **(4 pkt.)**

   Skonfiguruj dysk sieciowy NFS w architekturze klient-serwer. Podłącz dysk na kliencie do katalogu `/media/myexternal_nfs`. Wstaw dowolny plik do tego katalogu po stronie serwera. **(2 pkt.)**

   Skonfiguruj serwer i klienta FTP, aby klient mógł wstawić plik na serwer oraz pobrać z niego dowolny plik. Wgraj na serwer dowolny plik tekstowy z klienta oraz pobierz z serwera dowolny inny plik tekstowy. **(2 pkt.)**

   W raporcie umieść:
   * wywołanie komend instalacji potrzebnych zależności do działania NFS
   * komendę montującą dysk NFS lub rekord w plik `/etc/fstab`, rezultat wywołania komend `ls /media/myexternal_nfs` na kliencie oraz `cat /etc/exports` na serwerze
   * wywołanie komend instalacji potrzebnych zależności do działania serwera FTP
   * zawartość plików konfiguracji serwera FTP
   * komendy, które spowodowały pobranie i wgranie dowolnego pliku z serwera i na serwer

4. ***Serwer WWW*** (maszyna z GUI) **(4 + 0,5 pkt.)**

   Skonfiguruj:
   * serwer WWW **(2 pkt.)**
   * WordPressa **(2 pkt.)**
   * (`*`) dowolny inny serwis internetowy (np. *ownCloud*, *Joomla*) **(0,5 pkt.)**

   W raporcie umieść:
   * zawartość plików konfiguracji serwera WWW
   * zrzut ekranu założonej strony WordPressa, gdzie na głównej stronie widoczny jest Twój numer indeksu
   * (`*`) zrzut ekranu innego serwisu internetowego
   * zawartość folderu `/var/www`

5. ***Połączenie SSH*** (maszyny z laboratorium z routingu) **(3 + 0,5 pkt.)**

   * zainstaluj zależności na obu maszynach **(1 pkt.)**
   * skonfiguruj serwer by możliwe było tylko logowanie z użyciem klucza SSH, zablokuj logowanie na konto `root` oraz dodaj zabezpieczony hasłem klucz logowania na konto `debian` **(2 pkt.)**
   * (`*`) dodaj własny baner powitalny wyświetlany przy logowaniu przez SSH; powinien on zawierać Twoje imię i nazwisko oraz numer indeksu **(0,5 pkt.)**

   W raporcie umieść:
   * komendy instalujące potrzebne zależności
   * plik konfiguracyjny serwera ssh
   * połączenie przez ssh z drugiej maszyny wirtualnej (z flagą `-vvv`)
   * (`*`) wyświetlany baner
