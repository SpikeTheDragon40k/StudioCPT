# Using scripting tools: Perl and Python

## Python
```python
#!/usr/bin/env python3
import sys
from zipfile import PyZipFile
for zip_file in sys.argv[1:]:
    pzf = PyZipFile(zip_file)
    pzf.extractall()
```
Usage:
```bash
./pyunzip.py master.zip 
```
or
```bash
python3 pyunzip.py master.zip
```


## Perl
```perl
#!/usr/bin/env perl
use Archive::Extract;
foreach my $filepath (@ARGV){
    my $archive = Archive::Extract->new( archive => $filepath );
    $archive->extract;
}
```
Usage:
```bash
./perlunzip master.zip
```
or
```bash
perl perlunzip.pl master.zip
```