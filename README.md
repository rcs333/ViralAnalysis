# VAPiD
VAPiD is a ultra-lightweight script for quickly annotating and preparing sequences of well characterized human viruses for sending to NCBI Genbank. All you need to get started is a working installation of Python, some fasta files of human viruses and an NCBI generated author list.

Currently tested and working on Windows 10, Ubuntu 10.4, and Mac OS X.

Viruses that VAPiD has been tested with:
Nipah, Sendai, Measles, Mumps, Parainfluenzas, Ebola, Rotavirus, Coronaviruses, West Nile Virus, HTLV, HIV, Hepatitis, Norovirus, Enterovirus, JC, BK. 

# Installation
Installation differs greatly for Unix systems and for Windows

**Mac or Linux**

1. Install all dependencies (Shoutout to the wonderful people who wrote these!)

Python - tested almost exclusively on 2.7.4 python 3 and above have syntax issues and actually break when you try to manually enter metadata.

If you're running Python < 2.7.4 follow this guide to install pip (You may need administrator privileges.)
https://pip.pypa.io/en/stable/installing/ 

Download the get-pip.py and run it from the command line by typing `python get-pip.py`

If you're using Python >= 2.7.4 or you've successfully installed pip:
Open a command line and type

`pip install numpy`

`pip install biopython`

Now download and unzip this repository to your computer (anywhere will work)

Then you'll need to install tbl2asn and put it on your path. (Another option is to download tbl2asn and simply unpack it directly into the folder you've unpacked this repository into.) 
tbl2asn can be found at https://www.ncbi.nlm.nih.gov/genbank/tbl2asn2/

**Windows**

1. Install Python 2.7.14 https://www.python.org/downloads/
  Make sure that when this is installing on the third step 'Customize your python installation' the box that says put python on your path is checked. This makes installing and running this annotator much easier. 
  If no such box exists, open a command prompt window and type:

`set PATH=%PATH%;C:\python27\`

  or change C:\python27\ to wherever Python was downloaded.  

2. Install Numpy and Biopython 

`python -m pip install numpy`

`python -m pip install biopython`


3. Then download and unzip this repository (which can be downloaded with the green link on the right to the top of the page) to your computer (anywhere will do)

4. Download tbl2asn for windows https://www.ncbi.nlm.nih.gov/genbank/tbl2asn2/  and unzip it, then copy and paste every single file in the tbl2asn folder (there should be like 7 .dlls and an .exe that is simply called tbl2asn) into the folder that you just made in step 3.

**After the above has completed there's still a bit of setup that needs to be done.** 

1. Download MAFFT https://mafft.cbrc.jp/alignment/software/ for your appropriate system. (THIS IS ALREADY DONE FOR WINDOWS and is included under the GPL licence) Make sure you select the all-in-one version. 

2. Unzip the folder and place all files in the folder where this repository is living.

2. A example.sbt file that contains your name and information about the organization that you wish to submit your sequences to NCBI under. This .sbt file can be generated by filling out the form here https://submit.ncbi.nlm.nih.gov/genbank/template/submission/

3. Put the newly generated .sbt file onto your computer. (Generally it's easiest to just put it in the VAPiD folder). If you'll be submitting multiple sequences from different people you can generate more than one, put them all in this folder, and choose which one you want to use at run time. 

4. (Optional)
You can generate a .csv file with most metadata that you wish to associate with your sequences. The file should have across the top Strain (the name of the fasta sequences that you'll import) followed by columns with NCBI approved metadata https://www.ncbi.nlm.nih.gov/Sequin/modifiers.html
 
An important note is that the strain names in the metadata csv MUST match the sequence names in your fasta files. (i.e. the part after the >) 

An example metadata file has been provided under the name example_metadata.csv - this metadata file will work with the example fasta file provided (example.fasta).

If you don't have very many sequences at a time or you include a fasta not in your metadata sheet the program will automatically prompt you for strain name, collection date, country of collection and coverage (for ngs reads). You must fill out strain name, collection date and location or NCBI will not accept your submission. Coverage is not necessary and if skipped during the automatic prompting, will not create any issues.

# Usage - vapid.py
**General Usage**

`python vapid.py fasta_file_path author_template_path --metadata_loc metadata_info_path`

**Example Usage (With metadata in the sheet)**

`python vapid.py example.fasta example.sbt --metadata_loc example_metadata.csv`

**Example Usage (No metadata sheet)**

`python vapid.py example.fasta example.sbt`


Create your fasta file with all of the sequences that you would like to annotate. You can have as many sequences as you want. And you should name the sequences in your fasta file what you would like the strain name to be. (For the provided fasta file, example.fasta, this name would be 'test'). **FASTA STRAIN NAMES SHOULD NOT HAVE WHITESPACE OR SPECIAL CHARACTERS IN THEM!!**

Then you would need to run the vapid.py script from the command line, i.e. cd to the directory that this is living in - if you cloned from github it'll be VAPiD/ 

`python vapid.py -h` prints out a list of arguments and some help information

The last argument is optional and if you don't provide a metadata location the program will prompt you for it.

You can just put relative paths to your sbt and fasta file, I find it is easiest to store everything in the ClinVirusSeq folder - that way you don't have to worry about typing paths. 

# Output

The program will run for a bit and generate a folder for each sequence in your fasta file. The folder will be named the same as what you named the strain in the fasta file. (So if the first line of your fasta file is >SC12309 then there will be a folder called SC12309 with all of the .gbf, .tbl, and ect files that you need and love.) 

tbl2asn will also spit out some errors and warnings onto the command line.

The program itself will also examine each sequence record for stop codons and notify you of which ones contain stops.

Inside each folder will be some files, the .sqn file is what you email to gb-sub@ncbi.nlm.nih.gov and then shortly after your sequences will be deposited on NCBI. 

# Implementation Details and Important Notes

Right now we accomplish the annotation by searching blast for the best hit that is a complete genome and using a maaft alignment to generate annotations for the supplied sequence. We can handle some ribosomal slippage as well as RNA editing in all human viruses. Coronavirus 229E and HPIV3 are annotated off hardcoded records due to variability in accuracy for these records in blast. - Following this note - a large problem is actually inconsistent spelling in genbank sequence records or sequence records that do not have every protein annotated. I continually try to update this program to deal with problems that come up, but this is a major source of error right now.


# Future directions
Mainly just bugfixing and streamlining, paper writing in progress.
