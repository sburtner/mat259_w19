# Proj 2 - 2D Visualization
### Susan Burtner
----------

## Concept description

Create a network where nodes represent subjects and titles of maps and edges represent whether the subjects are in the same collection. If there is time, I would also like to make the size of the nodes correspond to the number of checkouts (by subject and by title.)  

## MySQL queries

### Query

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
### Output

| subject | numberOfTitlesInSubject | collectionCode |
| :------ | :---------------------- | :------------- |
| (No subject) | 88 | camap |
| Topographic maps | 11 | camap |
| Maps | 10 | camap |
| Mount Baker Snoqualmie National Forest Wash Maps | 6 | camap |
| Willamette National Forest Or Maps | 5 | camap |
| Spotted owl | 3 | canf |
| Outdoor recreation Oregon Maps | 3 | camap |
| Wildlife management California | 3 | canf |
| Wildlife management Northwest Pacific | 3 | canf |
| Olympic National Forest Wash Maps	3 | camap |
| Outdoor recreation Washington State Mount Baker Snoqualmie National Forest Maps | 3 | camap
| Olympic National Park Wash Maps | 3 | camap |
| Endangered species California | 3 | canf |
| Endangered species Northwest Pacific | 3 | canf |
| World maps | 3 | camap |

### Processing time

> 2.10 s

## Sketches and work-in-progress screenshots of your project with descriptions

## Final results & analysis
