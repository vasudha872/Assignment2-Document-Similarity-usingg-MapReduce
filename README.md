# Assignment 2: Document Similarity using MapReduce

**Name:*Vasudha Maganti* 

**Student ID:*801419675* 

## Approach and Implementation

### Mapper Design
Input: Each line of a document (document ID and words).

Logic: The mapper tokenizes the words and emits intermediate key-value pairs representing document IDs and words.

Output: Document ID as the key and words (comma-separated) as the value.

### Reducer Design
Input: Key = Document pair, Values = word sets.

Logic: For each pair of documents, the reducer calculates the intersection and union of words.

Output: Key = (doc1, doc2), Value = Jaccard similarity score (intersection / union).

### Overall Data Flow
Input datasets (ds1.txt, ds2.txt, ds3.txt) are uploaded into HDFS.
Mapper reads and emits document-word sets.
Shuffle & sort groups words by document pairs.
Reducer calculates Jaccard similarity for each pair and writes results into HDFS output.

---

## Setup and Execution

### ` Note: The below commands are the ones used for the Hands-on. You need to edit these commands appropriately towards your Assignment to avoid errors. `

### 1. **Start the Hadoop Cluster**

Run the following command to start the Hadoop cluster:

```bash
docker compose up -d
```

### 2. **Build the Code**

Build the code using Maven:

```bash
mvn clean package
```

### 4. **Copy JAR to Docker Container**

Copy the JAR file to the Hadoop ResourceManager container:

```bash
docker cp target/DocumentSimilarity-0.0.1-SNAPSHOT.jar resourcemanager:/opt/hadoop-3.2.1/share/hadoop/mapreduce/
```

### 5. **Move Dataset to Docker Container**

Copy the dataset to the Hadoop ResourceManager container:

```bash
docker cp shared-folder/input/data/input.txt resourcemanager:/opt/hadoop-3.2.1/share/hadoop/mapreduce/
```

### 6. **Connect to Docker Container**

Access the Hadoop ResourceManager container:

```bash
docker exec -it resourcemanager /bin/bash
```

Navigate to the Hadoop directory:

```bash
cd /opt/hadoop-3.2.1/share/hadoop/mapreduce/
```

### 7. **Set Up HDFS**

Create a folder in HDFS for the input dataset:

```bash
hadoop fs -mkdir -p /input/data
```

Copy the input dataset to the HDFS folder:

```bash
hadoop fs -put ./input.txt /input/data
```

### 8. **Execute the MapReduce Job**

Run your MapReduce job using the following command: Here I got an error saying output already exists so I changed it to output1 instead as destination folder

```bash
hadoop jar /opt/hadoop-3.2.1/share/hadoop/mapreduce/DocumentSimilarity-0.0.1-SNAPSHOT.jar com.example.controller.DocumentSimilarityDriver /input/data/input.txt /output1
```

### 9. **View the Output**

To view the output of your MapReduce job, use:

```bash
hadoop fs -cat /output1/*
```

### 10. **Copy Output from HDFS to Local OS**

To copy the output from HDFS to your local machine:

1. Use the following command to copy from HDFS:
    ```bash
    hdfs dfs -get /output1 /opt/hadoop-3.2.1/share/hadoop/mapreduce/
    ```

2. use Docker to copy from the container to your local machine:
   ```bash
   exit 
   ```
    ```bash
    docker cp resourcemanager:/opt/hadoop-3.2.1/share/hadoop/mapreduce/output1/ shared-folder/output/
    ```
3. Commit and push to your repo so that we can able to see your output


---

## Challenges and Solutions

ClassNotFoundException when Running the JAR

Challenge: When I executed the Hadoop job, I encountered a ClassNotFoundException error. This usually happens when Hadoop cannot locate the driver, mapper, or reducer class inside the JAR.

Cause: My pom.xml initially did not specify the correct Main-Class in the manifest, and sometimes the fully qualified class name in the run command didn’t match the package structure.

Solution:

I rebuilt the project with Maven to generate an updated JAR (mvn clean package).

I verified the JAR contents using jar tf target/<jar-name>.jar to ensure the classes (com.example.controller.DocumentSimilarityDriver, DocumentSimilarityMapper, DocumentSimilarityReducer) were included.

I corrected the Hadoop execution command to match the exact package and class name:
```bash
   hadoop jar DocumentSimilarity-0.0.1-SNAPSHOT.jar com.example.controller.DocumentSimilarityDriver /input/data/input.txt /output1

   ```
Output Directory Errors

Challenge: Hadoop failed when the output directory already existed.

Solution: Deleted the directory with hadoop fs -rm -r /output1 or changed the output path to /output2.

Challenge: classNotfoundexception

Solution: added a java folder inside src folder 

Path Handling in Docker and HDFS


Path Handling in Docker and HDFS

Challenge: Input files sometimes existed in the container filesystem but not in HDFS.

Solution: Verified paths carefully with hadoop fs -ls /input/data and always copied datasets into HDFS using hadoop fs -put.
---
## Sample Input

**Input from `small_dataset.txt`**
```
Document1 This is a sample document containing words
Document2 Another document that also has words
Document3 Sample text with different words
```
## Sample Output
```
(Document1, Document2 Similarity: 0.56)
(Document1, Document3 Similarity: 0.42)
(Document2, Document3 Similarity: 0.50)
```

**Output from `small_dataset.txt`**

## Obtained Output: (Place your obtained output here.)
```
Document2, Document3 Similarity: .54	
Document1, Document3 Similarity: .19	
Document1, Document2 Similarity: .48	
```

## Observation

dataNode1: Is the output for the 1st datanode

dataNode3: Is the output of all three datanodes together

