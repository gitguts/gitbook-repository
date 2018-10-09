# Commit

Aby zapisać nasz commit potrzebujemy trzeciego typu obiektu - commitu.

## Struktura

##### ![](/assets/commit/commit.png)

Tree składa się z:
* indentyfikatora **commit**
* informacji o wielkości zawartości
* specjalnego delimitera - \0
* zawartości

Zawartość składa się z następujących czynników:

* autor
* commiter
* data
* wiadomość (message)
* przodek (parent commit) - może być jeden lub więcej

## Zapis

```bash
irb
require 'digest/sha1'
require 'zlib'
require 'fileutils'
require 'date'
```

### Wygenerowanie klucza SHA1

```ruby
now = Time.new.strftime("%s %z")
me = "Lukasz Rybka <lukasz@email.com>"
tree_sha = "b6552d9c9c72edad72fbf3ad8ce5ab892070eef3"
content = "tree " + tree_sha + "\nauthor " + me + " " + now + "\ncommitter " + me + " " + now + "\n\nFirst commit\n"
header = "commit #{content.length}\0"
commit = header + content
sha1 = Digest::SHA1.hexdigest(commit)
```

### Zapis

```ruby
zlib_content = Zlib::Deflate.deflate(commit)
path = '.git/objects/' + sha1[0,2] + '/' + sha1[2,38]
FileUtils.mkdir_p(File.dirname(path))
File.open(path, 'w') { |f| f.write zlib_content }
```