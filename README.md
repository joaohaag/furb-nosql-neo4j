Exercício 1- Retrieving Nodes

1.1: Retrieve all nodes from the database 

MATCH (n) RETURN n

1.2: Examine the data model of the graph

---verificar

Exercise 1.3: Retrieve all Person nodes.

MATCH (p:Person) RETURN p

Exercise 1.4: Retrieve all Movie nodes.

MATCH (m:Movie) RETURN m

Exercício 2 – Filtering queries using property
values

Exercise 2.1: Retrieve all movies that were released in a specific year.

MATCH (m:Movie) where m.released = 2012 RETURN m

Exercise 2.2: View the retrieved results as a table.

{
  "title": "Cloud Atlas",
  "tagline": "Everything is connected",
  "released": 2012
}


Exercise 2.3: Query the database for all property keys.

CALL db.propertyKeys

Exercise 2.4: Retrieve all Movies released in a specific year, returning
their titles.

MATCH (m:Movie) where m.released = 2006 RETURN m.title


Exercise 2.5: Display title, released, and tagline values for every Movie
node in the graph.

MATCH (m:Movie) RETURN m.title, m.released, m.tagline

Exercise 2.6: Display more user-friendly headers in the table

MATCH (m:Movie) RETURN m.title as `Titulo`, m.released as `Lancamento`, m.tagline as `Slogan`
