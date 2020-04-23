Exercício 1- Retrieving Nodes
Coloque os comandos utilizado em cada item a seguir:

• Exercise 1.1: Retrieve all nodes from the database.

MATCH (n) 
RETURN n

• Exercise 1.2: Examine the data model for the graph.

CALL db.schema.visualization()

• Exercise 1.3: Retrieve all Person nodes.

MATCH (p:Person) 
RETURN p

• Exercise 1.4: Retrieve all Movie nodes.Exercício 1- Retrieving Nodes

MATCH (m:Movie) 
RETURN m

Exercício 2 – Filtering queries using propertyvalues
Coloque os comandos utilizado em cada item a seguir:

• Exercise 2.1: Retrieve all movies that were released in a specific year.

MATCH (m:Movie) where m.released = 2012 RETURN m

• Exercise 2.2: View the retrieved results as a table.

{
  "title": "Cloud Atlas",
  "tagline": "Everything is connected",
  "released": 2012
}

• Exercise 2.3: Query the database for all property keys.

CALL db.propertyKeys

• Exercise 2.4: Retrieve all Movies released in a specific year, returning their titles.

MATCH (m:Movie) where m.released = 2006 
RETURN m.title

• Exercise 2.5: Display title, released, and tagline values for every Movie node in the graph.

MATCH (m:Movie) 
RETURN m.title, m.released, m.tagline

• Exercise 2.6: Display more user-friendly headers in the table

MATCH (m:Movie) 
RETURN m.title as `Titulo`, m.released as `Lancamento`, m.tagline as `Slogan`

Exercício 3 - Filtering queries using relationships
Coloque os comandos utilizado em cada item a seguir:

• Exercise 3.1: Display the schema of the database.

CALL db.schema.visualization()

• Exercise 3.2: Retrieve all people who wrote the movie Speed Racer.

MATCH (p:Person)-[:WROTE]->(m:Movie {title: 'Speed Racer'}) 
RETURN p.name

• Exercise 3.3: Retrieve all movies that are connected to the person, Tom Hanks.

MATCH (m:Movie)<--(p:Person {name: 'Tom Hanks'}) 
RETURN m.title

• Exercise 3.4: Retrieve information about the relationships Tom Hanks had with the set of movies retrieved earlier.

MATCH (m:Movie)-[rel]-(:Person {name: 'Tom Hanks'}) 
RETURN m.title, type(rel)

• Exercise 3.5: Retrieve information about the roles that Tom Hanks acted in.

MATCH (m:Movie)-[rel:ACTED_IN]-(:Person {name: 'Tom Hanks'}) 
RETURN m.title, rel.roles

Exercício 4 – Filtering queries using WHERE clause
Coloque os comandos utilizado em cada item a seguir:
• Exercise 4.1: Retrieve all movies that Tom Cruise acted in.

MATCH (a:Person)-[:ACTED_IN]->(m:Movie) 
WHERE a.name = 'Tom Cruise' 
RETURN m.title as Movie

• Exercise 4.2: Retrieve all people that were born in the 70’s.

MATCH (p:Person) 
WHERE p.born >= 1970 AND p.born < 1980  
RETURN p.name, p.born

• Exercise 4.3: Retrieve the actors who acted in the movie The Matrix who were born after 1960.

MATCH (p:Person)-[:ACTED_IN]->(m:Movie) 
WHERE p.born > 1960 AND m.title = 'The Matrix' 
RETURN p.name, p.born, m.title

• Exercise 4.4: Retrieve all movies by testing the node label and a property.

MATCH (m) 
WHERE m:Movie AND m.title =~ 'The.*' 
RETURN m.title

• Exercise 4.5: Retrieve all people that wrote movies by testing the relationship between two nodes.

MATCH (a)-[rel]->(m) 
WHERE a:Person AND type(rel) = 'WROTE' AND m:Movie 
RETURN a.name as Name, m.title as Movie

• Exercise 4.6: Retrieve all people in the graph that do not have a property.

