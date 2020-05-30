Konfigurowanie i uruchamianie maszyn wirtualnych w systemach Linux. Zabezpieczanie serwera. Zdalne administrowanie systemem
============================================================

## Wymagania wstępne

1. Wykorzystaj maszyny VM1 I VM2 z laboratorium routingu (login i hasło: *debian*/*password*, przejście na konto root: `sudo -i`).

2. Skasuj konfigurację maszyn używając skryptu `cook-vms`.

3. Zaktualizuj konfigurację maszyny VM2 aby uzyskała ona dostęp do Internetu:

   ```
   vboxmanage modifyvm vm2 --nic3 nat
   ```

4. Uruchom maszyny używając skryptu start-vms.

5. Udostępnij Internet przez maszynę VM2: 
na maszynie VM2 zidentyfikuj kartę sieciową podłączoną do Internetu, a następnie skonfiguruj opcję translacji adresów (*masquerading*) w usłudze iptables (zastąp kartę `enp0s9` nazwą twojej karty sieciowej, karta sieciowa zwykle ma nazwę `enp0s9` lub `eth2`):

   ```console
   $ sudo -i
   # ip ad
   # iptables -t nat -A POSTROUTING -o enp0s9 -j MASQUERADE
   ```

   Na maszynie VM1 powinno poprawnie działać polecenie:

   ```console
   $ ping 8.8.8.8
   ```

6. Na maszynie VM1 skonfiguruj usługę DNS:

   ```console
   $ echo "nameserver 8.8.8.8" | sudo tee /etc/resolv.conf 
   ```

   Wówczas, na maszynie VM1 powinno działać polecenie:

   ```console
   $ ping dns.google.com
   ```

## Zadania

> **SSH** (**Secure Shell**) jest protokołem sieciowym używanym do zdalnego zarządzania maszynami Architektura SSH obejmuje serwer SSH oraz klienta SSH.

1. **Instalacja usługi *OpenSSH Server***

   a. Upewnij się że maszyny VM1 i VM2 są włączone i mają dostęp do Internetu. Maszyna VM2 będzie pełnić funkcję serwera, zaś VM1 – klienta.

   b. Na maszynie VM2, zainstaluj serwer SSH:

      ```console
      # apt-get update
      # apt-get install openssh-server
      ```

      Sprawdź czy instalacja się udała wywołując
      
      ```console
      # systemctl status sshd
      ```

   c. Domyślnie serwer SSH działa na porcie 22. To jest domyślny port przypisany usłudze SSH . Sprawdź stan portu 22 używając polecenia netstat:

      ```console
      $ netstat -tulpn | grep 22
      ```

      **Zamieść zrzut ekranu z wykonanego polecenia w raporcie.**
      
   d. Otwórz port SSH w firewallu - jeśli masz włączony i domyślnie skonfigurowany firewall UFW musisz zezwolić na zdalne połączenia do twojego hosta. Używając poniższych poleceń zainstalujesz i włączysz firewall, a następnie zezwolisz na przychodzące połączenia SSH:

      ```console
      # apt-get update
      # apt-get install ufw
      # ufw enable
      # ufw allow ssh
      ```

   e. Serwer SSH został uruchomiony jako usługa na twoim hoście. Najprawdopodobniej usługa została skonfigurowana tak aby uruchamiała się przy starcie systemu. Aby upewnić się czy tak się rzeczywiście dzieje uruchom polecenie:

      ```console
      # systemctl list-unit-files | grep enabled | grep ssh
      ```

      Jeśli nie zobaczysz żadnej informacji to znaczy że serwer SSH nie jest uruchamiany przy starcie. Włącz automatyczne uruchamianie serwera SSH poniższym poleceniem i przetestuj ponownie czy serwer uruchamia się automatycznie:

      ```console
      # systemctl enable ssh
      ```

2. **Konfiguracja serwera SSH**

   a. Pierwszym krokie do zabezpieczenia serwera SSH jest zmiana domyślnego portu. Zmodyfikuj plik `/etc/ssh/sshd_config` usuwając komentarz z linii `#Port 22`, a następnie zmień numer portu na `2222` (ten port nie jest przypisany do innych protokołów).

   b. Zmień ustawienia firewalla tak, aby przyjmował połączenia z podsieci `10.1.2.0/24`. W przykładzie użyto portu `2222`:

      ```console
      # ufw allow from 10.1.2.0/24 to any port 2222
      ```

   c. Wyłącz możliwość logowania się na konto root przez serwer SSH: Zmień plik konfiguracyjny `/etc/ssh/sshd_config`. Znajdź linię `#PermitRootLogin prohibit-password`, usuń znak komentarza i zmień ustawienie opcji na "no": `PermitRootLogin no`.

   d. Zrestartuj usługę `sshd` poleceniem `systemctl restart` i sprawdź jej status poleceniem `systemctl status`. **Zamieść zrzut ekranu z wykonania tych poleceń w raporcie.**

   e. Sprawdź czy możesz połączyć się z maszyną VM2 wywołując na maszynie VM1:

      ```console
      $ ssh -p 2222 debian@10.1.2.102
      ```

      **Zamieść zrzut ekranu z udanego połączenia w raporcie.** Opuść sesję wpisując `exit`.

3. **Zdalny dostęp przy użyciu protokołu SSH**

   a. Na maszynie VM2 zainstaluj aplikacje, które będą uruchamiane zdalnie:

      * Zainstaluj aplikacje okienkowe X11 (`xauth` jest wymagany do zdalnego uruchamiania aplikacji)

        ```console
        # apt update
        # apt install -y xauth x11-apps midori
        ```
      
      * Znajdź i zmodyfikuj następujące linie w pliku `/etc/ssh/sshd_config` aby możliwe było przekazywanie X11:

        ```
        X11Forwarding yes
        X11DisplayOffset 10
        X11UseLocalhost no
        ```
      * Zrestartuj usługę `sshd` i sprawdź jej stan. Jeśli zmieniłeś port na którym działa usługa sshd upewnij się, że działa ona poprawnie (patrz: pkt. 1c)

   b. Na maszynie VM1 zainstaluj środowisko okienkowe X11:

      * Znajdź i zmodyfikuj następujące linie w pliku `/etc/ssh/ssh_config` aby pozwolić na przekazywanie X11

        ```
        ForwardX11 yes
        ForwardX11Trusted yes
        ```

      * Zainstaluj minimalne środowisko graficzne:

        ```console
        # apt update
        # apt install -y xserver-xorg-core xorg xdm openbox
        $ startx
        ```

        **Uwaga**: Jeśli maszyna nie chce aktualizować pakietów przez `apt update`, wyłącz tymczasowo firewall na maszynie VM2 (`ufw disable`) i włącz go po zakończeniu całej instalacji (`ufw enable`).
   
   c. W środowisku graficznym openbox na VM1 kliknij prawym klawiszem myszy i uruchom z menu opcję *Terminal emulator*, a następnie podłącz się do maszyny VM2:

      ```console
      $ ssh -Xp 2222 debian@10.1.2.102
      ```
   
   d. Uruchom trzy proste aplikacje na maszynie VM2. Aplikacje działają na maszynie VM2, ale są wyświetlane na maszynie VM1:

      ```console
      $ xclock &
      $ xeyes &
      $ midori &
      ```

      **Wykonaj zrzut ekranu maszyny VM1 pokazujący uruchomione aplikacje oraz konsolę.**

   e. Wykorzystaj `scp` do skopiowania plików pomiędzy maszynami. Polecenie `scp` działa tak jak `cp`, ale pozwala na kopiowanie plików między maszynami dzięki użyciu protokołu ssh. Skopiuj plik `/etc/hosts` ze zdalnej maszyny VM2 do domowego katalogu użytkownika na maszynie VM1:

      ```console
      debian@vm1:~$ scp -P 2222 debian@10.1.2.102:/etc/hosts /home/debian
      ```

      **Wykonaj zrzut ekranu pomyślnego skopiowania pliku oraz polecenia `ls` wykonanego w katalogu `/home/debian` maszyny VM1.**

   f. Skopiuj plik `hosts` z maszyny VM1 na maszynę VM2:

      ```console
      debian@vm1:~$ scp -P 2222 /home/debian/hosts debian@10.1.2.102:/home/debian
      ```

      **Wykonaj zrzut ekranu pomyślnego skopiowania pliku. Zaloguj się przez ssh do maszyny VM2 i wykonaj zrzut ekranu polecenia `ls` wykonanego w katalogu `home/debian`.**

4. **Uwierzytelnianie przy pomocy klucza. Dostęp przez SSH bez wpisywania hasła**

   a. Aby skonfigurować zdalny dostęp bez hasła, z użyciem pary kluczy (prywatny/publiczny) wygeneruj na maszynie VM1 parę kluczy poleceniem `ssh-keygen`. Dla uproszczenia nie podawaj hasła chroniącego klucz prywatny.

      ```console
      $ ssh-keygen -t dsa
      ```

      Sprawdź argumenty polecenia `ssh-keygen` i wywołaj je tak, aby uzyskać klucz szyfrowany algorytmem RSA o długości 4096 bitów. **Zamieść zrzut ekranu z wykonanego polecenia w raporcie.**

      > Polecenie `ssh-keygen` generuje parę kluczy oraz umieszcza je w katalogu `~/.ssh`. Jeśli podkatalog `~/.ssh` nie istnieje, to `ssh-keygen` tworzy go z odpowiednimi uprawnieniami. Jeśli zakładasz katalog `~/.ssh` samodzielnie, musisz zmienić uprawnienia na `700`! W innym przypadku aplikacja `ssh` odmówi użycia kluczy uznając, że klucze dostępne dla kogokolwiek poza właścicielem nie są bezpieczne!

      > Polecenie `ssh-keygen` tworzy dwa klucze w katalogu `.ssh`. Dla RSA plik z kluczem publicznym nazywa się domyślnie `id_rsa.pub`, a prywatnym: `id_rsa`. W przypadku algorytmu DSA nazwy to odpowiednio `id_dsa.pub` oraz `id_dsa`.

      Usuń z katalogu `.ssh` klucze `id_dsa` oraz `id_dsa.pub`.

      **Zamieść w raporcie zrzut ekranu zawartości pliku z kluczem publicznym.**

   b. Skopiuj klucz publiczny z maszyny VM1 do VM2, używając polecenia `ssh-copy-id`:

      ```console
      $ ssh-copy-id -p 2222 debian@10.1.2.102
      ```

      > Klucze publiczne użytkowników logujących się bez hasła są umieszczane w pliku `~/.ssh/authorized_keys`. Zaufani użytkownicy mogą logować się na to konto przez SSH używając swoich kluczy prywatnych, bez podawania hasła.

   c. Wyświetl zawartość pliku `authorized_keys` na maszynie VM2. **Zamieść zrzut ekranu tego pliku w raporcie.**

   d. Użytkownik `debian` na maszynie VM1 może się teraz połączyć przez ssh z maszyną VM2 bez podawania hasła. Dzięki możliwości wykonywania poleceń przez SSH na zdalnej maszynie, użytkownik może uruchamiać potoki poleceń na wielu różnych maszynach. Sprawdź to wywołując na maszynie VM1 polecenie:

      ```console
      $ ssh -p 2222 debian@10.1.2.102
      $ hostname
      ```

      **Zamieść zrzut ekranu z udanego połączenia w raporcie.**

   e. Sprawdź czy uprawnienia do plików z kluczami serwera są poprawne: wszyscy powinni móc odczytać klucze publiczne, natomiast tylko administrator (root) może mieć dostęp do kluczy prywatnych:

      ```console
      $ ls -l /etc/ssh/ssh_host_*
      ```

   f. Sprawdź czy na maszynie działa ssh-agent:

      ```console
      $ ps fax | grep ssh-agent
      ```

5. **Klucz zabezpieczony hasłem**
   
   > Wykonaj poniższe zadania posiłkując się manualem, notatkami z wykładu oraz źródłami internetowymi

   a. Utwórz na maszynie VM1 nową parę kluczy RSA 4096, tym razem zabezpieczoną hasłem (pamiętaj by podać inną nazwę klucza). **Zamieść zrzut ekranu z wykonania polecenia w raporcie.**
   
   b. Usuń z pliku `authorized_keys` stary klucz.
   
   c. Skopiuj nowy klucz na maszynę VM2 wykorzystując polecenie `ssh-copy-id` z odpowiednimi parametrami.

   d. Dodaj nowy klucz do agenta ssh (`ssh-agent`).

   e. Podłącz się do maszyny VM2 poleceniem:

      ```console
      ssh -p 2222 debian@10.12.102 -v
      ```

      Flaga `-v` pozwala na zwiększenie poziomu `verbosity`. **Zamieść w raporcie zrzut ekranu z wykonania tego polecenia (przynajmniej fragment, który będzie widoczny w oknie).** Przy poprawnej konfiguracji powinna być możliwość podłączenia do maszyny VM2 bez podawania hasła.


## Literatura:

 * slajdy z wykładu nr 13
 * materiały online dotyczące instalacji i konfiguracji serwera www oraz instalacji Wordpress
 * `man`:
   * `ssh`, `ssh-agent`, `ssh-keygen`, `ssh-copy-id`, `scp`