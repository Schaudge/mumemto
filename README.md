# **mumento**: finding multi-MUMs and MEMs using prefix-free parsing for big BWTs

<img src="logo.png" alt="logo" width="280" align="left"/>
This code is based on the [pfp-thresholds](https://github.com/maxrossi91/pfp-thresholds) repository written by [Massimiliano Rossi](https://github.com/maxrossi91) and [docprofiles](https://github.com/oma219/docprofiles) repository written by [Omar Ahmed](https://github.com/oma219). 

This tool identifies **maximal unique matches (multi-MUMs)** present across a collection of sequences. Multi-MUMs are defined as maximally matching substrings present in each sequence in a collection *exactly once*. Additionally, this tool can identify **multi-MEMs**, maximal exact matches present across sequences, without the uniqueness property. This method is uses the prefix-free parse (PFP) algorithm for suffix array construction on large, repetitive collections of text.

This tool uses PFP to efficiently identify multi-MUM/MEMs. Note that this applies only to highly repetitive texts (such as a collection of closely related genomes, likely intra-species such as a pangenome). We plan to support multi-MUM/MEM finding in more divergent sequences (inter-species, etc.) soon, however this would be less efficient without the PFP pre-processing step.


## Installation

For starting out, use the commands below to download the repository and build the executable. After running the make command below,
the `pfp_mum` executable will be found in the `build/` folder.

```sh
git clone git@github.com:vshiv18/pfp-mum.git
cd pfp-mum

mkdir build 
cd build && cmake ..
make install
```

## Getting started

The basic workflow with `pfp_mum` is to compute the PFP over a collection of sequences, and identify multi-MUMs while computing the SA/LCP/BWT of the input collection. 

### Find multi-MUMs

```sh
pfp_mum -f <input_list> -o <output_prefix>
```
or 
```sh
pfp_mum -o <output_prefix> [input_fasta [...]]
```

The command above takes in a file-list of multiple genomes or a list of positional arguments and then generates output files using the output prefix. In the file-list, you can specify a list of 
genomes and then specify which document/class each genome belongs in. If you pass in fastas as positional arguments, a filelist will be created which contains the order of the sequences in the output *.mums* file.

**Example of file-list file:**
```sh
/path/to/ecoli_1.fna 1
/path/to/salmonella_1.fna 2
/path/to/bacillus_1.fna 3
/path/to/staph_2.fna 4
```

**Format of the \*.mums file:**
```sh
[MUM length] [comma-delimited list of offsets within each sequence, in order of filelist] [comma-delimited strand indicators (one of +/-)]
```
The `*.mums` file contains each MUM as a separate line, where the first value is the match length, and the second is 
a comma-delimited list of positions where the match begins in each sequence. An empty entry indicates that the MUM was not found in that sequence (only applicable with *-k* flag). The MUMs are sorted in the output file
lexicographically based on the match sequence.
