
To find which collections have the most subjects:

```sql
-- Look at maps whose titles share the same subjects

USE spl_2016;

SELECT COUNT(subject) AS numberOfSubjectsInCollection, collectionCode
FROM `subject`, itemType, itemToBib, collectionCode
WHERE itemType.itemtype LIKE '%map' AND
    collectionCode.itemNumber = itemType.itemNumber AND
    itemType.itemNumber = itemToBib.itemNumber AND
    itemToBib.bibNumber = subject.bibNumber
GROUP BY collectionCode
ORDER BY numberOfSubjectsInCollection DESC;
```
