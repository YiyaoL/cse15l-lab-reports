# Lab Report Week 5: Researching Commands

*In this report we will look at three command-line options to use the command `find`. Find will recursivelly search for files matching some criteria. For each criteria (primary) there will be 3 examples to use it on the files from ./technical.* 

---
### The first primary is `-type c`. This will only search for files that match the given type `c`, where c is 'b', 'c', 'd', 'l', 'p', 'f', or 's' for block special file, character special file, directory, symbolic link, FIFO, regular file, or socket, respectively. This can be useful if we have multiple types of the same file and want to access one of them. 
- Eg1. Find all directories in technical (output includes itself)
    ```
    yil126@LAPTOP-FIMLB8QQ:/mnt/c/Users/liuyi/Desktop/docsearch$ cd technical
    yil126@LAPTOP-FIMLB8QQ:/mnt/c/Users/liuyi/Desktop/docsearch/technical$ find . -type d
    .
    ./911report
    ./biomed
    ./government
    ./government/About_LSC
    ./government/Alcohol_Problems
    ./government/Env_Prot_Agen
    ./government/Gen_Account_Office
    ./government/Media
    ./government/Post_Rate_Comm
    ./plos
    s```
- Eg2. Find all text files named "preface". 
    ```
    find . -path '*/test/*.py' -type f
    yil126@LAPTOP-FIMLB8QQ:/mnt/c/Users/liuyi/Desktop/docsearch/technical$ find -name preface.txt -type f
    ./911report/preface.txt
    ```
- Eg3. Find all .txt files that have the folder 911report in their path
    ```
    yil126@LAPTOP-FIMLB8QQ:/mnt/c/Users/liuyi/Desktop/docsearch/technical$ find . -type f -path "*/911report/*"
    ./911report/chapter-1.txt
    ./911report/chapter-10.txt
    ./911report/chapter-11.txt
    ./911report/chapter-12.txt
    ./911report/chapter-13.1.txt
    ./911report/chapter-13.2.txt
    ./911report/chapter-13.3.txt
    ./911report/chapter-13.4.txt
    ./911report/chapter-13.5.txt
    ./911report/chapter-2.txt
    ./911report/chapter-3.txt
    ./911report/chapter-5.txt
    ./911report/chapter-6.txt
    ./911report/chapter-7.txt
    ./911report/chapter-8.txt
    ./911report/chapter-9.txt
    ./911report/preface.txt
    ```
---
### A second primary we can use is `-mtime n`. This will return files whose modification time in seconds, subtracted from the initialization time, divided by 86400, is `n` (i.e., n is measured in days). Note that +n means more than n, n means exactly n, and -n means less than n. This can be useful if we cannot remember the name of a file but can remember that we modified it in a certain time range.

*In a similar pattern, we have:*

*`-atime  n`: The primary shall evaluate as true if the file access time subtracted from the initialization time, divided by 86400 (with any remainder discarded), is n.*

*`-ctime  n`: The primary shall evaluate as true if the time of last change of file status information subtracted from the initialization time, divided by 86400 (with any remainder discarded), is n.*

- Eg1. Find all files modified in the last day
    ```
    yil126@LAPTOP-FIMLB8QQ:/mnt/c/Users/liuyi/Desktop/docsearch$ find . -mtime -1
    .
    ./.git/FETCH_HEAD
    ./.git/logs/refs/remotes/origin/HEAD
    ./.git/logs/refs/remotes/upstream/HEAD
    ./.git/objects
    ./.git/refs/remotes/origin
    ./.git/refs/remotes/origin/HEAD
    ./.git/refs/remotes/upstream
    ./.git/refs/remotes/upstream/HEAD
    ./technical
    ```
- Eg2. Create a file and find all files modified in the last five minutes. The directory and the file are both modified.
    ```
    yil126@LAPTOP-FIMLB8QQ:/mnt/c/Users/liuyi/Desktop/docsearch$ touch "technical/911report/TestDoc.txt"
    yil126@LAPTOP-FIMLB8QQ:/mnt/c/Users/liuyi/Desktop/docsearch$ find technical/911report -mtime -0.00347
    technical/911report
    technical/911report/TestDoc.txt
    ```
