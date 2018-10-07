# Blob

Wszystkie pliki w naszym repozytorium przechowywane są w postaci blob'ów.

## Struktura

##### ![](/assets/commit/blob.png)

Każżdy blob ma ściśle określoną strukturę:

Kiedy git zapisuje dane, tworzony jest specjalny obiekt zwany blob'em. Blob składa się z:
* identyfikatora **blob**
* informacji o wielkości
* specjalnego delimitera - \0
* skompresowanej zawartości

## Zapis

Na potrzeby tego ćwiczenia skorzystamy z interaktywnej konsoli języka Ruby:

```bash
irb
require 'digest/sha1'
require 'zlib'
require 'fileutils'
```

### Wygenerowanie klucza SHA1

```ruby
content = "Test file content"
header = "blob #{content.length}\0"
blob = header + content
sha1 = Digest:SHA1.hexdigest(blob)
```

### Kompresja

Wszystkie pliki w repozytorium git są zapisywane po uprzednim skompresowaniu. Aby skompresować nasz blob należy:

```ruby
zlib_content = Zlib::Deflate.deflate(blob)
```

## Zapis obiektu

```ruby
path = '.git/objects/' + sha1[0,2] + '/' + sha1[2,38]
FileUtils.mkdir_p(File.dirname(path))
File.open(path, 'w') { |f| f.write zlib_content }
```