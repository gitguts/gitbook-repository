# Obiekty

U swoich podstaw, git przechowuje wszystkie dane w postaci obiektów. Obiekty te natomiast są zapisywane w konwencji klucz-wartość.

## Klucz-wartość

Kluczem w systemie git jest kryptograficzna funkcja hash'ująca (SHA1) - dla każdej danej przechowywanej w systemie generowany jest 40 znakowy klucz heksadecymalny. Dla dwóch identycznych danych - wygenerowany klucz będzie identyczny.

## Typy obiektów

W gicie istnieje kilka różnych rodzajów obiektów:

* blob - przechowujący dane plikowe
* tree - przechowujący strukturę drzewiastą naszego repozytorium (struktura zagnieżdżona)
* commit - wskazuje na drzewo wraz z dodatkowymi meta-informacjami. Jest to snapshot naszego repozytorium
* tag - wskaźnik na wskazany commit