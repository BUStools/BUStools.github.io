---
layout: page
title: "Manual"
description: ""
group: navigation
---
{% include JB/setup %}

Typing `bustools` produces a list of usage options, which are:

To see a list of available commands type `bustools` in the terminal

~~~
bustools 0.39.1

Usage: bustools <CMD> [arguments] ..

Where <CMD> can be one of: 

sort            Sort bus file by barcodes and UMI
text            Output as tab separated text file
correct         Error correct bus files
count           Generate count matrices from bus file
capture         Capture reads mapping to a transcript capture list

Running bustools <CMD> without arguments prints usage information for <CMD>

~~~

### Streaming

Many of the `bustools` commands can read from the standard input (`stdin`), by specifying `-` as the input file and write to standard output (`stdout`) using the `-p` flag if available. Note that in many cases using `-p` will write a binary file to `stdout` which may garble your terminal unless parsed by a subsequent `bustools` command. 


### Sorting

Raw BUS output from pseudoalignment programs may be unsorted. To simply and accelerate downstream processing BUS files can be sorted using `bustools sort`

~~~
Usage: bustools sort [options] bus-files

Options: 
-t, --threads         Number of threads to use
-m, --memory          Maximum memory used
-T, --temp            Location and prefix for temporary files 
                      required if using -p, otherwise defaults to output
-o, --output          File for sorted output
-p, --pipe            Write to standard output
~~~

This will create a new BUS file where the BUS records are sorted by barcode first, UMI second, and equivalence class third. Maximum memory used for sorting can be specified using `-m` with an `M` or `G` suffix with a default of `2G`. If small numbers (less than 128) are used this is interpreted as gigabytes, and the minumum memory used is `64M`.

In cases where the input BUS file does not fit into memory the intermediate results are sorted and written to disk, finally all the intermediate results are merged to the sorted BUS output file. By default `bustools` will use the same location as the output file for storing the temporary files. This can be overwritten using the `-T` flag to specify where temporary storage is available, the value can either be a filename or a folder. All intermediary files are removed once the output has been constructed.

Sorting accepts `-` as an input file to read from `stdin` and can write to `stdout` using the `-p` flag. When specifying the `-p` flag the `-T` is required (for most systems using `-T /tmp` is a sensible default).

### Text

BUS files can be converted to a tab-separated format for easy inspection and processing using shell scripts or high level languages. `bustools text` 

~~~
Usage: bustools text [options] bus-files

Options: 
-o, --output          File for text output
-p, --pipe            Write to standard output

~~~

The output is written either to an output file specified with `-o` or to `stdout` using `-p`. The `text` command accepts `-` as an input file to read from `stdin`.

### correct

BUS files can be barcode error corrected w.r.t. a technology specific whitelist of barcodes. The `correct` command will correct all barcodes that are at Hamming distance 1 (i.e. one substitution) away from a single barcode in the whitelist.

~~~
Usage: bustools correct [options] bus-files

Options: 
-o, --output          File for corrected bus output
-w, --whitelist       File of whitelisted barcodes to correct to
-p, --pipe            Write to standard output
~~~

The BUS input file does not need to be sorted.

The `correct` command accepts `-` as an input file to read from `stdin`, and can write to `stdout` using `-p`. 

### count

BUS files can be converted into a barcode-feature matrix, where the feature can be TCCs (Transcript Compatibility Counts) or genes. The output is in a standard [Matrix Market Exchange format](https://math.nist.gov/MatrixMarket/formats.html#MMformat)

~~~
Usage: bustools count [options] bus-files

Options: 
-o, --output          File for corrected bus output
-g, --genemap         File for mapping transcripts to genes
-e, --ecmap           File for mapping equivalence classes to transcripts
-t, --txnames         File with names of transcripts
--genecounts          Aggregate counts to genes only
~~~

The input BUS file needs to be sorted via the `sort` command.

The `correct` command accepts `-` as an input file to read from `stdin`, but does not write to `stdout`.


### Capture

`bustools capture` can separate BUS files into multiple files according to the capture criteria 

~~~
Usage: bustools capture [options] bus-files

Options: 
-o, --output          Directory for output 
-c, --capture         List of transcripts to capture
-e, --ecmap           File for mapping equivalence classes to transcripts
-t, --txnames         File with names of transcripts
~~~

The input BUS file must be sorted via the `sort` command.

The `capture` command accepts `-` as an input file to read from `stdin`, but does not write to `stdout`. The BUS file is split into two parts, `captured` and `split` in the output folder, both with their own `.bus` and `.ec` parts. The `captured` part contains all BUS records where each of the records with identical barcode and UMIs contain equivalence classes that are all consistent with transcripts from the capture list. The `split` part contains the remainder of the BUS records.


