# EBI Search domain information template guide

This page provides information on how to fill the 
[EBI Search domain information form](https://forms.gle/QVqvhuenEtBtKwDe9). 
The form allows data providers to provide information to efficiently and quickly 
set up the new index in EBI Search. It also allows the EBI Search team to identify 
potential issues with the data and the search functionality at an early stage and 
give timely suggestions.

The domain provider should fill the 
[EBI Search domain information form](https://forms.gle/QVqvhuenEtBtKwDe9) 
with as much information they can.

Please be aware of our [data update policy](Data_update_policy.md), in particular the guidance on update times.

More information about EBI Search can be found on the EBI Search 
[Documentation page](https://www.ebi.ac.uk/ebisearch/documentation)


# Section 1: Domain information

This section covers the base domain details \- name, description, contact details, etc.

## To fill in

| Properties | Value | Comments/Description |
| ----- | ----- | ----- |
| Domain Name |   | For e.g. UniProtKB |
| Description |   | Short Description |
| Main URL |   | For example, [https://www.uniprot.org](https://www.uniprot.org) |
| Visible | true/false | Defines whether the domain is visible through the web interface. If **false**, the domain content will only be retrievable through the API. For domains containing specialised data, used by a specific site outside of EBI Search, this should generally be **false**. |
| Primary contacts |   | Name, email. A general email address ([info@xyz.com](mailto:info@xyz.com)) or mailing list is preferable \- this avoids addresses becoming stale or unreachable if the original contact leaves. |
| Links, references if any |   |  |
| Developer contact |  | An email to use during the development period, to verify details, resolve issues with content, etc. |


# Section 2: Data format

EBI Search can parse and index data files in its own XML and JSON format. For large data sets, XML is preferable for indexing.

Whatever the format chosen the entries can be present in one or several files. There is no restriction on the number of entries per file.

## To fill in

Please specify which format your data set will be created (choose either of three formats):

|   FORMAT |   OPTION |   COMMENTS/DESCRIPTION |
| ----- | ----- | ----- |
| [EBI Search XML format](http://www.ebi.ac.uk/ebisearch/XML4dbDumps.xsd) | YES/NO |   |
| [EBI Search JSON format](http://www.ebi.ac.uk/ebisearch/schemas/data_schema.json) | YES/NO |   |
| Other | YES/NO | If yes then share more documentation related to the format (grammar, description, ...) |

## Examples

See the following:

- [XML specification](XML_specification.md) for examples and more detail on 
the XML format.

- [JSON specification](JSON_specification.md) for examples and more detail on
the JSON format.


# Section 3: Data update

In order to ease the maintenance of the EBI Search engine and to guarantee the 
most up-to-date data, an automatic data update mechanism has been implemented.

This step checks the data sources to identify possible updates. If updates are 
available the new data is downloaded, then re-indexed and redeployed to be visible 
to the users. Additionally, metadata (release, release date, number of entries, 
etc.) are generated from the data or a release note for verification and 
information purposes.

The following information is needed for this step:

### Root source URI

This is the root of the source files. The URI can define a path on the file system (for EBI sources), or a URL (https or similar). When defining a URL, the destination file may be compressed (zip, tar, tgz, gz, bz2 formats are supported).

### File system sources only

These options only apply to file system sources. If supplying a URL, this section can be ignored.

#### File pattern

This is the regular expression of the files to download. The files can be compressed (The following formats are supported : zip, jar, tar, tgz, tbz2, gz, bz2).

#### Excluded sub directories

By default the files are retrieved from all the sub directories of the root source. You can exclude sub directories by defining a regular expression matching these directories' names.

#### Metadata file

A metadata file is required for non-EBI Search formats (i.e. not EBI Search XML or JSON-formatted files). With the EBI Search formats, the metadata is included as part of the file content. The information is used to validate the new index, and supplying domain metadata through the API and web interface.

The metadata file must at least contain the release number, the release date and the number of entries. This information can be retrieved from an existing release note (or from the data file itself if it appears at the beginning of the file). If no such file exists a file with the following simple format has to be created.

A sample metadata file is given here:

`# Sample Metadata File`  
`release=[release number or release date if no release defined]`  
`release_date=[DD-MMM-YYYY]`  
`entries=[number of entries]`

## To fill in

| Properties | Value | Comments/Description |
| ----- | ----- | ----- |
| Root source URI |   | path can be file system, an ftp URL or an HTTP URL  e.g. `/nfs/ftp/pub/databases/uniprot/current\_release/knowledgebase/complete/`, [http://snapshot.geneontology.org/ontology/go.obo](http://snapshot.geneontology.org/ontology/go.obo). Note: a path with symbolic link recommended if your path is dynamic for every release, check permission if its file system path |
| File pattern (regular expression) |   | e.g.  uniprot\_sprot\\.xml\\.gz |
| Excluded sub dirs |   | The system will try to index all files in the subfolder. Express here in the form of a regular expression matching folder names the system has to skip.  |
| Metadata file. \[in case of other file dumps different from EBI Search ones\] |   | path can be file system, an ftp URL or an HTTP URL Metadata file should contain below properties release= \[release number or release date if no release defined\] release\_date=\[DD-MMM-YYYY\] entries= \[number of entries\]  e.g. /nfs/ftp/pub/software/ensembl/rnacentral-dumps/release\_note.txt |


# Section 4: Indexing

Indexing is the process of creating the indexes used at search time by EBI Search. The contents of the data files are parsed and processed to convert into searchable indexes. 

During the parsing process the system needs to decide which fields to include in the indices: fields can be included for search and/or retrieval (visualization). The way the fields are included influence the relevance of the search results and the size of the index. Fields are divided into normal fields and cross references; the latter are used to link entries coming from the same or different domain.

 
## To fill in

### Fields spreadsheet

You need to provide 5 pieces of information for each field to be indexed:

* STORED/NOT\_STORED: a STORED field will be retrievable (for display) via the web services, a NOT\_STORED field will not be retrievable.  
* INDEXED/NOT\_INDEXED: an INDEXED field is searchable, while a NOT\_INDEXED field will not be searchable. A NOT\_INDEXED field may still be STORED for display.  
* The field type. This will affect search behaviour.  
* SORT: Should the field be sortable? A sortable field should meet the following conditions:  
  * The field should be indexed  
  * The field should be a numeric or keyword type. Text fields are not suitable for sorting.  
  * The field should be single-valued.  
* FACET: Should the field be used to facet the data. You need to provide additional details about facets you wish to define:   
  * Facet label (e.g. 'organisms' for Uniprot TAXONOMY based facet: 
  [http://www.ebi.ac.uk/ebisearch/search.ebi?db=proteinSequences\&t=tp53](http://www.ebi.ac.uk/ebisearch/search.ebi?db=proteinSequences&t=tp53))  
  * Facet selection type, single or multi-selection (e.g. multi-selection: 'organisms' in Uniprot,  single-selection: 'status' in Uniprot)  
  * Other requirements (e.g. date type facet)


### Cross-references spreadsheet

This requires the following information about each cross-reference field in
the index:

* Field name
* Description
* Referenced domain name
* Referenced field
* Any comments

These cross-references can point to either internal databases that are indexed 
by the EBI Search (domains) or to external resources.

The external xrefs are not displayed for the time being but you must specify 
them for future use.

The internal xrefs defined in the data can use different database names from 
the ones the EBI Search uses and can also use a specific field for the 
identifier.  

For example, databases contain xrefs to dbname="swiss-prot", dbkey="Q62594". 
This xref needs actually to point to the domain 'UniProtKB' and use the 
accession number 'Q62594'.

You can add a suffix to the database name to add some semantic detail to the 
cross-reference. For example if you have xrefs to Ensembl which actually are 
xrefs to either transcripts or genes you can name the fields as either 
ENSEMBL\_TRANSCRIPT or ENSEMBL\_GENE so that users will be able to make the 
difference between the two. They will both internally point to the domain Ensembl.

The data providers need to go through the xrefs present and establish to which 
database and field they point to.


# Section 5: Display

This section is to acquire details of how the results should be displayed
in the results lists.

The layout may be customised for each domain. If not customised, a default
template will be used, consisting of:

1. entry ID and/or name

2. URL to display entry. This URL links back to the domain provider's site,
providing detailed information related to the entry. (EBI Search only displays
results, individual items link back to the domain provider.)

3. Source database: name of the domain that entry belongs to

4. Description: a brief description of the entry

5. Related data (if any): comma separated list of related entry ids/names

6. Cross references (if any): a list of cross references

7. External resource links (if any): a list of external resources links that can 
display or open the entry in an external resource (for example, in a specific 
format like FASTA)


## To fill in

| Properties | Description | Values | Comments/Description |
| ----- | ----- | ----- | ----- |
| URL template | Details of how to link to the domain provider entry | | The URL needs to be a template that permits the system to generate the link based on provided fields. For e.g. [http://www.ebi.ac.uk/uniprot/unisave/app/\#/search/P23497](http://www.ebi.ac.uk/uniprot/unisave/app/#/search/P23497) |
| External templates | Details of how to link to external resources | | |
| List of fields | Fields to be displayed in the results list | | Details of layout customisations, if required. |

