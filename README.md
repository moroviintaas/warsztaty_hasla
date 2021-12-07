# Hasła - warsztaty
Warsztaty z tematyki kryptologii dla licealistów, zagadnienie dotyczące haseł - mające na celu zapoznanie z zasadmi obchodzenia się z nimi.
___


## 1. Juyter - konfiguracja
Jupyter umożliwia prezentowanie kodów źródłowych (między innymi w języky Python) i ich wykonywanie po fragmencie. Jest wygodniejszy w demonstorwaniu kodu sekwencyjnego w małych kawałkach.

Możemy uruchomić Jupyter na następujące sposoby:
1. Google Colab (zalecane);
2. W kontenerze (Docker);
3. Natywnie na systemie operacyjnym.


___
### 1.1. Google Colab

#### __Zalety__:
+ Wymaga jedynie przeglądarki;
+ Najprostsze w uruchomieniu;
+ Zapisuje na dysku google dokonane modyfikacje;
+ Wykonuje się zdalnie (bezpiecznie - nie na naszym komputerze);

#### __Wady__: 
- Wymaga konta google;
- Ograniczona wydajność;
- Wymagane stałe połączenie z internetem;
- Jeśli zepsuje sie google, albo wyłączy usługę można stracić dane;

#### __Uruchomienie__:

