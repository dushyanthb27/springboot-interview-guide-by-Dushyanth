# How do you implement database sharding? 🟠

## Question

How do you implement database sharding to scale data horizontally?

## Answer

Database sharding distributes data across multiple database instances based on a shard key. Each shard contains a subset of the data, allowing horizontal scaling. Sharding can be implemented at the application level using a routing datasource or using database-specific sharding features.

Implementation involves choosing a shard key (e.g., user ID, region), implementing a routing datasource that directs queries to the appropriate shard, and ensuring data is consistently routed to the correct shard.

## Code Example

```java
@Configuration
public class ShardingConfig {
    @Bean
    public DataSource shardingDataSource() {
        Map<Object, Object> targetDataSources = new HashMap<>();
        targetDataSources.put("shard1", shard1DataSource());
        targetDataSources.put("shard2", shard2DataSource());

        ShardingDataSource routingDataSource = new ShardingDataSource();
        routingDataSource.setTargetDataSources(targetDataSources);
        routingDataSource.setShardKeyResolver(this::resolveShard);
        return routingDataSource;
    }

    private String resolveShard(String shardKey) {
        // Hash-based sharding
        int hash = shardKey.hashCode();
        return hash % 2 == 0 ? "shard1" : "shard2";
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of database sharding and your ability to implement horizontal scaling.

## ⚠️ Common Mistake
Choosing a poor shard key that leads to uneven data distribution across shards.

## ⚡ Follow-up Questions

1. How do you choose a shard key?
2. What is the difference between sharding and partitioning?
3. How do you handle cross-shard queries?

## 📚 Related Topics

- [Read Replicas](./07-read-replicas.md)
- [Database per Service](./01-database-per-service.md)
- [Database Best Practices](./13-database-best-practices.md)

## Navigation

[← Previous Question](./08-database-migrations.md) | [Next Question →](./10-caching-strategies.md)
