# Redis Architecture

### Avantages of Redis

Redis is a key-value store which allows data to be stored and accessed at lightning fast speeds.
Redis holds its database entirely in the memory, using the disk only for persistence.
Redis can replicate data to any number of slaves.

Following are certain advantages of Redis:
- Exceptionally fast: Redis is very fast and can perform about 110000 SETs per
second, about 81000 GETs per second.
- Supports rich data types: Redis natively supports most of the datatypes that
developers already know such as list, set, sorted set, and hashes. This makes it
easy to solve a variety of problems as we know which problem can be handled
better by which data type.
- Operations are atomic: All Redis operations are atomic, which ensures that if
two clients concurrently access, Redis server will receive the updated value.
- Multi-utility tool: Redis is a multi-utility tool and can be used in a number of use
cases such as caching, messaging-queues (Redis natively supports
Publish/Subscribe), any short-lived data in your application, such as web
application sessions, web page hit counts, etc.

Redis is an in-memory database but persistent on disk database, hence it
represents a different trade off where very high write and read speed is achieved
with the limitation of data sets that can't be larger than the memory. 

