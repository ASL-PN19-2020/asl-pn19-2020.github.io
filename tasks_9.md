Konfiguracja i zarządzanie serwerem plików (NFS, Samba, FTP)
============================================================

## Wymagania wstępne

1. Zadania wykonywane będą przy pomocy maszyn VM1 i VM2 zmodyfikowanych w ramach listy dot. serwerów DNS i DHCP

2. Dla przypomnienia: dane logowania to `debian`/`password`, uprawnienia podnosimy przy pomocy `sudo -i`.

3. Zmodyfikuj ilość pamięci RAM dla maszyn VM1 i VM2 używając polecenia:

   ```console
   vboxmanage modifyvm vm1  --memory 512
   vboxmanage modifyvm vm2  --memory 512
   ```

## Zadania

1. **NFS**

   a. Na maszynie VM2 zainstaluj pakiet:

      ```console
      apt update
      apt install -y nfs-kernel-server
      ```
   
   b. W pliku `/etc/idmapd.conf` skonfiguruj domenę - odkomentuj linię `# Domain = localdomain` i podaj dowolną nazwę domeny, np. `Domain = linuxlab`.

   c. Stwórz następujące foldery i plik:

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
    
   d. Skonfiguruj NFS modyfikując plik `/etc/exports`. Dodaj w nim poniższą linijkę, zastępując odpowiednio opcje:

      ```
      /nfs/export A.B.C.D/E(rw,sync,fsid=0,no_root_squash,no_subtree_check)
      ```

      * `/nfs/export` - udostępniany katalog
      * `A.B.C.D/E` - adres i maska podsieci pomiędzy maszynami VM1 i VM2
      * `rw` - prawo odczytu i zapisu
      * `sync` - synchronizacja
      * `no_root_squash` - nadaje uprawnienia roota
      * `no_subtree_check` - wyłącza weryfikację poddrzewa

   e. Zrestartuj usługę:
      
      ```console
      # systemctl restart nfs-server.service
      ```

      **Zamieść w raporcie zrzut ekranu ze statusu usługi:**

      ```console
      $ systemctl status nfs-server.service
      ```

   f. Na maszynie VM1 zainstaluj pakiet:

      ```console
      # apt update
      # apt install nfs-common
      ```

   g. Skonfiguruj domenę (jak w pkt. b.).

   h. Stwórz następujące foldery:

      ```
      /
      |
      +-- nfs
          |
          +-- import
      ```
    
   i. Zamontuj katalog export z VM2 na maszynie VM1 w katalogu import (podaj adres ip maszyny VM2 `A.B.C.G`):

      ```console
      # mount -t nfs A.B.C.G:/nfs/export /nfs/import
      ```
      
      **Zamieść w raporcie wynik wykonania polecenia.**
    
   j. Sprawdź czy katalog został zamontowany poleceniem `df -h`. Sprawdź, czy w folderze `/nfs/import` znajduje się plik `plik.txt` oraz czy zawiera tekst wpisany na maszynie VM2. **Zamieść w raporcie zrzut ekranu z wykonanych poleceń oraz zawartości pliku.**

   k. Zmodyfikuj plik `plik.txt` na maszynie VM1. Sprawdź, czy zmiana pojawiła się również na maszynie VM2.

   l. Odmontuj katalog poleceniem `umount /nfs/import`.




      

## Literatura:

 * `man`:
   * 
 * slajdy z wykładu nr 11