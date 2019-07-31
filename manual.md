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
bustools 0.39.3

Usage: bustools <CMD> [arguments] ..

Where <CMD> can be one of: 

capture         Capture records from a BUS file
correct         Error correct a BUS file
count           Generate count matrices from a BUS file
inspect         Produce a report summarizing a BUS file
linker          Remove section of barcodes in BUS files
project         Project a BUS file to gene sets
sort            Sort a BUS file by barcodes and UMIs
text            Convert a binary BUS file to a tab-delimited text file
whitelist       Generate a whitelist from a BUS file

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

### Correct

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

### Count

BUS files can be converted into a barcode-feature matrix, where the feature can be TCCs (Transcript Compatibility Counts) or genes. The output is in a standard [Matrix Market Exchange format](https://math.nist.gov/MatrixMarket/formats.html#MMformat)

~~~
Usage: bustools count [options] sorted-bus-files

Options: 
-o, --output          File for corrected bus output
-g, --genemap         File for mapping transcripts to genes
-e, --ecmap           File for mapping equivalence classes to transcripts
-t, --txnames         File with names of transcripts
--genecounts          Aggregate counts to genes only
-m, --multimapping    Include bus records that pseudoalign to multiple genes
~~~

The input BUS file needs to be sorted via the `sort` command.

The `correct` command accepts `-` as an input file to read from `stdin`, but does not write to `stdout`.

By default `count` does not count records that map to multiple genes, to include records that map to multiple genes use the `-m` flag, this results in the count being split equally by the number of genes it maps to.


### Capture

`bustools capture` can separate BUS files into multiple files according to the capture criteria 

~~~
Usage: bustools capture [options] bus-files

Options: 
-o, --output          File for captured output 
-x, --complement      Take complement of captured set
-c, --capture         Capture list
-e, --ecmap           File for mapping equivalence classes to transcripts
-t, --txnames         File with names of transcripts
-s, --transcripts     Capture list is a list of transcripts to capture
-u, --umis            Capture list is a list of UMIs to capture
-b, --barcode         Capture list is a list of barcodes to capture
-p, --pipe            Write to standard output

~~~

The `capture` command accepts `-` as an input file to read from `stdin`, and can write to `stdout` using `-p`. The BUS file is filtered by the `--capture` list, which can specify a list of transcripts (`-s`), a list of barcodes (`-b`), or UMIs (`-u`). The default behaviour is to include only records that match the capture list, to reject records from the capture list the user can specify `-x`. Matching on barcodes and UMIs is exact. Matching on equivalence classes with respect to the capture list is an intersection.


### Inspect

`bustools inspect` gives a report summarizing the contents of a sorted BUS file can be output either to standard out or to a JSON file for further analysis using bustools inspect.

~~~
Usage: bustools inspect [options] sorted-bus-file

Options:
-o, --output          File for JSON output (optional)
-e, --ecmap           File for mapping equivalence classes to transcripts
-w, --whitelist       File of whitelisted barcodes to correct to
-p, --pipe            Write to standard output
~~~

The input BUS file must be sorted via the `sort` command.

Sample output

~~~
Read in 3148815 BUS records
Total number of reads: 3431849

Number of distinct barcodes: 162360
Median number of reads per barcode: 1.000000
Mean number of reads per barcode: 21.137281

Number of distinct UMIs: 966593
Number of distinct barcode-UMI pairs: 3062719
Median number of UMIs per barcode: 1.000000
Mean number of UMIs per barcode: 18.863753

Estimated number of new records at 2x sequencing depth: 2719327

Number of distinct targets detected: 70492
Median number of targets per set: 2.000000
Mean number of targets per set: 3.091267

Number of reads with singleton target: 1233940

Estimated number of new targets at 2x seuqencing depth: 6168

Number of barcodes in agreement with whitelist: 92889 (57.211752%)
Number of reads with barcode in agreement with whitelist: 3281671 (95.623992%)
~~~


### Linker

`bustools linker` removes specified section of barcode in BUS files.

~~~
Usage: bustools linker [options] bus-files

Options: 
-s, --start           Start coordinate for section of barcode to remove (0-indexed, inclusive)
-e, --end             End coordinate for section of barcode to remove (0-indexed, exclusive)
-p, --pipe            Write to standard output
~~~

If `--start` is `-1`, the removed section begins at beginning of barcode. Likewise, if `--end` is `-1`, the removed section ends at the end of the barcode. BUS files should contain barcodes of the same length.


### Project

The kallisto bus command maps reads to a set of transcripts. bustools project takes as input a sorted BUS file and a transcript to gene map (`tr2g` file), and outputs a BUS file, a `matrix.ec` file, and a list of genes, which collectively map each read to a set of genes.

~~~
Usage: bustools project [options] sorted-bus-file

Options: 
-o, --output          File for project bug output and list of genes (no extension)
-g, --genemap         File for mapping transcripts to genes
-e, --ecmap           File for mapping equivalence classes to transcripts
-t, --txnames         File with names of transcripts
-p, --pipe            Write to standard output
~~~

The input BUS file must be sorted via the `sort` command.

### Whitelist

`bustools whitelist` generates a whitelist based on the barcodes in a sorted BUS file.

~~~
Usage: bustools whitelist [options] sorted-bus-file

Options: 
-o, --output        File for the whitelist
-f, --threshold     Minimum number of times a barcode must appear to be included in whitelist
~~~

The input BUS file must be sorted via the `sort` command.

`--threshold` is a  optional parameter. If not provided, bustools whitelist will determine a threshold based on the first `200` to `100200` records.
