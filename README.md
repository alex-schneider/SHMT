[![Software license][ico-license]](LICENSE)
[![Build status][ico-travis]][link-travis]

# SHMT (Static Hash Map Table)

SHMT is an implementation of a very fast key-value read-only
hash map table for PHP7.

We have developed SHMT as a faster, dependency-free replacement for the PECL
[CHDB](https://pecl.php.net/package/chdb) extension.

## Features

SHMT

* is written in C.
* creates and uses its own memory-mapped binary file. This
  enable it to cache and share the loaded pages of the
  file across multiple processes.
* has an extremely fast initial load, regardless of the
  size of the binary file.
* internally implements a "perfect hash function" and
  guarantees O(1) lookup time in the worst cases.
* internally uses the very fast "[MurmurHash3](https://en.wikipedia.org/wiki/MurmurHash)"
  hashing algorithm.
* doesn't require any external libraries.
* is PHP 7.x ready.


## Limitations

* Supported maximum number of data array elements is
  2^26 (67,108,864 on 32 bit systems) and 2^31 (2,147,483,648
  on 64 bit systems).
* The data array keys and values are always cast to string.
* Data files cannot be exchanged between 32 bit and 64 bit 
  systems or systems with different endianness.
* The code compiles and runs on Linux systems. Other platforms
  have not been tested.


## PHP Class

```
public static boolean SHMT::create(string $filename, array $array)
```

* Creates a SHMT from the `$array` and writes it into the file `$filename`
* Returns `true` on success
* Throws exceptions on errors


```
public SHMT::__construct(string $filename)
```

* Constructs a new SHMT object and maps the SHMT file `$filename` into memory
* Throws exceptions on errors


```
public (string|null) SHMT::get(string $string)
```

* Attempts to find the value stored under the key `$string`
* Returns the value if the key `$string` exists, otherwise `null`


```
public array SHMT::keys()
```

* Returns the list of all keys stored in the SHMT.


### Example

Create a SHMT:

```
$array = [
	'str_key' => 'test',
	123456789 => 12345
	-1 => ''
];

SHMT::create($filename = 'map.shmt', $array);
```

Read from a SHMT:

```
$shmt = new SHMT($filename);

echo $shmt->get('str_key');    // string(4) "test"
echo $shmt->get(123456789);    // string(5) "12345"
echo $shmt->get('123456789');  // string(5) "12345"
echo $shmt->get(-1);           // string(0) ""
echo $shmt->get('abc_xyz');    // NULL
```

[ico-license]: https://img.shields.io/github/license/mashape/apistatus.svg
[ico-travis]: https://travis-ci.org/sevenval/SHMT.svg?branch=master
[link-travis]: https://travis-ci.org/sevenval/SHMT
