@@preview

@@img https://miro.medium.com/v2/resize:fit:1000/format:webp/1*qa-KfbDEBFFMYVFjMheKDw.png|Cover image: Sharding and Indexes|

- Sharding splits one big database into several smaller ones. The shard key decides who owns each row.
- Even by row count is not even by load. One shard fills first, and a full shard is an outage for its slice of users.
- A directory (chunk to shard) makes rebalancing a metadata change plus a copy, not a rehash of every key.
- A query that does not use the shard key must ask every shard. Its speed is the slowest shard’s speed.
- Every index is a second write and a second claim on memory. On a sharded table it is also a placement decision.

### The incident: Foursquare, October 2010

Foursquare kept check-ins in MongoDB. About two months before the outage they had moved from one 66GB EC2 instance to a two-shard cluster. Each shard ran on its own 66GB instance with a replica slave. The data was cut into 200 chunks by user ID and split evenly, and each shard held about 33GB in RAM. It ran smoothly for two months.

Then growth stopped being even. According to the post-mortem 10gen published on 7 October 2010, one shard grew to about 67GB, past the 66GB of RAM on its host, while the other sat near 50GB.
