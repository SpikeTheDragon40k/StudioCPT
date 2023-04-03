# Using scripting tools: Perl and Python
Many answers here mention tools that require installation, but nobody has mentioned that two of Ubuntu's scripting languages, Perl and Python, already come with all the necessary modules that allow you to unzip a zip archive, which means you don't need to install anything else. Just use either of the two scripts presented below to do the job. They're fairly short and can even be condensed to a one-liner command if we wanted to.

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