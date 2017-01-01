# AccessHistory
This is a (not so) poor man's framework to add system versioning to Microsoft Access databases.

## What is this need for? (or a brief introduction into temporal databases)
Did you ever wish you could track the history of your database entries in order to preserve an old state of the facts and the deductions at a given point in time? An elegant answer to this need is the concept of a [temporal database](https://en.m.wikipedia.org/wiki/Temporal_database). A concept suggesting to augment your datasets with a validity timespan, allowing for storing multiple versions of the very same record in your database. At the cost of some harddisk space you're receiving a time machine allowing for travelling back and forth in the knowledge stored in your database.
