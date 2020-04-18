Aktualizacja systemu, instalacja, aktualizacja i deinstalacja dodatkowego oprogramowania z wykorzystaniem pakietów instalacyjnych i repozytoriów oprogramowania.
================================================================================================================================================================

Do instalacji oprogramowania przy użyciu shella konieczne jest podniesienie uprawnień:
  *  instalacja jako root

     ```bash
     su -
     ROOT_COMMAND
     ```

  *  wykorzystanie grupy `sudoers`

     ```bash
     su -
     usermod -aG sudo USERNAME
     reboot
     ...
     sudo ROOT_COMMAND
     ```

## Zadania:

1. a
   -  a


## Literatura:
 * **`man`**: 
   *  ``
 * slajdy z wykładu nr 6
 * [The Linux Command Line](http://linuxcommand.org/) - [link do książki](https://sourceforge.net/projects/linuxcommand/files/TLCL/19.01/TLCL-19.01.pdf/download):
   *  rozdział 14 - zarządzanie pakietami
