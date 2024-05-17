# Assignment-01
files and code used in this Assignment 

**Practical Work Report**

I order to make this Assignment, I gathered the sequences from the research article (Exploring the Radiation of a Diverse Reef Fish Family: Phylogenetics of the Damselfishes (Pomacentridae), with New Classifications Based on Molecular Analyses of All Genera - ScienceDirect, n.d.) in NIH database using the accession numbers provided in the paper. 

First of all, I had to download the sequence files (with all taxas included) and separate the species manually to another fasta file, according to the two different phylogenetic trees I had to remake. After 6 hours of suffering I used the software Aliview to check if the sequences were aligned just to find out they weren't. So I used mafft software to align those sequences and proceeded to the file treatment. In this step I wanted to remove the header of the sequences leaving just the taxa names so I used two CMD scripts to do this for me. With the files cleaned, I moved to the alignments concatenation. For this, we used the MEGA software to concatenate and create a phylogenetic tree. We tried to test the phylogeny using MrBayes software, but for some reason the NEXUS file we created from concatenation, wasn't in the right format and that's why we opted for MEGA (and because it was closer in terms of likeliness in relation to the article's trees).

**Commands and Scripts**

Command used to align the sequences using mafft:

```bash
mafft --auto input.fasta > output.fasta
```
To install MrBayes:
```bash
git clone https://github.com/NBISweden/MrBayes/tree/develop
cd MrBayes 
./configure
make && sudo make install
```
To install Aliview: https://ormbunkar.se/aliview/downloads/

The simplest install is to download the file: aliview.install.run

after downloading you will likely need to change the execution rights of the install file:
```bash
chmod +x aliview.install.run
```
install by issuing command:
```bash
sudo ./aliview.install.run
```
start the program with command:
```bash
aliview 
```
To install MEGA software:

https://www.megasoftware.net/dload_debian_gui

after downloading the compressed file, use this command to extract:
```bash
tar xfvz Mega.tgz 
```
For the alignments treatment, we used two CMD scripts: 
```bash
# This code inputs the fasta file you want, and outputs the sequence without the first 11 characters after ">" sign, removing the accession numbers
@echo off
setlocal enabledelayedexpansion

rem Input and output file names
set "input_fasta=algnFirstTreeRAG-2.fasta"       # you choose the file by typing its name here 
set "output_fasta=algnFirstTreeRAG-2Processed.fasta"

rem Clear the output file if it exists
if exist "%output_fasta%" del "%output_fasta%"

rem Process each line in the input FASTA file
for /f "usebackq tokens=*" %%A in ("%input_fasta%") do (
    set "line=%%A"
    rem Check if the line starts with '>'
    if "!line!" neq "" (
        if "!line:~0,1!" == ">" (
            rem Remove the first 11 characters after '>'
            set "line=>!line:~12!"
        )
    )
    rem Append the processed line to the output file
    echo !line!>>"%output_fasta%"
)

endlocal
```
and 
```bash
# This code inputs the fasta file you want, and outputs the sequences with only the taxa name
@echo off
setlocal enabledelayedexpansion

rem Input and output file names
set "input_fasta=algnFirstTreeRAG-2Processed.fasta"     # you choose the file by typing its name here 
set "output_fasta=algnFirstTreeRAG-2Processed2.fasta"

rem Clear the output file if it exists
if exist "%output_fasta%" del "%output_fasta%"

rem Process each line in the input FASTA file
for /f "usebackq tokens=*" %%A in ("%input_fasta%") do (
    set "line=%%A"
    rem Check if the line starts with '>'
    if "!line!" neq "" (
        if "!line:~0,1!" == ">" (
            rem Extract the species name (first two words after '>')
            set "header=!line:~1!"
            for /f "tokens=1,2" %%B in ("!header!") do (
                set "line=>%%B %%C"
            )
        )
    )
    rem Append the processed line to the output file
    echo !line!>>"%output_fasta%"
)

endlocal
```
