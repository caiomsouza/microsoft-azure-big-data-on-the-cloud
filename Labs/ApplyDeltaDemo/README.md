# Apply Delta Lab

My main object was to create a repeatable process to apply Delta using U-SQL and Azure Data Lake.

First, we have 3 files:
-	Base file 
-	Delta 1
-	Delta 2

U-SQL Script:

```

// Assumptions: 
//      col1, 5 and 6 is the intented unique key
//      each file includes a date in the filename

DECLARE @baseFilesLocation string = "/SampleData/ApplyDelta/input/base_{filedate}.txt";
DECLARE @deltaFilesLocation string = "/SampleData/ApplyDelta/input/delta_{filedate}.txt";


// Get the base files
@baseFiles =
    EXTRACT col1 int,
            col2 decimal,
            col3 decimal,
            col4 int,
            col5 int,
            col6 int,
            col7 int,
            filedate string

    FROM @baseFilesLocation
    USING Extractors.Text(delimiter : ' ');


// Get the delta files
@deltaFiles =
    EXTRACT col1 int,
            col2 decimal,
            col3 decimal,
            col4 int,
            col5 int,
            col6 int,
            col7 int,
            filedate string

    FROM @deltaFilesLocation
    USING Extractors.Text(delimiter : ' ', silent: true);


@working = 
    SELECT *
    FROM @baseFiles
    UNION ALL
    SELECT *
    FROM @deltaFiles;


// Work out the (col1, 5 and 6) and max filedate combination
@maxDates = 
    SELECT col1, col5, col6, MAX(filedate) AS filedate
    FROM @working
    GROUP BY col1, col5, col6;


// Join the original set (all base data, all delta files) to the max dates rowset, to get last record for each col1
@output = SELECT w.*
    FROM @working AS w
        SEMIJOIN @maxDates AS d ON w.col1 == d.col1
            AND w.col5 == d.col5
            AND w.col6 == d.col6
            AND w.filedate == d.filedate;


OUTPUT @output
    TO "/SampleData/ApplyDelta/output/output.csv"
ORDER BY col1, col2, col3
USING Outputters.Csv();

```



To test it, place this SampleData folder in your USQLDataRoot and run the U-SQL Script:
C:\Users\caio.f.moreno\AppData\Local\USQLDataRoot

Important:
Azure Data Lake Store is an append-only file system, like most big data stores.<BR>
In this systems, records can only be added to the end of a file. <BR>
Various analytics applications such as Azure Data Lake Analytics and Hive can be used to logically merge these base and delta streams.<BR>
