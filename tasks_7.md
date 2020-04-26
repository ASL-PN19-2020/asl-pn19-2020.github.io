Zapory sieciowe (firewall) i sterowanie ruchem sieciowym
========================================================

## Zadania:

> Zadania 1. i 2. wykonaj wykorzystując maszyny wirtualne z poprzedniej listy (routing). Pozostałe wykonaj używając Twojej maszyny wirtualnej ze środowiskiem graficznym. Niezalecane jest wykonywanie ćwiczeń dotyczących firewalla w systemie, z którego korzystasz na codzień.

1. **`iptables`**
   -  `iptables` powinien być już zainstalowany - gdyby nie był zainstaluj go poleceniem:

      ```bash
      # apt-get update
      # apt-get install iptables
      ```

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

        ```bash
        # iptables -A <chain> -i <interface> -p <protocol> -s <source> -dport <port> -j <target>
        ```

      * usuwanie reguły - zastąp `-A` przez `-D`

        ```bash
        # iptables -D <chain> -i <interface> -p <protocol> -s <source> -dport <port> -j <target>
        ```

      * usuwanie wszystkich reguł

        ```bash
        # iptables -F
        ```

      * usuwanie wszystkich reguł w chainie

        ```bash
        # iptables -F <chain>
        ```

      * listowanie reguł

        ```bash
        # iptables -L --line-numbers
        ```

        na tej podstawie można również usuwać reguły

        ```bash
        # iptables -D <chain> <number>
        ```

      Inne reguły można znaleźć w tutorialu (sprawdź literaturę).

   a.  Sprawdź status `iptables`:

      ```bash
      # iptables -L -v
      ```

   b.  Załóżmy, że chcielibyśmy ukryć przed innymi maszynami router VM3, tzn. uniemożliwić 

2. **`ufw`**

   a. 

3. **`firewalld`** i **`firewall-applet`**

   a. 

4. **`gufw`**

   a. 

## Literatura:
 * **`man`**: 
   *  iptables
   *  ufw
   *  
 * slajdy z wykładu nr 9
 * [tutorial iptables](https://www.hostinger.com/tutorials/iptables-tutorial)
