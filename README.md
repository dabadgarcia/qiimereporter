[![Anaconda-Server Badge](https://anaconda.org/dabadgarcia/qiimereporter/badges/version.svg)](https://anaconda.org/dabadgarcia/qiimereporter)  
[![Anaconda-Server Badge](https://anaconda.org/dabadgarcia/qiimereporter/badges/latest_release_date.svg)](https://anaconda.org/dabadgarcia/qiimereporter)

<br>

## Index  
  * [About](#about)
  * [Installation](#installation)
  	* [Install Miniconda](#install-miniconda)
	* [Install QiimeReporter](#install-qiimereporter)
  * [Usage](#usage)
  * [Metadata](#metadata)  
  * [Output](#output)
  * [Already using Qiime2?](#already-using-qiime2)
  * [Citation](#citation)
  * [License](#license)

<br>

## About

QiimeReporter is a straightforward pipeline for the analysis of amplicon sequences directly from raw Illumina paired-end data. It integrates the main [Qiime2](https://github.com/qiime2/qiime2) commands with [R](https://cran.r-project.org/) in order to generate a final html report that can be opened in any web browser and easily shared between researchers.  

<br>

## Installation

QiimeReporter has been developed as a [Conda](https://docs.conda.io/projects/conda/en/latest/user-guide/install/) environment to make things easier. 

### Install Miniconda

[Miniconda](https://docs.conda.io/en/latest/miniconda.html) provides the [Conda](https://docs.conda.io/projects/conda/en/latest/user-guide/install/) environment and package manager, and is the recommended way to install QiimeReporter: 

```
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
bash Miniconda3-latest-Linux-x86_64.sh
conda update conda
```

### Install QiimeReporter

Once you have [Miniconda](https://docs.conda.io/en/latest/miniconda.html) installed, run the following commands to install QiimeReporter:

```
conda create -n qiimereporter
conda activate qiimereporter
conda install -c dabadgarcia qiimereporter
qiimereporter-setup
```
<br>

## Usage
```
usage: qiimereporter <options>

OBLIGATORY OPTIONS:
	-i/--input 	Path to the directory where the raw reads are located
	-m/--metadata	Path to the file with the metadata regarding the samples
			The file must have an specific organization for the program to work
			If you don't have any or you would like to have an example or extra information, 
			please type: 
			$0 example-metadata
	-o/--output	Path and name of the output directory

OTHER OPTIONS:
	--citation	Show citation
	--color 	Color multivariate analysis from a categorical variable in sample-metadata 
	-d/--database 	Path to prefered database, must be .qza file (default SILVA132)
	--depth		Minimun number of reads for a sample to be included in the rarefaction figure
	-f/--forward	Truncates length of the forward reads (default='0')
	--group 	Column number of the variable in sample-metadata to group the taxa-bar-plot 
	-h/--help	Show this help
	--no-diversity	Excludes the diversity index figures from the report
	--no-heatmap	Excludes the heatmap from the report
	--no-network	Excludes the network from the report
	--no-pcoa	Excludes the pcoa figures from the report
	--no-rarefactionExcludes the rarefaction curves from the report
	--multivariate	Includes other multivariate analysis in the report (NMDS, DCA, CCA, RCA)
	--report-only	To generate only html report document, must be executed in the report_files directory
	-r/--reverse	Truncates length of the reverse reads (default='0')
	-t/--threads	Number of threads to use (default=$CPUS) <integer>
	--title		Path to a file containing the title of the report, avoid using special characters 
			QiimeReporter will use a default title if this option is not passed
	-v/--version	Show version

```
<br>

Example:
```
qiimereporter -i raw_reads -m sample-metadata.tsv --output output_folder -t 32
```
<br>

## Metadata

A Qiime2 metadata text file is needed for QiimeReporter to work by using the `-m/--metadata` option. This file will include all the information regarding the sample and requires an specific organization:  
- Columns must be tab separated
	- First row: 
		- First column must me called "#SampleID" and harbor samples names (avoid special characters)
		- Second column must be called "BarcodeSequence" and should be left blank
		- Third column must be called "LinkerPrimerSequence" and should be left blank
		- Fourth (and so on) columns are descriptive. They are no needed for the program but the information will be used and appear in the       report. Add as much as needed
	- Second row: 
		- First column must me called "#q2:types" and specifies type of variable for each column
		- Columns should be clasiffied into "categorical" if they are non-numeric values and "numeric" if the column consists only of             numbers. Missing data (i.e. empty cells) are supported in categorical columns as well as numeric columns

<br>

If you type `qiimereporter example-metadata`, a Qiime2 template file called `sample-metadata.tsv` will be created in your working directory. You can also download the file [here](https://dabadgarcia.github.io/qiimereporter/sample-metadata.tsv). More information about metadata can be found [here](https://docs.qiime2.org/2018.8/tutorials/metadata/).

<br>

## Output
QiimeReporter stores every file generated during the analysis in three different directories, all of them included within the main output directory specified with the `-o/--output` option:  

- **complete_taxonomy_otutables**: the otu tables with the complete taxonomy for the six main leves of [SILVA](https://www.arb-silva.de/).
- **qiime2_artifacts**: all the artifacts produced during the analysis with [Qiime2](https://github.com/qiime2/qiime2).
- **report_files**: the files used to generate the report with [R](https://cran.r-project.org/).

<br>

Once the analysis is finished, QiimeReporter summarizes the results in a interactive html report in your output directory. An example of the report file can be visualized [here](https://dabadgarcia.github.io/qiimereporter/example_report.html).

<br>

## Already using Qiime2? 

Previous [Qiime2](https://github.com/qiime2/qiime2) users can convert their results to be used with QiimeReporter.

```
usage: qiimereporter-format <options>

OBLIGATORY OPTIONS:
	-i/--input 	Path to the directory where the files are located
	-m/--metadata	Path to the Qiime2 metadata file
OTHER OPTIONS:
	-t/--threads	Number of threads to use if tree option is passed (default=$CPUS) <integer>
	--tree		Pass this option to create a tree
	
Please be sure that stats-dada2.qza, table.qza, taxonomy.qza, and tree.nwk are in the input directory 
before proceeding

If you have not created the tree, please also copy the rep-seqs.qza file into the input directory 
and pass the option --tree 

After formatting is completed, you should run qiimereporter with the option --report-only to create the html report. 

```
<br>

Example:

```
qiimereporter-format -i qiime2_files -m sample-metadata.tsv -t 32

qiimereporter --report-only
```

<br>

## Citation

If you use QiimeReporter before publication is released, please cite as:  
  
David Abad, Narciso M. Quijada and Marta Hernandez. QiimeReporter. (2019) https://github.com/dabadgarcia/qiimereporter

Users are also highly encouraged to cite [Qiime2](https://github.com/qiime2/qiime2) and [R](https://cran.r-project.org/) when using QiimeReporter.

<br>

## License
QiimeReporter is a open-source software licensed under [GPLv3](https://github.com/dabadgarcia/qiimereporter/blob/master/LICENSE).
