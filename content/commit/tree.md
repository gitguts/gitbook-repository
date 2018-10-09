# Tree

Do odwzorawnia struktury plików i katalogów git posłuchuje się drugim typem obiektów - tree.

## Struktura

##### ![](/assets/commit/tree.png)

Tree składa się z:
* indentyfikatora **tree**
* informacji o wielkości zawartości
* specjalnego delimitera - \0
* zawartości

Kanonicznie możemy zapisać to w następujący sposób:

```
tree ZN(A FNS)*
```

Gdzie:
* N - NULL (\0)
* Z - wielkość zawartości w bajtach
* A - uniksowy tryb dostępu do zasobu, bez wiodącego zera
* F - nazwa zasobu
* S - zakodowana SHA1 obiektu

**TIP:** kolejne pozycje drzewa (kolejne obiekty) muszą być wpisane w sposób posortowany, według nazw obiektów (a dokładniej ich kodów ASCII).

## Tryby dostępu

W systemie git możemy mieć spotkać następujące tryby dostępu do obiektu:

* 40000 - katalog (tree) 
* 100644 - plik (blob)
* 100755 - plik wykonywalny (blob) 
* 120000 - link symboliczny (blob). 
* 160000 - submoduł (commit). 

Warto zwrócić uwagę na fakt iż w UNIXie katalog ma tryb dostępu 040000 natomiast w w gicie 40000 - jest to związane ze sposobem zapisu drzewa (forma kanoniczna).

## Zapis identyfikatora obiektu

W obiekcie tree git zapisuje identyfikator każdego obiektu w sposób skompresowany. Algorytm wygląda następująco:

Załóżmy, że chcemy zapisać następujący identyfikator obiektu:

```
28fd1f6a8db1463ad03aacb1b77d331c2a0442e8
```

Podzielmy ten identyfikator na grupy składające się z 2 kolejnych znaków:

```
28 | fd | 1f | 6a | 8d | b1 | 46 | 3a | d0 | 3a | ac | b1 | b7 | 7d | 33 | 1c | 2a | 04 | 42 | e8
```

Każda z tych par może być potraktowana jako heksadecymalny numer znaku w tablicy ASCII:

##### ![](/assets/commit/ascii_table.gif)

Kiedy przekształcimy numery na znaki ASCII otrzmamy następujący ciąg:

```

( | \xFD | \x1F | j | \x8D | \xB1 | F | : | \xD0 | : | \xAC | \xB1 | \xB7 | } | 3 | \x1C | * | \x04 | B | \xE8

```

Taki zabieg pozwala nam zaoszczędzić 50% miejsca i zapisać każdy identyfikator za pomocą 20 bajtów zamiast 40!

Aby zatomatyzować konwersję skorzystamy z poniższego skryptu:

```ruby
def encode_sha(sha)
	i = 0
	encoded = ""

	while i < sha.length do
		encoded += sha[i, 2].to_i(16).chr
		i += 2
	end

	return encoded
end
```

## Zagnieżdżona struktura

##### ![](/assets/commit/tree_pointers.png)

## Zapis obiektu do pliku

```bash
irb
require 'digest/sha1'
require 'zlib'
require 'fileutils'
```

### Wygenerowanie klucza SHA1

```ruby
blob_sha = "28fd1f6a8db1463ad03aacb1b77d331c2a0442e8"
content = "100644 test.txt\0" + encode_sha(blob_sha)
header = "tree #{content.length}\0"
tree = header + content
sha1 = Digest::SHA1.hexdigest(tree)
```

### Zapis

```ruby
zlib_content = Zlib::Deflate.deflate(tree)
path = '.git/objects/' + sha1[0,2] + '/' + sha1[2,38]
FileUtils.mkdir_p(File.dirname(path))
File.open(path, 'w') { |f| f.write zlib_content }
```