MATCH (p:Person) 
WHERE NOT exists (p.born) 
RETURN p.name

• Exercise 4.7: Retrieve all people related to movies where the relationship has a property.

MATCH (a:Person)-[rel]->(m:Movie) 
WHERE exists(rel.rating) 
RETURN a.name, m.title,  type(rel), rel.rating

• Exercise 4.8: Retrieve all actors whose name begins with James.

MATCH (p:Person)-[:ACTED_IN]->(m:Movie) 
WHERE p.name STARTS WITH 'James' 
RETURN p.name

• Exercise 4.9: Retrieve all all REVIEW relationships from the graph with filtered results.

MATCH (p:Person)-[:REVIEWED]->(m:Movie) 
WHERE p.name STARTS WITH 'James' 
RETURN p.name

• Exercise 4.10: Retrieve all people who have produced a movie, but have not directed a movie.

MATCH (a:Person)-[:PRODUCED]->(m:Movie) 
WHERE NOT ((a)-[:DIRECTED]->(:Movie)) 
RETURN a.name, m.title

• Exercise 4.11: Retrieve the movies and their actors where one of the actors also directed the movie.

MATCH (a:Person)-[:ACTED_IN]->(m:Movie)<-[:ACTED_IN]-(a2:Person) 
WHERE exists((a2)-[:DIRECTED]->(m)) 
RETURN a.name, m.title, a2.name AS `Diretor`

• Exercise 4.12: Retrieve all movies that were released in a set of years.

MATCH (m:Movie) 
WHERE m.released IN [2008, 2005] 
RETURN  m.title, m.released

• Exercise 4.13: Retrieve the movies that have an actor’s role that is the name of the movie.

MATCH (a:Person)-[r:ACTED_IN]->(m:Movie) 
WHERE m.title in r.roles 
RETURN  m.title as Movie, a.name as Actor, r.roles


Exercício 5 – Controlling query processing
Coloque os comandos utilizado em cada item a seguir:
• Exercise 5.1: Retrieve data using multiple MATCH patterns.

MATCH (a:Person)-[:ACTED_IN]->(m:Movie)<-[:DIRECTED]-(d:Person),
      (a2:Person)-[:ACTED_IN]->(m)
WHERE a.name = 'Gene Hackman'
RETURN m.title as movie, d.name AS director , a2.name AS `co-actors`

• Exercise 5.2: Retrieve particular nodes that have a relationship.

MATCH (follower:Person)-[:FOLLOWS]-(p:Person) 
WHERE follower.name = 'James Thompson' 
RETURN follower, p

• Exercise 5.3: Modify the query to retrieve nodes that are exactly three hops away.

MATCH (follower:Person)-[:FOLLOWS*3]-(p:Person) 
RETURN follower, p

• Exercise 5.4: Modify the query to retrieve nodes that are one and two hops away.

MATCH (follower:Person)-[:FOLLOWS*1..2]-(p:Person) 
RETURN follower, p

• Exercise 5.5: Modify the query to retrieve particular nodes that are connected no matter how many hops are required.

MATCH (follower:Person)-[:FOLLOWS*]-(p:Person) 
WHERE follower.name = 'James Thompson' 
RETURN follower, p

• Exercise 5.6: Specify optional data to be retrieved during the query.

MATCH (p:Person) 
WHERE p.name STARTS WITH 'James' 
OPTIONAL MATCH (p)-[r:DIRECTED]->(m:Movie) 
RETURN p.name, type(r), m.title

• Exercise 5.7: Retrieve nodes by collecting a list.

MATCH (a:Person)-[:ACTED_IN]->(m:Movie) 
WITH  m, collect(m.title) as movies 
RETURN a.name, movies

• Exercise 5.9: Retrieve nodes as lists and return data associated with the corresponding lists.

MATCH (a:Person)-[:REVIEWED]->(m:Movie) 
WITH  m, count(a) AS qtd_revisores, collect(a.name) as revisores 
RETURN m.title, revisores, qtd_revisores

• Exercise 5.10: Retrieve nodes and their relationships as lists.

