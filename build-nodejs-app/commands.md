## To build 
docker build --tag pack-node .

## Run
docker run --init --publish 3000:3000 pack-node whoami 
docker run --init --publish 3000:3000 pack-node pwd                        

## ADD vs COPY

ADD - pulls from network, unzip

COPY - use copy most of the time, does less stuff