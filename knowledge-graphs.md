# Overview

KGs are interconnected entities under shared semantics.  

Knowledge graphs are a specific type of graph with an emphasis on contextual understanding. Critically, knowledge graphs must have an organizing
principle so that a user (or a computer system) can reason about the underlying data

# When to use Graph DBs

1. Does your problem need graph data?
  * shape of the data
  * queries are looking for patterns to the data
2. Do relationship in and across data help you understand your problem?  

Graph technology is optimized for relationship-first data organization so as to provide direct access to an entity’s relationships in the database. Given this, you will want to explore graph technology if the relationships between your entities are the most important features of your data.In contrast to relational technology, graph technology was created to minimize the transition from mental model to data
storage and retrieval. 

# Types of Knowledge Graphs

## Actioning KG

follows an entity (customer) across multiple systems like in a data fabric.  without the client having any knowledge that multiple backend systems were used.  making it possible to validate across systems and check for inconsistencies or violations of the semantics of the data. This improves the quality and correctness of the data and increases inter‐operability across the enterprise.

keeping data fresh is a metadata hub, which has an actioning knowledge graph at its core.

builds trust in data and promotes self-service data consumption across the enterprise. It can also map data assets to concepts in the enterprise ontologies to make them discoverable and accessible and promote self-service data consumption. It is the foundation for data management functions like data quality, data stewardship, and data governance.

## Decisioning KG 

does not drive actions directly but surfaces trends in the data, which can be used in several ways such as to extract a view or subgraph for:

* Specific analyses (e.g., monopartite graphs like customer-bought-product) yielding actionable knowledge that can be
written back into an actioning KG
* Human analysis (assisted by tooling) for data science exploration and experimentation, eventually possibly yielding insight
that is written to the actioning KG or influences organizational structure
* Further processing by downstream systems (training machine-learning models)

This KG can be separate or physically colocated with actioning KG

Use cases:

  * Finding and preventing fraud based on detecting communities of like behavior
  * Improving customer experience and patient outcomes by surfacing complex sequences of activities for journey analysis.
  * Preventing churn by combining individual data with community behavior and influential individuals in that network. 
  * Forecasting supply chain needs using a holistic view of dependencies and identified bottlenecks.
  * Recommending products based on customer history, seasonality, warehouse stock, and product influence on sales of other items.
  * Eliminating duplicates and ambiguous entities in data based on highly correlated attributes and actions.
  * Finding missing data using an existing data structure to predict undocumented relationships and attributes. 
  * What-if scenario planning and _next best action_ recommendations using alternative pathways and similarities, typically consumed by experts in the first instance and ultimately written back to an actioning knowledge graph so that online systems can have better NBAs.

## Other

  * metadata KGs:  basically, data catalogs
  * identity KGs:  things like KYC, entity resolution, MDM
  * pattern detection KGs
    * fraud detection
      * understand a customer's broader connected context vs rule-based system
    * skills matching
  * dependency KGs
    * OSS dependencies
    * UBO (Ultimate Beneficial Owner)
    * each dependency is modeled as a node with a relationship.  You find this using graph algos and pattern matching.  
    * is there a hidden dependency between this and that?
    * SQL doesn't handle recursion well
    * basically, these are DAGs.  The presence of a cycle means something is wrong with the graph.  
    * SPOF:  single point of failure analysis

# Organizing Principles

The type of organizing principle for a knowledge graph should always be driven by its intended use. There is little value in
building rich and expressive features if there is no associated process or agent (human or software) that makes use of them.
It is a very common mistake to aim for an overly ambitious metamodel up front.

Trying to _average out_ a network generally won’t work well for investigating relationships or forecasting, because real-world networks have uneven distributions of nodes and relationships. Because highly connected data does not adhere to an average distribution, network scientists use graph analytics to search for and interpret structures and relationship distributions in real-world data. Furthermore, dynamic behavior, particularly around sudden events and bursts, can’t be seen with a snapshot.

In general, when creating organizing principles for knowledge graphs you should embrace _just-enough_ semantics. Introduce semantic metadata for the use cases you currently have and add more as your use cases demand. You should not build a complex ontology when a simpler taxonomy or even just a property graph is enough to deliver the current capabilities required.

Some intuition and thought have to go into the design of an organizing principle (such as a taxonomy), and from there the data can be explored to discover its useful properties. 

an organizing principle can be an ontology, a concept scheme, or a vocabulary, and it describes the different entities in a particular domain as well as how they relate to one another.

# Ontologies

## Widely Used Ontologies

always use an existing one vs creating your own

* Library of Congress Classification (LCC)
* SNOMED CT:  SNOMED CT Standard Ontology, a semantic model that describes the meaning of core medical terms (such as clinical
findings, symptoms, diagnoses, medical procedures, and others) by grouping them into concepts, providing them with
synonyms and definitions, and relating them to each other through hierarchical and other types of relations
* FIBO (Financial Industry Business Ont)
* Schema.org (web)
* MeSH:  medical subject headings.  comprehensive, controlled vocab for indexing journal articles, books, etc.  

