Konfiguracja i zarządzanie serwerem plików (NFS, Samba, FTP)
============================================================

## Wymagania wstępne

1. Zadania wykonywane będą przy pomocy maszyn VM2 i VM3 oraz Twojej maszyny z systemem Debian (z interfejsem graficznym).

2. Dla przypomnienia: dane logowania to `debian`/`password`, uprawnienia podnosimy przy pomocy `sudo -i`.

3. Przywróć maszyny do stanu początkowego, uruchamiając skrypt `cook-vms`.

4. Zmodyfikuj ilość pamięci RAM dla maszyn VM2 i VM3 używając polecenia:

   ```console
   vboxmanage modifyvm vm2 --memory 512
   vboxmanage modifyvm vm3 --memory 512
   ```

4. Podaj obu maszynom sieć NAT (jej adres to `10.10.10.0/24`):

   ```console
   vboxmanage natnetwork add --netname labnet --network "10.10.10.0/24" --enable
   vboxmanage modifyvm vm2 --nic3 natnetwork --nat-network3 labnet
   vboxmanage modifyvm vm3 --nic3 natnetwork --nat-network3 labnet
   ```

5. W Twojej maszynie wirtualnej deaktywuj wszystkie adaptery sieci z wyjątkiem pierwszego, a pierwszy skonfiguruj tak, by korzystał z sieci `labnet`:

   ![labnet.png](images/labnet.png)

6. Uruchom maszyny poleceniem `start-vms` oraz uruchom Twoją maszynę wirtualną.

## Zadania

1. **NFS**

   a. Na maszynie VM2 zainstaluj pakiet:

      ```console
      # apt update
      # apt install -y nfs-kernel-server
      ```

   b. Sprawdź adresy obu maszyny w nowej sieci (powinna być wymieniona na końcu po wywołaniu polecenia `ip ad`).
   
   c. W pliku `/etc/idmapd.conf` skonfiguruj domenę - odkomentuj linię `# Domain = localdomain` i podaj dowolną nazwę domeny, np. `Domain = linuxlab`.

   d. Stwórz następujące foldery i plik:

      ```
      /
      |
      +-- nfs
          |
          +-- export
              |
              +-- plik.txt
      ```

      W pliku `plik.txt` umieść dowolny tekst.
    
   e. Skonfiguruj NFS modyfikując plik `/etc/exports`. Dodaj w nim poniższą linijkę, zastępując odpowiednio opcje:

      ```
      /nfs/export A.B.C.D/E(rw,sync,fsid=0,no_root_squash,no_subtree_check)
      ```

      * `/nfs/export` - udostępniany katalog
      * `A.B.C.D/E` - adres i maska podsieci pomiędzy maszynami VM1 i VM2
      * `rw` - prawo odczytu i zapisu
      * `sync` - synchronizacja
      * `no_root_squash` - nadaje uprawnienia roota
      * `no_subtree_check` - wyłącza weryfikację poddrzewa

   f. Zrestartuj usługę:
      
      ```console
      # systemctl restart nfs-server.service
      ```

      **Zamieść w raporcie zrzut ekranu ze statusu usługi:**

      ```console
      $ systemctl status nfs-server.service
      ```

   g. Na maszynie VM3 zainstaluj pakiet:

      ```console
      # apt update
      # apt install -y nfs-common
      ```

   h. Skonfiguruj domenę (jak w pkt. c.).

   i. Stwórz następujące foldery:

      ```
      /
      |
      +-- nfs
          |
          +-- import
      ```
    
   j. Zamontuj katalog export z VM2 na maszynie VM3 w katalogu import (podaj adres ip maszyny VM2 `A.B.C.G`):

      ```console
      # mount -t nfs A.B.C.G:/nfs/export /nfs/import
      ```
      
      **Zamieść w raporcie wynik wykonania polecenia.**
    
   k. Sprawdź czy katalog został zamontowany poleceniem `df -h`. Sprawdź, czy w folderze `/nfs/import` znajduje się plik `plik.txt` oraz czy zawiera tekst wpisany na maszynie VM2. **Zamieść w raporcie zrzut ekranu z wykonanych poleceń oraz zawartości pliku.**

   l. Zmodyfikuj plik `plik.txt` na maszynie VM3. Sprawdź, czy zmiana pojawiła się również na maszynie VM2.

   m. Odmontuj katalog poleceniem `umount /nfs/import`.

2. **SAMBA**

   > SAMBA to standard komunikacji pomiędzy systemami Linux oraz Windows. Pozwala w prosty i bezpieczny sposób udostępniać pliki, drukarki oraz inne serwisy.

   a. Zainstaluj na maszynie VM2 pakiet:

      ```console
      # apt install -y samba
      ```

   b. Przygotuj udział:

      *  Utwórz folder `/samba`
      *  Nadaj mu uprawnienia `777`
      *  Zmodyfikuj plik `/etc/samba/smb.conf`:

         - pod `[global]` dodaj linijkę:
           
           ```
           unix charset = UTF-8
           ```
         
         - w linii 29 zmodyfikuj grupę roboczą:

           ```
           workgroup = LINUXLAB
           ```
        
         - na końcu pliku dodaj linie:
          
           ```
           [MyWinDir]
               path = /samba
               writable = yes
               guest ok = yes
               guest only = yes
               create mode = 0777
               directory mode = 0777
           ```
      
   c.  Sprawdź poprawność konfiguracji. **Zamieść zrzuty ekranu z wykonania poleceń w raporcie.**

       ```console
       $ testparm /etc/samba/smb.conf
       # systemctl restart smbd
       $ systemctl status smbd
       ```
   d. Na Twojej maszynie wirtualnej uruchom program Nautilus (menedżer plików). Wciśnij kombinację `Ctrl+L` aby móc wpisać adres. Następnie wpisz `smb://A.B.C.D`, gdzie  `A.B.C.D` to adres IP maszyny VM2. **Zamieść zrzut ekranu okna w raporcie.**

   e. Utwórz w folderze folder o dowolnej nazwie oraz dowolny plik. Sprawdź, czy pojawiły się one w folderze `/samba` na maszynie VM2. **Zamieść zrzut ekranu polecenia `ls -la` w folderze `/samba` maszyny VM2.**

## Literatura:

 * `man`:
   * 
 * slajdy z wykładu nr 11