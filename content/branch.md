# Branch

W systemie git branch (gałąź) to tylko wskaźnik na dany commit.

## Stworzenie referencji

W celu stworzenia nowej gałęzi (odpowiednik polecenia git branch) o nazwie develop wystarczy polecenie:

```bash
echo '5ee62eae03c6ada904c3f0982110bab8d0e2f9ba' > .git/refs/heads/develop
```

## Zmiana brancha

Plikowym odpowiednikiem przełączenia brancha (git checkout) jest zmiana specjalnego pliku HEAD:

```bash
echo 'ref: refs/heads/develop' > HEAD
```