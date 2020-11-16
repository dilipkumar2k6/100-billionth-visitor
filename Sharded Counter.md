# Sharded Counter 
- Create N counters in parallel.
- Pick a shard to increment transactionally at random for each item counted.
- To get the real current count sum up all the sharded counters.
- Contention is reduced by 1/N. Writes have been optimized because they have been spread over the different shards. 
# Paging Through Comments
How can comments be stored such that they can be paged through in roughly the order they were entered?

Approach 1:
- Get a sequence number and that's the order comments are displayed. 
- shared state like a single counter won't scale in high write environments.

Approach 2: 
- A sharded counter won't work in this situation either because summing the shared counters isn't transactional. 
- There's no way to guarantee each comment will get back the sequence number it allocated so we could have duplicates.

Approach 3:
- Generate (time stamp + user ID + user comment ID) as an ID
- Ordering by date is obvious
- The good thing is getting a time stamp is a local decision, it doesn't rely on writes and is scalable. 
-  The problem is timestamps are not unique, especially with a lot of users.

Approach 4:
- Create a counter per user.
- When a user adds a comment it's added to a user's comment list and a sequence number is allocated.
- Comments are added in a transactional context on a per user basis using Entity Groups.
- So each comment add is guaranteed to be unique because updates in an Entity Group are serialized.
