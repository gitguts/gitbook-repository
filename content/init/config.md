# Konfiguracja

Podstawowa meta-konfiguracja repozytorium zawarta jest w kilku plikach.

## description

Plik description zawiera opis naszego repozytorium. Plik ten wykorzystywany jest wyłącznie przez program [GitWeb](https://git-scm.com/docs/gitweb).

```bash
echo 'My first manual repository' > description
```

## info/exclude

W systemie kontroli wersji git lista ignorowanych plików jest wyliczana na podstawie konfiguracji znajdującej się w 3 plikach:

* plik .gitignore w repozytorium
* plik .gitignore_global w naszym katalogu domowym
* plik exclude znajdujący się w katalogu .git/info naszego repozytorium

Typowe wykorzystanie pliku info/exclude jest w sytuacji, gdy pracujemy z wyjątkowym repozytorium (więc nie wykorzystamy pliku .gitignore_global) w sposób inny niż pozostali członkowie zespołu (tak więc nie będziemy chcieli "zaśmiecać" pliku .gitignore) np. inny system operacyjny, inne IDE etc.

```bash
mkdir info
touch info/exclude
```

## hooks

*Hooks* to mechanizm git pozwalający na uruchamianie skryptów użytkownika przed lub po wskazanej akcji takiej jak commit, push czy też receive.

Standardowo klient gita generuje dla nas listę przykładowych skryptów lecz w naszym przypadku nie będą one nam potrzebne.

```bash
mkdir hooks
```

## config

Centralnym miejscem konfiguracji naszego lokalnego repozytorium jest plik **config**. Plik config może zawierać dużą ilość ustawień naszego repozytorium, dlatego na tym etapie ograniczymy się do niezbędnego minimum - tego, który generowany jest przez konsolowego klienta systemu.

### core.repositoryformatversion

Każde repozytorium ma ustawione numeryczną wersję formatu. W przypadku, gdy dany format nie jest obsługiwany przez klienta - powinien on zakończyć pracę z danym repozytorium. Różne numery odpowiadają różnym sposobom obsługi dyskowej repozytorium i tak:

* 0 - inicjalna wersja formatu plików git, wskazuje na typowy (omawiany dzisiaj) sposób przechowywania plików, katalogów i referencji
* 1 - wersja identyczna z "0" z tą różnicą, że każde odchylenie od standardu musi być dodatkowo skonfigurowane w ustawieniu extensions.*

Pomimo identycznego zachowania wersji 0 i 1 ()gdy ta druga nie posiada żadnych niestandardowych ustawień) format naszego repozytorium powinien być ustawiony na 0 aby było kompatybilne wstecz dla dawnych klientów.

### core.filemode

Ustawienie to pozwala ustalić, czy bit wykonywalności w ustawieniach pliku przy porównywaniu zawartości indeksu i przestrzeni ma być ignorowany (gdy opcja ma wartość false) czy też nie (dla wartości opcji true, domyślnej).

### core.bare

Repozytorium bare (ang. "gołe") to takie, które zawiera jedynie konfigurację (katalog .git) repozytorium lecz nie można dokonuwać na nim operacji plikowych (nie posiada przestrzeni roboczej). Ustawienie jest przydatne w przypadku serwisów hostujących nasze zdalne repozytoria (jak GitHub czy też Bitbucket). Domyślnie lokalne repozytorium nie jest "gołe" - opcja ma wartość false.

### core.logallrefupdates

Ustawienie tej opcji na true uruchamia mechanizm Reflogu w naszym repozytorium. Więcej o Reflogu w dalszej części warsztatu.

### core.ignorecase

Wewnętrzna opcja git'a pozwalająca na poprawną obsługę repozytorium i zawartości jego plików (obiektów) na przestrzeni różnych systemów plikowych takich jak FAT, NTFS, APFS czy też HFS+. 

### core.precomposeunicode

Opcja wykorzystywana wyłącznie przez implementację git'a na OSX (Mac). Kiedy ustawiona jest na true, git cofa dekompozycję znaków unicode dokonywaną przez system OS X. 

### Zawartość

Tworzymy plik config i ustawiamy mu następującą zawartość:

```bash
touch config
```

```bash
[core]
	repositoryformatversion = 0
	filemode = true
	bare = false
	logallrefupdates = true
	ignorecase = true
	precomposeunicode = true
```

## Pozostałe katalogi

W swojej początkowej strukturze nasze repozytorium powinno posiadać jeszcze inne katalogi takie jak objects i refs oraz plik HEAD. Na chwilę obecną utworzymu ich pustą strukturę, a do zawartości i znaczenia przejdziemy w dalszej części warsztatu.

```bash
mkdir objects
mkdir objects/info
mkdir objects/pack
mkdir refs
mkdir refs/heads
mkdir refs/tags
```

Plik HEAD nie może być inicjalnie pusty, dlatego jego zawartość ustawimy w następujący sposób:

```bash
echo 'ref: refs/heads/master' > HEAD
```