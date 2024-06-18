xkcdpass-offensive
========

.. image:: https://badges.gitter.im/Join%20Chat.svg
   :alt: Join the chat at https://gitter.im/redacted/XKCD-password-generator
   :target: https://gitter.im/redacted/XKCD-password-generator?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge

A flexible and scriptable password generator which generates offensive, but strong, passphrases, inspired by `XKCD 936 <http://xkcd.com/936/>`_::

    $ xkcdpass
    > beep beeeeeep beep-beep beep (censored for your protection)

.. image:: http://imgs.xkcd.com/comics/password_strength.png

This is a fork of the original `xkcdpass <

Wordlists provided by: https://github.com/LDNOOBW/List-of-Dirty-Naughty-Obscene-and-Otherwise-Bad-Words

Install
=======

``xkcdpass-offensive`` can be installed manually::

    git clone https://github.com/girino/Offensive-XKCD-password-generator
    cd Offensive-XKCD-password-generator
    git submodule update --init --recursive # to get the wordlist submodule
    python setup.py install



Source
~~~~~~
The latest development version can be found on github: https://github.com/LDNOOBW/List-of-Dirty-Naughty-Obscene-and-Otherwise-Bad-Words (it's a fork from the original at: https://github.com/redacted/XKCD-password-generator)



Requirements
============

Python 2 (version 2.7 or later), or Python 3 (version 3.4 or later). Running module unit tests on Python 2 requires ``mock`` to be installed.



Running ``xkcdpass``
====================

``xkcdpass`` can be called with no arguments::

    $ xkcdpass
    > pinball previous deprive militancy bereaved numeric

which returns a single password, using the default dictionary and default settings. Or you can mix whatever arguments you want::

    $ xkcdpass --count=5 --acrostic='chaos' --delimiter='|' --min=5 --max=6 --valid-chars='[a-z]'
    > collar|highly|asset|ovoid|sultan
    > caper|hangup|addle|oboist|scroll
    > couple|honcho|abbot|obtain|simple
    > cutler|hotly|aortae|outset|stool
    > cradle|helot|axial|ordure|shale

which returns

* ``--count=5``   5 passwords to choose from
* ``--acrostic='chaos'``   the first letters of which spell 'chaos'
* ``--delimiter='|'``   joined using '|'
* ``--min=5 --max=6``  with words between 5 and 6 characters long
* ``--valid-chars='[a-z]'``   using only lower-case letters (via regex).


A concise overview of the available ``xkcdpass`` options can be accessed via::

    xkcdpass --help

    Usage: xkcdpass [options]

    Options:
        -h, --help
                                    show this help message and exit
        -w WORDFILE, --wordfile=WORDFILE
                                    Specify that the file WORDFILE contains the list of
                                    valid words from which to generate passphrases. Multiple 
                                    wordfiles can be provided, separated by commas.
                                    Provided wordfiles: eff-long (default), eff-short,
                                    eff-special, legacy, spa-mich (Spanish), fin-kotus (Finnish)
                                    ita-wiki (Italian), ger-anlx (German), nor-nb (Norwegian),
                                    fr-freelang (French), pt-ipublicis / pt-l33t-ipublicis (Portuguese)
                                    swe-short (Swedish)
        --min=MIN_LENGTH
                                    Minimum length of words to make password
        --max=MAX_LENGTH
                                    Maximum length of words to make password
        -n NUMWORDS, --numwords=NUMWORDS
                                    Number of words to make password
        -i, --interactive
                                    Interactively select a password
        -v VALID_CHARS, --valid-chars=VALID_CHARS
                                    Valid chars, using regexp style (e.g. '[a-z]')
        -V, --verbose
                                    Report various metrics for given options, including word list entropy
        -a ACROSTIC, --acrostic=ACROSTIC
                                    Acrostic to constrain word choices
        -c COUNT, --count=COUNT
                                    number of passwords to generate
        -d DELIM, --delimiter=DELIM
                                    separator character between words
        -R, --random-delimiters
                                    use randomised delimiters
        -D DELIMITERS, --valid-delimiters=DELIMETERS
                                    delimeters to choose from, used with -
        -s SEP, --separator SEP
                                    Separate generated passphrases with SEP.
        -C CASE, --case CASE  
                                    Choose the method for setting the case of each word in
                                    the passphrase. Choices: ['alternating', 'upper',
                                    'lower', 'random', 'capitalize', 'as-is'] (default: 'lower').
        --allow-weak-rng     
                                     Allow fallback to weak RNG if the system does not
                                    support cryptographically secure RNG. Only use this if
                                    you know what you are doing.


Word lists
==========

Word lists are provided by https://github.com/LDNOOBW/List-of-Dirty-Naughty-Obscene-and-Otherwise-Bad-Words imported as a submodule. 
The word list is licensed under the `CC BY-SA 4.0 <https://creativecommons.org/licenses/by-sa/4.0/>`_ license. The dafault is "en" (for english) 
but you can use the following languages: "en", "es", "fi", "it", "de", "no", "fr", "pt", "sv", among others (see wordlist project for full list).

