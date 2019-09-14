<div align="center">
  <img src="docs/img/vearch_logo.png">
</div>

[![Build Status](https://travis-ci.org/ansjsun/vearch.svg?branch=master)](https://travis-ci.org/ansjsun/vearch)

## Overview

Vearch is a scalable distributed system for efficient similarity search of deep learning vectors. 


## Architecture

![arc](docs/img/VearchArch.jpg)
[![FOSSA Status](https://app.fossa.io/api/projects/git%2Bgithub.com%2Fansjsun%2Fvearch.svg?type=shield)](https://app.fossa.io/projects/git%2Bgithub.com%2Fansjsun%2Fvearch?ref=badge_shield)

* Data Model

  space, documents, vectors, scalars

* Components

  `Master`, `Router` and `PartitionServer` 

* Master 

  Responsible for schema mananagement, cluster-level metadata, and resource coordination. 
  
* Router

  Provides RESTful API: `create`  , `delete`  `search` and `update` ; request routing, and result merging. 

* PartitionServer (PS)

  Hosts document partitions with raft-based replication.

  Gamma`is the core vector search engine. It provides the ability of storing, indexing and retrieving the vectors and scalars.


## Quick start

* Quickly build a distributed vector search system with RESTful API, please see [docs/Deploy.md](docs/Deploy.md).


* Vearch can be leveraged to build a complete visual search system to index billions of images. The image retrieval plugin for object detection and feature extraction is also required. For more information, please refer to [docs/Quickstart.md](docs/Quickstart.md).


## APIs and Use Cases


### LowLevelAPI
* [docs/APILowLevel.md](docs/APILowLevel.md)


### VisualSearchAPI
* [docs/APIVisualSearch.md](docs/APIVisualSearch.md)



## License
Licensed under the Apache License, Version 2.0. For detail see LICENSE and NOTICE.


[![FOSSA Status](https://app.fossa.io/api/projects/git%2Bgithub.com%2Fansjsun%2Fvearch.svg?type=large)](https://app.fossa.io/projects/git%2Bgithub.com%2Fansjsun%2Fvearch?ref=badge_large)