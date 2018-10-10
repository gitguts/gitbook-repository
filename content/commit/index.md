# Index

Jeśli w tym momencie sprawdzimy status naszego repozytorium zobaczymy poniższą informację:

##### ![](/assets/commit/git_status_after_refs.png)

Informacja na początku może wywadać myląca ponieważ mamy odpowiednie obiekty w katalogu .git/objects jednak sprowadza się to do zachowania spójności i tego jak wewnętrznie zbudowany jest git (zgodnie ze wzorcem mikroserwisów).

## Struktura indeksu

Index (potocznie nazywany również staging area) ma dosyć skomplikowaną strukturę, która jest opisana w [oficjalnym repozytorium](https://github.com/git/git/blob/master/Documentation/technical/index-format.txt).

Ponieważ jest to plik zawierający informacje binarne na potrzeby tego warsztatu (oszczędność czasu) nie będziemy tworzyć jego pliku (.git/index) ręcznie a posłużymy się wbudowanym poleceniem git:

```bash
git update-index --add test.txt
```

Teraz po wykonaniu polecenia git status otrzymamy następującą informację:

```bash
On branch master
nothing to commit, working tree clean
```