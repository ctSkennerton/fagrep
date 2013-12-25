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

## Fasta and Fastq Parsing
GNU grep is fast for a number of reasons but one of them is that it
reads a file into a large buffer then searches the entire buffer.  If it
finds a match grep then finds the beginning and end of a line and prints
it out.  Consider the opposite scenario where the file is parsed first
to identify the bounds of each fasta or fastq record, after which a
search can then take place.  The grep way is faster because there will
probably be large portions of an input file that will not contain a
match and therefore the parse-then-search method will send alot of time
reading in records that will never need to be printed out.

When switching from line orientated output to record output using
`--fasta` or `--fastq`, the following logic is used:

When in fasta mode, fagrep will look for the '>' character and set this
as the bounds of the record.  This character is assumed to be unique so
if it exists in the sequence name or description it will cause
formatting errors.

When in fastq mode, fagrep will look for the '@' at the beginning of a
line that is not preceeded by the '+' character.  This means that the
fastq file must not contain any extra information in the 'comment' line,
the one that begins with '+'.
  
```
@seq1
AAAAA
+
hahah
@seq2
TTTTT
+
@haah  <- An @ char as the first qual will not match
@seq3
GGGGG
+seq3
@haha <- This will be called the start of a record by accident

```

## Current known WORKING options
```
--color
-c
-l
-n
-h/-H
-v
```
## Current known NOT WORKING options
```
-v with -c, but either on their own is fine
-A
-B
-C
```

## Options that may not make sense without line orientated output
Grep can prefix lines with a variety of things such as the line number,
file offset, file name.  These options work but they are printed before
the record separator and not on any of the following lines of the
record, which makes the output have invalid formatting.
