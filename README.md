## Animorphs Common vs. Unique Morphs
![Status](https://img.shields.io/badge/status-work--in--progress-yellow)


As a 90s kid, Animorphs was an extremely influential books series that shaped my taste in books. The series focused on a group of teenagers who acquired the power to "morph" into different animals and used their new found power to fight off a secret alien invasion. 

By default, I tend to think of data in terms of structured rows and columns. Does it neatly fit into a spreadsheet? However, in order to expand how I think about data, I want to learn more about graph data models that focuses on understanding and visualizing relationships between nodes, rather than looking up specific values. 

This Neo4j will explore which animal morphs were unique to specific characters vs. shared across the team.


## Tech: 
* Neo4j

## Findings:

What morphs do the Animorphs have in common? 

```cypher
MATCH (animorph)-[:MEMBER_OF]->({name: 'Animorphs'})
WITH collect(animorph) AS members
MATCH (a)
WHERE all(m IN members WHERE (a)<-[:ACQUIRES]-(m))
MATCH q = (a)<-[:ACQUIRES]-()
RETURN q
```
![Common Morphs](common_morphs.svg)



What are each character's unique morphs?

```cypher
MATCH (animorph)-[:MEMBER_OF]->({name: 'Animorphs'})
WITH collect(animorph) AS members
MATCH (a)<-[:ACQUIRES]-(m)
WHERE m IN members
WITH a, collect(DISTINCT m) AS acquirers
WHERE size(acquirers) = 1
WITH a, acquirers[0] AS soleAcquirer
MATCH p = (a)<-[:ACQUIRES]-(soleAcquirer)
RETURN p
```
![Unique Morphs](unique_morphs.svg)





## Roadmap
- [x] Upload proof of concept data schema focusing on morphs
- [x] Create Graphs 
- [ ] Record lessons learned

## Lessons Learned
