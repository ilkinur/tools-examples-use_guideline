# Docker




## Docker Registry

Əgər hədəf sistemdə docker registry varsa və APİ versiyası görünürsə onla əlaqəyə keçmək, pentest etmək üçün:

#### Discovering Repositories 
We need to send a GET request to http://docker-rodeo.thm:5000/v2/_catalog to list all the repositories registered on the registry.

Send a GET request to http://docker-rodeo.thm:5000/v2/repository/name/tags/list to query all published tags

Grabbing the Data - http://docker-rodeo.thm:5000/v2/repository/name/manifests/tag_name

