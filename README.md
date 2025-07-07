# WordCount-Benchmark-on-Spark-and-Hadoop
Learned about setting up AWS EMR on AWS Cloud and setting up a local testing environment in Docker. <br>
Implemented and compared performance of **MapReduce** and **Spark** Word Count programs on AWS.

- **MapReduce Program**:
  - `map()` function emits key-value pairs of the form `(word, 1)` for each word.
  - `reduce()` function sums these values to count total occurrences per word.
  - **Shuffle and Sort phase** is automatically handled by the MapReduce framework.
  - Used `context.write(word, one)` to emit intermediate data.
  - Job configuration: 1 Map task and 7 Reduce tasks.
  - Completed execution in **48 seconds** on AWS (3 low-cost instances).
  - **Reduce phase identified as a performance bottleneck** due to aggregation on many unique words.

- **Spark Program**:
  - Used `flatMap` and `map` to replicate the behavior of MapReduce's `map()` function.
  - Used `reduceByKey` to replicate the behavior of MapReduce's `reduce()` function.
  - Key distinction:
    - `reduce()` in Spark aggregates to a single result.
    - `reduceByKey()` performs reduction per key and returns key-value pairs.
    - Using `reduce()` instead of `reduceByKey()` would result in an error in this context.
  - Spark lineage traced through RDD transformations:
    - Started from `HadoopRDD`, ending with `ShuffledRDD` via `reduceByKey`.
  - Completed execution in **15 seconds**, significantly faster than MapReduce.

- **Conclusion**:
  - Spark demonstrated superior performance for the Word Count task.
  - MapReduce's bottleneck lies in the Reduce phase, especially with a high number of unique keys.
