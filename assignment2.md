# Proj 2 - 2D Visualization
### Susan Burtner
----------

## Concept description

Create a network where nodes represent subjects of maps and edges represent whether the subjects are in the same collection. The size of the nodes correspond to the number of titles in that subject. While I have used the shared attributed of `collectionCode` for this visualization, you could use something like `deweyClass` as well.

## MySQL queries

### Query

```sql
SELECT `subject`, COUNT(title.title) AS numberOfTitlesInSubject, collectionCode.collectionCode#,
    #COUNT(outraw.itemNumber) AS numberOfCheckouts, 
FROM title, `subject`, itemType, itemToBib, collectionCode#, outrraw
WHERE itemType.itemtype LIKE '%acmap' AND
    collectionCode.itemNumber = itemType.itemNumber AND
    itemType.itemNumber = itemToBib.itemNumber AND
    itemToBib.bibNumber = subject.bibNumber AND
    subject.bibNumber = title.bibNumber
    #title.bibNumber = outraw.bibNumber
GROUP BY `subject`, collectionCode
ORDER BY numberOfTitlesInSubject DESC
LIMIT 20;
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
| Olympic National Forest Wash Maps | 3 | camap |
| Outdoor recreation Washington State Mount Baker Snoqualmie National Forest Maps | 3 | camap
| Olympic National Park Wash Maps | 3 | camap |
| Endangered species California | 3 | canf |
| Endangered species Northwest Pacific | 3 | canf |
| World maps | 3 | camap |

### Processing time

> 2.10 s

## Sketches and work-in-progress screenshots of your project with descriptions

### Initial sketch

| ![sketch1](https://raw.githubusercontent.com/sburtner/mat259_w19/master/images/sketch1.JPG) |
|:--:|
| *The first sketch of my idea for Assignment \#2. While I wanted to do `deweyClass` originally, there were so few maps with Dewey classifications. I went with the `collectionCode` instead.* |

### Work-in-progress screenshots

| ![error1](https://raw.githubusercontent.com/sburtner/mat259_w19/master/images/error1.png) |
|:--:|
| *Placing nodes randomly on the screen.* |

| ![error2](https://raw.githubusercontent.com/sburtner/mat259_w19/master/images/error2.png) |
|:--:|
| *Trying to place nodes on a circle.* |

| ![error3](https://raw.githubusercontent.com/sburtner/mat259_w19/master/images/error3.png) |
|:--:|
| *Trying to get more meaningful sizes and colors.* |

| ![error4](https://raw.githubusercontent.com/sburtner/mat259_w19/master/images/error4.png) |
|:--:|
| *Normalizing the size of the nodes using `log`.* |


## Final results & analysis
| ![final](https://raw.githubusercontent.com/sburtner/mat259_w19/master/images/final.png) |
|:--:|
| *The final product (so far.) I rotated the nodes, modified the colors, and added labels and text.* |

This 2D visualization can tell us about how map items are often grouped together by subject. For example, if you look at "Spotted Owl," you can see that it is in the same collection as subjects concerning "wildlife management, California," "wildlife management Northwest Pacific," "Endangered species California," and "Endangered species Northwest Pacific." It is interesting to note that maps with no subject have the most titles in them, followed by topographic maps. It also seems like the more specific the map, such as "Peru maps" and "Carson National Forest Maps," the less likely it is to be a part of a collection.

Overall I'm happy with how the visualization turned out. The most difficult part was putting the labels on the graph such that they were close to the nodes, but offset by the width of the node, and this changes according to whichever quadrant you are in (if you think in terms of the "top-left" quadrant, "bottom-right," etc.) In the future, I would like to add an action, in particular, if you move your mouse over a node, it highlights which edges and nodes it is connected to.
