- **Resume after failure**; Pick up where you left off
- **Scalable consumers**; Multiple consumers share partition ownership; replay still requires idempotency
- **At-least-once delivery**; Resume from retained events after failure; not an unlimited no-loss guarantee
1. **Use dedicated container**; Separate checkpoint data from application data
2. **Match consumer groups**; Each consumer group needs its own checkpoints
3. **Balance frequency**; Too frequent = overhead, too rare = reprocessing on failure
4. **Handle idempotency**; Design for at-least-once delivery (events may replay)
5. **Monitor checkpoint lag**; Track time between event enqueue and checkpoint
6. **Test failure scenarios**; Verify resume works correctly after crashes
7. **Use async store**; The async BlobCheckpointStore is more performant
8. **Checkpoint on success only**; Don't checkpoint failed events