## OWL and Ontology "Languages"

OWL  Ontology Web Language:  allows you to build your ontology

You can use NL to describe the semantics of a custom ontology, but then they aren't machine readable.  Or use one of the standard languages like

* RDF Schema (resource description framework)
  * is a model for data exchange, not a guide for storage or querying
  * describes how data is serialized as triples of subj, predicate, object (like startnode, relationship, end node)
* OWL
* SKOS for taxonomical classification schemes

For example, suppose you use OWL as the framework for the organizing principle of a knowledge graph. In that case, a program capable of running
OWL-based reasoning would be able to - within computational limits - derive new facts from your data without it having to understand the domain of that data.

# Data Modeling

Graph data modeling is very similar to relational data modeling; the main difference is in the consideration of relationships between entities. With graph technologies, the conceptual data model is the actual physical data model.

## Elements

the lego pieces: 

* Entities
  * abstract
    * harder to model
    * `classes`:  abstract entities that represent kinds of things in the world that may serve as semantic type of other entities
      * `instances`:  entities that belong to a given class
      * sometimes these are known as `concepts`
      * an abstract entity can be a class if there are other entities that can be instances of it.  
    * `individuals`
    * some entities can be modeled as both a class and an individual
      * Secretary can be modeled as class and can also be an individual belonging to the class Occupation
      * some languages do not allow an entity to be both a class and an individual (OWL).  
  * concrete
* Relations
  * expresses a way that 2 or more entities are related to each other.
  * part-whole relation:  "a wheel is part of a car".  
    * known as a meronym/holonym
    * abbreviated BTP (broader term(partitive)) or NTP (narrower term(partitive))
  * semantic relatedness
    * a thermostat is related to temperature control
    * this is associative and abbrev RT (related term)
* Attributes
  * characteristic of an entity that is a literal value instead of a relation with another entity  
  * ...or a characteristic of the relation
  * it's not always clear when a characteristic should be a attribute or another relation.  
  * Complex Axioms and Rules
    * allow the modeling of more elaborate data semantics and enable reasoning.  
    * Person A is an uncle of Person B if he is the brother of some A's parent.  
    * not all languages have this
* Lexicalization
  * links a model element (entity, relation, attribute, etc) to one or more terms that express it in natural language (synonyms or lexical variants)
  * Rome = Vatican City
* contraints and rules

## Schema Requirements

* Competency Questions:  q's expressed in NL that the graph should be able to answer.  Enables stakeholders to tell us what elements they want the model to have w/o having expertise in semantic data modeling.  

* Defining a Class
  * determine instantiation criteria
    * what characteristics and criteria does an entity need to satisfy in order to belong to the class?  
  * determine identity criteria
    * What makes any 2 entities of the class distinct?  

