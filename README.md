Open IE
======
This project contains the principal Open Information Extraction (Open IE)
system from the University of Washington (UW).  An Open IE system runs over
sentences and creates extractions that represent relations in text.  For
example, consider the following sentence.

    The U.S. president Barack Obama gave his speech on Tuesday to thousands of people.

There are many binary relations in this sentence that can be expressed as a
triple `(A, B, C)` where `A` and `B` are arguments, and `C` is the relation
between those arguments.  Since Open IE is not aligned with an ontology, the
relation is a phrase of text.  Here is a possible list of the binary relations
in the above sentence:

    (Barack Obama, is the president of, the U.S.)
    (Barack Obama, gave, his speech)
    (Barack Obama, gave his speech, on Tuesday)
    (Barack Obama, gave his speech, to thousands of people)

The first extraction in the above list is a "noun-mediated extraction", because
the extraction has a relation phrase is described by the noun "president".  The
other extractions are very similar.  In fact, they can be represented more
informatively as an n-ary extraction.  An n-ary extraction can have 0 or more
secondary arguments.  Here is a possible list of the n-ary relations in the
sentence:

    (Barack Obama, is the president of, the U.S.)
    (Barack Obama, gave, [his speech, on Tuesday, to thousands of people])

Extractions can include more than just the arguments and relation as well.  For
example, we might be interested in whether the extraction is a negative
assertion or a positive assertion, or if it is conditional in some way.
Consider the following sentence:

    Some people say Barack Obama was born in Kenya.

We would not want to extract that `(Barack Obama, was born, in Kenya)` alone
because this is not true.  However, if we have the condition as well, we can
have a correct extraction.

    Some people say:(Barack Obama, was born in, Kenya)

To see an example of Open IE being used, please visit http://openie.cs.washington.edu/.

## Notifications

* [01/15/2016] The version 4.2.0 is released ([release notes](https://github.com/knowitall/openie/blob/master/release/release_notes.md)).

## Google Group

* [knowitall_openie](https://groups.google.com/forum/#!forum/knowitall_openie)

## Research

Open IE 4.x is the successor to [Ollie](http://www.gitub.com/knowitall/ollie).
Whereas Ollie used bootstrapped dependency parse paths to extract relations
(see [Open Language Learning for Information Extraction](https://homes.cs.washington.edu/~mausam/papers/emnlp12a.pdf)),
Open IE 4.x uses similar argument and relation expansion heuristics to create
Open IE extractions from SRL frames.  Open IE 4.x also extends the defintion of
Open IE extractions to include n-ary extractions (extractions with 0 or more arguments 2s).

## Buiding

`openie` uses java-7-openjdk & the [sbt build system](http://www.scala-sbt.org/), so downloading
dependencies and compiling is simple.  Just run:

    sbt compile

## Running

You can run `openie` with sbt or create a stand-alone jar.  `openie` requires
substantial memory.  `sbt` is configured to use these options by default:

   -Xmx4G -XX:+UseConcMarkSweepGC

### Running with sbt

    sbt 'run-main edu.knowitall.openie.OpenIECli'

### Running from a stand-alone jar.

First create the stand-alone jar.

    sbt clean compile assembly

You may need to add the above memory options.

    sbt -J-Xmx2700M clean compile assembly

Then you can run the resulting jar file as normal.

    java -jar openie-assembly.jar

You may need to add the above memory options.

    java -Xmx4g -XX:+UseConcMarkSweepGC -jar openie-assembly.jar

### Command Line Interface

`openie` takes one sentence per line unless `--split` is specified.  If
`--split` is specified, the input text will be split into sentences.  You can
either pipe input from Standard Input, specify an input file (an option first
argument), or type sentences interactively.  Output will be written to Standard
Output unless a second option argument is specified for an output file.

`openie` takes a number of command line arguments.  To see them all run
`java -jar openie-assembly.jar --usage`.  Of particular interest are
`--ignore-errors` which continues running even if an exception is encountered, `--binary` which gives the binary(triples) output and `--split` which splits the input document text into sentences.

There are two formats--a simple format made for ease of reading and a
columnated format used for machine processing.  The format can be specified
with either `--format simple` or `--format column`.  The simple format is
chosen by default.

### Java Demo

A simple java demo which uses openIE (https://github.com/OpenIE-HelperCodes/OpenIEDemo1)

#### Simple Format

```
> John ran down the road to fetch a pail of water.
John ran down the road to fetch a pail of water.
0.86 (John; ran; down the road; to fetch a pail of water)
0.82 John ran:(John; ran down the road to fetch; a pail of water)
```

#### Columnated Format

Columns are separated by tab, making it hard to read in this README.

```
0.8576784836790008	John	ran	down the road; to fetch a pail of water	John ran down the road to fetch a pail of water.
0.8195727266148489	John ran	John	ran down the road to fetch	a pail of water	John ran down the road to fetch a pail of water.
```

## Contributors
* Michael Schmitz (http://www.schmitztech.com/)
* Harinder Pal (http://www.cse.iitd.ac.in/~mcs142123/)
* Bhadra Mani

