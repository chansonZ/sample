sample
======
| Status | Conda Downloads | Conda Version | Platforms |
| --- | --- | --- | --- |
| [Build Status](http://travis-ci.org/hroptatyr/sample) | [![Conda Downloads](https://img.shields.io/conda/dn/conda-forge/sample.svg)](https://anaconda.org/conda-forge/sample) | [![Conda Version](https://img.shields.io/conda/vn/conda-forge/sample.svg)](https://anaconda.org/conda-forge/sample) | [![Conda Platforms](https://img.shields.io/conda/pn/conda-forge/sample.svg)](https://anaconda.org/conda-forge/sample) |


Produce a sample of lines from files.  The sample size is either fixed
or proportional to the size of the file.  Additionally, the header and
footer can be included in the sample.


Red tape
--------

- no dependencies other than a POSIX system and a C99 compiler.
- licensed under [BSD3c][1]


Features
--------

- proportional sampling of streams and files
- header and footer can be included in the sample
- reservoir sampling (fixed sample size) of streams and files
- stable reservoir sampling (i.e. the order is preserved)


Motivation
----------

Practically ubiquitous, there's `shuf -n` of [GNU coreutils][5], a tool
that, in principle, solves the problem at hand.  However, `shuf` buffers
all input and is therefore useless for files that don't fit in memory.

So, looking for alternatives one may come across [paulgb's subsample][2]
or [earino's fast_sample][3].  They usually do the trick and everyone
seems to agree (judged by github stars).  However, both tools have
short-comings: they try to make sense of the line data semantically, and
secondly, they are slow!

The first issue is such a major problem that their bug trackers are
full of reports.  `subsample` needs lines to be UTF-8 strings and
`fast_sample` wants CSV files whose correctness is checked along the
way.  This project's tool, `sample`, on the other hand does not care
about the line's content, all it needs are those line breaks at the
end.

The speed issue is addressed by

- using the most appropriate programming language for the problem
- using radix sort
- using the [PCG family][4] to obtain randomness
- oversampling

Installation
--------

    $ autoreconf -vfi
    $ ./configure
    $ make


Examples
--------

To get 10 random words from the `words` file:

    $ sample -n 10 -H 0 /usr/share/dict/words
    ...
    benzopyrene
    calamondins
    cephalothorax
    copulate
    garbology's
    Kewadin
    Peter's
    reassembly
    Vienna's
    Wagnerism's
    ...

The `-H 0` produces 0 lines of header output which defaults to 5.

For proportional sampling use `-r|--rate`:

    $ wc -l /usr/share/dict/words
    305089
    $ sample -r 1% /usr/share/dict/words | wc -l
    3080

which is close to the true result bearing in mind that by default the
header and footer of the file is printed as well.

Sampling with a rate of 0 replaces awkward scripts that use multios
and `head` and `tail` to produce the same result.

    $ sample -r 0 /usr/share/dict/words
    A
    AA
    AAA
    Aachen
    aah
    ...
    Zyuganov
    Zyuganov's
    zyzzyva
    zyzzyvas
    ZZZ


Similar projects
================

In no particular order and without any claim to completeness:

+ subsample: <https://github.com/paulgb/subsample>
+ fast_sample: <https://github.com/earino/fast_sample>
+ shuf: <https://www.gnu.org/software/coreutils/coreutils.html>


  [1]: http://opensource.org/licenses/BSD-3-Clause
  [2]: https://github.com/paulgb/subsample
  [3]: https://github.com/earino/fast_sample
  [4]: http://www.pcg-random.org/
  [5]: https://www.gnu.org/software/coreutils/coreutils.html
