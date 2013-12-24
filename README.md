#fagrep
This is a modified version of GNU grep that allows it to print out
fasta/fastq records.  The modifications a basic and currently most of
the options are broken or I've not tried to see if they are broken.

I'd love contributions by anyone who knows a bit of C and has some time
up their hands.

## Usage
for printing fasta records from fasta formatted files:
```
grep --fasta ...
```
for printing fastq record from fastq formatted files:
```
grep --fastq ...
```
without the fasta/q options this version of grep should (I hope) perform
identically to regular GNU grep.

## Current known WORKING options
```
--color
-c
-l
-n
-h/-H
```
## Current known NOT WORKING options
```
-v
```

## Options that may not make sense without line orientated output
Grep can prefix lines with a variety of things such as the line number,
file offset, file name.  These options work but they are printed before
the record separator and not on any of the following lines of the
record, which makes the output have invalid formatting.
