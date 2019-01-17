# Proj 1 - MySQL Assignment | Knowledge Discovery
## Susan Burtner

**THE CONCEPT (abridged version)**: In any database, there lies hidden knowledge. What does a database contain, and what can MySQL queries reveal? Your first assignment is to find something of interest based on your own cultural / knowledge interests. Here are some options: 

1) **Topics of Cultural Interest**

2) **The Database Organizational Structure**

3) Data Analytics Query Methods

----------

## GETTING THE DATA:

- Use the MySQL Workbench to write a query by which to retrieve the data from the SPL database.
- Use the spl_2016 database which gets updated daily.

## DO THE ASSIGNMENT:

Do a report on your research, and propose what to visualize based on time, or volume of activity. Possibly make a comparison between book(s) and movie(s) and soundtracks (cd).

Once you have all the material - click on "POST REPLY" to this [link](http://w2.mat.ucsb.edu/forum/viewtopic.php?f=77&t=313) and add your info to complete the assignment.

----------
# Map Use and Musings at the Seattle Public Library - 2016

## Research Questions relating to Organizational Structure:
### RQ1: How many maps are available at the Seattle Public Libary (SPL)?

***Why am I asking this?*** It would be helpful to first know how many maps are available at SPL before knowing which ones are checked out, and before performing any analyses of cultural interest. This will be helpful for later visualizations that require normalization.

```sql
SELECT COUNT(barcode)
FROM spl_2016.inraw
WHERE itemtype LIKE '%map';
```
> 287

![results1](https://github.com/adam-p/markdown-here/raw/master/src/common/images/icon48.png "Logo Title Text 1")


### RQ2: How many map collections does SPL have and what are they?

** *Why am I asking this?* ** The map collections can be an indication of the *coverage* of the globe. Collections with more maps may imply increased interest in that particular area, or perhaps, a greater diversity of of map representation. For example, areas with more "things" will have more maps related to each of those "things."

```sql
SELECT COUNT(DISTINCT collcode)
FROM spl_2016.inraw
WHERE itemtype LIKE '%map';
```

> 8

```sql
SELECT DISTINCT collcode
FROM spl_2016.inraw
WHERE itemtype LIKE '%map';
```
| **collcode** |
|:-------------|
|cadocpt|
|camap|
|camapr|
|camus|
|canf|
|caref|
|naatlr|
|nanf|

*Follow-up question*... what do those collections mean?

We know from [this link](https://www.mat.ucsb.edu/~g.legrady/academic/courses/15w259/d/MetadataDef.pdf) that **collcode**:

> This is a string of characters that encodes several data for each item, including the physical home (aka
branch), collection type, and collection name.

But it's unclear what the names of the collections actually mean. We may have to do additional exploraiton on SPL's [website](https://www.spl.org/). I'm assuming that "map" is just that, and "ref" is just that as well, but otherwise, I couldn't find much information on the collection metadata.


## RQ3: Do maps have a Dewey class? And if so what are they?

** *Why am I asking this?* ** We know that maps are *generally* non-fiction, however, they aren't *read* like texts are. If the maps do have Dewey class values, what are they?


```sql
SELECT inraw.bibNumber, deweyClass.deweyClass, inraw.subj
FROM spl_2016.inraw, spl_2016.deweyClass
WHERE inraw.bibNumber = deweyClass.bibNumber AND
    inraw.itemtype LIKE '%map'
ORDER BY deweyClass DESC;
```

| bibNumber | deweyClass | subj |
| :-------- | :--------- | :--- |
| 1848209 | 979 | NULL |
| 1757400 | 912 | NULL |
| 1757400 | 912	| NULL |
| 1693625 | 911 | NULL |
| 1693625 | 911 | NULL |
| 1693625 | 911 | NULL |
| 1693625 | 911 | NULL |
| 1693625 | 911 | NULL |
| 1693625 | 911 | NULL |
| 558499 | 910 | NULL |
| **447232** | **784** | NULL |
| 2659183 | 355 | NULL |
| 3031147 | 230	| NULL |
| 2010269 | 224	| NULL |
| **2694116** | **188** | NULL |
| 1603624 | 177	| NULL |
| **1663926** | **173**	| NULL |

When we grab just the distinct Dewey classes, we get:
> 979,
912,
911,
910,
784,
355,
230,
224,
188,
177,
173

If we cross reference these classes with the [Dewey classification csv](https://www.mat.ucsb.edu/~g.legrady/academic/courses/19w259/19w259.html) given on the course website, we see that the maps fall under these subjects:

| Dewey class | Description |
| :---------- | :---------- |
| **173** | **Ethics of family relationships** |
| 177 | Ethics of social relations |
| **188** | **Stoic philosophy** |
| 224 | Prophetic books of Old Testament|
| 230 | Christianity |
| 355 | Military science |
| **784** | **Instruments & Instrumental ensembles & their music** |
| 910 | Geography & travel |
| 911 | Historical geography |
| 912 | Graphic representations of surface of earth and of extraterrestrial worlds |
| 979 | Great Basin & Pacific Slope region of United States |

*Follow-up question* (and sanity check)... is this right? Let's pick a few `bibNumbers` (in bold) and check the [SPL website](https://www.spl.org/).

```sql
SELECT DISTINCT(deweyClass.deweyClass, deweyClass.bibNumber, title.title, subject.subject)
FROM spl_2016.title, spl_2016.deweyClass, spl_2016.subject, spl_2016.inraw
WHERE title.bibNumber = deweyClass.bibNumber AND
    deweyClass.bibNumber = subject.bibNumber AND
    subject.bibNumber = inraw.bibNumber AND
    deweyClass.bibNumber = '1663926' OR
    deweyClass.bibNumber = '2694116' OR
    deweyClass.bibNumber = '447232' AND
    inraw.itemtype LIKE '%map';
```

| Dewey class | bibNumber | title | subject |
| :---------- | :-------- | :---- | :------ | 
|173 | 1663926 | Ouachita National Forest pocket guide | Ouachita National Forest Ark and Okla |
| 188 | 2694116 | Shasta Trinity National Forest California	| Outdoor recreation California Shasta National Forest Maps |
| 188 | 2694116	Shasta Trinity National Forest California | Outdoor recreation California Trinity National Forest Maps |
| 188 | 2694116	Shasta Trinity National Forest California | Shasta National Forest Calif Maps |
| 188 | 2694116	Shasta Trinity National Forest California | Trinity National Forest Calif Maps |
| 784 | 447232 | Sing a song of holidays and seasons home neighborhood and community | Childrens songs |

Not sure why that last one has the subject "Children's songs," so maybe this is an error. Regardless, these entries do indeed seem to be maps!


### RQ3: How many of these maps were left 'Uncategorized'?

** *Why am I asking this?* ** Its apparent from the previous research questions that maps seem like they would be hard to organize. I am curious if most of the maps are physical or digital copies, and if so, how they are organized both in storage and from publically accessible locations in the library.

RQ1 in particular raises an interesting thought. I used `barcode` for that query, which implies these are physical maps. But are there digital maps as well? It's not clear we would see this, as this database has tables like `inraw` and `outraw`, which implies that this database contains transactions for physical objects, but perhaps not digital entities (like a digitized map.) Regardless, I would like to see how many maps have no `callNumber`, so I modify my first query.

```sql
SELECT COUNT(barcode)
FROM spl_2016.inraw
WHERE itemtype LIKE '%map' AND
    callNumber = 'UNCAT';
```
> 32

Thirty two out of 287 maps have no `callNumber`, which is essentially the "address" of the item. How would one find these in the library?

----------

## Research Questions relating to Cultural Interest:

### RQ4: What subjects do the maps have?

### RQ5: Which regions of the world are covered by the given maps?

Another possibility of more maps being created (and this relates to RQ2) is that certain regions of the world are undergoing significant changes in their political boundaries.

What's **not** being classified??

| Dewey class | Description |
| :---------- | :---------- |
| 914 | Geography of & travel in Europe |
| 915 | Geography of & travel in Asia |
| 916 | Geography of & travel in Africa |
| 917 | Geography of & travel in North America |
| 918 | Geography of & travel in South America |
| 919 | Geography of & travel in Australasia, Pacific Ocean islands, Atlantic Ocean islands, Arctic islands, Antarctica, & on extraterrestrial worlds |


----------

## Conclusion


