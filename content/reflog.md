# Dziennik zdarzeń

Każde repozytorium git ma opcję prowadzenia tzw. dziennika zdarzeń - Reflogu. Dziennik ten jest szczególnie przydatny gdy potrzebujemy odzyskać "utracone" dane po wykonaniu destrukcyjnego polecenia (jak np. reset).

Przykładowy reflog może wyglądać następująco:

##### ![](/assets/reflog.png)

Jeśli spróbujemy odczytać wpisy w dzienniku zdarzeń naszego repozytorium poleceniem:

```bash
git reflog
```

Otrzymamy pusty dziennik.

## Tworzenie dziennika

Git gromadzi informacje o naszym repozytorium i istotnych zdarzeniach (jak commit, reset etc.) w plikach katalogu .git/logs.

Aby stworzyć dziennik dla naszego brancha master musimy utworzyć odpowiednią strukturę katalogów:

```bash
mkdir .git/logs
mkdir .git/logs/refs
mkdir .git/logs/refs/heads
```

Poniższe polecenie stworzy plik dziennika z odpowiednią pozycją (nasz dziennik będzie posiadać jedynie jeden wpis ponieważ wykonaliśmy do tej pory jedynie jedną znaczącą zmianę w repozytorium):

```bash
echo '0000000000000000000000000000000000000000 013983c730e093c4282a8e77e6c7e2aecc37e95e Lukasz Rybka <lukasz@email.com> 1539205980 +0200 commit (initial): First commit' > .git/logs/refs/heads/master
```

Jeśli teraz wywołamy polecenie reflog zobaczymy pojedynczy wpis mówiący o stworzeniu inicjalnego commitu.

### Dziennik zdarzeń bieżącego brancha

Dla przyspieszenia działania dziennika, przy każdej zmianie brancha git kopiuje odpowiedni plik dziennika do specjalnego pliku: .git/logs/HEAD. W celu zapewnienia spójności wykonajmy to samo:

```bash
cp .git/logs/refs/heads/master .git/logs/HEAD
```