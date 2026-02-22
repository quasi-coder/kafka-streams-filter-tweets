# Kafka Streams — Filter Tweets

A Java application using Kafka Streams to filter tweets in real time, forwarding only tweets from users with more than 10,000 followers to an output topic.

---

## Tech Stack

| Tool | Purpose |
|------|---------|
| Apache Kafka Streams | Stream processing |
| Gson | JSON parsing |
| Maven | Build tool |

---

## How It Works

1. **Input topic** — Reads raw tweet JSON from `twitter_tweets` (produced by `kafka-producer-twitter`)
2. **Filter** — Extracts the `user.followers_count` field from each tweet using Gson and keeps only tweets where followers > 10,000
3. **Output topic** — Filtered tweets are forwarded to `important_tweets`
4. **Topology** — Built using `StreamsBuilder` and started as a `KafkaStreams` application

---

## Stream Topology

```
[twitter_tweets] → filter(followers > 10000) → [important_tweets]
```

---

## Configuration

| Setting | Value |
|---------|-------|
| Kafka bootstrap server | `127.0.0.1:9092` |
| Application ID | `demo-kafka-streams` |
| Input topic | `twitter_tweets` |
| Output topic | `important_tweets` |
| Key/Value Serde | `StringSerde` |

---

## Getting Started

1. **Create topics**
   ```bash
   kafka-topics --zookeeper 127.0.0.1:2181 --topic twitter_tweets --create --partitions 3 --replication-factor 1
   kafka-topics --zookeeper 127.0.0.1:2181 --topic important_tweets --create --partitions 3 --replication-factor 1
   ```

2. **Start the Twitter producer** (`kafka-producer-twitter`) to populate `twitter_tweets`

3. **Build and run**
   ```bash
   mvn clean package
   # Run StreamsFilterTweets via IDE or maven exec
   ```

4. **Consume filtered tweets**
   ```bash
   kafka-console-consumer --bootstrap-server localhost:9092 --topic important_tweets --from-beginning
   ```