1. Wchodzimy na stronę: [https://colab.research.google.com/](https://colab.research.google.com/);
2. Logujemy się;
3. Wchodzimy w _File -> Upload notebook_ i wybieramy nasz zeszyt `.ipynb` (możemy podać z dysku google, z komputera lokalnego lub z [github](https://github.com));
___

### 1.2. Uruchomienie w kontenerze:

#### __Zalety__:
+ Dostarczony gotowy Dockerfile do zbudowania obrazu;
+ Konteneryzacja ułatwia wyczyszczenie systemu z instalacji;
+ Izolacja dockera chroni przed wykonaniem szkodliwego kodu w systemie plków maszyny hostującej (kod jest uruchamiany na naszym procesorze, ale odizolowany na zasobach);

#### __Wady__:
- Domyślnie nie zapisuje efektów prac (zapisane zostanie to co jest umieszczone w katalogu współdzielonym między kontenerem i hostem)
- Wymaga silnika dockera;
- Problematyczne na MS Windows (wymaga WSL 2 i przewiduję problemy z instalacją Dockera);
- Modyfikacje środowiska są trudniejsze dla nie doświadczonych w technologii Docker;


#### __Instalacja i Uruchomienie__:
1. Instalacja dockera - zalecam wejść na stronę [Docker](https://docs.docker.com/engine/install/) i postępować zgodnie z procedurą dla swojej dystrybucji systemu (Ubuntu/Debian/CentOS) lub na oficjalną stronę dystrybucji   
    1.1 Dodać użytkownika lokalnego do grupy docker (żeby móc sterować demonem dockera bez uprawnień roota) 

    ```
    $ sudo usermod -a -G docker nazwa_uzytkownika
    ```  
    1.2. Przelogować sie użytkownikiem;
2. Zainstalować `git` - procedura może` się różnić od dystrybucji:

    debian/ubuntu:
    ```
    $ sudo apt install git
    ```  
    centOS/redhat:
    ```
    $ sudo yum install git
    ```

    arch/manjaro: 
    ```
    $ sudo pacman -Syu git
    ```

3. Pobrać repozytorium z przygotowanym środowiskiem:  
    ```
    $ git pull https://github.com/moroviintaas/warsztaty_hasla
    ```
4. Przejść do katalogu:  
    ```
    $ cd warsztaty_hasla/docker
    ```
5. Zbudować obraz dockerowy:
    ```  
    $ docker -t hashimg:latest build .
    ```
6. Uruchomić konener:  
    ```
    $ docker run  -p 8888:8888 -v notebooks:/home/labuser/notebooks \
     -v hashes:/home/labuser/hashes -it  --name hashcont hashimg
     ```  
    W terminalu wyświetli się link z tokenem, który należy wkleić do przeglądarki
7. Podłączyć się dodatkowym terminalem do kontenera: 

    ```
    $ docker exec -it hashcont /bin/bash
    ```
8. (Opcjonalnie) W przypadku gdy konerer się wyłączy  
    ```
    $ docker start hashcont
    ```

    w terminalu (w kontenerze) uruchomić jupyter ręcznie:  
    ```
    $ /usr/local/bin/jupyter-notebook --ip 0.0.0.0 --notebook-dir /home/labuser/`
    ```

___

### 1.3. Natywnie na komputerze

#### __Zalety__:
+ Natywnie na maszynie - maksymalna wydajność;
+ Dostępne w manadżerach Pip i Conda;

#### __Wady__:
- Uruchamiany kod ma dostęp do zasobów hosta (system plików);
- Pakiet może zawierać niepotrzebne oprogramowanie (niewykorzystywane);

#### __Uruchamianie z Pip__ (zalecane na GNU/Linux):
Pip jest manadżerem pakietów dla Pythona. Ścieżka dla użytkowników Pip (domyślna ścieżka dla systemu Ubuntu Linux). Należy mieć zainstalowany `python3` i `python3-pip` (`pip3`)

1. Instalowanie paczek do `python`

    ```
    $ pip3 install cryptography
    ````
2. Instalowanie juyter notebook:  

    ```
    $ pip3 install notebook
    ```
3. Uruchamianie:  

    ```
    $ jupter notebook --notebook-dir=sciezka_do_katalogu_z_zeszytem
    ```   
    W terminalu wyświetli się link z tokenem, który należy wkleić do przeglądarki


#### __Uruchamianie z Conda__ (zalecane na MS Windows):
Konda jest manadżerem, który dostarcza jednocześnie `python` oraz potrzebne pakiety. Tę drogę bym polecał na systemie MS Windows (choć sam system odradzam _#linuxmaserrace_).

Zainstalowanie Conda - polecam postępować z oficjalną instrukcja dla systemu operacyjnego: [instalacja condy](https://conda.io/projects/conda/en/latest/user-guide/install/index.html). Zalecam wersję `miniconda` (nie zajmie 3 GiB, tylko z 500 MiB)



___
## 2. Hashcat
Hashacat to otwarto źródłowy program do łamania hashy haseł. Podczas zajęć pokażę jego podstawowe możliwości. 

Możlwiści konfiguracji środowiska:
1. Natywnie;
2. W maszynie wirtualnej;
3. W kontenerze (Docker);
___
### 2.1 Natywnie

#### __Zalety__:
+ Najlepsza wydajność;
+ Niski koszt przetrzeniu dyskowej;
+ Najlepsza współpraca ze sterownikiem karty graficznej (sterowniki);

#### __Wady__:
+ Zaśmiecanie sobie komputera jednorazowym programem (chyba że się spodoba);
#### __Instalacja__:
Program można pobrać z oficjalnej strony [https://hashcat.net](https://hashcat.net/hashcat/) (tak należy postępować dla MS Windows). 

Na systemach linux w wielu dystrybucjach można go zainstalować w poprzez natywny manadżer pakietów: 
+ Ubuntu/Mint/Debian:  
    ```
    $ sudo apt install hashcat
    ```  
+ Arch/Manjaro:  
    ```
    $ sudo pacman -Syu hashcat
    ```  
+ Kali Linux: __już jest__

___
### 2.2 Maszyna wirtualna

#### __Zalety__:
+ Osbne środowisko, możliwe do łatwego usunięcia po zajęciach
+ Gotowy obraz systemu z działającym `hashcat`
#### __Wady__:
+ Obniżona wydajność (przez wirtualizację);
+ Najwyższy koszt w przestrzeni dyskowej;
+ Irytujący sposób przełączania się pomiędzy hostem i maszyną wirtualną;
+ Możliwe problemy z konfiguracją sieci i pamięci współdzielonej

Maszyna wirtualna: Można postawić sobie maszynę wirtualną z hashcatem. W tym wypadku polecałbym użycie obrazu Kali Linux (ze względu na posiadany przez niego już zainstalowany program).
Można pobrać z oficjalnej strony Kali Linux [https://www.kali.org/](https://www.kali.org/get-kali/#kali-virtual-machines).
___

### 2.3. Docker

 Dla użytkowników Dockera, obraz zbudowany według dostarczonego tu przepisu (ten z juyterem) ma zainstalowany `hashcat`, więc użytkownicy używający dockera mogą postępować jak w 1.2.

#### __Zalety__:
+ Dla użytkowników Dockera przygotowany obraz z zainstaloawnym `hashcat`;
+ Wysoka wydajność na CPU;
+ Bezpieczne środowisko do instalowania i testowania oprogramowania;

#### __Wady__:
+ Wymaga Docker Engine;
+ Obraz nie zawiera sterowników do karty graficznej, więc nie uda się odpalić na kartach (można zmodyfikować obrazaby były, ale na potrzeby prezentacji nie było to konieczne);
+ Skomplikowane użytkowanie dla niedoświadczonych;
___
## 3. Porada dla wygodnego środowiska
Nie wszystkie powyższe kombinacje ze sobą dobrze współdziałają w mojej opinii wygodniejsze stanowiska będą skonfigurowane w następujące sposoby, w ogólności osobiście unikałbym maszyny wirtualnej:
| Ranking | System Operacyjny | __Notebook__     | __Hashcat__ |
|:-:|--------------|:---------:|:----------:|
| 1 | GNU/Linux (dowolny wybrany) | Colab | zainstalowany |
| 2| GNU/Linux (dowolny wybrany) | Zainstalowany przez `pip`| zainstalowany |
| 3 | GNU/Linux (dowolny wybrany) \* | Docker | Docker |
| 4| MS Windows | Colab  | zainstalowany (jeśli się uda) |
| 5 | MS Windows | Conda  | zainstalowany (jeśli się uda) |


\* Moja osobista preferencja