MATCH (a:Person)-[:DIRECTED]->(m:Movie)<-[:ACTED_IN]-(a2:Person) 
WITH a, count(a2) AS qtd_atores, collect(a2.name) as Atores 
RETURN a.name as `Diretor`, Atores, qtd_atores

• Exercise 5.11: Retrieve the actors who have acted in exactly five movies.

MATCH (a:Person)-[:ACTED_IN]->(m:Movie) 
WITH  a, count(m) AS qtd_filmes 
WHERE qtd_filmes = 5 RETURN a.name as `Ator`, qtd_filmes

• Exercise 5.12: Retrieve the movies that have at least 2 directors with other optional data.

MATCH (p:Person)-[:DIRECTED]->(m:Movie) 
OPTIONAL MATCH (p2:Person)-[:REVIEWED]->(m)
WITH  m, count(p) AS qtd_diretores, collect(p.name) as Diretores, collect(p2.name) as Revisores
WHERE qtd_diretores = 2
RETURN m.title,Diretores, Revisores

Exercício 6 – Controlling results returned
Coloque os comandos utilizado em cada item a seguir:
• Exercise 6.1: Execute a query that returns duplicate records.

MATCH (p:Person)-[:ACTED_IN]->(m:Movie) 
WHERE m.released >= 1990 AND m.released < 2000
RETURN m.released, m.title, p.name

• Exercise 6.2: Modify the query to eliminate duplication.

MATCH (p:Person)-[:ACTED_IN]->(m:Movie) 
WHERE m.released >= 1990 AND m.released < 2000
RETURN m.released, collect(m.title), collect(p.name)

• Exercise 6.3: Modify the query to eliminate more duplication.

MATCH (p:Person)-[:ACTED_IN]->(m:Movie) 
WHERE m.released >= 1990 AND m.released < 2000
RETURN m.released, collect(distinct m.title), collect(p.name)

• Exercise 6.4: Sort results returned.

MATCH (p:Person)-[:ACTED_IN]->(m:Movie) 
WHERE m.released >= 1990 AND m.released < 2000
RETURN m.released, collect(distinct m.title), collect(p.name)
ORDER BY m.released DESC

• Exercise 6.5: Retrieve the top 5 ratings and their associated movies.

MATCH (:Person)-[r:REVIEWED]->(m:Movie)
RETURN  m.title AS movie, r.rating AS rating
ORDER BY r.rating DESC LIMIT 5

• Exercise 6.6: Retrieve all actors that have not appeared in more than 3 movies.

MATCH (a:Person)-[:ACTED_IN]->(m:Movie) 
WITH  a, count(m) AS qtd_filmes 
WHERE qtd_filmes <= 3
RETURN a.name as `Ator`, qtd_filmes

Exercício 7 – Working with cypher data
Coloque os comandos utilizado em cada item a seguir:
• Exercise 7.1: Collect and use lists.

MATCH (a:Person)-[:ACTED_IN]->(m:Movie)<-[:PRODUCED]-(a2:Person) 
WITH m, collect(distinct a.name) as Atores, collect(distinct a2.name) as Produtores 
RETURN m.title, Atores, Produtores
ORDER BY size(Atores)

• Exercise 7.2: Collect a list.

MATCH (a:Person)-[:ACTED_IN]->(m:Movie) 
WITH  a, collect(m.title) as Filmes 
WHERE size(Filmes) > 5
RETURN a.name as `Ator`, Filmes

• Exercise 7.3: Unwind a list.

MATCH (p:Person)-[:ACTED_IN]->(m:Movie)
WITH p, collect(m) AS movies
WHERE size(movies)  > 5
WITH p, movies UNWIND movies AS movie
RETURN p.name, movie.title

• Exercise 7.4: Perform a calculation with the date type.

MATCH (p:Person)-[:ACTED_IN]->(m:Movie)
WHERE p.name = 'Tom Hanks'
RETURN p.name, m.title, m.released, date().year - m.released AS Tempo_Lancamento, m.released - p.born AS IdadeAtor

