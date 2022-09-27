## Setup Environment
Run the below from your git directory:
```
git clone git@github.com:Evelyn-Bayes/kafka-monitor.git
cd kafka-monitor
./gradlew jar
cd docker
export PREFIX=localbuild/xinfra
export TAG=latest
make container
```
Run the below from the demo-environments xinfra directory
```
docker-compose up -d
```
## Create Artificial Load
```
docker exec -ti kafka1 kafka-topics --bootstrap-server kafka1:9092 --create --topic test --partitions 10 --replication-factor 3
```
Run each of the below in their own commandline terminal:
```
docker exec -ti kafka1 kafka-producer-perf-test --producer-props bootstrap.servers=kafka1:9092 --topic test --throughput 1000 --num-records 1000000 --record-size 1000

docker exec -ti kafka1 kafka-console-consumer --bootstrap-server kafka1:9092 --from-beginning --property "print.key=true" --topic test --group test

```
Additional, you can run the below if you want to see the clusters health under load:
```
docker exec -ti kafka1 kafka-producer-perf-test --producer-props bootstrap.servers=kafka1:9092 --topic test --throughput -1 --num-records 1000000000 --record-size 1000
```
## View The Metrics
Type the below into your browser:
```
localhost:3000
```
Use the following credentials:
```
Username=admin
Password=password
```
Then click on "General" in the upper lefthand corner then select the Xinfra folder.
## Cleanup The Environent
```
docker-compose down -v
docker rmi localbuild/xinfra:latest
```