**Each primary can be combined with others. For example, another one is `-exec utility_name [argument ...] ;`. In [this opengroup shell doc](https://pubs.opengroup.org/onlinepubs/9699919799/utilities/find.html), the usage of `-exec` is as explained as follows:**

**If the primary expression is punctuated by a semicolon, the utility utility_name shall be invoked once for each pathname and the primary shall evaluate as true if the utility returns a zero value as exit status.**

**A utility_name or argument containing only the two characters "{}" shall be replaced by the current pathname.** 

**If a utility_name or argument string contains the two characters "{}", but not just the two characters "{}", it is implementation-defined whether find replaces those two characters or uses the string without change.**

- Eg3. Combining exec with mtime, we can delete all the text files that were modified in the last five minutes(which should just be the TesDoc.txt we just touched). Execute remove on them, and then show that they really are removed. 
    ```
    yil126@LAPTOP-FIMLB8QQ:/mnt/c/Users/liuyi/Desktop/docsearch$ find -type f -mtime -0.00347 -exec rm {} \;
    yil126@LAPTOP-FIMLB8QQ:/mnt/c/Users/liuyi/Desktop/docsearch$ find technical/911report -mtime -0.00347
    technical/911report
    ```
---
### A third primary we can set is `-size  n[c]`. The unit `[c]` can be k or M or G, if the unit is not specified, then the size shall be in bytes. This can be useful when we combine it with converting types. For example, we might want to zip all the directories that are larger than a certain size.

- Eg1. Find all txt files with size in range 90k to 100k. 
    ```
    yil126@LAPTOP-FIMLB8QQ:/mnt/c/Users/liuyi/Desktop/docsearch$ find -type f -size +90k -size -100k
    ./technical/911report/chapter-5.txt
    ./technical/government/Alcohol_Problems/Session3-PDF.txt
    ./technical/government/Gen_Account_Office/ai00134.txt
    ./technical/government/Gen_Account_Office/Testimony_cg00010t.txt
    ```
- Eg2. Find all directories with size in range 3k to 5k
    ```
    yil126@LAPTOP-FIMLB8QQ:/mnt/c/Users/liuyi/Desktop/docsearch$ find -type d -size +3k -size -5k
    .
    ./.git
    ./.git/hooks
    ./.git/info
    ./.git/logs
    ./.git/logs/refs
    ...(24 more lines)  
    ./lib
    ./technical
    ./technical/911report
    ./technical/biomed
    ./technical/government
    ./technical/government/About_LSC
    ./technical/government/Alcohol_Problems
    ./technical/government/Env_Prot_Agen
    ./technical/government/Gen_Account_Office
    ./technical/government/Media
    ./technical/government/Post_Rate_Comm
    ./technical/plos
    ```
*The primaries can be combined using the following operators (in order of decreasing precedence):*

`( expression )`: *True if expression is true.*

`!  expression`: *Negation of a primary; the unary NOT operator.*

`expression  [-a]  expression`: *Conjunction of primaries; the AND operator is implied by the juxtaposition of two primaries or made explicit by the optional -a operator. The second expression shall not be evaluated if the first expression is false.*

`expression  -o  expression`: *Alternation of primaries; the OR operator. The second expression shall not be evaluated if the first expression is true.*
- Eg3. In government, find all files larger than or equal to (not less than) 245 kb.
    ```
    yil126@LAPTOP-FIMLB8QQ:/mnt/c/Users/liuyi/Desktop/docsearch/technical/government$ find ! -size -245k
    ./Env_Prot_Agen/bill.txt
    ./Gen_Account_Office/d01591sp.txt
    ./Gen_Account_Office/GovernmentAuditingStandards_yb2002ed.txt
    ./Gen_Account_Office/Statements_Feb28-1997_volume.txt
    ```
---