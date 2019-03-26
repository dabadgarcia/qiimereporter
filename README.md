[![Anaconda-Server Badge](https://anaconda.org/dabadgarcia/qiimereporter/badges/version.svg)](https://anaconda.org/dabadgarcia/qiimereporter)  
[![Anaconda-Server Badge](https://anaconda.org/dabadgarcia/qiimereporter/badges/latest_release_date.svg)](https://anaconda.org/dabadgarcia/qiimereporter)

<br>

# QiimeReporter 
One step pipeline for amplicon sequence analysis.  

## Contents  
  * [What is QiimeReporter?](#what-is-qiimereporter)
  * [Installation](#installation)
      * [Required dependencies](#required-dependencies)
  * [Usage](#usage)
      * [Metadata](#metadata)  
  * [Output](#output)
  * [Citation](#citation)
  * [License](#license)

<br>

## What is QiimeReporter?

QiimeReporter is a straightforward pipeline for the analysis of amplicon sequences directly from raw Illumina paired-end data. It integrates the most common [Qiime2](https://github.com/qiime2/qiime2) commands with [R](https://cran.r-project.org/) in order to create a final html report that can be opened in any web browser and easily shareable between researchers.  

<br>

## Installation

The philosophy of QiimeReporter is to be user-friendly so it has been prepared as a conda environment. Run the following commands to  install QiimeReporter an its dependencies:  

```
wget https://anaconda.org/
conda env create -n qiimereporter --file qiimereporter.yml
```
<br>

To activate QiimeReporter environment run:  

```
conda activate qiimereporter
```
<br>

Additionally, the first time you are using QiimeReporter, run (after activating QiimeReporter environment):

```
qiimereporter-setup
```

This step will install additional dependencies not available in conda and the [SILVA](https://www.arb-silva.de/) database (reselase 132).

<br>

### Required dependencies
QiimeReporter is a pipeline that requires a few dependencies to work:  
  * [Qiime2](https://github.com/qiime2/qiime2)
  * [R](https://cran.r-project.org/)
    * R packages: [ggtree](https://bioconductor.org/packages/release/bioc/html/ggtree.html), [knitr](https://cran.r-project.org/web/packages/knitr/index.html), [plotly](https://cran.r-project.org/web/packages/plotly/index.html),[rmarkdown](https://cran.r-project.org/web/packages/rmarkdown/index.html)
  * [SILVA](https://www.arb-silva.de/)
   
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
	--color 	Multivariate analysis color from sample-metadata
	-d/--database 	Path to database, must be .qza file (default SILVA132)
	-f/--forward	Truncates length of the forward read (default='0')
	-h/--help	Show this help
	--no-diversity	Excludes the diversity indexes from the report
	--no-heatmap	Excludes the heatmap from the report
	--no-network	Excludes the network from the report
	--no-pcoa	Excludes the pcoa from the report
	--no-rarefactionExcludes the rarefaction curves from the report
	--multivariate	Includes other multivariate analysis in the report (NMDS, DCA, PCA, RCA)
	--report-only	If report files already present, generates the html document 
	-r/--reverse	Truncates length of the reverse read (default='0')
	-t/--threads	Number of threads to use (default=$CPUS) <integer>
	--title		Path to a file containing the title of the project that will be used as title in the report
			Avoid using special characters. QiimeReporter will use a default title if this option is not used
	-v/--version	Show version

```
<br>

Example:
```
qiimereporter -i raw_reads -m sample-metadata.tsv --output output_folder -t 32
```
<br>

### Metadata
A metadata text file is needed for QiimeReporter to work by using the `-m/--metadata` option. This file will include all the information regarding the sample and requires an specific organization:  
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

If you type `qiimereporter example-metadata`, a file called `sample-metadata.tsv` will be created in your working directory that can be used as a template for your own dataset.

<br>

## Output
QiimeReporter stores every file generated during the analysis in three different directories, all of them included within the main output directory specified with the `-o/--output` option:  

- **complete_taxonomy_otutables**: the otu tables with the complete taxonomy for the six main leves of [SILVA](https://www.arb-silva.de/).
- **qiime2_artifacts**: all the artifacts produced during the analysis with [Qiime2](https://github.com/qiime2/qiime2).
- **report_files**: the files used to generate the report with [R](https://cran.r-project.org/).

<br>

Once the analysis is finished, QiimeReporter summarizes the results in a interactive web-like report. An example of a report file can be visualized [here](https://dabadgarcia.github.io/qiimereporter/files/).

<br>

## Citation

If you use QiimeReporter before publication is released, please cite as:  
  
David Abad and Marta Hernandez. QiimeReporter. (2019) https://github.com/dabadgarcia/qiimereporter

The dependencies described in [this section](#required-dependencies) are the backbone of QiimeReporter and users are encouraged to cite them when using it.

<br>

## License
QiimeReporter is a free software licensed under [GPLv3](https://github.com/dabadgarcia/qiimereporter/blob/master/LICENSE).
