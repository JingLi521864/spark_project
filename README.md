# spark_project

## Pre-requisits
- Spark
- GitBash
- PowerShell

## Commands:

1. Find an interesting article online use GitBash to download, useing command: ``` curl " URL " -o " File name will be used" ``` I used ``` curl "http://shakespeare.mit.edu/hamlet/full.html" -o "sparkhamlet.txt" ```

2. Clean the data in txt file, using this command: 
``` $ sed -E 's/<[^>]*>/''/g' sparkhamlet.txt > hamlet.txt ``` then we can clean more words in this text file and useing this command: ``` $ sed -E 's/\b(is|IS|am|AM|are|are|he|He|She|she|them|Them|was|Was|Were|i|I|for|For|you|You|her|Her|him|Him|but|But|So|so|Will|will|Would|would|his|do|Do|it|It|my|My|on|as|at|in|to|me|with|of|that|those|a|up|To|may|In|And|and|if|If|no|No|yes|Yes|all|All|Is|A|be|or|Or|now|Now|we|WE|The|the|what|What|When|when|Where|where)\b/''/g' hamlet.txt > hamlet_clean.txt ```

## Spark:
Now I can use Spark to process the data from Hamlet

1. Open the Spark ``` spark-shell ```

2. Create a Resilient Distributed Dataset from the text file Command:
``` val rddHamlet = sc.textFile("hamlet_clean.txt") ```

3. map the words and reduceBykey words:
```  val rddHamletSplit = rddHamlet.flatMap(line => line.split(" ")).map(word => (word, 1)).reduceByKey((a, b)=> a + b) ```

4. sort the results: 
``` val rddSorted = rddHamletSplit.sortBy(_._2, false) ```

5. Becasue there are some ' . '; ' , ' so I count top13 ``` rddSorted.take(13)``` AS top 10 words and use ``` rddSorted.saveAsTextFile("top10") ``` to save results.
