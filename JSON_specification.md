# JSON data format specification

This document goes into details about the EBI Search JSON format
specification.

The JSON format is similar to the [XML Specification](XML_specification.md),
although somewhat simpler in presentation.


## Introduction

EBI Search expects data from Providers in a common JSON Format.

The architecture of EBI Search starts with an JSON file that 
contains the information from all datasets in a given database. 
JSON files are retrieved from providers nightly, and every 
new dataset in the provided JSON file is added to EBI Search 
automatically.

Each file is indexed using the EBI Search System and the final 
information is made available via web services. The EBI Search 
System also contains indexes of other major databases such as 
Uniprot, Ensembl and PubMed, allowing data providers to cross-link 
biological entities in their datasets with those resources.

For any queries about EBI Search JSON Format or data submissions 
to EBI Search please contact: ebisearch-support@ebi.ac.uk


## High-level structure

The JSON format uses the following outline structure for its
datasets:

```
{
    "name": "Database name",
    "description": "Description of the database",
    "release": "Release tag or number",
    "release_date": "Release date",
    "entry_count": "Number of entries",
    "entries": [
        {
            "fields": [
                { "name": "id", "value": "id1" },
                { "name": "acc", "value": "accession1" }
            ],
            "cross_references": [
                { "dbname": "CHEBI", "dbkey": "CHEBI:16651" },
                { "dname": "UNIPROTKB", "dbkey": "P15498" }
            ]
        }
    ]
}
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
| name      | Name of the database or provider | "Pride" | M |
| description | A short description of the provider. This description is shown in the web interface. | "The proteomics ..." | R |
| release | The tag for the database release to which the data belongs. | "Release-May-2016" | A |
| release\_date | The date of the database release to which the data belongs. This field may be used to store the date the data was generated, if applicable. | "2015-05-13" | R |
| entry\_count | The number of entries in the file. This field is used for validation purposes. | 2 | R |

Note that it makes sense for small databases to provide their data to BY-COVID as a single full-repository JSON file. However, some resources contain a large number of datasets, making it impracticable to exchange their data in a single file. Such resources may provide their data via multiple JSON files in the same format as described above, each containing a distinct subset of dataset entries. Note that `entry_count` in each JSON file should correspond to the number of entries in **that file only**, not the overall number of entries provided from that database.

If providing multiple files, they may be supplied as a single archive file - for
example, a `zip`, `tar` or `tgz` file.


### Entries section

The entries section is an array containing all of the datasets provided in a 
given JSON file.

Each entry consists of two different sections: *Fields* and *Cross-references*.

A dataset in EBI Search must have these attributes: an *identifier*, a *name*, a *description,* a *repository,* an accessible *link,* and one significant *date*. Several types of dates may be provided, as listed in the table below:


#### Entry: Fields section

A number of fields are expected in every entry.

| Field | Description | Example | Notes |
| ----- | ----------- | ------- | ----- |
| id    | A unique identifier for the dataset | PXD000001 | Mandatory |
| acc   | Accession of the dataset | PXD000001 | Optional |
| name  | Name, title of the dataset, can be considered as the title of the publication | TMT Spikes | Mandatory |
| description | A short description or abstract of the dataset. It can be considered similar to a "publication abstract" | Expected ... | Mandatory |
| repository | The name of the repository or provider | PRIDE | Mandatory |
| full_dataset_link | a link to the original dataset on the provider's web service: this will be used to link from the results back to the original dataset, and should be a universal, reliable URL | https://.../PXD000001 | Mandatory |
| publication_date | Date of publication of the dataset | 2014-09-22 | See date note below |
| creation_date | Date of initial creation of dataset or submission in the database | 2014-09-22 | See date note below |
| submission_date | Date of successful submission to the database | 2014-09-22 | See date note below |
| updated_date | Date of the latest update to the dataset | 2014-09-22 | See date note below |

**Dates**: at least one of the four dates listed above must be supplied for each
dataset.


#### Entry: Cross-references section

The cross references section allows for linking the dataset 
to external databases. The `dbname` contains the linked database,
while the `dbkey` contains the dataset identifier in that database.

Cross-references may be to datasets from within EMBL-EBI or external.
Internal cross-references will appear in the results list.