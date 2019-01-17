# Proj 1 - MySQL Assignment | Knowledge Discovery
## Susan Burtner

**THE CONCEPT (abridged version)**: In any database, there lies hidden knowledge. What does a database contain, and what can MySQL queries reveal? Your first assignment is to find something of interest based on your own cultural / knowledge interests. Here are some options: 

1) **Topics of Cultural Interest**: 

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
### RQ1: How many are available at the Seattle Public Libary (SPL)?

***Why am I asking this?*** It would be helpful to first how many maps are available at SPL before knowing which ones are checked out, and before performing any analyses of cultural interest. This will be helpful for later visualizations that require normalization.

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

*Follow-up questions*... what do those collections mean?

We know from [this link](https://www.mat.ucsb.edu/~g.legrady/academic/courses/15w259/d/MetadataDef.pdf) that **collcode**:

> This is a string of characters that encodes several data for each item, including the physical home (aka
branch), collection type, and collection name.

But it's unclear what the names of the collections actually mean. We may have to do additional exploraiton on SPL's [website](https://www.spl.org/). I'm assuming that "ca" may indicated California, "map" is just that, and "ref" is just that as well, but otherwise, I couldn't find much information on the collection metadata.


## RQ3: Do maps have a Dewey class? And if so what are they?

** *Why am I asking this?* ** We know that maps are *generally* non-fiction, however, they aren't *read* like texts are. If the maps do have Dewey class values, what are they?

First try...

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
| 1693625 | 911	NULL |
| 558499 | 910 | NULL |
| 447232 | 784 | NULL |
| 2659183 | 355 | NULL |
| 3031147 | 230	| NULL |
| 2010269 | 224	| NULL |
| 2694116 | 188	| NULL |
| 1603624 | 177	| NULL |
| 1663926 | 173	| NULL |


### RQ3: How many of these maps were left 'Uncategorized'?

** *Why am I asking this?* ** Maps seem like they would be hard to organize. I am curious if most of the maps are physical or digital copies, and if so, how they are organized both in storage and from publically accessible locations in the library.


### RQ4: What subjects do the maps have?

### RQ5: Which regions of the world are covered by the given maps?

Another possibility of more maps being created (and this relates to RQ2) is that certain regions of the world are undergoing significant changes in their political boundaries.


## Research Questions relating to Cultural Interest:

