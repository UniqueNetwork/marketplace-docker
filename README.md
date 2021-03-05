# Marketplace Docker

## Configuration

### Submodules

```
git submodule update --recursive
```

### Database

1. Copy .env.sample file to .env and specify POSTGRES_USER and POSTGRES_PASSWORD

### Marketplace Backend (Escrow and REST API)



### Mint Backend

1. Create collection (collection). Set admin (admin's seed phrase will be saved in config file)

2. Copy mint_config.sample.json file in root to mint_backend/src/config.json and edit it:

2.1 wsEndpoint: Node endpoint
2.2 ownerSeed: Seed phrase for collection admin
2.3 collectionId: Id of collection



### 

### UI


## License Information

Copyright 2021, Unique Network, Usetech Professional

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
