# Data update policy

Please consider our data update policy when creating files for indexing.


## Indexing time

The EBI Search indexes are rebuilt at 2am every day. If files change while we are indexing them, this could corrupt the index. As a result, if we detect data updates during the update period, that domain will be ignored for the day.

Please ensure that your data is updated by 2am, especially if it changes frequently. We try to keep the data as up-to-date as possible, but if your data is being updated during our indexing period, it will be rejected.

## Data size

We carry out some basic size checks on updated data files before we index them. This avoids accidentally indexing data where there may be a generation error or similar issue, resulting in files that are smaller than expected or missing entries.

Data file sizes are checked against the previous version of the data. If the new total file size is less than 75% of the previous version, it will be rejected.

If you know that an update is likely to be smaller than the previous version (because of accidental duplication, for example), please let us know in advance so that we can temporarily override the file size checks.

## Entry count

Once the data has been indexed, we run a verification stage to ensure that the number of entries matches what was expected.

If data is supplied as an XML dump or in the EBI Search JSON format, the entry count is supplied in each file. For other formats, if the count is supplied, it will be used to verify the index size.

We compare the number of entries in the final index with the expected total from the files. If these do not match, the domain will not be updated.

## Data formatting

If the data does not match the expected XML or JSON schema, this will raise an error during the indexing stage, and the data will be rejected.

For EBI Search formats, the files can be validated using xmllint or a JSON validator.
