# merrer
M(app)er (and) R(educ)er

[G4G](https://www.geeksforgeeks.org/hadoop-streaming-using-python-word-count-problem/) with comments removed, utf tag added, and converted to Python3. 

Works over text files with a Python3 installed and execute permissions.

Edited to only count words.

```
#grab text
mkdir books
curl https://raw.githubusercontent.com/cd-public/books/main/pg1342.txt -o books/austen.txt

#enter the container and copy text into container
hdfs dfs -mkdir /user/sandbox/books
hdfs dfs -copyFromLocal -f books/* /user/sandbox/books
hdfs dfs -ls /user/sandbox/books

#assuming permissions are maximal, python is installed, and the scrips are in the same directory as this code, print:
hdfs dfs -rm -r /user/sandbox/words
mapred streaming \
  -input /user/sandbox/books \
  -output /user/sandbox/words \
  -mapper mapper.py \
  -reducer reducer.py \
  -file mapper.py \
  -file reducer.py

#view the output
hdfs dfs -ls /user/sandbox/words
hdfs dfs -head /user/sandbox/words/part-00000

#or copy to local file system to view
hdfs dfs -copyToLocal /user/sandbox/words
ls words
head words/part-00000
```

