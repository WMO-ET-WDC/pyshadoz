# pyshadoz

[![Build Status](https://travis-ci.org/tomkralidis/pyshadoz.png)](https://travis-ci.org/tomkralidis/pyshadoz)
[![Coverage Status](https://coveralls.io/repos/github/tomkralidis/pyshadoz/badge.svg?branch=master)](https://coveralls.io/github/tomkralidis/pyshadoz?branch=master)

## Overview

pyshadoz is a Python package to read and write [NASA Southern Hemisphere
ADditional OZonesondes](https://tropo.gsfc.nasa.gov/shadoz/) (SHADOZ) data.


## Installation

### Requirements
- Python 3.  Works with Python 2.7
- [virtualenv](https://virtualenv.pypa.io/)

### Dependencies
Dependencies are listed in [requirements.txt](requirements.txt). Dependencies
are automatically installed during pyshadoz installation.

### Installing pyshadoz

```bash
# setup virtualenv
virtualenv --system-site-packages -p python3 pyshadoz
cd pyshadoz
source bin/activate

# clone codebase and install
git clone https://github.com/tomkralidis/pyshadoz.git
cd pyshadoz
python setup.py build
python setup.py install
```

### Running

```bash
# parse a single shadoz file
pyshadoz -f </path/to/shadoz_file>

# parse a directory shadoz files
pyshadoz -d </path/to/directory>

# parse a directory shadoz files recursively
pyshadoz -d </path/to/directory> -r
```

### Using the API
```python
from pyshadoz import SHADOZ

# read SHADOZ data
with open('/path/to/directory') as ff:
    s = SHADOZ(ff)

    for key, value in s.metadata:
        print(key, value)

    print(s.data_fields)
    print(s.data_fields_units)
    print(len(s.data))

# write SHADOZ data
s = SHADOZ()
# build metadata dict
s.metadata['NASA/GSFC/SHADOZ Archive'] = 'http://croc.gsfc.nasa.gov/shadoz'
....
# build data fields
s.data_fields = ['Time', 'Press', 'Alt', 'Temp', 'RH', 'O3', 'O3', 'O3',
                 'W Dir', 'W Spd', 'T Pump', 'I O3', 'GPSLon', 'GPSLat',
                 'GPSAlt']

# build data field units
s.data_fields_units = ['sec','hPa','km', 'C', '%', 'mPa', 'ppmv', 'du', 'deg',
                       'm/s', 'C', 'uA', 'deg', 'deg', 'km']

# build data
s.data = [
    [0, 1013.85, 0.01, 24.22, 71.0, 32.91, 32.91, 0.0, 32.91, 5.29, 32.91, 9000.0, -155.049, 19.717, 0.041],
    [0, 1013.66, 0.012, 23.89, 70.0, 32.79, 32.79, 0.049, 32.79, 5.01, 32.79, 9000.0, -155.049, 19.717, 0.045]
]

# serialize data to file
shadoz_data = s.write()
with open('new_shadoz_file.dat', 'w') as ff:
    ff.write(shadoz_data)
```

### Running Tests

```bash
# install dev requirements
pip install -r requirements-dev.txt

# run tests like this:
python pyshadoz/tests/run_tests.py

# or this:
python setup.py test

# measure code coverage
coverage run --source=pyshadoz -m unittest pyshadoz.tests.run_tests
coverage report -m
```

### Code Conventions

* [PEP8](https://www.python.org/dev/peps/pep-0008)

### Bugs and Issues

All bugs, enhancements and issues are managed on [GitHub](https://github.com/tomkralidis/pyshadoz/issues).

## Contact

* [Tom Kralidis](https://github.com/tomkralidis)