# Exploring-Redis
Exploring Redis as a part of CS 157C Introduction to NoSQL Database Systems

## Redis Installation on Docker
Redis installed on Docker (Windows machine). Resources are mentioned below:
- https://hub.docker.com/_/redis
- https://redis.io/docs/stack/get-started/install/docker/

Installing Redis Server Using Docker Container
```
1. docker pull redis
2. docker run --name my-redis -p 6379:6379 -d redis
3. docker exec -it my-redis sh
4. redis-cli
5. ping (to check connection)
```

## Activity 1: Design Redis Data Model and Insert Car Data Instances

### Inserting to the database
a. One attribute with each command
```
HSET hegde_car_ads:1 title "Used 2018 Tesla Model X Performance"
HSET hegde_car_ads:1 car_brand "Tesla"
HSET hegde_car_ads:1 model "X"
HSET hegde_car_ads:1 year 2018
```
... and so on for each attribute
b. Multiple attributes at once
```
HSET hegde_car_ad:2 title "Used 2016 Tesla Model X 70D" car_brand
"Tesla" model "Model X" year 2016 mileage "79,494 miles"
transmission_type "Single-Speed Transmission" exterior "Obsidian
Black Met Exterior" drive_type "All Wheel Drive" interior "Black
Interior"
```
and so on ...

### Retrieval from database:
To get all the attributes: HGETALL hegde_car_ads:1
To get a single attribute: HGET hegde_car_ads:1 model

## Activity 2: Redis CLI Practice
### Datatype 1: String
Function: Update car mileage
#### Commands:
SET: sets the value of a key to a string.
GET: retrieves the value of a key.
INCRBY: increases mileage (in this case) by given amount
```
SET hegde_car_mileage:HondaCivic 35000
GET hegde_car_mileage:HondaCivic
INCRBY hegde_car_mileage:HondaCivic 1
```
#### Explanation:
The SET command sets the mileage of a car owned by Honda Civic to 35,000.
The GET command retrieves the mileage value for the car owned by Honda Civic.
#### Other useful features with STRING datatype:
● Store the car's make and model as a string for easy reference and filtering.
● Store the car's VIN number as a string to allow customers to look up its history and
verify its authenticity.
● Store the car's location as a string to allow customers to search for nearby listings.

### Datatype 2: Set
Function: Add car accessories
#### Commands:
SADD: adds one or more members to a set.
SMEMBERS: retrieves all the members of a set.
SISMEMBER: checks if an accessory is present in the car

```
SADD hegde_car_accessories:HondaCivic "Sunroof" "Leather Seats"
"Backup Camera"
SADD hegde_car_accessories:HondaCivic "DVD Player"
SMEMBERS hegde_car_accessories:HondaCivic
SISMEMBER hegde_car_accessories:HondaCivic "Sunroof" (returns 1)
SISMEMBER hegde_car_accessories:HondaCivic "Phone Mount" (returns 0)
```

#### Explanation:
The SADD command adds the given accessories to the car owned by HondaCivic.
The SMEMBERS command retrieves the list of accessories for the car owned by
HondaCivic.
#### Other useful features with SET datatype:
● Store a set of tags for each car listing (e.g. "electric", "SUV", "low mileage") to allow for easy filtering and search functionality.
● Store a set of images for each car listing to allow customers to browse through multiple pictures and get a better sense of the car's condition.

### Datatype 3: List
Function: Show car maintenance history
#### Commands:
LPUSH: adds a value to the beginning of a list.
LRANGE: retrieves values in the index range provided
RPOP: removes (pops) elements from the right (most recent)
LPOP: removes (pops) elements from the left (oldest)
```
LPUSH hegde_car_maintenance:HondaCivic "Brake Pad Replacement"
LPUSH hegde_car_maintenance:HondaCivic "Oil Change" “Tyre Change”
LRANGE car_maintenance:HondaCivic 0 -1
RPOP hegde_car_maintenance:HondaCivic
LPOP hegde_car_maintenance:HondaCivic
```

#### Explanation:
The LPUSH command adds the maintenance history items to the beginning of the list for the car owned by HondaCivic. The LRANGE command retrieves the full list of maintenance
history for the car owned by HondaCivic.
#### Other useful features with LIST datatype:
● Store a list of the car's previous owners to provide transparency and build trust with
potential buyers.
● Store a list of similar vehicles (e.g. make, model, year, mileage) to provide customers with recommendations and encourage them to explore similar listings.

### Datatype 4: Sorted Set
Function: Show car value history
#### Commands:
ZADD: adds members to a sorted set, or updates the score of an existing member.
ZRANGE: retrieves the members of a sorted set by their rank
ZREVRANGE: retrieves the reverse range (oldest value to newest)
```
ZADD hegde_car_value:HondaCivic 20220101 50000
ZADD hegde_car_value:HondaCivic 20220102 49500
ZRANGE hegde_car_value:HondaCivic 0 -1 WITHSCORES
ZREVRANGE hegde_car_value:HondaCivic 0 -1 WITHSCORES
```
#### Other useful features with SORTED SET datatype:
● Store a sorted set of car prices to allow for easy sorting and comparison across listings.
● Store a sorted set of car ratings (e.g. based on customer reviews or industry ratings) to provide customers with a quick way to compare different cars.
● Store a sorted set of car ages to allow customers to quickly filter out older or outdated listings.

### Datatype 5: Hash
Function: Search car by brand/model
#### Commands:
HSET: sets the value of a field in a hash.
HGET: retrieves the value of a field in a hash.
HGETALL: retrieves all the fields and values of a hash.
HKEYS: retrieves all keys for that particular ad (user can see the amount of details
provided)
HVALS: returns all the values in the hash
```
HSET hegde_car_inventory:Tesla "Model X" "Used 2018 Model X
Performance"
HSET hegde_car_inventory:Ford "F-150" "New 2022 F-150 XL"
HGET hegde_car_inventory:Tesla "Model X"
HGETALL hegde_car_inventory:Ford
HKEYS hegde_car_inventory:Tesla
```

The HSET command adds the given car brand and model to the car inventory hash with its
corresponding details. The HGET command retrieves the car details based on the given
brand and model.

#### Other features with HASH datatype:
● Store the car's year, make, and model as separate fields within a hash to allow for easy filtering and search functionality.
● Store the car's location and contact information as fields within a hash to allow
customers to quickly get in touch with the seller.
