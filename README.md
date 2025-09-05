# Assignment 2: Document Similarity using MapReduce

**Name:** 

**Student ID:** 

## Approach and Implementation

### Mapper Design
[Explain the logic of your Mapper class. What is its input key-value pair? What does it emit as its output key-value pair? How does it help in solving the overall problem?]

### Reducer Design
[Explain the logic of your Reducer class. What is its input key-value pair? How does it process the values for a given key? What does it emit as the final output? How do you calculate the Jaccard Similarity here?]

### Overall Data Flow
[Describe how data flows from the initial input files, through the Mapper, shuffle/sort phase, and the Reducer to produce the final output.]

---

## Step-by-Step Execution Guide

This project is designed to run in a GitHub Codespaces environment.

**1. Build the Project**

Compile the source code and package it into a JAR file using Maven:
```bash
# Command to build the project
mvn clean package
```

**2. Run the MapReduce Job**

Execute the Hadoop job using the following command. Replace `<input_path>` with the path to your dataset in HDFS (or the local filesystem if using standalone mode) and `<output_path>` with your desired output directory.

```bash
# Command to run the MapReduce Job
hadoop jar target/docsim-1.0-SNAPSHOT.jar edu.uncc.itcs6190.docsim.DocumentSimilarity <input_path> <output_path>
```
**Example with `small_dataset.txt`:**
```bash
# Example execution command
hadoop jar target/docsim-1.0-SNAPSHOT.jar edu.uncc.itcs6190.docsim.DocumentSimilarity input/small_dataset.txt output/small_output
```

**3. View the Output**

Check the contents of the output directory to see the results:
```bash
# Command to view results
cat output/small_output/part-r-00000
```

---

## Challenges and Solutions

[Describe any challenges you faced during this assignment. This could be related to the algorithm design (e.g., how to generate pairs), implementation details (e.g., data structures, debugging in Hadoop), or environmental issues. Explain how you overcame these challenges.]

---
## Sample Input

**Input from `small_dataset.txt`**
```
Document1 This is a sample document containing words
Document2 Another document that also has words
Document3 Sample text with different words
```
## Sample Output

**Output from `small_dataset.txt`**
```
"Document1, Document2 Similarity: 0.56"
"Document1, Document3 Similarity: 0.42"
"Document2, Document3 Similarity: 0.50"
```
