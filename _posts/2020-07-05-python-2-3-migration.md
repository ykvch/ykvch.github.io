---
title: Python 2 -> 3 migration notes
layout: post
date: '2020-07-05 07:18:09 +0300'
categories: update
---

Basic workflow:
===
1. Run futurize utility (part of future module) against all python files in project (http://python-future.org/automatic_conversion.html). Command used: `futurize -0nw --no-diffs <dir>`
2. Get acquainted with http://python-future.org/
3. Fix code for Python 3 compatiblity. Not all issues can be automatically fixed by `future`, chances are that one will face unreliable code after 'futurization' procedure. Most such errors appear due to semantic changes in Python 3 compared to Python 2 AND ignoring proper code practices in their project.


Notable observations:
===
* `futurize` does not distinguish between iterators and generators and wraps everything into lists.  
   !!!Make sure that _indefinite_ (or producing huge data) generators DO NOT get wrapped as lists.
* `Python 3` abandons `long` type and uses `int` for big numbers as well.  
   Numbers with `L` suffix (eg.: `1234L` denoting long type in `Python 2`) will cause `SyntaxError`.  
   Type checking against `long` type should be removed (although `future` handles this in existing code).
* Zero prefix (denoting octal numbers in `Python 2`) causes `SyntaxError` and should be changed to `0o` (Zero Oscar).
* `Python 3` strictly distinguishes between `bytes` and `str` types. `Python 3` does NOT allow mixing them.  
    For proper usage guides refer to: https://docs.python.org/3/howto/unicode.html  
    For better understanding of such differentiation and why it is important see (especially the section "There Ain’t No Such Thing As Plain Text"): [Joel Spolski blog post about unicode](https://www.joelonsoftware.com/2003/10/08/the-absolute-minimum-every-software-developer-absolutely-positively-must-know-about-unicode-and-character-sets-no-excuses/)
* `future` `builtins.str` type differs from `Python 2` `str`  
    Might be a good idea to use `isinstance(obj, basestring)` for string-like type checks (in which case futurizer inserts `from past.builtins import basestring` for compatibility).
* In `Python 3` many network and file related modules operate with `bytes` buffers, so it is required to implicitly `encode` one's text data into `bytes` type.  
    Consider opening files in `'rb'` mode whenever interaction with byte-based buffers is required (eg.: sending file data via tornado server).
* Consider using `codecs` module
* Replace `StringIO` usage with appropriate `BytesIO`/`StringIO` classes from `io` module.
* `JSON` related modules mostly use unicode, so make sure to properly inject non-text data (decode bytes where necessary, or use `codecs` module).  
    Some JSON errors like `UnicodeDecodeError` are specific to `Python 2` after futurizing. `Python 3` overcomes them with its bytes/text division model.
* Update external dependencies to use newer modules in favour of outdated ones:
    * BeautifulSoup -> bs4
    * ConfigParser -> configparser
    * jsonrpclib -> jsonrpclib-pelix
    * pycrypto -> pycryptodome
    * pylibpcap -> pcapy
    * StringIO -> io
    * SOAPpy -> SOAPpy-py3
    * suds -> suds-jurko
* Some modules require specific version numbers to become compatible with `Python 3`.
* Most built-in module imports are fixed by `future` utility itself (e.g. `urllib`).
* Strings formatting with `%` (percent sign) should be avoided where possible as it might cause issues with unicode strings.