You can represent class as 
* labels (easier but the can't define attribs and relations about the class)
* as nodes

Defining a Relation
* determine domain and range classes
  * what types of entities does the relation relate and in what direction
* determine instantiation reqs
  * what do 2 or more entities need to satisfy in order to be related through this relation?  
* representing lexicalizations
  * as properties
  * as nodes


## Schema Implementation

Using ChatGPT...

"Model in Neo4j the following information:"
* a person has a name and...
* an actor is a person
* a director is a person
* a movie has one or more directors
* a movie has one or more actors

transform competency questions into neo4j representation
* "Create a Neo4j model that can answer the following competency questions:
  * how much does a pizza weigh?  
  * What are the ingredients of a pizza margherita?  

I am building an ontology in OWL that should satifsy the following requirements:
1. a company can have clients
2. there are two types of clients, individuals and orgs

Define an OWL schema for these requirements...

Great, now expand the graph so that it is able to answer the following competency questions:  
Q1:  which movies are comedies?
Q2:  Are there any science fiction...

You are a semantic modeler, expert in Neo4j, I want you to help me model a knowledge graph about movies.  As a start, consider two entitties: PERSON and MOVIE and two relations between them, ACTED IN and PRODUCED.  

Great, now expand the graph so that it is able to answer the following compentency questions:  

Q1:  Which movies are comedies?  
Q2: Are there any science fiction movies that are also horror movies?  

I want to be able to also represent subgenres, for example that Black Comedy is a kind of Comedy.  

Which of the relations you have defined so far are transitive?  

Which of the relations you have defined so far are ambiguous?  

Which of the relations you have defined so far are vague?  

I want to also represent the character that an actor has played in a movie?  
  * this would be a ternary relationship
  * in this case it gets it wrong since we can't link a single actor to multiple characters in multiple movies.  The right way would be with a relation attribute.  

# Quality of Your KG

* semantic accuracy:  the degree to which the semantic asserts of a KG are accepted to be true 

Inaccuracies can be due to:
* inaccuracy of source data
* population issues
* misunderstand of modeling elements' semantics and knowledge
* lack of domain knowledge and expertise
* bias
* vagueness

* completeness
* inconsistencies
  * may be due to absence or nonenforcement of contraints and consistency rules
* understandability
  * the ease with which human consumers can understand and utilize the KG's elements, without misunderstnading or doubting their meaning.  
  * may be due to bad or inadequate descriptions, ambiguity, etc
* trustworthiness
* not linking quality to risks and benefits
  * using metrics with arbitrary value tresholds
  * OQuaRE metrics
    * class richness (CROnto):  mean number of instances per class
    * attribute richness (RROnto):  number of attribs/# of relations and attribs
    * see the table in the pdf for breakdown.  
* relevancy of ContextA vs relevancy to ContextB

# Graph Analytics

Graph analytics comes from graph theory and entails processing data based on relationships that connect it. It’s something we do to
answer specific questions using existing data (for social network analysis) or to discover how the connections in that data might evolve (for supply chain optimization).

Knowledge graphs provide the organizing principles to connect disparate datasets and a contextual platform for reasoning over linked information. Prime examples of this are POLE (Persons, Objects, Locations, and Events) databases often applied to governmental/law
enforcement use cases or in IT systems management where failures can be predicted or retrospectively analyzed using a knowledge
graph.

Despite their predictive power, most analytics and data science practices ignore relationships because it has been historically challenging
to process them at scale. 

Graph analytics excels at finding the unobvious because it can process patterns even when we
don’t exactly know what to look for, while graph-based ML canpredict how a graph might evolve. This is precisely what most data
scientists are trying to achieve!

* good at transitive relationships.  A causes B and B causes C therefore A causes C.  

marketing attribution use case: person A sees the marketing campaign; person A talks about it on social media;
person B is connected to person A and sees the comment; and, subsequently, person B buys the product. From the marketing
campaign manager’s perspective, the standard relational model fails to identify the attribution, since B did not see the
campaign and A did not respond to the campaign. The campaign looks like a failure, but its actual success (and
positive ROI) is discovered by the graph analytics algorithm through the transitive relationship between the marketing
campaign and the final customer purchase, through anintermediary (entity in the middle).

## Graph Queries

Most analysts start down the path of graph analytics with graph queries, which are (usually) human crafted and human readable.
They’re typically used for real-time pattern matching when we know the shape of the data we’re interested in. 

`Graph-local`: "the enemy of my enemy is my friend":  With a graph database and a graph query language, these kinds of
graph-local patterns are computationally cheap and straightforward to express.

`Graph-global`:  But what if we don’t know where to start the query or want to find patterns anywhere in the graph? We call these operations graph-global, and querying is not always the right way to tackle these challenges. A graph-global problem is often an indication that we should
instead consider a graph algorithm.

build a NL interface over Neo4J using spaCy with its rule-based matching capability
  * produces a Cypher query

## Graph Algorithms

Graph algorithms excel at finding global patterns and trends, but we’ll want to choose and tune the algorithms to suit our specific
questions. A decisioning knowledge graph should support a variety of algorithms and allow us to customize for future growth.

* popular paths
  * if we’re looking for the most influential person in a social graph, we’d use the PageRank algorithm, 1 which measures the
importance of a node in the graph relative to the others.
* influential nodes.

Graph algos fall into 5 categories:

* Community detection for finding clusters or likely partitions
* Centrality for determining the importance of distinct nodes in a network
* Similarity for evaluating how alike nodes are
* Heuristic link prediction for estimating the likelihood of nodes forming a relationship
* Pathfinding for evaluating optimal paths and determining route quality and availability
  * find the shortest path

## Graph Embeddings

Graph embeddings are a special type of algorithm that encodes the topology of a graph (its nodes and relationships) into a structure
suitable for consumption by ML processes. We use these when we know important data exists in the graph, but it’s unclear which pat‐
terns to look for and we’d like the ML pipeline to do the heavy lifting of discovering patterns. Graph embeddings can be used in conjunc‐
tion with graph queries and algorithms to enrich ML input data to provide additional features.

Embeddings encode a representation of what’s significant in our graph for our specific problem and then translate that into a vectorized format 

Graph embeddings are very useful because rather than running multiple algorithms to describe specific aspects of our graph topology, we can use graph structure itself as a predictor. Graph embeddings expand our predictive capabilities, but they typically takelonger to run and have more parameters to tune than other graphalgorithms. 

If we know what elements are predictive, we use queries and algorithms for feature engineering in ML. If we don’t know what is predictive, we use graph embeddings. Both are good ways to improve decisioning graphs.



# Tools

* [LEILA](https://www.mpi-inf.mpg.de/departments/databases-and-information-systems/research/yago-naga/leila):  pattern-based relation extraction tool.
* [FRED](http://wit.istc.cnr.it/stlab-tools/fred/demo/):  tool for automatically producing RDF/OWL ontologies and linked data from natural language sentences. 

# Common Things that Trip Up a KG

## Named Entity Linking/Entity Disambiguation

* UK, united kingdom, wales (as a part of UK), and EU.  
  * The way to overcome these limitations is to match the entities to known entries in an organizing principle to form a semantic
knowledge graph.

The process of matching entities to uniquely defined entries in a shared concept scheme is often referred to as named-entity linking or entity disambiguation.