Exercício 8 – Creating nodes
Coloque os comandos utilizado em cada item a seguir:
• Exercise 8.1: Create a Movie node.

CREATE (:Movie {title: 'Forrest Gump'})

• Exercise 8.2: Retrieve the newly-created node.

MATCH (m:Movie) 
WHERE m.title = 'Forrest Gump'
RETURN m

• Exercise 8.3: Create a Person node.

CREATE (:Person {name: 'Robin Wright'})

• Exercise 8.4: Retrieve the newly-created node.

MATCH (p:Person) 
WHERE p.name = 'Robin Wright'
RETURN p

• Exercise 8.5: Add a label to a node.

MATCH (m:Movie)
WHERE m.released < 2010
SET m:OlderMovie
RETURN labels(m)

• Exercise 8.6: Retrieve the node using the new label.

MATCH (m:OlderMovie)
RETURN m.title, m.released

• Exercise 8.7: Add the Female label to selected nodes.

MATCH (p:Person)
WHERE p.name STARTS WITH 'Robin' 
SET p:Female
RETURN labels(p)

• Exercise 8.8: Retrieve all Female nodes.

MATCH (f:Female)
RETURN f

• Exercise 8.9: Remove the Female label from the nodes that have this label.

MATCH (f:Female)
REMOVE f:Female

• Exercise 8.10: View the current schema of the graph.

CALL db.schema.visualization()

• Exercise 8.11: Add properties to a movie.

MATCH (m:Movie)
WHERE m.title = 'Forrest Gump'
SET m:OlderMovie, m.released = 1994, m.tagline = "Life is like a box of chocolates…you never know what you’re gonna get.", m.lengthInMinutes = 142
RETURN m

• Exercise 8.12: Retrieve an OlderMovie node to confirm the label and properties.

MATCH (m:OlderMovie)
WHERE m.title = 'Forrest Gump'
RETURN m

• Exercise 8.13: Add properties to the person, Robin Wright.

MATCH (p:Person)
WHERE p.name = 'Robin Wright'
SET p.born = 1966, p.birthPlace = "Dallas"
RETURN p

• Exercise 8.14: Retrieve an updated Person node.

MATCH (p:Person)
WHERE p.name = 'Robin Wright'
RETURN p

• Exercise 8.15: Remove a property from a Movie node.

MATCH (m:Movie)
WHERE m.title = 'Forrest Gump'
REMOVE m.lengthInMinutes
RETURN m

• Exercise 8.16: Retrieve the node to confirm that the property has been removed.

MATCH (m:Movie)
WHERE m.title = 'Forrest Gump'
RETURN m

• Exercise 8.17: Remove a property from a Person node.

MATCH (p:Person)
WHERE p.name = 'Robin Wright'
REMOVE p.birthPlace
RETURN p

• Exercise 8.18: Retrieve the node to confirm that the property has been removed.

MATCH (p:Person)
WHERE p.name = 'Robin Wright'
RETURN p


Exercício 9 – Creating relationships
Coloque os comandos utilizado em cada item a seguir:
• Exercise 9.1: Create ACTED_IN relationships.

MATCH (a:Person), (m:Movie)
WHERE a.name = 'Robin Wright' AND m.title = 'Forrest Gump'
CREATE (a)-[:ACTED_IN]->(m)
RETURN a, m

MATCH (a:Person), (m:Movie)
WHERE a.name = 'Tom Hanks' AND m.title = 'Forrest Gump'
CREATE (a)-[:ACTED_IN]->(m)
RETURN a, m

MATCH (a:Person), (m:Movie)
WHERE a.name = 'Gary Sinise' AND m.title = 'Forrest Gump'
CREATE (a)-[:ACTED_IN]->(m)
RETURN a, m

• Exercise 9.2: Create DIRECTED relationships.

MATCH (a:Person), (m:Movie)
WHERE a.name = 'Robert Zemeckis' AND m.title = 'Forrest Gump'
CREATE (a)-[:DIRECTED]->(m)
RETURN a, m

