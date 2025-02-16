# Guide to Building Azure Sentinel Watchlists

This guide provides an overview and details as to how one can create Watchlists to be used within Azure Sentinel. This can be useful when looking to generate and deploy Watchlists that contain large amounts of data or in bulk.

### How to use the json template?

Download the json template and rename it to as "WatchlistUseCaseName.json" (no spaces while naming the JSON file). Now you can start updating the json template to customize it for your connector definition. At any point of updating the json, ensure the json is correctly formatted to avoid syntactical errors. It's recommended to use a json editor like VS Code for this purpose.

### Specifics

The Watchlist template contains a few items that are required for it to work:

Workspace Name: The workspace name is required so that ARM knows the workspace that Azure Sentinel is using. This is used for deploying the content and function to the workspace.

Watchlist Alias: Name for the Watchlist in both Azure Sentinel and in the workspace when calling it via function. This should reflect what the Watchlist is for.

SearchKey Value: Title of a column that will be used for performing lookups and joins with other tables. It is recommended to choose the a column that will be the most used for joins and lookups.

### How to fill the template

When looking to use the template, the parameters do not need to be modified as they will be set at the time of deployment.

For the content of the csv that will be generated, the columns and values must be specified. The columns will appear first, followed by the data. An example appears as so:

    "rawContent": "[concat(parameters('searchkeyvalue'), ',SampleColumn1,SampleColumn2\r\n
        Samplevalue1,samplevalue2,samplevalue3\r\nsamplevalue4,samplevalue5,samplevalue6\r\n')]

RawContent represents that content of the CSV. The columns that should be used are listed first (SearchKey, SampleColumn, SampleColumn2). Once the columns are listed, "\r\n" needs to be used to signal that a new row needs to be started. This is used throughout the template. 

When it comes to values that should be under the column, each value should be separated by a comma. The comma is interpreted as the end of that cell. As shown in the example, samplevalue1 is one cell, samplevalue2 is a different cell. When all fo the values have been added for the row, \r\n needs to be used in order to start the next row.

An example of how that might look would be:

    "rawContent": "[concat(parameters('searchkeyvalue'), ',Account, Machine\r\n123.456.789.1, Admin, ContosoMachine1\r\n
    123.456.789.2, LocalUser, ContosoMachine2\r\n')]"

This example shows that the columns with be an IP (used as the search key value), an account, and a machine. The rows below the columns will contain those types of values in the CSV file. In htis case, the CSV will only have 3 columns and 2 rows of data.

