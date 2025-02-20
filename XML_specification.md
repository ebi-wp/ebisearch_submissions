# XML data format specification

This document goes into details about the EBI Search XML data format
specification. The XML format is similar to the 
[JSON format](JSON_specification.md), but is more structured.


## Introduction

EBI Search expects data from Providers in a common XML Format.

The architecture of EBI Search starts with an XML file that 
contains the information from all datasets in a given database. 
XML files are retrieved from providers nightly, and every 
new dataset in the provided XML file is added to EBI Search 
automatically.

Each file is indexed using the EBI Search System and the final 
information is made available via web services. The EBI Search 
System also contains indexes of other major databases such as 
Uniprot, Ensembl and PubMed, allowing data providers to cross-link 
biological entities in their datasets with those resources.

For any queries about EBI Search XML Format or data submissions 
to EBI Search please contact: ebisearch-support@ebi.ac.uk


## High-level structure

The XML format uses the following generic structure for its datasets:

```
<database>
  <!-- Header information -->
  <name>Database name</name>
  <description>Database description</description>
  <release>Release tag or number</release>
  <release_date>Release date</release_date>
  <entry_count>Number of entries</entry_count>

  <!-- Data -->
  <entries>
    <entry id="entry_id_1" acc="entry_accession_1">
      <name>Name of the dataset</name>
      <description>Description of the dataset</description>
      <cross_references>
        <ref dbkey="CHEBI:16651" dbname="CHEBI"/>
        <ref dbkey="P15498" dname="UNIPROTKB"/>
      </cross_references>
      <dates>
        <date type="submission" value="2019-02-22"/>
        <date type="publication" value="2019-03-31"/>
      </dates>
      <additional_fields>
        <field name="repository">Repository</field>
        <field name="tags">tag_1</field>
        <field name="tags">tag_2</field>
      </additional_fields>
    </entry>
  </entries>
</database>
```

Within the schema, fields may be categorised as follows:

- **Mandatory (M)**: These fields must be provided for the  
schema to be valid, and are part of the minimum information required
to represent a dataset;

- **Recommended (R)**: These fields should be provided to be searchable 
and displayed adequately in the web interface and web services;

- **Additional (A)**: These fields should be provided to add value to 
the dataset - the more metadata a dataset contains, the more sense 
the infrastructure can make out of the data. For example, if the 
proteins, genes or metabolites are provided for each dataset,
the search engine is able to find other datasets where those 
biological entities have been found or studied.


### Database section

The metadata for the domain is included in the database header section.

| Field     | Comment | Example | Type |
|-----------|---------|---------| :---: |
| name      | Name of the database or provider | \<name\>Pride\</name\> | M |
| description | A short description of the provider. This description is shown in the web interface. | \<description\>The proteomics ...\</description\> | R |
| release | The tag for the database release to which the data belongs. | \<release\>Release-May-2016\</release\> | A |
| release\_date | The date of the database release to which the data belongs. This field may be used to store the date the data was generated, if applicable. | \<release\_date\>2015-05-13\</release\_date\> | R |
| entry\_count | The number of entries in the XML file. This field is used for validation purposes. | \<entry\_count\>2\</entry\_count\> | R |

Providers may add further information to the database section but it will not be captured during the indexing process, for example
*\<license\>Apache 2.0\</license\>.*

Note that it makes sense for small databases to provide their data to EBI Search
as a single full-repository XML file. However, some resources contain a large 
number of datasets, making it impracticable to exchange their data in a single 
file. Such resources may provide their data via multiple XML files in the same 
format as described above, each containing a distinct subset of dataset entries. 
Note that `entry_count` in each XML file should correspond to the number of 
entries in **that file only**, not the overall number of entries provided from 
that database.

If providing multiple files, they may be supplied as a single archive file - for
example, a `zip`, `tar` or `tgz` file.


### Entries section

The entries section contains all the datasets provided in a given XML file. The 
*\<entries\>* tag is used to list all the entries. Each dataset is enclosed in 
an *\<entry\>* tag.

Each entry consists of four different sections: *General information*, *Dates*
*Cross-references* and *Additional Fields*.

A dataset in EBI Search must have these attributes: an *identifier*, a *name*, a *description,* a *repository,* an accessible *link,* and one significant *date*. Several types of dates may be provided, as listed in the table below:


#### Entry: General information

| Field | Comment | Example | Type |
| :---- | :---- | :---- | :---: |
| id | Original and UNIQUE identifier across the repository, database or provider | \<entry id="PXD000001"\>\</entry\> | M |
| acc | Accession of the dataset | \<entry acc="PXD000001"\>\</entry\> | R |
| name | Name, title of the dataset, can be considered as the title of the publication | \<name\>TMT spikes\</name\> | M |
| description | A short description or abstract of the dataset. It can be considered similar to a "publication abstract" | \<description\>Expected ... \</description\> | M |


#### Entry: Dates section

Dates have their own block in the entry. The type of date is given in the `type`
attribute, with the `value` attribute containing the actual date. When these are
indexed, they will have `_date` appended to the `type`. This block:

    <dates>
      <date type="publication" value="2014-09-22"/>
      <date type="submission" value="2014-08-22"/>
    </dates>

will result in indexed date fields called `publication_date` and `submission_date`.

The dates section in the entry should contain at least one of the following
date types:

- publication
- creation
- submission
- updated


#### Entry: Additional fields section

The `additional_fields` block contains any fields which fall outside of the
general information, and are not dates or cross-references.

The following additional fields are mandatory for data generated externally
to EMBL-EBI that will be included in search results:

- `repository` - the name of the repository or data provider;
- `full_dataset_link` - a link to the original dataset on the provider's
web service. This will be used to link from the results back to the original
dataset, and should be a universal, reliable URL.


#### Entry: Cross-references section

The cross references section allows for linking the dataset 
to external databases. The `dbkey` contains the dataset 
identifier in the linked database, itself identified via `dbname`.

Cross-references may be to datasets from within EMBL-EBI or external.
Internal cross-references will appear in the results list.