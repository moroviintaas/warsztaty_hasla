1. sha256 brutalny; alfabet [a-z][0-9]; długość 4  
```
hashcat -a 3 -m 1400 -1 ?l?d  sha256hash4.txt ?1?1?1?1
```
2. sha256crypt hasła(Unix) słownikowy;  
```
hashcat -a 0 -m 1700 hashfile.hash 10-million-password-list-top-100000.txt
```
3. sha256 hybrydowy; słownik + maska [a-z]*4
```
hashcat -a 6 -m 1800 bob.hash boblist.txt  ?a?a?a?a
```

## Uwaga
To nie są rozwiązania zadań, ale pomoce i pomysły, które pewnie należy przemyśleć i zmodyfikować


## Wskazówki
Jeśli ktoś będzie chciał rozwiązać zadanie i będzie miał problem to piscie na discord:
_moroviintaas#7749_
