# EBI Search submissions

This repository contains documentation for the EBI Search data submission process.


## Adding resource metadata for indexing

To add a new resource for indexing in the Portal, resource owners need to provide a file with resource metadata in a specified format.

The guiding principle should be "discoverability, not completeness", with the guiding question "Would someone want to search on this data item?"

The file should be provided in XML or JSON format. See further details below.

### File location

The file should be provided in a publicly readable location - a URL is ideal.
In production mode, the portal scripts will check the location daily, and if updated, download the file and index it.

In development mode or if there are technical constraints, the file (or a notification on an update) can be sent by email.


## EBI Search domain creation form

This is a Google form for specifying the full details of a new domain (dataset), including data set metadata, field specifications, and so on.
https://forms.gle/QVqvhuenEtBtKwDe9

Further details on how to fill in the form can be found in the
[Guide](Domain_information_form_guide.md).


## Format guides

- [XML format specification](XML_specification.md)
- [JSON format specification](JSON_specification.md)
