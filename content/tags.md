# Tagi

Analogicznie do tworzenia branchy - tagi są wskaźnikami na obiekty w repozytorium.

## Rodzaje tagów

W systemie git dostępne są dwa rodzaje tagów:
* Lightweight - są wyłącznie wskaźnikami na commit
* Annotated - to wskaźnik zawierający dodatkowe metainformacje

## Tworzenie tagu typu Lightweight

Aby stworzyć tag typu Lightweight wystarczy utworzyć plik w katalogu .git/refs/tags o nazwie identycznej z naszym tagiem oraz zawartością równą SHA1 commitu, na który ma wskazywać tag.

```bash
echo '5ee62eae03c6ada904c3f0982110bab8d0e2f9ba' > .git/refs/tags/v1.4
```

## Tworzenie tagu typu Annotated

Aby stworzyć tag z dodatkowymi metainformacjami musimy stworzyć nowy obiekt:

```bash
irb
require 'digest/sha1'
require 'zlib'
require 'fileutils'
require 'date'

now = Time.new.strftime("%s %z")
me = "Lukasz Rybka <lukasz@email.com>"

commit_sha = "5ee62eae03c6ada904c3f0982110bab8d0e2f9ba"
content = "object " + commit_sha + "\ntype commit\ntag v1.5\ntagger " + me + " " + now + "\n\nVersion 1.5\n"
header = "commit #{content.length}\0"
tag = header + content
sha1 = Digest::SHA1.hexdigest(tag)

zlib_content = Zlib::Deflate.deflate(tag)
path = '.git/objects/' + sha1[0,2] + '/' + sha1[2,38]
FileUtils.mkdir_p(File.dirname(path))
File.open(path, 'w') { |f| f.write zlib_content }
```

Mając SHA1 nowego obiektu (tagu) możemy stworzyć referencję do niego:

```bash
echo '6c7e38857b12c6a381013ce8f48a5cdfabb0b0e9' > .git/refs/tags/v1.5
```