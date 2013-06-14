bagit
=====

[![Build Status](https://secure.travis-ci.org/libraryofcongress/bagit.png)](http://travis-ci.org/libraryofcongress/bagit)

bagit is a Python library and command line utility for working with  [BagIt](http://purl.org/net/bagit) style packages.

Installation
------------

bagit.py is a single-file python module that you can drop into your project as 
needed or you can install globally with:

    pip install bagit

Python v2.4+ is required.

Usage
-----

From python you can use the bagit module to make a bag like this: 

```python
import bagit
bag = bagit.make_bag('mydir', {'Contact-Name': 'Ed Summers'})
```

Or if you've got an existing bag

```python
import bagit
bag = bagit.Bag('/path/to/bag')
```

Or from the command line:

    bagit.py --contact-name 'Ed Summers' mydir

If you want to validate a bag you can:

```python
bag = bagit.Bag('/path/to/bag')
if bag.is_valid():
    print "yay :)"
else:
    print "boo :("
```

If you'd like to get a detailed list of checksum errors during validation, 
just catch the ChecksumMismatchError which has the property `errors` which
is a list of ChecksumError, each of which has `path`, `algorithm`, 
`expected` and `found` properties.

So, if you wanted to print out a list of files that failed checksum validation
you could do this:

```python
try:
  bag.validate()

except BagValidationErrorSet, e:
  print "eek, some files failed checksum validation, here they are:"
  for error in e.errors:
    print error

except BagValidationError, e:
  print "uhoh, something else went wrong: %s" % e
```

If you'd like to generate the checksums using parallel system processes, 
instead of single process:

```python
bagit.make_bag('mydir', {'Contact-Name': 'Ed Summers'}, processes=4) 
```

or:

    bagit.py --processes 4 --contact-name 'Ed Summers' mydir

bag --help will give the full set of options.

Development
-----------

    % git clone git://github.com/libraryofcongress/bagit.git
    % cd bagit 
    % python test.py

If you'd like to see how increasing parallelization of bag creation on 
your system effects the time to create a bag try using the included bench 
utility:

    % ./bench.py

License
-------

[![cc0](http://i.creativecommons.org/p/zero/1.0/88x31.png)](http://creativecommons.org/publicdomain/zero/1.0/)
