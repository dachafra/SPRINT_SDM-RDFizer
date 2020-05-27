
# Installing and Running the SDM-RDFizer 
The SDM-RDFizer can run by building a docker container or by installing the RDFfizer locally. 


## Accessing the SDM-RDFizer via a docker

Building docker container.
Note: All documents in the same folder of the Dockerfile will be copied to the container.

```
docker build -t rdfizer .
```

To run the application, you need to map your data volume to `/data` folder of the container where data, mappings and config files should be located:

```
docker-compose up -d
```

Send a POST request with the configuration file to RDFizer the file

```
curl localhost:4000/graph_creation/mappings/your-config-file.ini
```

# Parameters to Run the SDM-RDFizer
The SDM-RDFizer receives as input a configuration file that indicates the location of the RML triple maps and the output RDF knowledge graph. This file indicates the values of additional variables required during the process of RDF knowledge graph creation:
```
main_directory: path to where the file RML triple maps are located. 
number_of_datasets: number of datasets to be semantified
output_folder: path and file where the output RDF knowledge graph will be stored
all_in_one_file: in case multiple datasets are semantified, if the resulting RDF knowledge graphs will be stored in one or multiple file. Options: yes: all the RDF triples will be stored in one file; no: otherwise.
remove_duplicate: indicates if duplicates will be removed from the output RDF knowledge graph. Options: yes: duplicated RDF triples will be eliminated; no: otherwise.
name: name of the file that will store the integrated RDF knowledge graph, whenever the all_in_one_file option is yes.
enrichment: If optimized duplicate removal functionalities are turned on or off. Options: yes: optimized duplicate removal functionalities are turned on; no: otherwise.
```

Additionally, for each dataset an entry of the dataset needs to be defined; the variables to be defined are as follows:

```
name: name of the file that will store the RDF knowledge graph resulting from executing the mappings indicated in the variable mapping in the entry dataset.
mapping: location of the file where the RML triple maps are defined and will be executed over the dataset in the entry dataset.
dbType: the type of database management system from where data is collected. Options: mysql, postgres
```

Finally, if the data sources are stored in a database, the following properties have to be also defined for each dataset:
```
user: the name of the user in the database
password: the password of the account 
host: host of the database management system
port: port to access the database management system

```

## Examples of configurations

#### Example of a config file for accessing two CSV datasets integrating them in an unique RDF knowledge graph

This configuration file indicates that one RDF knowledge graphs will be created from the execution of the RDF triple maps
${default:main_directory}/mappingDataset1.ttl and  ${default:main_directory}/mappingDataset2.ttl. Duplicates are eliminated in the RDF knowledge graph.

```
[default]
main_directory: /path/to/datasets

[datasets]
number_of_datasets: 2
output_folder: ${default:main_directory}/graph
all_in_one_file: yes
remove_duplicate: yes
enrichment: yes
name: OutputRDFkg1

[dataset1]
mapping: ${default:main_directory}/mappingDataset1.ttl 

[dataset2]
mapping: ${default:main_directory}/mappingDataset2.ttl 
```

#### Example of a config file for accessing two CSV datasets with separated RDF knowledge graph creation

This configuration file indicates that two RDF knowledge graphs will be created from the execution of the RDF triple maps
${default:main_directory}/mappingDataset1.ttl and  ${default:main_directory}/mappingDataset2.ttl. Duplicates are eliminated in both RDF knowledge graphs.

```
[default]
main_directory: main_directory: /path/to/datasets

[datasets]
number_of_datasets: 2
output_folder: ${default:main_directory}/graph
all_in_one_file: no
remove_duplicate: yes
enrichment: yes

[dataset1]
name: OutputRDFkg1
mapping: ${default:main_directory}/mappingDataset1.ttl 

[dataset2]
name: OutputRDFkg2
mapping: ${default:main_directory}/mappingDataset2.ttl 
```

#### Example of a config file for accessing two datasets in MySQL
This configuration file indicates that two RDF knowledge graphs will be created from the execution of the RDF triple maps
${default:main_directory}/mappingDataset1.ttl and  ${default:main_directory}/mappingDataset2.ttl. Duplicates are eliminated in both RDF knowledge graphs. The following variables need to be defined for accessing each dataset from the database management system.


```
[default]
main_directory: /path/to/datasets

[datasets]
number_of_datasets: 2
output_folder: ${default:main_directory}/graph
all_in_one_file: no
remove_duplicate: yes
enrichment: yes
dbType: mysql


[dataset1]
user: root
password: 06012009mj
host: localhost
port: 3306
name: OutputRDFkg1
mapping: ${default:main_directory}/mappingDataset1.ttl

[dataset2]
user: root
password: 06012009mj
host: localhost
port: 3306
name: OutputRDFkg2
mapping: ${default:main_directory}/mappingDataset2.ttl

```

#### Example of a config file for accessing data in Postgres
This configuration file indicates that one RDF knowledge graph will be created from the execution of the RDF triple map
${default:main_directory}/mappingDataset1.ttl. Duplicates are eliminated from the RDF knowledge graph. The variable ```db``` indicates the database in Postgres that will be accessed. 

```
[default]
main_directory:  $HOME$/rml-test-cases/test-cases

[datasets]
number_of_datasets: 1
output_folder: ${default:main_directory}/graph
all_in_one_file: no
remove_duplicate: yes
enrichment: yes
dbType: postgres


[dataset1]
user: postgres
password: postgres
host: localhost
db: databaseInProgess 
name: OutputRDFkg
mapping: ${default:main_directory}/mappingDataset1.ttl 

```
## Version 
```
3.2
```
## RML-Test Cases
See the results of the SDM-RDFizer over the RML test-cases at the [RML Implementation Report](http://rml.io/implementation-report/). Last test date: 13/04/2019

## License
This work is licensed under Apache 2.0
