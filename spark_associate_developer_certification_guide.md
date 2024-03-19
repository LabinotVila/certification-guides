This content is all about what is needed to pass the `Databricks: Spark Associate Developer` exam.

##### Books

1. [Spark: The Definitive Guide](https://www.oreilly.com/library/view/spark-the-definitive/9781491912201/)
2. [Learning Spark: 2nd Edition](https://www.oreilly.com/library/view/learning-spark-2nd/9781492050032/)
3. [The Data Engineering's Guide to Apache Spark](https://www.databricks.com/resources/ebook/the-data-engineers-guide-to-apache-spark-and-delta-lake)

##### Lectures -> Youtube
1. [Advanced Apache Spark Training - Sameer Farooqui (Databricks)](https://www.youtube.com/watch?v=7ooZ4S7Ay6Y&ab_channel=SparkSummit)
2. [Apache Spark Core—Deep Dive](https://www.youtube.com/watch?v=daXEp4HmS-E&t=7s&ab_channel=Databricks)

##### Udemy -> Udemy
1. [Apache Spark 3 - Beyond Basics](https://www.udemy.com/course/apache-spark-3-beyond-basics/?couponCode=KEEPLEARNING)
2. [Apache Spark 3 - Databricks Certified](https://www.udemy.com/course/apache-spark-3-databricks-certified-associate-developer/?couponCode=KEEPLEARNING)

##### Exams -> Udemy
1. [Databricks Apache Spark 3.0 Dev Certification - Tests(Scala)](https://www.udemy.com/course/databricks-apache-spark-dev-certification-tests-scala/?couponCode=KEEPLEARNING)
2. [Databricks Certified Apache Spark 3.0 TESTS (Scala & Python)](https://www.udemy.com/course/databricks-certified-apache-spark-3-tests-scala-python/?couponCode=KEEPLEARNING)
3. [Databricks Certified Developer for Spark 3.0 Practice Exams](https://www.udemy.com/course/databricks-certified-developer-for-apache-spark-30-practice-exams/?couponCode=KEEPLEARNING)

##### PDF Exams -> Internet
1. [Databricks Certified Developer for Spark 3.0 Practice Exams
2. [PDF Exams](https://files.training.databricks.com/assessments/practice-exams/PracticeExam-DCADAS3-Python.pdf)
3. [More Demo Dumps](https://www.dumpsbase.com/freedumps/2022-real-databricks-certified-associate-developer-for-apache-spark-3-0-exam-dumps-dumpsbase.html)

##### Topics touched on the exam
1. When does a Spark application fail? (when executor fails, when driver fails, when data is not fully cached, etc.)
2. What is the most granular unit in the Spark hierarchy? (jobs, stages, tasks, etc.)
3. What does NOT help in optimizing a Spark application? (related to partitions, column merging, etc.)
4. What happens if there are more slots than tasks to process in a worker node? (resources are not fully utilized, etc.)
5. What is a task? (a unit of work that can fit into an executor, a unit of work that can fit into a machine, etc.)
6. What is a job?
7. What is the difference between actions and transformations?
8. Which one of Dataset API methods is most likely to invoke a shuffle? (`union`, `groupBy`, `filter`, etc.)
9. How many % of the following code will cache the dataframe? (a `.show()` is called on a Scala `range`)
10. How many jobs will the following code create? (a dataframe reading and schema infering)
11. A wide partitions exchanges data between which units? (partitions, executors, clusters, etc.)
12. We want to generate 25 partitions after a join, what is the right configuration to use?
13. What are valid Spark deployment modes? (YARN, Local, Standalone, etc.)
14. Which of the options helps garbage collecting? (increasing java heap space, serialization or deserialization, etc.)
15. Dataset API Questions
16. `split` function
17. `explode` function
18. Joins (`inner`, `left`, `crossJoin` and `anti`)
19. Renaming column
20. Overwriting column
21. Filtering with multiple conditions
22. Using `where` vs using `filter` difference
23. `date` and `time` manipulation (to and from unix, formatting, etc.)
24. Sorting `asc` and `desc` with and without nulls
25. Literals
26. `repartition` and `coalesce` (more than 2 questions)
27. `UDF`s
28. Aggregate functions (`dense_rank` and `rank`)
29. Printing schema
30. Finding transformations and actions
31. Collecting a dataset, extracting values and casting
32. `cast` columns of a dataset
33. Dataset reading and writing
34. Reading a raw CSV file
35. Reading a CSV file with schema and with separators
36. Read and write modes
37. Writing and overwriting a parquet
38. Partitioning by a column and writing

Do not rely on documentation online!
