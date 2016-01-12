Thread-safe Python bindings for [LevelDB](http://code.google.com/p/leveldb/).

# Features #

  * **everything** from the [LevelDB API](http://leveldb.googlecode.com/svn/trunk/doc/index.html), **except** for:
    * arbitrary key comparison
    * all iteration except for single-step forward/backwards

# Example Usage #

```
import leveldb

db = leveldb.LevelDB('./db')

# single put
db.Put('hello', 'world')
print db.Get('hello')

# single delete
db.Delete('hello')
print db.Get('hello')

# multiple put/delete applied atomically, and committed to disk
batch = leveldb.WriteBatch()
batch.Put('hello', 'world')
batch.Put('hello again', 'world')
batch.Delete('hello')

db.Write(batch, sync = True)
```

# Installation #

```
# get source code from SVN
$ svn checkout http://py-leveldb.googlecode.com/svn/trunk/ py-leveldb-read-only
$ cd py-leveldb-read-only
# build the leveldb library
$ ./compile_leveldb.sh
# build the Python extensions
$ python setup.py build
# (optional) install it
$ sudo python setup.py install
```

# Documentation #

These are just verbatim from the source code.

```
These are just verbatim from the source code.

 class LevelDB(__builtin__.object)
  |  LevelDB(filename, **kwargs) -> leveldb object
  |  
  |  Open a LevelDB database, from the given directory.
  |  
  |  Only the parameter filename is mandatory.
  |  
  |  filename                                    the database directory
  |  create_if_missing (default: True)           if True, creates a new database if none exists
  |  error_if_exists   (default: False)          if True, raises and error if the database already exists
  |  paranoid_checks   (default: False)          if True, raises an error as soon as an internal corruption is detected
  |  block_cache_size  (default: 8 * (2 << 20))  maximum allowed size for the block cache in bytes
  |  write_buffer_size (default  2 * (2 << 20))  
  |  block_size        (default: 4096)           unit of transfer for the block cache in bytes
  |  max_open_files:   (default: 1000)
  |  block_restart_interval           
  |  
  |  Snappy compression is used, if available.
  |  
  |  Some methods support the following parameters, having these semantics:
  |  
  |   verify_checksum: iff True, the operation will check for checksum mismatches
  |   fill_cache:      iff True, the operation will fill the cache with the data read
  |   sync:            iff True, the operation will be guaranteed to sync the operation to disk
  |  
  |  Methods supported are:
  |  
  |   Get(key, verify_checksums = False, fill_cache = True): get value, raises KeyError if key not found
  |  
  |      key: the query key
  |  
  |   Put(key, value, sync = False): put key/value pair
  |  
  |      key: the key
  |      value: the value
  |  
  |   Delete(key, sync = False): delete key/value pair, raises no error kf key not found
  |  
  |      key: the key
  |  
  |   Write(write_batch, sync = False): apply multiple put/delete operations atomatically
  |  
  |      write_batch: the WriteBatch object holding the operations
  |  
  |   RangeIter(key_from = None, key_to = None, include_value = True, verify_checksums = False, fill_cache = True): return iterator
  |  
  |      key_from: if not None: defines lower bound (inclusive) for iterator
  |      key_to:   if not None: defined upper bound (inclusive) for iterator
  |      include_value: if True, iterator returns key/value 2-tuples, otherwise, just keys
  |  
  |   GetStats(): get a string of runtime information

  class WriteBatch(__builtin__.object)
  |  WriteBatch() -> write batch object
  |  
  |  Create an object, which can hold a list of database operations, which
  |  can be applied atomically.
  |  
  |  Methods supported are:
  |  
  |   Put(key, value): add put operation to batch
  |  
  |      key: the key
  |      value: the value
  |  
  |   Delete(key): add delete operation to batch
  |  
  |      key: the key
  |  

```