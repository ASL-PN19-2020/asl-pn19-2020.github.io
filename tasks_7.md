Zapory sieciowe (firewall) i sterowanie ruchem sieciowym
========================================================

## Zadania:

> Zadanie 1. wykonaj wykorzystując maszyny wirtualne z poprzedniej listy (routing).

1. **`iptables`**

      > `iptables` składa się z tablic (*tables*), reguł (*chains*) oraz operacji na pakiecie (*targets*).

      > 4 typy tablic:
      > * *filter* (filtrowanie - najczęściej używane, domyślne)
      > * *mangle* (zmiana nagłówków pakietów)
      > * *nat* (zmiana źródłowych i docelowych adresów pakietów)
      > * *raw* (do analizy pakietów wynikających ze zmiany stanu firewalla

      > 5 typów reguł:
      > * *PREROUTING* (przy pojawieniu się na interfejsie - dla tablic *mangle* i *filter*)
      > * *INPUT* (przed podaniem pakietu do lokalnego procesu - dla tablic *mangle* i *filter*)
      > * *OUTPUT* (po utworzeniu pakietu przez proces - dla tablic *raw*, *mangle*, *nat* i *filter*)
      > * *FORWARD* (dla każdego pakietu przekierowanego przez hosta - dla tablic *mangle* i *filter*)
      > * *POSTROUTING* (przy opuszczaniu interfejsu przez pakiet - dla tablic *nat* i *mangle*)

      > 3 główne operacje na pakiecie:
      > * *ACCEPT* - przyjęcie pakietu
      > * *DROP* - odrzucenie pakietu (przy połączeniu z systemem wygląda to tak, jakby system nie istniał)
      > * *REJECT* - odrzucenie pakietu (przy połączeniu pojawia się komunikat *connection reset* dla TCP lub *destination host unreachable* dla UDP lub ICMP)

      ![firewall-pipeline.png](images/firewall-pipeline.png)

      Główne komendy `iptables`:

      * dodawanie reguły:

        ```console
        # iptables -A <chain> -i <interface> -p <protocol> -s <source> -dport <port> -j <target>
        ```

      * usuwanie reguły - zastąp `-A` przez `-D`

        ```console
        # iptables -D <chain> -i <interface> -p <protocol> -s <source> -dport <port> -j <target>
        ```

      * usuwanie wszystkich reguł

        ```console
        # iptables -F
        ```

      * usuwanie wszystkich reguł w chainie

        ```console
        # iptables -F <chain>
        ```

      * listowanie reguł

        ```console
        # iptables -L --line-numbers
        ```

        na tej podstawie można również usuwać reguły

        ```console
        # iptables -D <chain> <number>
        ```

      * zapisanie reguł, by były dostępne po ponownym uruchomieniu

        ```console
        # /sbin/iptables-save
        ```

      Inne reguły można znaleźć w tutorialu (sprawdź literaturę).

   a. Sprawdź status `iptables`:

      ```console
      # iptables -L -v
      ```

      > Załóżmy, że chcielibyśmy ukryć przed maszyną VM4 router VM3, tzn. uniemożliwić pingowanie jego adresu oraz połączenie z nim przez `ssh`.

   b. Na maszynie VM4 sprawdź połączenie z maszyną VM3 (`ping 10.3.4.103`) oraz spróbuj połączyć się używając [SSH](https://pl.wikipedia.org/wiki/Secure_Shell) (`ssh 10.3.4.103`).

   c. na pytanie "*Are you sure you want to continue connecting (yes/no)*" wpisz `yes`. Aby wyjść z Secure Shell należy wpisać `exit` lub wykorzystać skrót klawiszowy `Ctrl+D`.

      > Pamiętaj o tym, że w zależności od sieci poszczególne maszyny mają inne adresy.

   d. Zablokuj możliwość pingowania VM3 przez VM4; na maszynie VM3 wpisz komendę:

      ```console
      # iptables -I INPUT -p icmp -j DROP -i eth1
      ```

      > w ten sposób blokowany jest połączenie na interfejsie `eth1` (połączenie VM3 i VM4) dla protokołu `icmp` wykorzystywanego przez ping

   e. Sprawdź możliwość pingowania VM3 z maszyn VM2 i VM4 (pierwsze powinno nadal działać, drugie - nie). **Umieść w raporcie zrzut ekranu z wywołania odpowiednich poleceń**

   f. Zablokuj możliwość połączenia przez `ssh` z VM4 na VM3 (analogicznie do powyższych kroków - protokół `ssh`). **W raporcie umieść zrzuty ekranów z próby połączenia przez ssh z VM3.**

   g. Zablokuj pingowanie i ssh z maszyny VM2. **Załącz zrzuty ekranów z prób pingowania i połączenia ssh z VM2 na VM3.**

   h. Sprawdź, czy nadal możliwe jest pingowanie maszyny VM4 z VM1.

> Pozostałe wykonaj używając Twojej maszyny wirtualnej ze środowiskiem graficznym. Niezalecane jest wykonywanie ćwiczeń dotyczących firewalla w systemie, z którego korzystasz na codzień.

> Skonfiguruj sieć swojej maszyny wirtualnej by móc połączyć się z nią z poziomu hosta. W tym celu w VirtualBox dodaj sieć:
> * W głównym oknie programu wybierz *Tools*, a z menu wybierz *Network*
> * Utwórz nową sieć wybierając *Create*: 
> ![create-net.png](images/create-net.png)
> * Zmień jej ustawienia wybierając properties:
> * Ustawienia Adaptera oraz serwera DHCP ustaw jak na poniższych zrzutach ekranu. Za każdym razem kliknij *Apply*: 
> ![create-net.png](images/net-adapter.png) 
> ![create-net.png](images/net-dhcp.png)
> * W ustawieniach Twojej maszyny wirtualnej dodaj adapter w polu *Network* -> *Adapter 2*, jak na zrzucie ekranu:
> ![create-net.png](images/vm-net.png)
> * Sprawdź czy sieć dostępna jest w maszynie poleceniem `ip ad`. Zapisz jej adres (uwaga: adres może być inny niż na zrzucie ekranu).
> ![create-net.png](images/net-in-vm.png)
   

2. **`ufw`**
   
   -  Zainstaluj `ufw` poleceniem:

      ```console
      # apt-get update
      # apt-get install ufw
      ```

   Główne komendy `ufw`:

   * aktywowanie ufw:

      ```console
      # ufw enable
      ```

   * deaktywowanie ufw:

      ```console
      # ufw disable
      ```

   * ponowne załadowanie reguł ufw:

      ```console
      # ufw reload
      ```

   * status ufw:

      ```console
      # ufw status verbose
      ```

   * usuwanie reguł ufw:

      ```console
      # ufw status numbered
      # ufw delete NUMBER
      ```

   Sprawdź również tutorial `ufw` z literatury.
   
   a. Uruchom maszynę wirtualną i sprawdź jej adres IP w sieci bridge. Możesz wykorzystać polecenie `ip ad`.

   b. Połącz się z poziomu hosta z maszyną wirtualną (`ssh ADRES_IP`). **Umieść zrzut ekranu z udanego połączenia w raporcie**.

   c. Aktywuj `ufw`. Sprawdź jego status. **Umieść zrzut ekranu w raporcie.**

   d. Spróbuj ponownie połączyć się z maszyną wirtualną z poziomu hosta.

   e. Aby umożliwić połączenie z maszyną należy dodać regułę, która zezwoli na połączenie przez port 22:

      ```console
      # ufw allow from A.B.C.D/E to any port 22
      ```

      gdzie A.B.C.D to adres IP skonfigurowanej na początku sieci

   f. Spróbuj ponownie połączyć się z maszyną. **Pokaż zrzut ekranu udanego połączenia oraz listę reguł ufw w raporcie.**

   g. Zablokuj połączenia http i https dwukrotnie wypełniając odpowiednio poniższą komendę.

      ```console
      # ufw deny out PORT_NUMBER
      ```

      Sprawdź czy reguły zostały dodane do ufw wyświetlając listę reguł. **Umieść zrzut ekranu w raporcie.**

   h. Dezaktywuj firewall i usuń `ufw` poleceniem
 
      ```console
      # apt-get purge ufw
      ```

3. **`firewalld`** i **`firewall-applet`** - konfiguracja firewalla w GUI

   -  Zainstaluj `firewall-applet` poleceniem:

      ```console
      # apt-get update
      # apt-get install firewall-applet firewalld
      ```

   a. Uruchom `firewall-applet`. Zmieniając typ strefy dla aktywnego połączenia sprawdź, od którego poziomu możliwe jest połączenie przez SSH.

   b. Zmień typ strefy dla aktywnego połączenia na *home* i spróbuj połączyć się z maszyną wirtualną przez ssh.

   c. Dezaktywuj *Service* ssh dla aktualnej strefy. Sprawdź czy możliwe jest teraz połączenie przez ssh. **Umieść zrzut ekranu z nieudanego połączenia w raporcie.**

   d. Na hoście spróbuj wykonać ping maszyny wirtualnej. Następnie w firewall-applet, w zakładce *Zones* przejdź do zakładki *ICMP Filter* dla aktualnego typu strefy. Zaznacz tam *echo-request* by uniemożliwić pingowanie tej maszyny. Wykonaj zrzut ekranu zmienionej konfiguracji i umieść w raporcie.** Sprawdź, jak zachowuje się maszyna przy pingowaniu po tej zmianie.

   e. Odinstaluj `firewalld` oraz `firewall-applet` wraz z konfiguracją.

      ```console
      # apt-get purge firewall-applet firewalld
      ```

## Literatura:
 * **`man`**: 
   *  iptables
   *  ufw
 * slajdy z wykładu nr 9
 * [tutorial iptables](https://www.hostinger.com/tutorials/iptables-tutorial)
 * [tutorial ufw](https://help.ubuntu.com/community/UFW)