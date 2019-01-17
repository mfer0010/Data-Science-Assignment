# Importing the Data to neo4j
## Setup

* Firstly, neo4j must be installed on the local machine and have its server running on the localhost.  
To check the status of neo4j: `sudo service neo4j status`  
To start neo4j if its not running: `sudo service neo4j start`  
* Then run 01 - Data Extraction.ipynb to create the data files  
* Copy the files by running `sudo cp [file.csv] /var/lib/neo4j/import/[file.csv]`  
* Remove quotes in articles.csv by running `sudo sed -i 's/\"//g' articles.csv`, this might take a while

## Import Commands

`USING PERIODIC COMMIT 500  
LOAD CSV WITH HEADERS FROM "file:///articles.csv" AS csvLine  
CREATE (a:Article {key: csvLine.key, title: csvLine.title})`

`USING PERIODIC COMMIT 500  
LOAD CSV WITH HEADERS FROM "file:///authors.csv" AS csvLine  
MERGE (a:Author {name: csvLine.name})`

`USING PERIODIC COMMIT 500  
LOAD CSV WITH HEADERS FROM "file:///published.csv" AS csvLine  
MATCH (author:Author {name: csvLine.name}),(article:Article {key: csvLine.key})  
CREATE (author)-[:PUBLISHED]->(article)`

`USING PERIODIC COMMIT 500  
LOAD CSV WITH HEADERS FROM "file:///authors.csv" AS csvLine  
MATCH (a:Author {name: csvLine.name})  
SET a.university = csvLine.university`