In a recent contribution by Foca Vey, we now have Tupi-Guarany words in the wordlist. use the flag "-w tupi" to enable this wordlist.

Using xkcdpass as an imported module
====================================

The built-in functionality of ``xkcdpass`` can be extended by importing the module into python scripts. An example of this usage is provided in `example_import.py <https://github.com/redacted/XKCD-password-generator/blob/master/examples/example_import.py>`_, which randomly capitalises the letters in a generated password. `example_json.py` demonstrates integration of xkcdpass into a Django project, generating password suggestions as JSON to be consumed by a Javascript front-end.

A simple use of import::

    from xkcdpass import xkcd_password as xp

    # create a wordlist from the default wordfile
    # use words between 5 and 8 letters long
    wordfile = xp.locate_wordfile()
    mywords = xp.generate_wordlist(wordfile=wordfile, min_length=5, max_length=8)

    # create a password with the acrostic "face"
    print(xp.generate_xkcdpassword(mywords, acrostic="face"))

When used as an imported module, `generate_wordlist()` takes the following args (defaults shown)::

    wordfile=None,
    min_length=5,
    max_length=9,
    valid_chars='.'

While `generate_xkcdpassword()` takes::

    wordlist,
    numwords=6,
    interactive=False,
    acrostic=False,
    delimiter=" "


Insecure random number generators
=================================
`xkcdpass` uses crytographically strong random number generators where possible (provided by `random.SystemRandom()` on most modern operating systems). From version 1.7.0 falling back to an insecure RNG must be explicitly enabled, either by using a new command line variable before running the script::

    xkcdpass --allow-weak-rng

or setting the appropriate environment variable::

    export XKCDPASS_ALLOW_WEAKRNG=1


Changelog
=========
- **0.0.1**  Numbering reset, since project was forked. Added support for offensive wordlists.
- **1.19.9** Remove usage of deprecated `assertEquals` in tests
- **1.19.8** Enables `python -m xkcdpass` usage
- **1.19.7** Adds Swedish wordlist, improvements to test suite, improvements to setup.py (excludes examples from install) 
- **1.19.6** Fixes randomly failing unit test
- **1.19.5** Adds "as-is" option for case
- **1.19.4** Makes randomised delimiters behavior consistent with fixed delimeters
- **1.19.3** Restore a randomly sampled version of eff_large_de wordlist 
- **1.19.2** Reduction in install size
- **1.19.1** Improvements to help text, handle rare case where arguments lead to empty wordlist
- **1.19.0** Initial support for multiple wordfiles
- **1.18.2** fixes for README
- **1.18.0** Added randomised delimiters
- **1.17.6** Bugfixes
- **1.17.5** Bugfixes
- **1.17.4** Improvements to French dictionary
- **1.17.3** Updated license and supported versions
- **1.17.2** Compatibility fix for 2.x/3.x 
- **1.17.1** Fix issue with README and unicode encoding
- **1.17.0** Add French, Norwegian, and Portuguese dictionaries. Bugfixes and improvements to tests (WIP).

License
=======
This is free software: you may copy, modify, and/or distribute this work under the terms of the BSD 3-Clause license.
See the file ``LICENSE.BSD`` for details.
