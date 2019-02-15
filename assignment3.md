
To find which collections have the most subjects:

```sql
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

To find for each collection:

```sql
USE spl_2016;

SELECT title, subject
FROM title, subject, itemType, itemToBib, collectionCode
WHERE itemType.itemtype LIKE '%map' AND
    collectionCode.collectionCode = 'camap' AND
    subject.subject != "" AND
    collectionCode.itemNumber = itemType.itemNumber AND
    itemType.itemNumber = itemToBib.itemNumber AND
    itemToBib.bibNumber = subject.bibNumber AND
    subject.bibNumber = title.bibNumber
ORDER BY title DESC;
```
