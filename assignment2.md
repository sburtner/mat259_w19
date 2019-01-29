# Proj 2 - 2D Visualization
### Susan Burtner
----------

## Concept description

Create a network where nodes represent subjects and titles of maps and edges represent whether the subjects are in the same collection. If there is time, I would also like to make the size of the nodes correspond to the number of checkouts (by subject and by title.)  

## MySQL queries

```sql
USE spl_2016;

SELECT `subject`, COUNT(title.title) AS numberOfTitlesInSubject, #COUNT(outraw.itemNumber) AS numberOfCheckouts,
    collectionCode.collectionCode
FROM title, `subject`, itemType, itemToBib, collectionCode#, outrraw
WHERE itemType.itemtype LIKE '%acmap' AND
    collectionCode.itemNumber = itemType.itemNumber AND
    itemType.itemNumber = itemToBib.itemNumber AND
    itemToBib.bibNumber = subject.bibNumber AND
    subject.bibNumber = title.bibNumber# AND
    #title.bibNumber = outraw.bibNumber
GROUP BY `subject`, collectionCode
ORDER BY numberOfTitlesInSubject DESC;
```



## Processing time

## Sketches and work-in-progress screenshots of your project with descriptions
## Final results & analysis
