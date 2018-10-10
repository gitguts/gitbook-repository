# Refs

Jeśli w tym momencie sprawdzimy zawartość katalogu .git będzie ona wyglądała podobnie do tej:

```bash
tree .git
```

##### ![](/assets/commit/git_folder.png)

Wykonajmy wobec tego polecenie sprawdzenia statusu naszego repozytorium skoro wszyskie składniki (blob, tree i commit) są już gotowe.

```bash
git status
```

##### ![](/assets/commit/git_status.png)

Natomiast jeśli wykonamy poniższe polecenie:

```bash
git log
```

Dostaniemy taki oto komunikat:

```bash
fatal: your current branch 'master' does not have any commits yet
```

Dlaczego?

## HEAD

Sprawdźmy jaka jest zawartość pliku HEAD - zawierającego wskaźnik na ostatni commit bieżącego brancha:

```bash
cat .git/HEAD
```

Otrzymamy następującą treść:

```bash
ref: refs/heads/master
```

## Poprawienie sytuacji

Aby poprawić bieżącą sytuację w repozytorium musimy stworzyć nową referencję - nasz branch master:

```bash
echo '5ee62eae03c6ada904c3f0982110bab8d0e2f9ba' > .git/refs/heads/master
```

W powyższym przykładzie "5ee62eae03c6ada904c3f0982110bab8d0e2f9ba" jest SHA1 commitu, który stworzyliśmy.