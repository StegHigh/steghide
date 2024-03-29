# Call for Maintainers/Contributors

Hi everyone,

Thank you all for your interest in Steghide. Steghihde was created by Stefan Herzl, under the GPL in the early 2000s.

I cloned this repo about 10 years ago when I was doing [digital image security research](https://patft.uspto.gov/netacgi/nph-Parser?patentnumber=9137415). In the past few years, interest in steganography in general and Steghide in particular has grown immensely.

Let's try to get this more active and add some maintainers to this repo.

[Discussions over here](https://github.com/orgs/StegHigh/discussions).

Thanks,


@StefanoDevuono



## Introduction :
==============

Steghide is a steganography program that is able to hide data in various kinds
of image- and audio-files. The color- respectively sample-frequencies are not
changed thus making the embedding resistant against first-order statistical
tests.

The current version of steghide is 0.5.1

Features:
*) compression of embedded data
*) encryption of embedded data
*) embedding of a checksum to verify the integrity of the extracted data
*) support for JPEG, BMP, WAV and AU files

## Steganography :
===============

Steganography literally means covered writing. Its goal is to hide the fact
that communication is taking place. This is often achieved by using a (rather
large) cover file and embedding the (rather short) secret message into this
file. The result is an innocuous looking file (the stego file) that contains
the secret message.

## Compilation and Installation :
==============================

Dependencies :
--------------
You should have the following libraries installed to use steghide.

* libmhash
  A library that provides various hash algorithms and cryptographic key
  generation algorithms. Steghide needs this library to convert a passphrase
  into a form that can be used as input for cryptographic and steganographic
  algorithms.
  Available at: http://mhash.sourceforge.net/

* libmcrypt  
  A library that provides a lot of symmetric encryption algorithms. If you
  compile steghide without libmcrypt, you will not be able to use steghide to
  encrypt data before embedding or to extract encrypted data (even if you know
  the correct passphrase).
  Available at: http://mcrypt.sourceforge.net/

* libjpeg
  A library implementing jpeg image compression. Without this library, you will
  not be able to embed data in jpeg files or to extract data from jpeg files.
  Available at: http://www.ijg.org/

* zlib
  A lossless data compression library. If you compile steghide without having
  this library installed, you will not be able to use steghide to compress data
  before embedding or to extract compressed data from a stego-file.
  Available at: http://www.gzip.org/zlib/

Libmhash is absolutely required to compile steghide. While you can compile it
without the other libraries, they are highly recommended as major functionality
will not be available without them.

Linux / Unix :
--------------
After unpacking the source distribution, enter the following commands:

1) ./configure 
2) make
3) make check
4) make install (as root)

For more information see the generic installation instructions in the file
INSTALL that came with the distribution.

If any of these commands fails, please send a mail to the steghide development
mailing list (steghide-devel@lists.sourceforge.net) describing the error.
 
Windows :
---------
The easiest way is to download the precompiled binary (including Windows
versions of the necessary libraries) from the steghide website at:
http://steghide.sourceforge.net/index.php

If you want to compile the sources yourself you need a C++ compiler. How you
need to compile the source code depends on the compiler you are using: Please
consult your compiler's documentation.

Steghide can be compiled with gcc in the cygwin environment
(http://www.cygwin.com/) which is a unix emulation layer for Windows using the
procedure mentioned above for the Linux/Unix compilation.

## Quick-Start :
=============

Here are some examples of how steghide can be used. Take a look at these to get
a first impression. If you want more detailed information please read the
manpage.

The basic usage is as follows:

  $ steghide embed -cf picture.jpg -ef secret.txt
  Enter passphrase:
  Re-Enter passphrase:
  embedding "secret.txt" in "picture.jpg"... done

This command will embed the file secret.txt in the cover file picture.jpg.

After you have embedded your secret data as shown above you can send the file
picture.jpg to the person who should receive the secret message. The receiver
has to use steghide in the following way:

  $ steghide extract -sf picture.jpg
  Enter passphrase:
  wrote extracted data to "secret.txt".

If the supplied passphrase is correct, the contents of the original file
secret.txt will be extracted from the stego file picture.jpg and saved
in the current directory.

If you have received a file that contains embedded data and you want to get
some information about it before extracting it, use the info command:

  $ steghide info received_file.wav
  "received_file.wav":
    format: wave audio, PCM encoding
    capacity: 3.5 KB
  Try to get information about embedded data ? (y/n) y
  Enter passphrase:
    embedded file "secret.txt":
      size: 1.6 KB
      encrypted: rijndael-128, cbc
      compressed: yes

After printing some general information about the stego file (format, capacity) you will be
asked if steghide should try to get information about the embedded data. If you answer with
yes you have to supply a passphrase. Steghide will then try to extract the embedded data
with that passphrase and - if it succeeds - print some information about it.

## Contact :
=========

### GitHub :
---------
You can get the latest version of steghide as well as some additional
information and documentation here on [GitHub](https://github.com/orgs/StegHigh/steghide)

### Discussions :
---------------
If you have found a bug or if you have questions, comments, suggestions, etc.
please join our [discussions on GitHub](https://github.com/orgs/StegHigh/discussions).
