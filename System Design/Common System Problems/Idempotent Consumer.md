### Resources
- [Article on how the consumer works in outbox pattern](https://www.linkedin.com/pulse/kafka-idempotent-consumer-transactional-outbox-rob-golder/)

To create an idempotent consumer, we have a db which keeps track of the event ids which has been acked. It works like this:

For likes count:
```json
//consumer polls batch -> say from an outbox table's kafka messages
//de-dupe pk: user_id_video_id

consumer polls batch from Kafka/PubSub
dedupe PK: (user_id, video_id)

BEGIN TRANSACTION
    INSERT INTO video_likes (user_id, video_id)
		ON CONFLICT DO NOTHING                                   -- the insert IS the check
    IF 1 row inserted:
        UPDATE video_stats SET like_count = like_count + 1
            WHERE video_id = :vid                                -- or upsert if row may not exist
    -- if 0 rows: duplicate → do nothing
COMMIT

ack()                                                            -- after commit, always (even for duplicates)
```