• Exercise 9.3: Create a HELPED relationship.

MATCH (p1:Person)
WHERE p1.name = 'Tom Hanks'
MATCH (p2:Person)
WHERE p2.name = 'Gary Sinise'
CREATE (p1)-[:HELPED]->(p2)

• Exercise 9.4: Query nodes and new relationships.

MATCH (p:Person)-[r]-(m:Movie)
WHERE m.title = 'Forrest Gump'
Return p, r, m

• Exercise 9.5: Add properties to relationships.

MATCH (a:Person), (m:Movie)
WHERE a.name = 'Tom Hanks' AND m.title = 'Forrest Gump'
CREATE (a)-[rel:ACTED_IN]->(m)
SET rel.roles = 'Forrest Gum'
RETURN a, m

MATCH (a:Person), (m:Movie)
WHERE a.name = 'Robin Wright' AND m.title = 'Forrest Gump'
CREATE (a)-[rel:ACTED_IN]->(m)
SET rel.roles = 'Jenny Curran'
RETURN a, m

MATCH (a:Person), (m:Movie)
WHERE a.name = 'Gary Sinise' AND m.title = 'Forrest Gump'
CREATE (a)-[rel:ACTED_IN]->(m)
SET rel.roles = 'Lieutenant Dan Taylor'
RETURN a, m

• Exercise 9.6: Add a property to the HELPED relationship.

MATCH (p1:Person)-[rel:HELPED]->(p2:Person)
WHERE p1.name = 'Tom Hanks' AND p2.name = 'Gary Sinise'
SET rel.research = 'war history'

• Exercise 9.7: View the current list of property keys in the graph.

call db.propertyKeys

• Exercise 9.8: View the current schema of the graph.

CALL db.schema.visualization()

• Exercise 9.9: Retrieve the names and roles for actors.

MATCH (p:Person)-[rel:ACTED_IN]->(m:Movie)
WHERE m.title = 'Forrest Gump'
RETURN p.name, rel.roles

• Exercise 9.10: Retrieve information about any specific relationships.

MATCH (p:Person)-[rel:HELPED]-(p2:Person)
RETURN p, p2

• Exercise 9.11: Modify a property of a relationship.

MATCH (p:Person)-[rel:ACTED_IN]->(m:Movie)
WHERE m.title = 'Forrest Gump' AND p.name = 'Gary Sinise'
SET rel.roles =['Lt. Dan Taylor']

• Exercise 9.12: Remove a property from a relationship.

MATCH (a:Person)-[rel:HELPED]->(p2:Person)
WHERE a.name = 'Tom Hanks' AND p2.name = 'Gary Sinise'
REMOVE rel.research
RETURN a, rel, p2

• Exercise 9.13: Confirm that your modifications were made to the graph.

MATCH (p:Person)-[rel:ACTED_IN]->(m:Movie)
WHERE m.title = 'Forrest Gump'
return p, rel, m

Exercício 10 – Deleting nodes and relationships
Coloque os comandos utilizado em cada item a seguir:
• Exercise 10.1: Delete a relationship.

MATCH (a:Person)-[r:HELPED]-(p2:Person)
DELETE r

• Exercise 10.2: Confirm that the relationship has been deleted.

MATCH (p:Person)-[r]-(m:Movie)
WHERE m.title = 'Forrest Gump'
Return p, r, m

• Exercise 10.3: Retrieve a movie and all of its relationships.

MATCH (p:Person)-[r]-(m:Movie)
WHERE m.title = 'Forrest Gump'
Return p, r, m

• Exercise 10.4: Try deleting a node without detaching its relationships.

Cannot delete node<171>, because it still has relationships. To delete this node, you must first delete its relationships.

• Exercise 10.5: Delete a Movie node, along with its relationships.

MATCH (m:Movie)
WHERE m.title = 'Forrest Gump'
DETACH DELETE m

• Exercise 10.6: Confirm that the Movie node has been deleted.

MATCH (m:Movie)
WHERE m.title = 'Forrest Gump'
RETURN  m





