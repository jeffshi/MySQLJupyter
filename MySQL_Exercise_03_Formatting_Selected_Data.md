
Copyright Jana Schaich Borg/Attribution-NonCommercial 4.0 International (CC BY-NC 4.0)

# MySQL Exercise 3: Formatting Selected Data

In this lesson, we are going to learn about three SQL clauses or functionalities that will help you format and edit the output of your queries.  We will also learn how to export the results of your formatted queries to a text file so that you can analyze them in other software packages such as Tableau or Excel.

**Begin by loading the SQL library into Jupyter, connecting to the Dognition database, and setting Dognition as the default database.**

```python
%load_ext sql
%sql mysql://studentuser:studentpw@mysqlserver/dognitiondb
%sql USE dognitiondb
```


```python
%load_ext sql
%sql mysql://studentuser:studentpw@mysqlserver/dognitiondb
%sql USE dognitiondb
```

    0 rows affected.





    []




## 1. Use AS to change the titles of the columns in your output

The AS clause allows you to assign an alias (a temporary name) to a table or a column in a table.  Aliases can be useful for increasing the readability of queries, for abbreviating long names, and for changing column titles in query outputs.  To implement the AS clause, include it in your query code immediately after the column or table you want to rename.  For example, if you wanted to change the name of the time stamp field of the completed_tests table from "created_at" to "time_stamp" in your output, you could take advantage of the AS clause and execute the following query:

```mySQL
SELECT dog_guid, created_at AS time_stamp
FROM complete_tests
```

Note that if you use an alias that includes a space, the alias must be surrounded in quotes:

```mySQL
SELECT dog_guid, created_at AS "time stamp"
FROM complete_tests
```

You could also make an alias for a table:

```mySQL
SELECT dog_guid, created_at AS "time stamp"
FROM complete_tests AS tests
```

Since aliases are strings, again, MySQL accepts both double and single quotation marks, but some database systems only accept single quotation marks. It is good practice to avoid using SQL keywords in your aliases, but if you have to use an SQL keyword in your alias for some reason, the string must be enclosed in backticks instead of quotation marks.

**Question 1: How would you change the title of the "start_time" field in the exam_answers table to "exam start time" in a query output?  Try it below:**


```python
%%sql
SELECT start_time AS "exam start time"
FROM exam_answers LIMIT 10
```

    10 rows affected.





<table>
    <tr>
        <th>exam start time</th>
    </tr>
    <tr>
        <td>2013-02-05 03:58:13</td>
    </tr>
    <tr>
        <td>2013-02-05 03:58:31</td>
    </tr>
    <tr>
        <td>2013-02-05 03:59:03</td>
    </tr>
    <tr>
        <td>2013-02-05 03:59:10</td>
    </tr>
    <tr>
        <td>2013-02-05 03:59:22</td>
    </tr>
    <tr>
        <td>2013-02-05 03:59:36</td>
    </tr>
    <tr>
        <td>2013-02-05 03:59:41</td>
    </tr>
    <tr>
        <td>2013-02-05 04:00:00</td>
    </tr>
    <tr>
        <td>2013-02-05 04:00:16</td>
    </tr>
    <tr>
        <td>2013-02-05 04:00:35</td>
    </tr>
</table>



## 2. Use DISTINCT to remove duplicate rows

Especially in databases like the Dognition database where no primary keys were declared in each table, sometimes entire duplicate rows can be entered in error.  Even with no duplicate rows present, sometimes your queries correctly output multiple instances of the same value in a column, but you are interested in knowing what the different possible values in the column are, not what each value in each row is. In both of these cases, the best way to arrive at the clean results you want is to instruct the query to return only values that are distinct, or different from all the rest.  The SQL keyword that allows you to do this is called DISTINCT.  To use it in a query, place it directly after the word SELECT in your query.

For example, if we wanted a list of all the breeds of dogs in the Dognition database, we could try the following query from a previous exercise:

```mySQL
SELECT breed
FROM dogs;
```

However, the output of this query would not be very helpful, because it would output the entry for every single row in the breed column of the dogs table, regardless of whether it duplicated the breed of a previous entry.  Fortunately, we could arrive at the list we want by executing the following query with the DISTINCT modifier:

```mySQL
SELECT DISTINCT breed
FROM dogs;
```

**Try it yourself (If you do not limit your output, you should get 2006 rows in your output):**


```python
%sql SELECT DISTINCT breed FROM dogs LIMIT 10
```

    10 rows affected.





<table>
    <tr>
        <th>breed</th>
    </tr>
    <tr>
        <td>Labrador Retriever</td>
    </tr>
    <tr>
        <td>Shetland Sheepdog</td>
    </tr>
    <tr>
        <td>Golden Retriever</td>
    </tr>
    <tr>
        <td>Shih Tzu</td>
    </tr>
    <tr>
        <td>Siberian Husky</td>
    </tr>
    <tr>
        <td>Mixed</td>
    </tr>
    <tr>
        <td>Shih Tzu-Poodle Mix</td>
    </tr>
    <tr>
        <td>German Shepherd Dog-Pembroke Welsh Corgi Mix</td>
    </tr>
    <tr>
        <td>Vizsla</td>
    </tr>
    <tr>
        <td>Pug</td>
    </tr>
</table>



If you scroll through the output, you will see that no two entries are the same.  Of note, if you use the DISTINCT clause on a column that has NULL values, MySQL will include one NULL value in the DISTINCT output from that column.

<mark> When the DISTINCT clause is used with multiple columns in a SELECT statement, the combination of all the columns together is used to determine the uniqueness of a row in a result set.</mark>  
     
For example, if you wanted to know all the possible combinations of states and cities in the users table, you could query:

```mySQL
SELECT DISTINCT state, city
FROM users;
```
**Try it (if you don't limit your output you'll see 3999 rows in the query result, of which the first 1000 are displayed):**



```python
%sql SELECT DISTINCT state, city FROM users LIMIT 10
```

    10 rows affected.





<table>
    <tr>
        <th>state</th>
        <th>city</th>
    </tr>
    <tr>
        <td>ND</td>
        <td>Grand Forks</td>
    </tr>
    <tr>
        <td>MA</td>
        <td>Barre</td>
    </tr>
    <tr>
        <td>CT</td>
        <td>Darien</td>
    </tr>
    <tr>
        <td>IL</td>
        <td>Winnetka</td>
    </tr>
    <tr>
        <td>NC</td>
        <td>Raleigh</td>
    </tr>
    <tr>
        <td>WA</td>
        <td>Auburn</td>
    </tr>
    <tr>
        <td>CO</td>
        <td>Fort Collins</td>
    </tr>
    <tr>
        <td>WA</td>
        <td>Seattle</td>
    </tr>
    <tr>
        <td>WA</td>
        <td>Bainbridge Island</td>
    </tr>
    <tr>
        <td>WA</td>
        <td>Bremerton</td>
    </tr>
</table>



If you examine the query output carefully, you will see that there are many rows with California (CA) in the state column and four rows that have Gainesville in the city column (Georgia, Arkansas, Florida, and Virginia all have cities named Gainesville in our user table), but no two rows have the same state and city combination. 

When you use the DISTINCT clause with the LIMIT clause in a statement, MySQL stops searching when it finds the number of *unique* rows specified in the LIMIT clause, not when it goes through the number of rows in the LIMIT clause. 

For example, if the first 6 entries of the breed column in the dogs table were:

Labrador Retriever  
Shetland Sheepdog  
Golden Retriever  
Golden Retriever  
Shih Tzu  
Siberian Husky  

The output of the following query:

```mySQL
SELECT DISTINCT breed
FROM dogs LIMIT 5;
```
would be the first 5 different breeds:

Labrador Retriever  
Shetland Sheepdog  
Golden Retriever  
Shih Tzu  
Siberian Husky  

*not* the distinct breeds in the first 5 rows:

Labrador Retriever  
Shetland Sheepdog  
Golden Retriever  
Shih Tzu  

**Question 2: How would you list all the possible combinations of test names and subcategory names in complete_tests table? (If you do not limit your output, you should retrieve 45 possible combinations)**



```python
%%sql
SELECT DISTINCT test_name, subcategory_name
FROM complete_tests
```

    45 rows affected.





<table>
    <tr>
        <th>test_name</th>
        <th>subcategory_name</th>
    </tr>
    <tr>
        <td>Yawn Warm-up</td>
        <td>Empathy</td>
    </tr>
    <tr>
        <td>Yawn Game</td>
        <td>Empathy</td>
    </tr>
    <tr>
        <td>Eye Contact Warm-up</td>
        <td>Empathy</td>
    </tr>
    <tr>
        <td>Eye Contact Game</td>
        <td>Empathy</td>
    </tr>
    <tr>
        <td>Treat Warm-up</td>
        <td>Communication</td>
    </tr>
    <tr>
        <td>Arm Pointing</td>
        <td>Communication</td>
    </tr>
    <tr>
        <td>Foot Pointing</td>
        <td>Communication</td>
    </tr>
    <tr>
        <td>Watching</td>
        <td>Cunning</td>
    </tr>
    <tr>
        <td>Turn Your Back</td>
        <td>Cunning</td>
    </tr>
    <tr>
        <td>Cover Your Eyes</td>
        <td>Cunning</td>
    </tr>
    <tr>
        <td>Watching - Part 2</td>
        <td>Cunning</td>
    </tr>
    <tr>
        <td>One Cup Warm-up</td>
        <td>Memory</td>
    </tr>
    <tr>
        <td>Two Cup Warm-up</td>
        <td>Memory</td>
    </tr>
    <tr>
        <td>Memory versus Pointing</td>
        <td>Memory</td>
    </tr>
    <tr>
        <td>Memory versus Smell</td>
        <td>Memory</td>
    </tr>
    <tr>
        <td>Delayed Cup Game</td>
        <td>Memory</td>
    </tr>
    <tr>
        <td>Inferential Reasoning Warm-up</td>
        <td>Reasoning</td>
    </tr>
    <tr>
        <td>Inferential Reasoning Game</td>
        <td>Reasoning</td>
    </tr>
    <tr>
        <td>Physical Reasoning Warm-up</td>
        <td>Reasoning</td>
    </tr>
    <tr>
        <td>Physical Reasoning Game</td>
        <td>Reasoning</td>
    </tr>
    <tr>
        <td>Navigation Warm-up</td>
        <td>Spatial Navigation</td>
    </tr>
    <tr>
        <td>Navigation Learning</td>
        <td>Spatial Navigation</td>
    </tr>
    <tr>
        <td>Navigation Game</td>
        <td>Spatial Navigation</td>
    </tr>
    <tr>
        <td>Impossible Task Warm-up</td>
        <td>Impossible Task</td>
    </tr>
    <tr>
        <td>Impossible Task Game</td>
        <td>Impossible Task</td>
    </tr>
    <tr>
        <td>Numerosity Warm-Up</td>
        <td>Numerosity</td>
    </tr>
    <tr>
        <td>5 vs 1 Game</td>
        <td>Numerosity</td>
    </tr>
    <tr>
        <td>3 vs 1 Game</td>
        <td>Numerosity</td>
    </tr>
    <tr>
        <td>Warm-Up</td>
        <td>Social Bias</td>
    </tr>
    <tr>
        <td>1 vs 1 Game</td>
        <td>Social Bias</td>
    </tr>
    <tr>
        <td>5 vs 1 Game</td>
        <td>Social Bias</td>
    </tr>
    <tr>
        <td>Warm-up</td>
        <td>Smell Game</td>
    </tr>
    <tr>
        <td>Smell Game</td>
        <td>Smell Game</td>
    </tr>
    <tr>
        <td>Warm-Up</td>
        <td>Shell Game</td>
    </tr>
    <tr>
        <td>Switch</td>
        <td>Shell Game</td>
    </tr>
    <tr>
        <td>Slide</td>
        <td>Shell Game</td>
    </tr>
    <tr>
        <td>Warm-Up</td>
        <td>Self Control Game</td>
    </tr>
    <tr>
        <td>Self Control Game</td>
        <td>Self Control Game</td>
    </tr>
    <tr>
        <td>Warm-Up</td>
        <td>Expression Game</td>
    </tr>
    <tr>
        <td>Expression Game</td>
        <td>Expression Game</td>
    </tr>
    <tr>
        <td>Different Perspective</td>
        <td>Perspective Game</td>
    </tr>
    <tr>
        <td>Shared Perspective</td>
        <td>Perspective Game</td>
    </tr>
    <tr>
        <td>Stair Game</td>
        <td>Laterality</td>
    </tr>
    <tr>
        <td>Shaker Warm-Up</td>
        <td>Shaker Game</td>
    </tr>
    <tr>
        <td>Shaker Game</td>
        <td>Shaker Game</td>
    </tr>
</table>



## 3. Use ORDER BY to sort the output of your query

As you might have noticed already when examining the output of the queries you have executed thus far, databases do not have built-in sorting mechanisms that automatically sort the output of your query.  However, SQL permits the use of the powerful ORDER BY clause to allow you to sort the output according to your own specifications.  Let's look at how you would implement a simple ORDER BY clause.  

Recall our query outline:

<img src="https://duke.box.com/shared/static/l9v2khefe7er98pj1k6oyhmku4tz5wpf.jpg" width=400 alt="SELECT FROM WHERE ORDER BY" />

Your ORDER BY clause will come after everything else in the main part of your query, but before a LIMIT clause.  

If you wanted the breeds of dogs in the dog table sorted in alphabetical order, you could query:

```mySQL
SELECT DISTINCT breed
FROM dogs 
ORDER BY breed
```

**Try it yourself:**


```python
%%sql
SELECT DISTINCT breed
FROM dogs 
ORDER BY breed
```

    2006 rows affected.





<table>
    <tr>
        <th>breed</th>
    </tr>
    <tr>
        <td>-American Eskimo Dog Mix</td>
    </tr>
    <tr>
        <td>-American Pit Bull Terrier Mix</td>
    </tr>
    <tr>
        <td>-Anatolian Shepherd Dog Mix</td>
    </tr>
    <tr>
        <td>-Australian Cattle Dog Mix</td>
    </tr>
    <tr>
        <td>-Australian Shepherd Mix</td>
    </tr>
    <tr>
        <td>-Beagle Mix</td>
    </tr>
    <tr>
        <td>-Bichon Frise Mix</td>
    </tr>
    <tr>
        <td>-Bluetick Coonhound Mix</td>
    </tr>
    <tr>
        <td>-Border Collie Mix</td>
    </tr>
    <tr>
        <td>-Boxer Mix</td>
    </tr>
    <tr>
        <td>-Cairn Terrier Mix</td>
    </tr>
    <tr>
        <td>-Cavalier King Charles Spaniel Mix</td>
    </tr>
    <tr>
        <td>-Chesapeake Bay Retriever Mix</td>
    </tr>
    <tr>
        <td>-Chihuahua Mix</td>
    </tr>
    <tr>
        <td>-Cocker Spaniel Mix</td>
    </tr>
    <tr>
        <td>-Collie Mix</td>
    </tr>
    <tr>
        <td>-German Shepherd Dog Mix</td>
    </tr>
    <tr>
        <td>-Golden Retriever Mix</td>
    </tr>
    <tr>
        <td>-Great Dane Mix</td>
    </tr>
    <tr>
        <td>-Great Pyrenees Mix</td>
    </tr>
    <tr>
        <td>-Labrador Retriever Mix</td>
    </tr>
    <tr>
        <td>-Maltese Mix</td>
    </tr>
    <tr>
        <td>-Manchester Terrier Mix</td>
    </tr>
    <tr>
        <td>-Otterhound Mix</td>
    </tr>
    <tr>
        <td>-Polish Lowland Sheepdog Mix</td>
    </tr>
    <tr>
        <td>-Pomeranian Mix</td>
    </tr>
    <tr>
        <td>-Poodle Mix</td>
    </tr>
    <tr>
        <td>-Pug Mix</td>
    </tr>
    <tr>
        <td>-Rhodesian Ridgeback Mix</td>
    </tr>
    <tr>
        <td>-Rottweiler Mix</td>
    </tr>
    <tr>
        <td>-Russell Terrier Mix</td>
    </tr>
    <tr>
        <td>-Shih Tzu Mix</td>
    </tr>
    <tr>
        <td>-Siberian Husky Mix</td>
    </tr>
    <tr>
        <td>-Staffordshire Bull Terrier Mix</td>
    </tr>
    <tr>
        <td>-Toy Fox Terrier Mix</td>
    </tr>
    <tr>
        <td>-Whippet Mix</td>
    </tr>
    <tr>
        <td>-Yorkshire Terrier Mix</td>
    </tr>
    <tr>
        <td>Affenpinscher</td>
    </tr>
    <tr>
        <td>Affenpinscher-Afghan Hound Mix</td>
    </tr>
    <tr>
        <td>Affenpinscher-Airedale Terrier Mix</td>
    </tr>
    <tr>
        <td>Affenpinscher-Alaskan Malamute Mix</td>
    </tr>
    <tr>
        <td>Affenpinscher-American English Coonhound Mix</td>
    </tr>
    <tr>
        <td>Affenpinscher-Brussels Griffon Mix</td>
    </tr>
    <tr>
        <td>Affenpinscher-Poodle Mix</td>
    </tr>
    <tr>
        <td>Affenpinscher-Rat Terrier Mix</td>
    </tr>
    <tr>
        <td>Affenpinscher-Spanish Water Dog Mix</td>
    </tr>
    <tr>
        <td>Afghan Hound</td>
    </tr>
    <tr>
        <td>Afghan Hound-Affenpinscher Mix</td>
    </tr>
    <tr>
        <td>Afghan Hound-Airedale Terrier Mix</td>
    </tr>
    <tr>
        <td>Afghan Hound-Akita Mix</td>
    </tr>
    <tr>
        <td>Afghan Hound-Bloodhound Mix</td>
    </tr>
    <tr>
        <td>Afghan Hound-Border Collie Mix</td>
    </tr>
    <tr>
        <td>Afghan Hound-Golden Retriever Mix</td>
    </tr>
    <tr>
        <td>Airedale Terrier</td>
    </tr>
    <tr>
        <td>Airedale Terrier-Afghan Hound Mix</td>
    </tr>
    <tr>
        <td>Airedale Terrier-Border Collie Mix</td>
    </tr>
    <tr>
        <td>Airedale Terrier-Catahoula Leopard Dog Mix</td>
    </tr>
    <tr>
        <td>Airedale Terrier-German Shepherd Dog Mix</td>
    </tr>
    <tr>
        <td>Airedale Terrier-German Shorthaired Pointer Mix</td>
    </tr>
    <tr>
        <td>Airedale Terrier-Golden Retriever Mix</td>
    </tr>
    <tr>
        <td>Airedale Terrier-Labrador Retriever Mix</td>
    </tr>
    <tr>
        <td>Airedale Terrier-Portuguese Podengo Pequeno Mix</td>
    </tr>
    <tr>
        <td>Akita</td>
    </tr>
    <tr>
        <td>Akita- Mix</td>
    </tr>
    <tr>
        <td>Akita-Affenpinscher Mix</td>
    </tr>
    <tr>
        <td>Akita-Afghan Hound Mix</td>
    </tr>
    <tr>
        <td>Akita-Airedale Terrier Mix</td>
    </tr>
    <tr>
        <td>Akita-American Pit Bull Terrier Mix</td>
    </tr>
    <tr>
        <td>Akita-Bearded Collie Mix</td>
    </tr>
    <tr>
        <td>Akita-Belgian Tervuren Mix</td>
    </tr>
    <tr>
        <td>Akita-Bernese Mountain Dog Mix</td>
    </tr>
    <tr>
        <td>Akita-German Shepherd Dog Mix</td>
    </tr>
    <tr>
        <td>Akita-Labrador Retriever Mix</td>
    </tr>
    <tr>
        <td>Akita-Siberian Husky Mix</td>
    </tr>
    <tr>
        <td>Alaskan Malamute</td>
    </tr>
    <tr>
        <td>Alaskan Malamute- Mix</td>
    </tr>
    <tr>
        <td>Alaskan Malamute-American Pit Bull Terrier Mix</td>
    </tr>
    <tr>
        <td>Alaskan Malamute-Australian Kelpie Mix</td>
    </tr>
    <tr>
        <td>Alaskan Malamute-Beagle Mix</td>
    </tr>
    <tr>
        <td>Alaskan Malamute-Bernese Mountain Dog Mix</td>
    </tr>
    <tr>
        <td>Alaskan Malamute-Border Collie Mix</td>
    </tr>
    <tr>
        <td>Alaskan Malamute-Collie Mix</td>
    </tr>
    <tr>
        <td>Alaskan Malamute-English Cocker Spaniel Mix</td>
    </tr>
    <tr>
        <td>Alaskan Malamute-German Shepherd Dog Mix</td>
    </tr>
    <tr>
        <td>Alaskan Malamute-Labrador Retriever Mix</td>
    </tr>
    <tr>
        <td>Alaskan Malamute-Pomeranian Mix</td>
    </tr>
    <tr>
        <td>Alaskan Malamute-Siberian Husky Mix</td>
    </tr>
    <tr>
        <td>American English Coonhound</td>
    </tr>
    <tr>
        <td>American English Coonhound-Golden Retriever Mix</td>
    </tr>
    <tr>
        <td>American English Coonhound-Redbone Coonhound Mix</td>
    </tr>
    <tr>
        <td>American English Coonhound-Rhodesian Ridgeback Mix</td>
    </tr>
    <tr>
        <td>American English Coonhound-Weimaraner Mix</td>
    </tr>
    <tr>
        <td>American Eskimo Dog</td>
    </tr>
    <tr>
        <td>American Eskimo Dog- Mix</td>
    </tr>
    <tr>
        <td>American Eskimo Dog-Australian Shepherd Mix</td>
    </tr>
    <tr>
        <td>American Eskimo Dog-Border Collie Mix</td>
    </tr>
    <tr>
        <td>American Eskimo Dog-Chihuahua Mix</td>
    </tr>
    <tr>
        <td>American Eskimo Dog-Cocker Spaniel Mix</td>
    </tr>
    <tr>
        <td>American Eskimo Dog-Collie Mix</td>
    </tr>
    <tr>
        <td>American Eskimo Dog-English Springer Spaniel Mix</td>
    </tr>
    <tr>
        <td>American Eskimo Dog-Golden Retriever Mix</td>
    </tr>
    <tr>
        <td>American Eskimo Dog-Papillon Mix</td>
    </tr>
    <tr>
        <td>American Eskimo Dog-Pembroke Welsh Corgi Mix</td>
    </tr>
    <tr>
        <td>American Eskimo Dog-Pomeranian Mix</td>
    </tr>
    <tr>
        <td>American Eskimo Dog-Poodle Mix</td>
    </tr>
    <tr>
        <td>American Eskimo Dog-Russell Terrier Mix</td>
    </tr>
    <tr>
        <td>American Eskimo Dog-Shiba Inu Mix</td>
    </tr>
    <tr>
        <td>American Eskimo Dog-Siberian Husky Mix</td>
    </tr>
    <tr>
        <td>American Eskimo Dog-Staffordshire Bull Terrier Mix</td>
    </tr>
    <tr>
        <td>American Foxhound</td>
    </tr>
    <tr>
        <td>American Foxhound- Mix</td>
    </tr>
    <tr>
        <td>American Foxhound-Anatolian Shepherd Dog Mix</td>
    </tr>
    <tr>
        <td>American Foxhound-Beagle Mix</td>
    </tr>
    <tr>
        <td>American Foxhound-German Shepherd Dog Mix</td>
    </tr>
    <tr>
        <td>American Foxhound-Labrador Retriever Mix</td>
    </tr>
    <tr>
        <td>American Foxhound-Pointer Mix</td>
    </tr>
    <tr>
        <td>American Hairless Terrier</td>
    </tr>
    <tr>
        <td>American Hairless Terrier-Shetland Sheepdog Mix</td>
    </tr>
    <tr>
        <td>American Leopard Hound</td>
    </tr>
    <tr>
        <td>American Pit Bull Terrier</td>
    </tr>
    <tr>
        <td>American Pit Bull Terrier- Mix</td>
    </tr>
    <tr>
        <td>American Pit Bull Terrier-American English Coonhound Mix</td>
    </tr>
    <tr>
        <td>American Pit Bull Terrier-American Leopard Hound Mix</td>
    </tr>
    <tr>
        <td>American Pit Bull Terrier-American Staffordshire Terrier Mix</td>
    </tr>
    <tr>
        <td>American Pit Bull Terrier-Australian Cattle Dog Mix</td>
    </tr>
    <tr>
        <td>American Pit Bull Terrier-Australian Shepherd Mix</td>
    </tr>
    <tr>
        <td>American Pit Bull Terrier-Beagle Mix</td>
    </tr>
    <tr>
        <td>American Pit Bull Terrier-Border Collie Mix</td>
    </tr>
    <tr>
        <td>American Pit Bull Terrier-Boston Terrier Mix</td>
    </tr>
    <tr>
        <td>American Pit Bull Terrier-Boxer Mix</td>
    </tr>
    <tr>
        <td>American Pit Bull Terrier-Brittany Mix</td>
    </tr>
    <tr>
        <td>American Pit Bull Terrier-Bull Terrier Mix</td>
    </tr>
    <tr>
        <td>American Pit Bull Terrier-Bulldog Mix</td>
    </tr>
    <tr>
        <td>American Pit Bull Terrier-Bullmastiff Mix</td>
    </tr>
    <tr>
        <td>American Pit Bull Terrier-Cane Corso Mix</td>
    </tr>
    <tr>
        <td>American Pit Bull Terrier-Cardigan Welsh Corgi Mix</td>
    </tr>
    <tr>
        <td>American Pit Bull Terrier-Chinese Shar-Pei Mix</td>
    </tr>
    <tr>
        <td>American Pit Bull Terrier-Doberman Pinscher Mix</td>
    </tr>
    <tr>
        <td>American Pit Bull Terrier-Dogue de Bordeaux Mix</td>
    </tr>
    <tr>
        <td>American Pit Bull Terrier-German Shepherd Dog Mix</td>
    </tr>
    <tr>
        <td>American Pit Bull Terrier-Great Dane Mix</td>
    </tr>
    <tr>
        <td>American Pit Bull Terrier-Great Pyrenees Mix</td>
    </tr>
    <tr>
        <td>American Pit Bull Terrier-Greyhound Mix</td>
    </tr>
    <tr>
        <td>American Pit Bull Terrier-Labrador Retriever Mix</td>
    </tr>
    <tr>
        <td>American Pit Bull Terrier-Mastiff Mix</td>
    </tr>
    <tr>
        <td>American Pit Bull Terrier-Parson Russell Terrier Mix</td>
    </tr>
    <tr>
        <td>American Pit Bull Terrier-Plott Mix</td>
    </tr>
    <tr>
        <td>American Pit Bull Terrier-Pointer Mix</td>
    </tr>
    <tr>
        <td>American Pit Bull Terrier-Rhodesian Ridgeback Mix</td>
    </tr>
    <tr>
        <td>American Pit Bull Terrier-Rottweiler Mix</td>
    </tr>
    <tr>
        <td>American Pit Bull Terrier-Siberian Husky Mix</td>
    </tr>
    <tr>
        <td>American Pit Bull Terrier-Staffordshire Bull Terrier Mix</td>
    </tr>
    <tr>
        <td>American Pit Bull Terrier-Treeing Tennessee Brindle Mix</td>
    </tr>
    <tr>
        <td>American Pit Bull Terrier-Vizsla Mix</td>
    </tr>
    <tr>
        <td>American Pit Bull Terrier-Whippet Mix</td>
    </tr>
    <tr>
        <td>American Staffordshire Terrier</td>
    </tr>
    <tr>
        <td>American Staffordshire Terrier- Mix</td>
    </tr>
    <tr>
        <td>American Staffordshire Terrier-Airedale Terrier Mix</td>
    </tr>
    <tr>
        <td>American Staffordshire Terrier-American Pit Bull Terrier Mix</td>
    </tr>
    <tr>
        <td>American Staffordshire Terrier-Australian Shepherd Mix</td>
    </tr>
    <tr>
        <td>American Staffordshire Terrier-Beagle Mix</td>
    </tr>
    <tr>
        <td>American Staffordshire Terrier-Border Collie Mix</td>
    </tr>
    <tr>
        <td>American Staffordshire Terrier-Boston Terrier Mix</td>
    </tr>
    <tr>
        <td>American Staffordshire Terrier-Boxer Mix</td>
    </tr>
    <tr>
        <td>American Staffordshire Terrier-Bull Terrier Mix</td>
    </tr>
    <tr>
        <td>American Staffordshire Terrier-Bulldog Mix</td>
    </tr>
    <tr>
        <td>American Staffordshire Terrier-Bullmastiff Mix</td>
    </tr>
    <tr>
        <td>American Staffordshire Terrier-Catahoula Leopard Dog Mix</td>
    </tr>
    <tr>
        <td>American Staffordshire Terrier-Chinese Shar-Pei Mix</td>
    </tr>
    <tr>
        <td>American Staffordshire Terrier-Chow Chow Mix</td>
    </tr>
    <tr>
        <td>American Staffordshire Terrier-Dalmatian Mix</td>
    </tr>
    <tr>
        <td>American Staffordshire Terrier-Doberman Pinscher Mix</td>
    </tr>
    <tr>
        <td>American Staffordshire Terrier-Dogo Argentino Mix</td>
    </tr>
    <tr>
        <td>American Staffordshire Terrier-Dogue de Bordeaux Mix</td>
    </tr>
    <tr>
        <td>American Staffordshire Terrier-German Shepherd Dog Mix</td>
    </tr>
    <tr>
        <td>American Staffordshire Terrier-German Wirehaired Pointer Mix</td>
    </tr>
    <tr>
        <td>American Staffordshire Terrier-Golden Retriever Mix</td>
    </tr>
    <tr>
        <td>American Staffordshire Terrier-Gordon Setter Mix</td>
    </tr>
    <tr>
        <td>American Staffordshire Terrier-Great Dane Mix</td>
    </tr>
    <tr>
        <td>American Staffordshire Terrier-Great Pyrenees Mix</td>
    </tr>
    <tr>
        <td>American Staffordshire Terrier-Greyhound Mix</td>
    </tr>
    <tr>
        <td>American Staffordshire Terrier-Labrador Retriever Mix</td>
    </tr>
    <tr>
        <td>American Staffordshire Terrier-Lancashire Heeler Mix</td>
    </tr>
    <tr>
        <td>American Staffordshire Terrier-Mastiff Mix</td>
    </tr>
    <tr>
        <td>American Staffordshire Terrier-Miniature Bull Terrier Mix</td>
    </tr>
    <tr>
        <td>American Staffordshire Terrier-Neapolitan Mastiff Mix</td>
    </tr>
    <tr>
        <td>American Staffordshire Terrier-Portuguese Podengo Mix</td>
    </tr>
    <tr>
        <td>American Staffordshire Terrier-Redbone Coonhound Mix</td>
    </tr>
    <tr>
        <td>American Staffordshire Terrier-Rhodesian Ridgeback Mix</td>
    </tr>
    <tr>
        <td>American Staffordshire Terrier-Staffordshire Bull Terrier Mix</td>
    </tr>
    <tr>
        <td>American Staffordshire Terrier-Treeing Walker Coonhound Mix</td>
    </tr>
    <tr>
        <td>American Staffordshire Terrier-Weimaraner Mix</td>
    </tr>
    <tr>
        <td>American Staffordshire Terrier-Whippet Mix</td>
    </tr>
    <tr>
        <td>American Water Spaniel</td>
    </tr>
    <tr>
        <td>American Water Spaniel-Dachshund Mix</td>
    </tr>
    <tr>
        <td>Anatolian Shepherd Dog</td>
    </tr>
    <tr>
        <td>Anatolian Shepherd Dog-Border Collie Mix</td>
    </tr>
    <tr>
        <td>Anatolian Shepherd Dog-Great Pyrenees Mix</td>
    </tr>
    <tr>
        <td>Anatolian Shepherd Dog-Labrador Retriever Mix</td>
    </tr>
    <tr>
        <td>Anatolian Shepherd Dog-Mastiff Mix</td>
    </tr>
    <tr>
        <td>Appenzeller Sennenhunde</td>
    </tr>
    <tr>
        <td>Appenzeller Sennenhunde- Mix</td>
    </tr>
    <tr>
        <td>Appenzeller Sennenhunde-Border Collie Mix</td>
    </tr>
    <tr>
        <td>Appenzeller Sennenhunde-Collie Mix</td>
    </tr>
    <tr>
        <td>Australian Cattle Dog</td>
    </tr>
    <tr>
        <td>Australian Cattle Dog- Mix</td>
    </tr>
    <tr>
        <td>Australian Cattle Dog-American Pit Bull Terrier Mix</td>
    </tr>
    <tr>
        <td>Australian Cattle Dog-American Staffordshire Terrier Mix</td>
    </tr>
    <tr>
        <td>Australian Cattle Dog-Anatolian Shepherd Dog Mix</td>
    </tr>
    <tr>
        <td>Australian Cattle Dog-Australian Kelpie Mix</td>
    </tr>
    <tr>
        <td>Australian Cattle Dog-Australian Shepherd Mix</td>
    </tr>
    <tr>
        <td>Australian Cattle Dog-Basenji Mix</td>
    </tr>
    <tr>
        <td>Australian Cattle Dog-Beagle Mix</td>
    </tr>
    <tr>
        <td>Australian Cattle Dog-Bearded Collie Mix</td>
    </tr>
    <tr>
        <td>Australian Cattle Dog-Belgian Sheepdog Mix</td>
    </tr>
    <tr>
        <td>Australian Cattle Dog-Bernese Mountain Dog Mix</td>
    </tr>
    <tr>
        <td>Australian Cattle Dog-Bluetick Coonhound Mix</td>
    </tr>
    <tr>
        <td>Australian Cattle Dog-Border Collie Mix</td>
    </tr>
    <tr>
        <td>Australian Cattle Dog-Boxer Mix</td>
    </tr>
    <tr>
        <td>Australian Cattle Dog-Bull Terrier Mix</td>
    </tr>
    <tr>
        <td>Australian Cattle Dog-Cardigan Welsh Corgi Mix</td>
    </tr>
    <tr>
        <td>Australian Cattle Dog-Catahoula Leopard Dog Mix</td>
    </tr>
    <tr>
        <td>Australian Cattle Dog-Chow Chow Mix</td>
    </tr>
    <tr>
        <td>Australian Cattle Dog-Cocker Spaniel Mix</td>
    </tr>
    <tr>
        <td>Australian Cattle Dog-Curly-Coated Retriever Mix</td>
    </tr>
    <tr>
        <td>Australian Cattle Dog-Doberman Pinscher Mix</td>
    </tr>
    <tr>
        <td>Australian Cattle Dog-German Longhaired Pointer Mix</td>
    </tr>
    <tr>
        <td>Australian Cattle Dog-German Shepherd Dog Mix</td>
    </tr>
    <tr>
        <td>Australian Cattle Dog-Greyhound Mix</td>
    </tr>
    <tr>
        <td>Australian Cattle Dog-Labrador Retriever Mix</td>
    </tr>
    <tr>
        <td>Australian Cattle Dog-Rat Terrier Mix</td>
    </tr>
    <tr>
        <td>Australian Cattle Dog-Redbone Coonhound Mix</td>
    </tr>
    <tr>
        <td>Australian Cattle Dog-Rottweiler Mix</td>
    </tr>
    <tr>
        <td>Australian Cattle Dog-Shetland Sheepdog Mix</td>
    </tr>
    <tr>
        <td>Australian Cattle Dog-Siberian Husky Mix</td>
    </tr>
    <tr>
        <td>Australian Cattle Dog-Staffordshire Bull Terrier Mix</td>
    </tr>
    <tr>
        <td>Australian Cattle Dog-Treeing Walker Coonhound Mix</td>
    </tr>
    <tr>
        <td>Australian Cattle Dog-Weimaraner Mix</td>
    </tr>
    <tr>
        <td>Australian Kelpie</td>
    </tr>
    <tr>
        <td>Australian Kelpie-Australian Cattle Dog Mix</td>
    </tr>
    <tr>
        <td>Australian Kelpie-Basenji Mix</td>
    </tr>
    <tr>
        <td>Australian Kelpie-Border Collie Mix</td>
    </tr>
    <tr>
        <td>Australian Kelpie-Labrador Retriever Mix</td>
    </tr>
    <tr>
        <td>Australian Kelpie-Miniature Schnauzer Mix</td>
    </tr>
    <tr>
        <td>Australian Kelpie-Rottweiler Mix</td>
    </tr>
    <tr>
        <td>Australian Kelpie-Staffordshire Bull Terrier Mix</td>
    </tr>
    <tr>
        <td>Australian Labradoodle</td>
    </tr>
    <tr>
        <td>Australian Labradoodle-Poodle Mix</td>
    </tr>
    <tr>
        <td>Australian Shepherd</td>
    </tr>
    <tr>
        <td>Australian Shepherd- Mix</td>
    </tr>
    <tr>
        <td>Australian Shepherd-Alaskan Malamute Mix</td>
    </tr>
    <tr>
        <td>Australian Shepherd-American Pit Bull Terrier Mix</td>
    </tr>
    <tr>
        <td>Australian Shepherd-Australian Cattle Dog Mix</td>
    </tr>
    <tr>
        <td>Australian Shepherd-Basenji Mix</td>
    </tr>
    <tr>
        <td>Australian Shepherd-Beagle Mix</td>
    </tr>
    <tr>
        <td>Australian Shepherd-Bearded Collie Mix</td>
    </tr>
    <tr>
        <td>Australian Shepherd-Bloodhound Mix</td>
    </tr>
    <tr>
        <td>Australian Shepherd-Border Collie Mix</td>
    </tr>
    <tr>
        <td>Australian Shepherd-Cardigan Welsh Corgi Mix</td>
    </tr>
    <tr>
        <td>Australian Shepherd-Catahoula Leopard Dog Mix</td>
    </tr>
    <tr>
        <td>Australian Shepherd-Chow Chow Mix</td>
    </tr>
    <tr>
        <td>Australian Shepherd-Cocker Spaniel Mix</td>
    </tr>
    <tr>
        <td>Australian Shepherd-Collie Mix</td>
    </tr>
    <tr>
        <td>Australian Shepherd-Doberman Pinscher Mix</td>
    </tr>
    <tr>
        <td>Australian Shepherd-English Cocker Spaniel Mix</td>
    </tr>
    <tr>
        <td>Australian Shepherd-Flat-Coated Retriever Mix</td>
    </tr>
    <tr>
        <td>Australian Shepherd-German Shepherd Dog Mix</td>
    </tr>
    <tr>
        <td>Australian Shepherd-Golden Retriever Mix</td>
    </tr>
    <tr>
        <td>Australian Shepherd-Great Pyrenees Mix</td>
    </tr>
    <tr>
        <td>Australian Shepherd-Labrador Retriever Mix</td>
    </tr>
    <tr>
        <td>Australian Shepherd-Pembroke Welsh Corgi Mix</td>
    </tr>
    <tr>
        <td>Australian Shepherd-Poodle Mix</td>
    </tr>
    <tr>
        <td>Australian Shepherd-Pug Mix</td>
    </tr>
    <tr>
        <td>Australian Shepherd-Rhodesian Ridgeback Mix</td>
    </tr>
    <tr>
        <td>Australian Shepherd-Rottweiler Mix</td>
    </tr>
    <tr>
        <td>Australian Shepherd-Schipperke Mix</td>
    </tr>
    <tr>
        <td>Australian Shepherd-Shetland Sheepdog Mix</td>
    </tr>
    <tr>
        <td>Australian Shepherd-Shiba Inu Mix</td>
    </tr>
    <tr>
        <td>Australian Shepherd-Shih Tzu Mix</td>
    </tr>
    <tr>
        <td>Australian Shepherd-Siberian Husky Mix</td>
    </tr>
    <tr>
        <td>Australian Shepherd-St. Bernard Mix</td>
    </tr>
    <tr>
        <td>Australian Shepherd-Standard Poodle Mix</td>
    </tr>
    <tr>
        <td>Australian Shepherd-Weimaraner Mix</td>
    </tr>
    <tr>
        <td>Australian Terrier</td>
    </tr>
    <tr>
        <td>Australian Terrier-Lowchen Mix</td>
    </tr>
    <tr>
        <td>Australian Terrier-Parson Russell Terrier Mix</td>
    </tr>
    <tr>
        <td>Australian Terrier-Rottweiler Mix</td>
    </tr>
    <tr>
        <td>Australian Terrier-Yorkshire Terrier Mix</td>
    </tr>
    <tr>
        <td>Barbet</td>
    </tr>
    <tr>
        <td>Basenji</td>
    </tr>
    <tr>
        <td>Basenji- Mix</td>
    </tr>
    <tr>
        <td>Basenji-American Staffordshire Terrier Mix</td>
    </tr>
    <tr>
        <td>Basenji-American Water Spaniel Mix</td>
    </tr>
    <tr>
        <td>Basenji-Australian Cattle Dog Mix</td>
    </tr>
    <tr>
        <td>Basenji-Beagle Mix</td>
    </tr>
    <tr>
        <td>Basenji-Cardigan Welsh Corgi Mix</td>
    </tr>
    <tr>
        <td>Basenji-English Springer Spaniel Mix</td>
    </tr>
    <tr>
        <td>Basenji-German Shepherd Dog Mix</td>
    </tr>
    <tr>
        <td>Basenji-German Shorthaired Pointer Mix</td>
    </tr>
    <tr>
        <td>Basenji-Manchester Terrier Mix</td>
    </tr>
    <tr>
        <td>Basenji-Russell Terrier Mix</td>
    </tr>
    <tr>
        <td>Basenji-Shiba Inu Mix</td>
    </tr>
    <tr>
        <td>Basenji-Siberian Husky Mix</td>
    </tr>
    <tr>
        <td>Basenji-Whippet Mix</td>
    </tr>
    <tr>
        <td>Basset Hound</td>
    </tr>
    <tr>
        <td>Basset Hound- Mix</td>
    </tr>
    <tr>
        <td>Basset Hound-Airedale Terrier Mix</td>
    </tr>
    <tr>
        <td>Basset Hound-American English Coonhound Mix</td>
    </tr>
    <tr>
        <td>Basset Hound-American Pit Bull Terrier Mix</td>
    </tr>
    <tr>
        <td>Basset Hound-American Staffordshire Terrier Mix</td>
    </tr>
    <tr>
        <td>Basset Hound-Australian Cattle Dog Mix</td>
    </tr>
    <tr>
        <td>Basset Hound-Australian Shepherd Mix</td>
    </tr>
    <tr>
        <td>Basset Hound-Beagle Mix</td>
    </tr>
    <tr>
        <td>Basset Hound-Boston Terrier Mix</td>
    </tr>
    <tr>
        <td>Basset Hound-Cardigan Welsh Corgi Mix</td>
    </tr>
    <tr>
        <td>Basset Hound-Cocker Spaniel Mix</td>
    </tr>
    <tr>
        <td>Basset Hound-Dachshund Mix</td>
    </tr>
    <tr>
        <td>Basset Hound-German Shepherd Dog Mix</td>
    </tr>
    <tr>
        <td>Basset Hound-Great Pyrenees Mix</td>
    </tr>
    <tr>
        <td>Basset Hound-Labrador Retriever Mix</td>
    </tr>
    <tr>
        <td>Basset Hound-Pembroke Welsh Corgi Mix</td>
    </tr>
    <tr>
        <td>Basset Hound-Pointer Mix</td>
    </tr>
    <tr>
        <td>Basset Hound-Poodle Mix</td>
    </tr>
    <tr>
        <td>Basset Hound-Rottweiler Mix</td>
    </tr>
    <tr>
        <td>Basset Hound-Russell Terrier Mix</td>
    </tr>
    <tr>
        <td>Basset Hound-Staffordshire Bull Terrier Mix</td>
    </tr>
    <tr>
        <td>Beagle</td>
    </tr>
    <tr>
        <td>Beagle- Mix</td>
    </tr>
    <tr>
        <td>Beagle-American English Coonhound Mix</td>
    </tr>
    <tr>
        <td>Beagle-American Eskimo Dog Mix</td>
    </tr>
    <tr>
        <td>Beagle-American Foxhound Mix</td>
    </tr>
    <tr>
        <td>Beagle-Australian Cattle Dog Mix</td>
    </tr>
    <tr>
        <td>Beagle-Basset Hound Mix</td>
    </tr>
    <tr>
        <td>Beagle-Bluetick Coonhound Mix</td>
    </tr>
    <tr>
        <td>Beagle-Border Collie Mix</td>
    </tr>
    <tr>
        <td>Beagle-Border Terrier Mix</td>
    </tr>
    <tr>
        <td>Beagle-Boston Terrier Mix</td>
    </tr>
    <tr>
        <td>Beagle-Boxer Mix</td>
    </tr>
    <tr>
        <td>Beagle-Bulldog Mix</td>
    </tr>
    <tr>
        <td>Beagle-Cavalier King Charles Spaniel Mix</td>
    </tr>
    <tr>
        <td>Beagle-Chihuahua Mix</td>
    </tr>
    <tr>
        <td>Beagle-Chinese Shar-Pei Mix</td>
    </tr>
    <tr>
        <td>Beagle-Chow Chow Mix</td>
    </tr>
    <tr>
        <td>Beagle-Cocker Spaniel Mix</td>
    </tr>
    <tr>
        <td>Beagle-Collie Mix</td>
    </tr>
    <tr>
        <td>Beagle-Dachshund Mix</td>
    </tr>
    <tr>
        <td>Beagle-German Shepherd Dog Mix</td>
    </tr>
    <tr>
        <td>Beagle-Golden Retriever Mix</td>
    </tr>
    <tr>
        <td>Beagle-Greyhound Mix</td>
    </tr>
    <tr>
        <td>Beagle-Jack Russell Terrier Mix</td>
    </tr>
    <tr>
        <td>Beagle-Labrador Retriever Mix</td>
    </tr>
    <tr>
        <td>Beagle-Miniature Schnauzer Mix</td>
    </tr>
    <tr>
        <td>Beagle-Parson Russell Terrier Mix</td>
    </tr>
    <tr>
        <td>Beagle-Pekingese Mix</td>
    </tr>
    <tr>
        <td>Beagle-Pointer Mix</td>
    </tr>
    <tr>
        <td>Beagle-Poodle Mix</td>
    </tr>
    <tr>
        <td>Beagle-Pug Mix</td>
    </tr>
    <tr>
        <td>Beagle-Rat Terrier Mix</td>
    </tr>
    <tr>
        <td>Beagle-Russell Terrier Mix</td>
    </tr>
    <tr>
        <td>Beagle-Schipperke Mix</td>
    </tr>
    <tr>
        <td>Beagle-Shetland Sheepdog Mix</td>
    </tr>
    <tr>
        <td>Beagle-Siberian Husky Mix</td>
    </tr>
    <tr>
        <td>Beagle-Smooth Fox Terrier Mix</td>
    </tr>
    <tr>
        <td>Beagle-Toy Fox Terrier Mix</td>
    </tr>
    <tr>
        <td>Beagle-Treeing Walker Coonhound Mix</td>
    </tr>
    <tr>
        <td>Beagle-Whippet Mix</td>
    </tr>
    <tr>
        <td>Beaglier</td>
    </tr>
    <tr>
        <td>Bearded Collie</td>
    </tr>
    <tr>
        <td>Bearded Collie-Beagle Mix</td>
    </tr>
    <tr>
        <td>Bearded Collie-Bernese Mountain Dog Mix</td>
    </tr>
    <tr>
        <td>Bearded Collie-Border Collie Mix</td>
    </tr>
    <tr>
        <td>Bearded Collie-Labrador Retriever Mix</td>
    </tr>
    <tr>
        <td>Bearded Collie-Shih Tzu Mix</td>
    </tr>
    <tr>
        <td>Bearded Collie-Soft Coated Wheaten Terrier Mix</td>
    </tr>
    <tr>
        <td>Bearded Collie-Tibetan Terrier Mix</td>
    </tr>
    <tr>
        <td>Beauceron</td>
    </tr>
    <tr>
        <td>Beauceron-Rottweiler Mix</td>
    </tr>
    <tr>
        <td>Bedlington Terrier</td>
    </tr>
    <tr>
        <td>Bedlington Terrier-Belgian Laekenois Mix</td>
    </tr>
    <tr>
        <td>Bedlington Terrier-Whippet Mix</td>
    </tr>
    <tr>
        <td>Belgian Laekenois</td>
    </tr>
    <tr>
        <td>Belgian Malinois</td>
    </tr>
    <tr>
        <td>Belgian Malinois- Mix</td>
    </tr>
    <tr>
        <td>Belgian Malinois-American Staffordshire Terrier Mix</td>
    </tr>
    <tr>
        <td>Belgian Malinois-Belgian Tervuren Mix</td>
    </tr>
    <tr>
        <td>Belgian Malinois-Border Collie Mix</td>
    </tr>
    <tr>
        <td>Belgian Malinois-Chinese Shar-Pei Mix</td>
    </tr>
    <tr>
        <td>Belgian Malinois-Chow Chow Mix</td>
    </tr>
    <tr>
        <td>Belgian Malinois-Dutch Shepherd Mix</td>
    </tr>
    <tr>
        <td>Belgian Malinois-German Shepherd Dog Mix</td>
    </tr>
    <tr>
        <td>Belgian Malinois-Labrador Retriever Mix</td>
    </tr>
    <tr>
        <td>Belgian Malinois-Siberian Husky Mix</td>
    </tr>
    <tr>
        <td>Belgian Sheepdog</td>
    </tr>
    <tr>
        <td>Belgian Sheepdog- Mix</td>
    </tr>
    <tr>
        <td>Belgian Sheepdog-Border Collie Mix</td>
    </tr>
    <tr>
        <td>Belgian Sheepdog-Chow Chow Mix</td>
    </tr>
    <tr>
        <td>Belgian Sheepdog-Dutch Shepherd Mix</td>
    </tr>
    <tr>
        <td>Belgian Sheepdog-German Shepherd Dog Mix</td>
    </tr>
    <tr>
        <td>Belgian Sheepdog-Golden Retriever Mix</td>
    </tr>
    <tr>
        <td>Belgian Sheepdog-Lhasa Apso Mix</td>
    </tr>
    <tr>
        <td>Belgian Tervuren</td>
    </tr>
    <tr>
        <td>Belgian Tervuren-Basenji Mix</td>
    </tr>
    <tr>
        <td>Belgian Tervuren-Border Collie Mix</td>
    </tr>
    <tr>
        <td>Belgian Tervuren-Boston Terrier Mix</td>
    </tr>
    <tr>
        <td>Belgian Tervuren-Whippet Mix</td>
    </tr>
    <tr>
        <td>Bergamasco-Old English Sheepdog Mix</td>
    </tr>
    <tr>
        <td>Berger Picard</td>
    </tr>
    <tr>
        <td>Bernese Mountain Dog</td>
    </tr>
    <tr>
        <td>Bernese Mountain Dog-Border Collie Mix</td>
    </tr>
    <tr>
        <td>Bernese Mountain Dog-Chow Chow Mix</td>
    </tr>
    <tr>
        <td>Bernese Mountain Dog-English Springer Spaniel Mix</td>
    </tr>
    <tr>
        <td>Bernese Mountain Dog-Golden Retriever Mix</td>
    </tr>
    <tr>
        <td>Bernese Mountain Dog-Great Pyrenees Mix</td>
    </tr>
    <tr>
        <td>Bernese Mountain Dog-Greater Swiss Mountain Dog Mix</td>
    </tr>
    <tr>
        <td>Bernese Mountain Dog-Labrador Retriever Mix</td>
    </tr>
    <tr>
        <td>Bernese Mountain Dog-Poodle Mix</td>
    </tr>
    <tr>
        <td>Bernese Mountain Dog-Rottweiler Mix</td>
    </tr>
    <tr>
        <td>Bernese Mountain Dog-Standard Poodle Mix</td>
    </tr>
    <tr>
        <td>Bichon Frise</td>
    </tr>
    <tr>
        <td>Bichon Frise- Mix</td>
    </tr>
    <tr>
        <td>Bichon Frise-American Eskimo Dog Mix</td>
    </tr>
    <tr>
        <td>Bichon Frise-Cavalier King Charles Spaniel Mix</td>
    </tr>
    <tr>
        <td>Bichon Frise-Chinese Crested Mix</td>
    </tr>
    <tr>
        <td>Bichon Frise-Cocker Spaniel Mix</td>
    </tr>
    <tr>
        <td>Bichon Frise-Golden Retriever Mix</td>
    </tr>
    <tr>
        <td>Bichon Frise-Havanese Mix</td>
    </tr>
    <tr>
        <td>Bichon Frise-Jack Russell Terrier Mix</td>
    </tr>
    <tr>
        <td>Bichon Frise-Lhasa Apso Mix</td>
    </tr>
    <tr>
        <td>Bichon Frise-Maltese Mix</td>
    </tr>
    <tr>
        <td>Bichon Frise-Miniature Schnauzer Mix</td>
    </tr>
    <tr>
        <td>Bichon Frise-Pomeranian Mix</td>
    </tr>
    <tr>
        <td>Bichon Frise-Poodle Mix</td>
    </tr>
    <tr>
        <td>Bichon Frise-Shih Tzu Mix</td>
    </tr>
    <tr>
        <td>Bichon Frise-West Highland White Terrier Mix</td>
    </tr>
    <tr>
        <td>Bichon Frise-Yorkshire Terrier Mix</td>
    </tr>
    <tr>
        <td>Black and Tan Coonhound</td>
    </tr>
    <tr>
        <td>Black and Tan Coonhound- Mix</td>
    </tr>
    <tr>
        <td>Black and Tan Coonhound-Beagle Mix</td>
    </tr>
    <tr>
        <td>Black and Tan Coonhound-German Shepherd Dog Mix</td>
    </tr>
    <tr>
        <td>Black and Tan Coonhound-Golden Retriever Mix</td>
    </tr>
    <tr>
        <td>Black and Tan Coonhound-Labrador Retriever Mix</td>
    </tr>
    <tr>
        <td>Black and Tan Coonhound-Plott Mix</td>
    </tr>
    <tr>
        <td>Black Russian Terrier</td>
    </tr>
    <tr>
        <td>Bloodhound</td>
    </tr>
    <tr>
        <td>Bloodhound-Beagle Mix</td>
    </tr>
    <tr>
        <td>Bloodhound-Black and Tan Coonhound Mix</td>
    </tr>
    <tr>
        <td>Bloodhound-Bull Terrier Mix</td>
    </tr>
    <tr>
        <td>Bloodhound-Irish Setter Mix</td>
    </tr>
    <tr>
        <td>Bloodhound-Labrador Retriever Mix</td>
    </tr>
    <tr>
        <td>Bluetick Coonhound</td>
    </tr>
    <tr>
        <td>Bluetick Coonhound-Beagle Mix</td>
    </tr>
    <tr>
        <td>Bluetick Coonhound-German Shorthaired Pointer Mix</td>
    </tr>
    <tr>
        <td>Bluetick Coonhound-Jack Russell Terrier Mix</td>
    </tr>
    <tr>
        <td>Bluetick Coonhound-Pointer Mix</td>
    </tr>
    <tr>
        <td>Bluetick Coonhound-Rottweiler Mix</td>
    </tr>
    <tr>
        <td>Bluetick Coonhound-Treeing Walker Coonhound Mix</td>
    </tr>
    <tr>
        <td>Boerboel</td>
    </tr>
    <tr>
        <td>Bolognese</td>
    </tr>
    <tr>
        <td>Border Collie</td>
    </tr>
    <tr>
        <td>Border Collie- Mix</td>
    </tr>
    <tr>
        <td>Border Collie-Affenpinscher Mix</td>
    </tr>
    <tr>
        <td>Border Collie-Akita Mix</td>
    </tr>
    <tr>
        <td>Border Collie-American Eskimo Dog Mix</td>
    </tr>
    <tr>
        <td>Border Collie-American Foxhound Mix</td>
    </tr>
    <tr>
        <td>Border Collie-American Pit Bull Terrier Mix</td>
    </tr>
    <tr>
        <td>Border Collie-American Staffordshire Terrier Mix</td>
    </tr>
    <tr>
        <td>Border Collie-Australian Cattle Dog Mix</td>
    </tr>
    <tr>
        <td>Border Collie-Australian Kelpie Mix</td>
    </tr>
    <tr>
        <td>Border Collie-Australian Shepherd Mix</td>
    </tr>
    <tr>
        <td>Border Collie-Beagle Mix</td>
    </tr>
    <tr>
        <td>Border Collie-Bearded Collie Mix</td>
    </tr>
    <tr>
        <td>Border Collie-Beauceron Mix</td>
    </tr>
    <tr>
        <td>Border Collie-Belgian Sheepdog Mix</td>
    </tr>
    <tr>
        <td>Border Collie-Belgian Tervuren Mix</td>
    </tr>
    <tr>
        <td>Border Collie-Bernese Mountain Dog Mix</td>
    </tr>
    <tr>
        <td>Border Collie-Bloodhound Mix</td>
    </tr>
    <tr>
        <td>Border Collie-Bluetick Coonhound Mix</td>
    </tr>
    <tr>
        <td>Border Collie-Borzoi Mix</td>
    </tr>
    <tr>
        <td>Border Collie-Boxer Mix</td>
    </tr>
    <tr>
        <td>Border Collie-Brittany Mix</td>
    </tr>
    <tr>
        <td>Border Collie-Bull Terrier Mix</td>
    </tr>
    <tr>
        <td>Border Collie-Cavalier King Charles Spaniel Mix</td>
    </tr>
    <tr>
        <td>Border Collie-Chow Chow Mix</td>
    </tr>
    <tr>
        <td>Border Collie-Cocker Spaniel Mix</td>
    </tr>
    <tr>
        <td>Border Collie-Collie Mix</td>
    </tr>
    <tr>
        <td>Border Collie-Dalmatian Mix</td>
    </tr>
    <tr>
        <td>Border Collie-English Cocker Spaniel Mix</td>
    </tr>
    <tr>
        <td>Border Collie-English Setter Mix</td>
    </tr>
    <tr>
        <td>Border Collie-English Springer Spaniel Mix</td>
    </tr>
    <tr>
        <td>Border Collie-Field Spaniel Mix</td>
    </tr>
    <tr>
        <td>Border Collie-Flat-Coated Retriever Mix</td>
    </tr>
    <tr>
        <td>Border Collie-German Shepherd Dog Mix</td>
    </tr>
    <tr>
        <td>Border Collie-German Shorthaired Pointer Mix</td>
    </tr>
    <tr>
        <td>Border Collie-Golden Retriever Mix</td>
    </tr>
    <tr>
        <td>Border Collie-Great Dane Mix</td>
    </tr>
    <tr>
        <td>Border Collie-Great Pyrenees Mix</td>
    </tr>
    <tr>
        <td>Border Collie-Greyhound Mix</td>
    </tr>
    <tr>
        <td>Border Collie-Icelandic Sheepdog Mix</td>
    </tr>
    <tr>
        <td>Border Collie-Irish Setter Mix</td>
    </tr>
    <tr>
        <td>Border Collie-Irish Water Spaniel Mix</td>
    </tr>
    <tr>
        <td>Border Collie-Jack Russell Terrier Mix</td>
    </tr>
    <tr>
        <td>Border Collie-Keeshond Mix</td>
    </tr>
    <tr>
        <td>Border Collie-Kooikerhondje Mix</td>
    </tr>
    <tr>
        <td>Border Collie-Labrador Retriever Mix</td>
    </tr>
    <tr>
        <td>Border Collie-Lancashire Heeler Mix</td>
    </tr>
    <tr>
        <td>Border Collie-Miniature Australian Shepherd Mix</td>
    </tr>
    <tr>
        <td>Border Collie-Miniature Pinscher Mix</td>
    </tr>
    <tr>
        <td>Border Collie-Newfoundland Mix</td>
    </tr>
    <tr>
        <td>Border Collie-Parson Russell Terrier Mix</td>
    </tr>
    <tr>
        <td>Border Collie-Pembroke Welsh Corgi Mix</td>
    </tr>
    <tr>
        <td>Border Collie-Pointer Mix</td>
    </tr>
    <tr>
        <td>Border Collie-Pomeranian Mix</td>
    </tr>
    <tr>
        <td>Border Collie-Poodle Mix</td>
    </tr>
    <tr>
        <td>Border Collie-Rhodesian Ridgeback Mix</td>
    </tr>
    <tr>
        <td>Border Collie-Rottweiler Mix</td>
    </tr>
    <tr>
        <td>Border Collie-Saluki Mix</td>
    </tr>
    <tr>
        <td>Border Collie-Shetland Sheepdog Mix</td>
    </tr>
    <tr>
        <td>Border Collie-Siberian Husky Mix</td>
    </tr>
    <tr>
        <td>Border Collie-Soft Coated Wheaten Terrier Mix</td>
    </tr>
    <tr>
        <td>Border Collie-Staffordshire Bull Terrier Mix</td>
    </tr>
    <tr>
        <td>Border Collie-Standard Poodle Mix</td>
    </tr>
    <tr>
        <td>Border Collie-Standard Schnauzer Mix</td>
    </tr>
    <tr>
        <td>Border Collie-Welsh Springer Spaniel Mix</td>
    </tr>
    <tr>
        <td>Border Collie-West Highland White Terrier Mix</td>
    </tr>
    <tr>
        <td>Border Collie-Whippet Mix</td>
    </tr>
    <tr>
        <td>Border Collie-Wire Fox Terrier Mix</td>
    </tr>
    <tr>
        <td>Border Terrier</td>
    </tr>
    <tr>
        <td>Border Terrier- Mix</td>
    </tr>
    <tr>
        <td>Border Terrier-Beagle Mix</td>
    </tr>
    <tr>
        <td>Border Terrier-Bernese Mountain Dog Mix</td>
    </tr>
    <tr>
        <td>Border Terrier-Cairn Terrier Mix</td>
    </tr>
    <tr>
        <td>Border Terrier-Dachshund Mix</td>
    </tr>
    <tr>
        <td>Border Terrier-English Cocker Spaniel Mix</td>
    </tr>
    <tr>
        <td>Border Terrier-Golden Retriever Mix</td>
    </tr>
    <tr>
        <td>Border Terrier-Lakeland Terrier Mix</td>
    </tr>
    <tr>
        <td>Border Terrier-Parson Russell Terrier Mix</td>
    </tr>
    <tr>
        <td>Border Terrier-Poodle Mix</td>
    </tr>
    <tr>
        <td>Border Terrier-Pug Mix</td>
    </tr>
    <tr>
        <td>Border Terrier-Russell Terrier Mix</td>
    </tr>
    <tr>
        <td>Border Terrier-Shih Tzu Mix</td>
    </tr>
    <tr>
        <td>Border Terrier-Staffordshire Bull Terrier Mix</td>
    </tr>
    <tr>
        <td>Border Terrier-Standard Schnauzer Mix</td>
    </tr>
    <tr>
        <td>Border Terrier-West Highland White Terrier Mix</td>
    </tr>
    <tr>
        <td>Borzoi</td>
    </tr>
    <tr>
        <td>Borzoi-Whippet Mix</td>
    </tr>
    <tr>
        <td>Boston Terrier</td>
    </tr>
    <tr>
        <td>Boston Terrier-American Pit Bull Terrier Mix</td>
    </tr>
    <tr>
        <td>Boston Terrier-American Staffordshire Terrier Mix</td>
    </tr>
    <tr>
        <td>Boston Terrier-Beagle Mix</td>
    </tr>
    <tr>
        <td>Boston Terrier-Bulldog Mix</td>
    </tr>
    <tr>
        <td>Boston Terrier-Chihuahua Mix</td>
    </tr>
    <tr>
        <td>Boston Terrier-Dachshund Mix</td>
    </tr>
    <tr>
        <td>Boston Terrier-Danish-Swedish Farmdog Mix</td>
    </tr>
    <tr>
        <td>Boston Terrier-Doberman Pinscher Mix</td>
    </tr>
    <tr>
        <td>Boston Terrier-French Bulldog Mix</td>
    </tr>
    <tr>
        <td>Boston Terrier-Labrador Retriever Mix</td>
    </tr>
    <tr>
        <td>Boston Terrier-Pomeranian Mix</td>
    </tr>
    <tr>
        <td>Boston Terrier-Pug Mix</td>
    </tr>
    <tr>
        <td>Boston Terrier-Rat Terrier Mix</td>
    </tr>
    <tr>
        <td>Boston Terrier-Russell Terrier Mix</td>
    </tr>
    <tr>
        <td>Bouvier des Flandres</td>
    </tr>
    <tr>
        <td>Bouvier des Flandres-Airedale Terrier Mix</td>
    </tr>
    <tr>
        <td>Bouvier des Flandres-Standard Poodle Mix</td>
    </tr>
    <tr>
        <td>Boxer</td>
    </tr>
    <tr>
        <td>Boxer- Mix</td>
    </tr>
    <tr>
        <td>Boxer-Akita Mix</td>
    </tr>
    <tr>
        <td>Boxer-American Foxhound Mix</td>
    </tr>
    <tr>
        <td>Boxer-American Pit Bull Terrier Mix</td>
    </tr>
    <tr>
        <td>Boxer-American Staffordshire Terrier Mix</td>
    </tr>
    <tr>
        <td>Boxer-Australian Cattle Dog Mix</td>
    </tr>
    <tr>
        <td>Boxer-Australian Shepherd Mix</td>
    </tr>
    <tr>
        <td>Boxer-Bloodhound Mix</td>
    </tr>
    <tr>
        <td>Boxer-Border Collie Mix</td>
    </tr>
    <tr>
        <td>Boxer-Boston Terrier Mix</td>
    </tr>
    <tr>
        <td>Boxer-Bull Terrier Mix</td>
    </tr>
    <tr>
        <td>Boxer-Bulldog Mix</td>
    </tr>
    <tr>
        <td>Boxer-Bullmastiff Mix</td>
    </tr>
    <tr>
        <td>Boxer-Cocker Spaniel Mix</td>
    </tr>
    <tr>
        <td>Boxer-Dachshund Mix</td>
    </tr>
    <tr>
        <td>Boxer-Dalmatian Mix</td>
    </tr>
    <tr>
        <td>Boxer-Doberman Pinscher Mix</td>
    </tr>
    <tr>
        <td>Boxer-Dutch Shepherd Mix</td>
    </tr>
    <tr>
        <td>Boxer-English Foxhound Mix</td>
    </tr>
    <tr>
        <td>Boxer-English Springer Spaniel Mix</td>
    </tr>
    <tr>
        <td>Boxer-German Shepherd Dog Mix</td>
    </tr>
    <tr>
        <td>Boxer-Golden Retriever Mix</td>
    </tr>
    <tr>
        <td>Boxer-Great Dane Mix</td>
    </tr>
    <tr>
        <td>Boxer-Great Pyrenees Mix</td>
    </tr>
    <tr>
        <td>Boxer-Greyhound Mix</td>
    </tr>
    <tr>
        <td>Boxer-Ibizan Hound Mix</td>
    </tr>
    <tr>
        <td>Boxer-Jindo Mix</td>
    </tr>
    <tr>
        <td>Boxer-Labrador Retriever Mix</td>
    </tr>
    <tr>
        <td>Boxer-Manchester Terrier Mix</td>
    </tr>
    <tr>
        <td>Boxer-Mastiff Mix</td>
    </tr>
    <tr>
        <td>Boxer-Poodle Mix</td>
    </tr>
    <tr>
        <td>Boxer-Pug Mix</td>
    </tr>
    <tr>
        <td>Boxer-Redbone Coonhound Mix</td>
    </tr>
    <tr>
        <td>Boxer-Rhodesian Ridgeback Mix</td>
    </tr>
    <tr>
        <td>Boxer-Rottweiler Mix</td>
    </tr>
    <tr>
        <td>Boxer-Russell Terrier Mix</td>
    </tr>
    <tr>
        <td>Boxer-Siberian Husky Mix</td>
    </tr>
    <tr>
        <td>Boxer-St. Bernard Mix</td>
    </tr>
    <tr>
        <td>Boxer-Staffordshire Bull Terrier Mix</td>
    </tr>
    <tr>
        <td>Boxer-Treeing Walker Coonhound Mix</td>
    </tr>
    <tr>
        <td>Boxer-Vizsla Mix</td>
    </tr>
    <tr>
        <td>Boxer-Weimaraner Mix</td>
    </tr>
    <tr>
        <td>Boykin Spaniel</td>
    </tr>
    <tr>
        <td>Boykin Spaniel- Mix</td>
    </tr>
    <tr>
        <td>Boykin Spaniel-Deutscher Wachtelhund Mix</td>
    </tr>
    <tr>
        <td>Boykin Spaniel-English Cocker Spaniel Mix</td>
    </tr>
    <tr>
        <td>Boykin Spaniel-Irish Setter Mix</td>
    </tr>
    <tr>
        <td>Braque du Bourbonnais</td>
    </tr>
    <tr>
        <td>Briard</td>
    </tr>
    <tr>
        <td>Brittany</td>
    </tr>
    <tr>
        <td>Brittany-Basset Hound Mix</td>
    </tr>
    <tr>
        <td>Brittany-Border Collie Mix</td>
    </tr>
    <tr>
        <td>Brittany-English Springer Spaniel Mix</td>
    </tr>
    <tr>
        <td>Brittany-German Shepherd Dog Mix</td>
    </tr>
    <tr>
        <td>Brittany-German Shorthaired Pointer Mix</td>
    </tr>
    <tr>
        <td>Brittany-Golden Retriever Mix</td>
    </tr>
    <tr>
        <td>Brittany-Irish Setter Mix</td>
    </tr>
    <tr>
        <td>Brittany-Labrador Retriever Mix</td>
    </tr>
    <tr>
        <td>Brittany-Pekingese Mix</td>
    </tr>
    <tr>
        <td>Brittany-Pointer Mix</td>
    </tr>
    <tr>
        <td>Brittany-Poodle Mix</td>
    </tr>
    <tr>
        <td>Brittany-Siberian Husky Mix</td>
    </tr>
    <tr>
        <td>Brussels Griffon</td>
    </tr>
    <tr>
        <td>Brussels Griffon-Border Terrier Mix</td>
    </tr>
    <tr>
        <td>Brussels Griffon-Poodle Mix</td>
    </tr>
    <tr>
        <td>Brussels Griffon-Pug Mix</td>
    </tr>
    <tr>
        <td>Brussels Griffon-Shih Tzu Mix</td>
    </tr>
    <tr>
        <td>Bugg</td>
    </tr>
    <tr>
        <td>Bull Terrier</td>
    </tr>
    <tr>
        <td>Bull Terrier-American Foxhound Mix</td>
    </tr>
    <tr>
        <td>Bull Terrier-American Staffordshire Terrier Mix</td>
    </tr>
    <tr>
        <td>Bull Terrier-Boxer Mix</td>
    </tr>
    <tr>
        <td>Bull Terrier-Labrador Retriever Mix</td>
    </tr>
    <tr>
        <td>Bull Terrier-Staffordshire Bull Terrier Mix</td>
    </tr>
    <tr>
        <td>Bull Terrier-Whippet Mix</td>
    </tr>
    <tr>
        <td>BullBoxer</td>
    </tr>
    <tr>
        <td>Bulldog</td>
    </tr>
    <tr>
        <td>Bulldog-American Pit Bull Terrier Mix</td>
    </tr>
    <tr>
        <td>Bulldog-American Staffordshire Terrier Mix</td>
    </tr>
    <tr>
        <td>Bulldog-Basset Hound Mix</td>
    </tr>
    <tr>
        <td>Bulldog-Beagle Mix</td>
    </tr>
    <tr>
        <td>Bulldog-Boston Terrier Mix</td>
    </tr>
    <tr>
        <td>Bulldog-Boxer Mix</td>
    </tr>
    <tr>
        <td>Bulldog-Bullmastiff Mix</td>
    </tr>
    <tr>
        <td>Bulldog-Chesapeake Bay Retriever Mix</td>
    </tr>
    <tr>
        <td>Bulldog-Cocker Spaniel Mix</td>
    </tr>
    <tr>
        <td>Bulldog-Dogo Argentino Mix</td>
    </tr>
    <tr>
        <td>Bulldog-French Bulldog Mix</td>
    </tr>
    <tr>
        <td>Bulldog-German Shepherd Dog Mix</td>
    </tr>
    <tr>
        <td>Bulldog-German Shorthaired Pointer Mix</td>
    </tr>
    <tr>
        <td>Bulldog-Great Dane Mix</td>
    </tr>
    <tr>
        <td>Bulldog-Labrador Retriever Mix</td>
    </tr>
    <tr>
        <td>Bulldog-Mastiff Mix</td>
    </tr>
    <tr>
        <td>Bulldog-Neapolitan Mastiff Mix</td>
    </tr>
    <tr>
        <td>Bulldog-Pug Mix</td>
    </tr>
    <tr>
        <td>Bulldog-Rhodesian Ridgeback Mix</td>
    </tr>
    <tr>
        <td>Bulldog-Rottweiler Mix</td>
    </tr>
    <tr>
        <td>Bulldog-Wirehaired Pointing Griffon Mix</td>
    </tr>
    <tr>
        <td>Bullmastiff</td>
    </tr>
    <tr>
        <td>Bullmastiff- Mix</td>
    </tr>
    <tr>
        <td>Bullmastiff-American Pit Bull Terrier Mix</td>
    </tr>
    <tr>
        <td>Bullmastiff-American Staffordshire Terrier Mix</td>
    </tr>
    <tr>
        <td>Bullmastiff-Border Collie Mix</td>
    </tr>
    <tr>
        <td>Bullmastiff-Boxer Mix</td>
    </tr>
    <tr>
        <td>Bullmastiff-Bull Terrier Mix</td>
    </tr>
    <tr>
        <td>Bullmastiff-Bulldog Mix</td>
    </tr>
    <tr>
        <td>Bullmastiff-Chinese Shar-Pei Mix</td>
    </tr>
    <tr>
        <td>Bullmastiff-Dogue de Bordeaux Mix</td>
    </tr>
    <tr>
        <td>Bullmastiff-German Shepherd Dog Mix</td>
    </tr>
    <tr>
        <td>Bullmastiff-Labrador Retriever Mix</td>
    </tr>
    <tr>
        <td>Bullmastiff-Redbone Coonhound Mix</td>
    </tr>
    <tr>
        <td>Bullmastiff-Staffordshire Bull Terrier Mix</td>
    </tr>
    <tr>
        <td>Cairn Terrier</td>
    </tr>
    <tr>
        <td>Cairn Terrier-Airedale Terrier Mix</td>
    </tr>
    <tr>
        <td>Cairn Terrier-Australian Terrier Mix</td>
    </tr>
    <tr>
        <td>Cairn Terrier-Cocker Spaniel Mix</td>
    </tr>
    <tr>
        <td>Cairn Terrier-German Shepherd Dog Mix</td>
    </tr>
    <tr>
        <td>Cairn Terrier-Jack Russell Terrier Mix</td>
    </tr>
    <tr>
        <td>Cairn Terrier-Lhasa Apso Mix</td>
    </tr>
    <tr>
        <td>Cairn Terrier-Pomeranian Mix</td>
    </tr>
    <tr>
        <td>Cairn Terrier-Poodle Mix</td>
    </tr>
    <tr>
        <td>Cairn Terrier-Russell Terrier Mix</td>
    </tr>
    <tr>
        <td>Cairn Terrier-Shih Tzu Mix</td>
    </tr>
    <tr>
        <td>Cairn Terrier-Smooth Fox Terrier Mix</td>
    </tr>
    <tr>
        <td>Cairn Terrier-West Highland White Terrier Mix</td>
    </tr>
    <tr>
        <td>Cairn Terrier-Yorkshire Terrier Mix</td>
    </tr>
    <tr>
        <td>Canaan Dog</td>
    </tr>
    <tr>
        <td>Canaan Dog- Mix</td>
    </tr>
    <tr>
        <td>Cane Corso</td>
    </tr>
    <tr>
        <td>Cardigan Welsh Corgi</td>
    </tr>
    <tr>
        <td>Cardigan Welsh Corgi-American Eskimo Dog Mix</td>
    </tr>
    <tr>
        <td>Cardigan Welsh Corgi-American Pit Bull Terrier Mix</td>
    </tr>
    <tr>
        <td>Cardigan Welsh Corgi-American Staffordshire Terrier Mix</td>
    </tr>
    <tr>
        <td>Cardigan Welsh Corgi-Australian Cattle Dog Mix</td>
    </tr>
    <tr>
        <td>Cardigan Welsh Corgi-Australian Shepherd Mix</td>
    </tr>
    <tr>
        <td>Cardigan Welsh Corgi-Beagle Mix</td>
    </tr>
    <tr>
        <td>Cardigan Welsh Corgi-Cairn Terrier Mix</td>
    </tr>
    <tr>
        <td>Cardigan Welsh Corgi-Catahoula Leopard Dog Mix</td>
    </tr>
    <tr>
        <td>Cardigan Welsh Corgi-German Shepherd Dog Mix</td>
    </tr>
    <tr>
        <td>Cardigan Welsh Corgi-Labrador Retriever Mix</td>
    </tr>
    <tr>
        <td>Cardigan Welsh Corgi-Lancashire Heeler Mix</td>
    </tr>
    <tr>
        <td>Cardigan Welsh Corgi-Pomeranian Mix</td>
    </tr>
    <tr>
        <td>Cardigan Welsh Corgi-Russell Terrier Mix</td>
    </tr>
    <tr>
        <td>Cardigan Welsh Corgi-Yorkshire Terrier Mix</td>
    </tr>
    <tr>
        <td>Catahoula Leopard Dog</td>
    </tr>
    <tr>
        <td>Catahoula Leopard Dog- Mix</td>
    </tr>
    <tr>
        <td>Catahoula Leopard Dog-American Leopard Hound Mix</td>
    </tr>
    <tr>
        <td>Catahoula Leopard Dog-American Pit Bull Terrier Mix</td>
    </tr>
    <tr>
        <td>Catahoula Leopard Dog-Australian Cattle Dog Mix</td>
    </tr>
    <tr>
        <td>Catahoula Leopard Dog-Australian Shepherd Mix</td>
    </tr>
    <tr>
        <td>Catahoula Leopard Dog-Bluetick Coonhound Mix</td>
    </tr>
    <tr>
        <td>Catahoula Leopard Dog-Border Collie Mix</td>
    </tr>
    <tr>
        <td>Catahoula Leopard Dog-Boxer Mix</td>
    </tr>
    <tr>
        <td>Catahoula Leopard Dog-German Shepherd Dog Mix</td>
    </tr>
    <tr>
        <td>Catahoula Leopard Dog-Labrador Retriever Mix</td>
    </tr>
    <tr>
        <td>Catahoula Leopard Dog-Plott Mix</td>
    </tr>
    <tr>
        <td>Catahoula Leopard Dog-Pointer Mix</td>
    </tr>
    <tr>
        <td>Catahoula Leopard Dog-Rottweiler Mix</td>
    </tr>
    <tr>
        <td>Catahoula Leopard Dog-Shetland Sheepdog Mix</td>
    </tr>
    <tr>
        <td>Catahoula Leopard Dog-Siberian Husky Mix</td>
    </tr>
    <tr>
        <td>Caucasian Ovcharka</td>
    </tr>
    <tr>
        <td>Cavalier King Charles Spaniel</td>
    </tr>
    <tr>
        <td>Cavalier King Charles Spaniel-Australian Shepherd Mix</td>
    </tr>
    <tr>
        <td>Cavalier King Charles Spaniel-Beagle Mix</td>
    </tr>
    <tr>
        <td>Cavalier King Charles Spaniel-Bichon Frise Mix</td>
    </tr>
    <tr>
        <td>Cavalier King Charles Spaniel-Cocker Spaniel Mix</td>
    </tr>
    <tr>
        <td>Cavalier King Charles Spaniel-Coton de Tulear Mix</td>
    </tr>
    <tr>
        <td>Cavalier King Charles Spaniel-English Springer Spaniel Mix</td>
    </tr>
    <tr>
        <td>Cavalier King Charles Spaniel-Maltese Mix</td>
    </tr>
    <tr>
        <td>Cavalier King Charles Spaniel-Papillon Mix</td>
    </tr>
    <tr>
        <td>Cavalier King Charles Spaniel-Pekingese Mix</td>
    </tr>
    <tr>
        <td>Cavalier King Charles Spaniel-Pomeranian Mix</td>
    </tr>
    <tr>
        <td>Cavalier King Charles Spaniel-Poodle Mix</td>
    </tr>
    <tr>
        <td>Cavalier King Charles Spaniel-Shih Tzu Mix</td>
    </tr>
    <tr>
        <td>Cavalier King Charles Spaniel-Standard Poodle Mix</td>
    </tr>
    <tr>
        <td>Central Asian Shepherd Dog</td>
    </tr>
    <tr>
        <td>Central Asian Shepherd Dog-Golden Retriever Mix</td>
    </tr>
    <tr>
        <td>Cesky Terrier</td>
    </tr>
    <tr>
        <td>Chesapeake Bay Retriever</td>
    </tr>
    <tr>
        <td>Chesapeake Bay Retriever-American Staffordshire Terrier Mix</td>
    </tr>
    <tr>
        <td>Chesapeake Bay Retriever-Beagle Mix</td>
    </tr>
    <tr>
        <td>Chesapeake Bay Retriever-German Shepherd Dog Mix</td>
    </tr>
    <tr>
        <td>Chesapeake Bay Retriever-Labrador Retriever Mix</td>
    </tr>
    <tr>
        <td>Chesapeake Bay Retriever-Shih Tzu Mix</td>
    </tr>
    <tr>
        <td>Chesapeake Bay Retriever-Siberian Husky Mix</td>
    </tr>
    <tr>
        <td>Chihuahua</td>
    </tr>
    <tr>
        <td>Chihuahua- Mix</td>
    </tr>
    <tr>
        <td>Chihuahua-Airedale Terrier Mix</td>
    </tr>
    <tr>
        <td>Chihuahua-American Pit Bull Terrier Mix</td>
    </tr>
    <tr>
        <td>Chihuahua-American Staffordshire Terrier Mix</td>
    </tr>
    <tr>
        <td>Chihuahua-American Water Spaniel Mix</td>
    </tr>
    <tr>
        <td>Chihuahua-Australian Terrier Mix</td>
    </tr>
    <tr>
        <td>Chihuahua-Beagle Mix</td>
    </tr>
    <tr>
        <td>Chihuahua-Bichon Frise Mix</td>
    </tr>
    <tr>
        <td>Chihuahua-Boston Terrier Mix</td>
    </tr>
    <tr>
        <td>Chihuahua-Cairn Terrier Mix</td>
    </tr>
    <tr>
        <td>Chihuahua-Cardigan Welsh Corgi Mix</td>
    </tr>
    <tr>
        <td>Chihuahua-Chinese Crested Mix</td>
    </tr>
    <tr>
        <td>Chihuahua-Cocker Spaniel Mix</td>
    </tr>
    <tr>
        <td>Chihuahua-Dachshund Mix</td>
    </tr>
    <tr>
        <td>Chihuahua-Doberman Pinscher Mix</td>
    </tr>
    <tr>
        <td>Chihuahua-Golden Retriever Mix</td>
    </tr>
    <tr>
        <td>Chihuahua-Italian Greyhound Mix</td>
    </tr>
    <tr>
        <td>Chihuahua-Jack Russell Terrier Mix</td>
    </tr>
    <tr>
        <td>Chihuahua-Japanese Chin Mix</td>
    </tr>
    <tr>
        <td>Chihuahua-Labrador Retriever Mix</td>
    </tr>
    <tr>
        <td>Chihuahua-Lhasa Apso Mix</td>
    </tr>
    <tr>
        <td>Chihuahua-Maltese Mix</td>
    </tr>
    <tr>
        <td>Chihuahua-Manchester Terrier Mix</td>
    </tr>
    <tr>
        <td>Chihuahua-Miniature Australian Shepherd Mix</td>
    </tr>
    <tr>
        <td>Chihuahua-Miniature Pinscher Mix</td>
    </tr>
    <tr>
        <td>Chihuahua-Miniature Schnauzer Mix</td>
    </tr>
    <tr>
        <td>Chihuahua-Norwich Terrier Mix</td>
    </tr>
    <tr>
        <td>Chihuahua-Papillon Mix</td>
    </tr>
    <tr>
        <td>Chihuahua-Parson Russell Terrier Mix</td>
    </tr>
    <tr>
        <td>Chihuahua-Pekingese Mix</td>
    </tr>
    <tr>
        <td>Chihuahua-Pembroke Welsh Corgi Mix</td>
    </tr>
    <tr>
        <td>Chihuahua-Pomeranian Mix</td>
    </tr>
    <tr>
        <td>Chihuahua-Poodle Mix</td>
    </tr>
    <tr>
        <td>Chihuahua-Pug Mix</td>
    </tr>
    <tr>
        <td>Chihuahua-Rat Terrier Mix</td>
    </tr>
    <tr>
        <td>Chihuahua-Russell Terrier Mix</td>
    </tr>
    <tr>
        <td>Chihuahua-Schipperke Mix</td>
    </tr>
    <tr>
        <td>Chihuahua-Shiba Inu Mix</td>
    </tr>
    <tr>
        <td>Chihuahua-Shih Tzu Mix</td>
    </tr>
    <tr>
        <td>Chihuahua-Smooth Fox Terrier Mix</td>
    </tr>
    <tr>
        <td>Chihuahua-Toy Fox Terrier Mix</td>
    </tr>
    <tr>
        <td>Chihuahua-Whippet Mix</td>
    </tr>
    <tr>
        <td>Chihuahua-Wire Fox Terrier Mix</td>
    </tr>
    <tr>
        <td>Chihuahua-Xoloitzcuintli Mix</td>
    </tr>
    <tr>
        <td>Chihuahua-Yorkshire Terrier Mix</td>
    </tr>
    <tr>
        <td>Chinese Crested</td>
    </tr>
    <tr>
        <td>Chinese Crested-Chihuahua Mix</td>
    </tr>
    <tr>
        <td>Chinese Crested-Jack Russell Terrier Mix</td>
    </tr>
    <tr>
        <td>Chinese Crested-Lhasa Apso Mix</td>
    </tr>
    <tr>
        <td>Chinese Crested-Maltese Mix</td>
    </tr>
    <tr>
        <td>Chinese Crested-Papillon Mix</td>
    </tr>
    <tr>
        <td>Chinese Crested-Poodle Mix</td>
    </tr>
    <tr>
        <td>Chinese Crested-Pug Mix</td>
    </tr>
    <tr>
        <td>Chinese Crested-Russell Terrier Mix</td>
    </tr>
    <tr>
        <td>Chinese Shar-Pei</td>
    </tr>
    <tr>
        <td>Chinese Shar-Pei- Mix</td>
    </tr>
    <tr>
        <td>Chinese Shar-Pei-American Pit Bull Terrier Mix</td>
    </tr>
    <tr>
        <td>Chinese Shar-Pei-Australian Cattle Dog Mix</td>
    </tr>
    <tr>
        <td>Chinese Shar-Pei-Basset Hound Mix</td>
    </tr>
    <tr>
        <td>Chinese Shar-Pei-Boerboel Mix</td>
    </tr>
    <tr>
        <td>Chinese Shar-Pei-Cocker Spaniel Mix</td>
    </tr>
    <tr>
        <td>Chinese Shar-Pei-German Shepherd Dog Mix</td>
    </tr>
    <tr>
        <td>Chinese Shar-Pei-Great Dane Mix</td>
    </tr>
    <tr>
        <td>Chinese Shar-Pei-Labrador Retriever Mix</td>
    </tr>
    <tr>
        <td>Chinese Shar-Pei-Pug Mix</td>
    </tr>
    <tr>
        <td>Chinese Shar-Pei-Staffordshire Bull Terrier Mix</td>
    </tr>
    <tr>
        <td>Chinook</td>
    </tr>
    <tr>
        <td>Chinook-German Shepherd Dog Mix</td>
    </tr>
    <tr>
        <td>Chow Chow</td>
    </tr>
    <tr>
        <td>Chow Chow-American Pit Bull Terrier Mix</td>
    </tr>
    <tr>
        <td>Chow Chow-Australian Shepherd Mix</td>
    </tr>
    <tr>
        <td>Chow Chow-Catahoula Leopard Dog Mix</td>
    </tr>
    <tr>
        <td>Chow Chow-Cocker Spaniel Mix</td>
    </tr>
    <tr>
        <td>Chow Chow-Collie Mix</td>
    </tr>
    <tr>
        <td>Chow Chow-German Shepherd Dog Mix</td>
    </tr>
    <tr>
        <td>Chow Chow-Golden Retriever Mix</td>
    </tr>
    <tr>
        <td>Chow Chow-Keeshond Mix</td>
    </tr>
    <tr>
        <td>Chow Chow-Labrador Retriever Mix</td>
    </tr>
    <tr>
        <td>Chow Chow-Newfoundland Mix</td>
    </tr>
    <tr>
        <td>Chow Chow-Norfolk Terrier Mix</td>
    </tr>
    <tr>
        <td>Chow Chow-Pembroke Welsh Corgi Mix</td>
    </tr>
    <tr>
        <td>Chow Chow-Poodle Mix</td>
    </tr>
    <tr>
        <td>Chow Chow-Rottweiler Mix</td>
    </tr>
    <tr>
        <td>Cirneco dell&quot;Etna</td>
    </tr>
    <tr>
        <td>Clumber Spaniel</td>
    </tr>
    <tr>
        <td>Clumber Spaniel-English Springer Spaniel Mix</td>
    </tr>
    <tr>
        <td>Cockapoo</td>
    </tr>
    <tr>
        <td>Cocker Spaniel</td>
    </tr>
    <tr>
        <td>Cocker Spaniel-Bichon Frise Mix</td>
    </tr>
    <tr>
        <td>Cocker Spaniel-Boxer Mix</td>
    </tr>
    <tr>
        <td>Cocker Spaniel-Cavalier King Charles Spaniel Mix</td>
    </tr>
    <tr>
        <td>Cocker Spaniel-Chow Chow Mix</td>
    </tr>
    <tr>
        <td>Cocker Spaniel-Dachshund Mix</td>
    </tr>
    <tr>
        <td>Cocker Spaniel-Doberman Pinscher Mix</td>
    </tr>
    <tr>
        <td>Cocker Spaniel-English Cocker Spaniel Mix</td>
    </tr>
    <tr>
        <td>Cocker Spaniel-English Springer Spaniel Mix</td>
    </tr>
    <tr>
        <td>Cocker Spaniel-Golden Retriever Mix</td>
    </tr>
    <tr>
        <td>Cocker Spaniel-Labrador Retriever Mix</td>
    </tr>
    <tr>
        <td>Cocker Spaniel-Maltese Mix</td>
    </tr>
    <tr>
        <td>Cocker Spaniel-Miniature Pinscher Mix</td>
    </tr>
    <tr>
        <td>Cocker Spaniel-Miniature Schnauzer Mix</td>
    </tr>
    <tr>
        <td>Cocker Spaniel-Pekingese Mix</td>
    </tr>
    <tr>
        <td>Cocker Spaniel-Pomeranian Mix</td>
    </tr>
    <tr>
        <td>Cocker Spaniel-Poodle Mix</td>
    </tr>
    <tr>
        <td>Cocker Spaniel-Pug Mix</td>
    </tr>
    <tr>
        <td>Cocker Spaniel-Rat Terrier Mix</td>
    </tr>
    <tr>
        <td>Cocker Spaniel-Shetland Sheepdog Mix</td>
    </tr>
    <tr>
        <td>Cocker Spaniel-Shih Tzu Mix</td>
    </tr>
    <tr>
        <td>Cocker Spaniel-Standard Schnauzer Mix</td>
    </tr>
    <tr>
        <td>Cocker Spaniel-Tibetan Spaniel Mix</td>
    </tr>
    <tr>
        <td>Collie</td>
    </tr>
    <tr>
        <td>Collie- Mix</td>
    </tr>
    <tr>
        <td>Collie-American Foxhound Mix</td>
    </tr>
    <tr>
        <td>Collie-Anatolian Shepherd Dog Mix</td>
    </tr>
    <tr>
        <td>Collie-Australian Cattle Dog Mix</td>
    </tr>
    <tr>
        <td>Collie-Belgian Malinois Mix</td>
    </tr>
    <tr>
        <td>Collie-Boxer Mix</td>
    </tr>
    <tr>
        <td>Collie-German Shepherd Dog Mix</td>
    </tr>
    <tr>
        <td>Collie-German Shorthaired Pointer Mix</td>
    </tr>
    <tr>
        <td>Collie-Golden Retriever Mix</td>
    </tr>
    <tr>
        <td>Collie-Labrador Retriever Mix</td>
    </tr>
    <tr>
        <td>Collie-Shetland Sheepdog Mix</td>
    </tr>
    <tr>
        <td>Coton de Tulear</td>
    </tr>
    <tr>
        <td>Coton de Tulear-Chow Chow Mix</td>
    </tr>
    <tr>
        <td>Coton de Tulear-Poodle Mix</td>
    </tr>
    <tr>
        <td>Curly-Coated Retriever</td>
    </tr>
    <tr>
        <td>Czechoslovakian Vlcak</td>
    </tr>
    <tr>
        <td>Dachshund</td>
    </tr>
    <tr>
        <td>Dachshund- Mix</td>
    </tr>
    <tr>
        <td>Dachshund-American Pit Bull Terrier Mix</td>
    </tr>
    <tr>
        <td>Dachshund-American Staffordshire Terrier Mix</td>
    </tr>
    <tr>
        <td>Dachshund-Basset Hound Mix</td>
    </tr>
    <tr>
        <td>Dachshund-Beagle Mix</td>
    </tr>
    <tr>
        <td>Dachshund-Bichon Frise Mix</td>
    </tr>
    <tr>
        <td>Dachshund-Black Russian Terrier Mix</td>
    </tr>
    <tr>
        <td>Dachshund-Boston Terrier Mix</td>
    </tr>
    <tr>
        <td>Dachshund-Cardigan Welsh Corgi Mix</td>
    </tr>
    <tr>
        <td>Dachshund-Chihuahua Mix</td>
    </tr>
    <tr>
        <td>Dachshund-Cocker Spaniel Mix</td>
    </tr>
    <tr>
        <td>Dachshund-Doberman Pinscher Mix</td>
    </tr>
    <tr>
        <td>Dachshund-Flat-Coated Retriever Mix</td>
    </tr>
    <tr>
        <td>Dachshund-French Bulldog Mix</td>
    </tr>
    <tr>
        <td>Dachshund-German Pinscher Mix</td>
    </tr>
    <tr>
        <td>Dachshund-German Shepherd Dog Mix</td>
    </tr>
    <tr>
        <td>Dachshund-German Shorthaired Pointer Mix</td>
    </tr>
    <tr>
        <td>Dachshund-Glen of Imaal Terrier Mix</td>
    </tr>
    <tr>
        <td>Dachshund-Golden Retriever Mix</td>
    </tr>
    <tr>
        <td>Dachshund-Italian Greyhound Mix</td>
    </tr>
    <tr>
        <td>Dachshund-Jack Russell Terrier Mix</td>
    </tr>
    <tr>
        <td>Dachshund-Labrador Retriever Mix</td>
    </tr>
    <tr>
        <td>Dachshund-Maltese Mix</td>
    </tr>
    <tr>
        <td>Dachshund-Miniature Pinscher Mix</td>
    </tr>
    <tr>
        <td>Dachshund-Miniature Schnauzer Mix</td>
    </tr>
    <tr>
        <td>Dachshund-Parson Russell Terrier Mix</td>
    </tr>
    <tr>
        <td>Dachshund-Pembroke Welsh Corgi Mix</td>
    </tr>
    <tr>
        <td>Dachshund-Pomeranian Mix</td>
    </tr>
    <tr>
        <td>Dachshund-Poodle Mix</td>
    </tr>
    <tr>
        <td>Dachshund-Pug Mix</td>
    </tr>
    <tr>
        <td>Dachshund-Rat Terrier Mix</td>
    </tr>
    <tr>
        <td>Dachshund-Russell Terrier Mix</td>
    </tr>
    <tr>
        <td>Dachshund-Scottish Terrier Mix</td>
    </tr>
    <tr>
        <td>Dachshund-Shih Tzu Mix</td>
    </tr>
    <tr>
        <td>Dachshund-Staffordshire Bull Terrier Mix</td>
    </tr>
    <tr>
        <td>Dachshund-Wire Fox Terrier Mix</td>
    </tr>
    <tr>
        <td>Dachshund-Yorkshire Terrier Mix</td>
    </tr>
    <tr>
        <td>Dalmatian</td>
    </tr>
    <tr>
        <td>Dalmatian-American Pit Bull Terrier Mix</td>
    </tr>
    <tr>
        <td>Dalmatian-Australian Cattle Dog Mix</td>
    </tr>
    <tr>
        <td>Dalmatian-Australian Shepherd Mix</td>
    </tr>
    <tr>
        <td>Dalmatian-Beagle Mix</td>
    </tr>
    <tr>
        <td>Dalmatian-Border Collie Mix</td>
    </tr>
    <tr>
        <td>Dalmatian-Great Pyrenees Mix</td>
    </tr>
    <tr>
        <td>Dalmatian-Labrador Retriever Mix</td>
    </tr>
    <tr>
        <td>Dandie Dinmont Terrier</td>
    </tr>
    <tr>
        <td>Dandie Dinmont Terrier-Yorkshire Terrier Mix</td>
    </tr>
    <tr>
        <td>Danish-Swedish Farmdog</td>
    </tr>
    <tr>
        <td>Danish-Swedish Farmdog-Czechoslovakian Vlcak Mix</td>
    </tr>
    <tr>
        <td>Deutscher Wachtelhund</td>
    </tr>
    <tr>
        <td>Deutscher Wachtelhund- Mix</td>
    </tr>
    <tr>
        <td>Deutscher Wachtelhund-Newfoundland Mix</td>
    </tr>
    <tr>
        <td>Doberman Pinscher</td>
    </tr>
    <tr>
        <td>Doberman Pinscher-American Staffordshire Terrier Mix</td>
    </tr>
    <tr>
        <td>Doberman Pinscher-Beagle Mix</td>
    </tr>
    <tr>
        <td>Doberman Pinscher-Beauceron Mix</td>
    </tr>
    <tr>
        <td>Doberman Pinscher-Black and Tan Coonhound Mix</td>
    </tr>
    <tr>
        <td>Doberman Pinscher-Boxer Mix</td>
    </tr>
    <tr>
        <td>Doberman Pinscher-Catahoula Leopard Dog Mix</td>
    </tr>
    <tr>
        <td>Doberman Pinscher-Dalmatian Mix</td>
    </tr>
    <tr>
        <td>Doberman Pinscher-German Shepherd Dog Mix</td>
    </tr>
    <tr>
        <td>Doberman Pinscher-Greyhound Mix</td>
    </tr>
    <tr>
        <td>Doberman Pinscher-Labrador Retriever Mix</td>
    </tr>
    <tr>
        <td>Doberman Pinscher-Poodle Mix</td>
    </tr>
    <tr>
        <td>Doberman Pinscher-Pug Mix</td>
    </tr>
    <tr>
        <td>Doberman Pinscher-Rottweiler Mix</td>
    </tr>
    <tr>
        <td>Dogo Argentino</td>
    </tr>
    <tr>
        <td>Dogo Argentino-American Pit Bull Terrier Mix</td>
    </tr>
    <tr>
        <td>Dogue de Bordeaux</td>
    </tr>
    <tr>
        <td>Dogue de Bordeaux-American Pit Bull Terrier Mix</td>
    </tr>
    <tr>
        <td>Dogue de Bordeaux-Boxer Mix</td>
    </tr>
    <tr>
        <td>Dogue de Bordeaux-Labrador Retriever Mix</td>
    </tr>
    <tr>
        <td>Drentsche Patrijshond</td>
    </tr>
    <tr>
        <td>Dutch Shepherd</td>
    </tr>
    <tr>
        <td>Dutch Shepherd-Appenzeller Sennenhunde Mix</td>
    </tr>
    <tr>
        <td>Dutch Shepherd-Labrador Retriever Mix</td>
    </tr>
    <tr>
        <td>Dutch Shepherd-Siberian Husky Mix</td>
    </tr>
    <tr>
        <td>English Cocker Spaniel</td>
    </tr>
    <tr>
        <td>English Cocker Spaniel-Cocker Spaniel Mix</td>
    </tr>
    <tr>
        <td>English Cocker Spaniel-Poodle Mix</td>
    </tr>
    <tr>
        <td>English Foxhound</td>
    </tr>
    <tr>
        <td>English Foxhound-Labrador Retriever Mix</td>
    </tr>
    <tr>
        <td>English Setter</td>
    </tr>
    <tr>
        <td>English Setter- Mix</td>
    </tr>
    <tr>
        <td>English Setter-Beagle Mix</td>
    </tr>
    <tr>
        <td>English Setter-Border Collie Mix</td>
    </tr>
    <tr>
        <td>English Setter-Brittany Mix</td>
    </tr>
    <tr>
        <td>English Setter-Dalmatian Mix</td>
    </tr>
    <tr>
        <td>English Setter-English Springer Spaniel Mix</td>
    </tr>
    <tr>
        <td>English Setter-Labrador Retriever Mix</td>
    </tr>
    <tr>
        <td>English Setter-Pointer Mix</td>
    </tr>
    <tr>
        <td>English Springer Spaniel</td>
    </tr>
    <tr>
        <td>English Springer Spaniel- Mix</td>
    </tr>
    <tr>
        <td>English Springer Spaniel-Australian Cattle Dog Mix</td>
    </tr>
    <tr>
        <td>English Springer Spaniel-Border Collie Mix</td>
    </tr>
    <tr>
        <td>English Springer Spaniel-Cocker Spaniel Mix</td>
    </tr>
    <tr>
        <td>English Springer Spaniel-Dalmatian Mix</td>
    </tr>
    <tr>
        <td>English Springer Spaniel-English Cocker Spaniel Mix</td>
    </tr>
    <tr>
        <td>English Springer Spaniel-German Shepherd Dog Mix</td>
    </tr>
    <tr>
        <td>English Springer Spaniel-Jack Russell Terrier Mix</td>
    </tr>
    <tr>
        <td>English Springer Spaniel-Labrador Retriever Mix</td>
    </tr>
    <tr>
        <td>English Springer Spaniel-Pointer Mix</td>
    </tr>
    <tr>
        <td>English Springer Spaniel-Poodle Mix</td>
    </tr>
    <tr>
        <td>English Springer Spaniel-Standard Poodle Mix</td>
    </tr>
    <tr>
        <td>English Toy Spaniel</td>
    </tr>
    <tr>
        <td>Entlebucher Mountain Dog</td>
    </tr>
    <tr>
        <td>Estrela Mountain Dog</td>
    </tr>
    <tr>
        <td>Eurasier</td>
    </tr>
    <tr>
        <td>Field Spaniel</td>
    </tr>
    <tr>
        <td>Finnish Lapphund</td>
    </tr>
    <tr>
        <td>Finnish Spitz</td>
    </tr>
    <tr>
        <td>Flat-Coated Retriever</td>
    </tr>
    <tr>
        <td>Flat-Coated Retriever- Mix</td>
    </tr>
    <tr>
        <td>Flat-Coated Retriever-Border Collie Mix</td>
    </tr>
    <tr>
        <td>Flat-Coated Retriever-Irish Water Spaniel Mix</td>
    </tr>
    <tr>
        <td>Flat-Coated Retriever-Labrador Retriever Mix</td>
    </tr>
    <tr>
        <td>Flat-Coated Retriever-Portuguese Water Dog Mix</td>
    </tr>
    <tr>
        <td>French Bulldog</td>
    </tr>
    <tr>
        <td>French Bulldog-Boston Terrier Mix</td>
    </tr>
    <tr>
        <td>French Bulldog-Bulldog Mix</td>
    </tr>
    <tr>
        <td>French Bulldog-Havanese Mix</td>
    </tr>
    <tr>
        <td>French Bulldog-Pug Mix</td>
    </tr>
</table>
<span style="font-style:italic;text-align:center;">2006 rows, truncated to displaylimit of 1000</span>



(You might notice that some of the breeds start with a hyphen; we'll come back to that later.)

The default is to sort the output in ascending order.  However, you can tell SQL to sort the output in descending order as well:

```mySQL
SELECT DISTINCT breed
FROM dogs 
ORDER BY breed DESC
```

Combining ORDER BY with LIMIT gives you an easy way to select the "top 10" and "last 10" in a list or column.  For example, you could select the User IDs and Dog IDs of the 5 customer-dog pairs who spent the least median amount of time between their Dognition tests:

```mySQL
SELECT DISTINCT user_guid, median_ITI_minutes
FROM dogs 
ORDER BY median_ITI_minutes
LIMIT 5
```
or the greatest median amount of time between their Dognition tests:

```mySQL
SELECT DISTINCT user_guid, median_ITI_minutes
FROM dogs 
ORDER BY median_ITI_minutes DESC
LIMIT 5
```

You can also sort your output based on a derived field.  If you wanted your inter-test interval to be expressed in seconds instead of minutes, you could incorporate a derived column and an alias into your last query to get the 5 customer-dog pairs who spent the greatest median amount of time between their Dognition tests in seconds:

```mySQL
SELECT DISTINCT user_guid, (median_ITI_minutes * 60) AS median_ITI_sec
FROM dogs 
ORDER BY median_ITI_sec DESC
LIMIT 5
```

Note that the parentheses are important in that query; without them, the database would try to make an alias for 60 instead of median_ITI_minutes * 60.

SQL queries also allow you to sort by multiple fields in a specified order, similar to how Excel allows to include multiple levels in a sort (see image below):

<img src="https://duke.box.com/shared/static/lbubaw9rkqoyv5xd61y57o3lpqkvrj10.jpg" width=600 alt="SELECT FROM WHERE" />

To achieve this in SQL, you include all the fields (or aliases) by which you want to sort the results after the ORDER BY clause, separated by commas, in the order you want them to be used for sorting.  You can then specify after each field whether you want the sort using that field to be ascending or descending.

If you wanted to select all the distinct User IDs of customers in the United States (abbreviated "US") and sort them according to the states they live in in alphabetical order first, and membership type second, you could query:

```mySQL
SELECT DISTINCT user_guid, state, membership_type
FROM users
WHERE country="US"
ORDER BY state ASC, membership_type ASC
```

**Go ahead and try it yourself (if you do not limit the output, you should get 9356 rows in your output):**


```python
%%sql
SELECT DISTINCT user_guid, state, membership_type
FROM users
WHERE country="US"
ORDER BY state ASC, membership_type ASC LIMIT 10
```

    10 rows affected.





<table>
    <tr>
        <th>user_guid</th>
        <th>state</th>
        <th>membership_type</th>
    </tr>
    <tr>
        <td>ce138312-7144-11e5-ba71-058fbc01cf0b</td>
        <td>AE</td>
        <td>1</td>
    </tr>
    <tr>
        <td>ce7587ba-7144-11e5-ba71-058fbc01cf0b</td>
        <td>AE</td>
        <td>1</td>
    </tr>
    <tr>
        <td>ce76f528-7144-11e5-ba71-058fbc01cf0b</td>
        <td>AE</td>
        <td>1</td>
    </tr>
    <tr>
        <td>ce221dbe-7144-11e5-ba71-058fbc01cf0b</td>
        <td>AE</td>
        <td>2</td>
    </tr>
    <tr>
        <td>ce70836e-7144-11e5-ba71-058fbc01cf0b</td>
        <td>AE</td>
        <td>2</td>
    </tr>
    <tr>
        <td>ce969298-7144-11e5-ba71-058fbc01cf0b</td>
        <td>AE</td>
        <td>3</td>
    </tr>
    <tr>
        <td>ce26e01a-7144-11e5-ba71-058fbc01cf0b</td>
        <td>AK</td>
        <td>1</td>
    </tr>
    <tr>
        <td>ce351572-7144-11e5-ba71-058fbc01cf0b</td>
        <td>AK</td>
        <td>1</td>
    </tr>
    <tr>
        <td>ce3c3b36-7144-11e5-ba71-058fbc01cf0b</td>
        <td>AK</td>
        <td>1</td>
    </tr>
    <tr>
        <td>ce4170ec-7144-11e5-ba71-058fbc01cf0b</td>
        <td>AK</td>
        <td>1</td>
    </tr>
</table>



You might notice that some of the rows have null values in the state field.  You could revise your query to only select rows that do not have null values in either the state or membership_type column:

```mySQL
SELECT DISTINCT user_guid, state, membership_type
FROM users
WHERE country="US" AND state IS NOT NULL and membership_type IS NOT NULL
ORDER BY state ASC, membership_type ASC
```

**Question 3: Below, try executing a query that would sort the same output as described above by membership_type first in descending order, and state second in ascending order:**


```python
%%sql
SELECT DISTINCT user_guid, state, membership_type
FROM users
WHERE country="US" AND state IS NOT NULL and membership_type IS NOT NULL
ORDER BY membership_type ASC, state DESC LIMIT 10
```

    10 rows affected.





<table>
    <tr>
        <th>user_guid</th>
        <th>state</th>
        <th>membership_type</th>
    </tr>
    <tr>
        <td>ce13750c-7144-11e5-ba71-058fbc01cf0b</td>
        <td>WY</td>
        <td>1</td>
    </tr>
    <tr>
        <td>ce287aec-7144-11e5-ba71-058fbc01cf0b</td>
        <td>WY</td>
        <td>1</td>
    </tr>
    <tr>
        <td>ce6dc6c4-7144-11e5-ba71-058fbc01cf0b</td>
        <td>WY</td>
        <td>1</td>
    </tr>
    <tr>
        <td>ce74375c-7144-11e5-ba71-058fbc01cf0b</td>
        <td>WY</td>
        <td>1</td>
    </tr>
    <tr>
        <td>ce74a85e-7144-11e5-ba71-058fbc01cf0b</td>
        <td>WY</td>
        <td>1</td>
    </tr>
    <tr>
        <td>ce7d4d4c-7144-11e5-ba71-058fbc01cf0b</td>
        <td>WY</td>
        <td>1</td>
    </tr>
    <tr>
        <td>ce7ef872-7144-11e5-ba71-058fbc01cf0b</td>
        <td>WY</td>
        <td>1</td>
    </tr>
    <tr>
        <td>ce7fd49a-7144-11e5-ba71-058fbc01cf0b</td>
        <td>WY</td>
        <td>1</td>
    </tr>
    <tr>
        <td>ce8377d0-7144-11e5-ba71-058fbc01cf0b</td>
        <td>WY</td>
        <td>1</td>
    </tr>
    <tr>
        <td>ce98d6de-7144-11e5-ba71-058fbc01cf0b</td>
        <td>WY</td>
        <td>1</td>
    </tr>
</table>



## 4. Export your query results to a text file 

Next week, we will learn how to complete some basic forms of data analysis in SQL.  However, if you know how to use other analysis or visualization software like Excel or Tableau, you can implement these analyses with the SQL skills you have gained already, as long as you can export the results of your SQL queries in a format other software packages can read.  Almost every database interface has a different method for exporting query results, so you will need to look up how to do it every time you try a new interface (another place where having a desire to learn new things will come in handy!). 

There are two ways to export your query results using our Jupyter interface.  

1.  You can select and copy the output you see in an output window, and paste it into another program.  Although this strategy is very simple, it only works if your output is very limited in size (since you can only paste 1000 rows at a time).

2.  You can tell MySQL to put the results of a query into a variable (for our purposes consider a variable to be a temporary holding place), and then use Python code to format the data in the variable as a CSV file (comma separated value file, a .CSV file) that can be downloaded.  When you use this strategy, all of the results of a query will be saved into the variable, not just the first 1000 rows as displayed in Jupyter, even if we have set up Jupyter to only display 1000 rows of the output.  

Let's see how we could export query results using the second method.

To tell MySQL to put the results of a query into a variable, use the following syntax:

>```python
variable_name_of_your_choice = %sql [your full query goes here];
```

In this case, you must execute your SQL query all on one line.  So if you wanted to export the list of dog breeds in the dogs table, you could begin by executing:

>```python
breed_list = %sql SELECT DISTINCT breed FROM dogs ORDER BY breed;
```

**Go ahead and try it:**



```python
breed_list = %sql SELECT DISTINCT breed FROM dogs ORDER BY breed;
```

    2006 rows affected.


Once your variable is created, using the above command tell Jupyter to format the variable as a csv file using the following syntax:

>```python
the_output_name_you_want.csv('the_output_name_you_want.csv')
```

Since this line is being run in Python, do NOT include the %sql prefix when trying to execute the line.  We could therefore export the breed list by executing:

>```python
breed_list.csv('breed_list.csv')
```

When you do this, all of the results of the query will be saved in the text file but the results will not be displayed in your notebook.  This is a convenient way to retrieve large amounts of data from a query without taxing your browser or the server.  
     
**Try it yourself:**


```python
breed_list.csv('breed_list.csv')
```




<a href="./files/breed_list.csv">CSV results</a>



You should see a link in the output line that says "CSV results." You can click on this link to see the text file in a tab in your browser or to download the file to your computer (exactly how this works will differ depending on your browser and settings, but your options will be the same as if you were trying to open or download a file from any other website.) 

You can also open the file directly from the home page of your Jupyter account.  Behind the scenes, your csv file was written to your directory on the Jupyter server, so you should now see this file listed in your Jupyter account landing page along with the list of your notebooks.  Just like a notebook, you can copy it, rename it, or delete it from your directory by clicking on the check box next to the file and clicking the "duplicate," "rename," or trash can buttons at the top of the page.

<img src="https://duke.box.com/shared/static/0k33vrxct1k03iz5u0cunfzf81vyn3ns.jpg" width=400 alt="JUPYTER SCREEN SHOT" />


## 5.  A Bird's Eye View of Other Functions You Might Want to Explore

When you open your breed list results file, you will notice the following:

1) All of the rows of the output are included, even though you can only see 1000 of those rows when you run the query through the Jupyter interface.

2) There are some strange values in the breed list.  Some of the entries in the breed column seem to have a dash included before the name.  This is an example of what real business data sets look like...they are messy!  We will use this as an opportunity to highlight why it is so important to be curious and explore MySQL functions on your own. 

If you needed an accurate list of all the dog breeds in the dogs table, you would have to find some way to "clean up" the breed list you just made.  Let's examine some of the functions that could help you achieve this cleaning using SQL syntax rather than another program or language outside of the database.

I included these links to MySQL functions in an earlier notebook:  
http://dev.mysql.com/doc/refman/5.7/en/func-op-summary-ref.html  
http://www.w3resource.com/mysql/mysql-functions-and-operators.php

The following description of a function called REPLACE is included in that resource:

"REPLACE(str,from_str,to_str)  
Returns the string str with all occurrences of the string from_str replaced by the string to_str. REPLACE() performs a case-sensitive match when searching for from_str."

One thing we could try is using this function to replace any dashes included in the breed names with no character:

```mySQL
SELECT DISTINCT breed,
REPLACE(breed,'-','') AS breed_fixed
FROM dogs
ORDER BY breed_fixed
```

In this query, we put the field/column name in the replace function where the syntax instructions listed "str" in order to tell the REPLACE function to act on the entire column.  The "-" was the "from_str", which is the string we wanted to replace.  The "" was the to_str, which is the character with which we want to replace the "from_str".  

**Try looking at the output:**



```python
%%sql
SELECT DISTINCT breed,
REPLACE(breed,'-','') AS breed_fixed
FROM dogs
ORDER BY breed_fixed LIMIT 10
```

    10 rows affected.





<table>
    <tr>
        <th>breed</th>
        <th>breed_fixed</th>
    </tr>
    <tr>
        <td>Affenpinscher</td>
        <td>Affenpinscher</td>
    </tr>
    <tr>
        <td>Affenpinscher-Afghan Hound Mix</td>
        <td>AffenpinscherAfghan Hound Mix</td>
    </tr>
    <tr>
        <td>Affenpinscher-Airedale Terrier Mix</td>
        <td>AffenpinscherAiredale Terrier Mix</td>
    </tr>
    <tr>
        <td>Affenpinscher-Alaskan Malamute Mix</td>
        <td>AffenpinscherAlaskan Malamute Mix</td>
    </tr>
    <tr>
        <td>Affenpinscher-American English Coonhound Mix</td>
        <td>AffenpinscherAmerican English Coonhound Mix</td>
    </tr>
    <tr>
        <td>Affenpinscher-Brussels Griffon Mix</td>
        <td>AffenpinscherBrussels Griffon Mix</td>
    </tr>
    <tr>
        <td>Affenpinscher-Poodle Mix</td>
        <td>AffenpinscherPoodle Mix</td>
    </tr>
    <tr>
        <td>Affenpinscher-Rat Terrier Mix</td>
        <td>AffenpinscherRat Terrier Mix</td>
    </tr>
    <tr>
        <td>Affenpinscher-Spanish Water Dog Mix</td>
        <td>AffenpinscherSpanish Water Dog Mix</td>
    </tr>
    <tr>
        <td>Afghan Hound</td>
        <td>Afghan Hound</td>
    </tr>
</table>



That was helpful, but you'll still notice some issues with the output.

First, the leading dashes are indeed removed in the breed_fixed column, but now the dashes used to separate breeds in entries like 'French Bulldog-Boston Terrier Mix' are missing as well. So REPLACE isn't the right choice to selectively remove leading dashes.

Perhaps we could try using the TRIM function:

http://www.w3resource.com/mysql/string-functions/mysql-trim-function.php

   
```sql
SELECT DISTINCT breed, TRIM(LEADING '-' FROM breed) AS breed_fixed
FROM dogs
ORDER BY breed_fixed
```
  
**Try the query written above yourself, and inspect the output carefully:**


```python
%%sql
SELECT DISTINCT breed, TRIM(LEADING '-' FROM breed) AS breed_fixed
FROM dogs
ORDER BY breed_fixed LIMIT 10
```

    10 rows affected.





<table>
    <tr>
        <th>breed</th>
        <th>breed_fixed</th>
    </tr>
    <tr>
        <td>Affenpinscher</td>
        <td>Affenpinscher</td>
    </tr>
    <tr>
        <td>Affenpinscher-Afghan Hound Mix</td>
        <td>Affenpinscher-Afghan Hound Mix</td>
    </tr>
    <tr>
        <td>Affenpinscher-Airedale Terrier Mix</td>
        <td>Affenpinscher-Airedale Terrier Mix</td>
    </tr>
    <tr>
        <td>Affenpinscher-Alaskan Malamute Mix</td>
        <td>Affenpinscher-Alaskan Malamute Mix</td>
    </tr>
    <tr>
        <td>Affenpinscher-American English Coonhound Mix</td>
        <td>Affenpinscher-American English Coonhound Mix</td>
    </tr>
    <tr>
        <td>Affenpinscher-Brussels Griffon Mix</td>
        <td>Affenpinscher-Brussels Griffon Mix</td>
    </tr>
    <tr>
        <td>Affenpinscher-Poodle Mix</td>
        <td>Affenpinscher-Poodle Mix</td>
    </tr>
    <tr>
        <td>Affenpinscher-Rat Terrier Mix</td>
        <td>Affenpinscher-Rat Terrier Mix</td>
    </tr>
    <tr>
        <td>Affenpinscher-Spanish Water Dog Mix</td>
        <td>Affenpinscher-Spanish Water Dog Mix</td>
    </tr>
    <tr>
        <td>Afghan Hound</td>
        <td>Afghan Hound</td>
    </tr>
</table>



That certainly gets us a lot closer to the list we might want, but there are still some entries in the breed_fixed column that are conceptual duplicates of each other, due to poor consistency in how the breed names were entered.  For example, one entry is "Beagle Mix" while another is "Beagle- Mix".  These entries are clearly meant to refer to the same breed, but they will be counted as separate breeds as long as their breed names are different.

Cleaning up all of the entries in the breed column would take quite a bit of work, so we won't go through more details about how to do it in this lesson.  Instead, use this exercise as a reminder for why it's so important to always look at the details of your data, and as motivation to explore the MySQL functions we won't have time to discuss in the course.  If you push yourself to learn new SQL functions and embrace the habit of getting to know your data by exploring its raw values and outputs, you will find that SQL provides very efficient tools to clean real-world messy data sets, and you will arrive at the correct conclusions about what your data indicate your company should do.

## Now it's time to practice using AS, DISTINCT, and ORDER BY in your own queries.


**Question 4: How would you get a list of all the subcategories of Dognition tests, in alphabetical order, with no test listed more than once (if you do not limit your output, you should retrieve 16 rows)?**


```python
%%sql
SELECT DISTINCT subcategory_name
FROM complete_tests
ORDER BY subcategory_name ASC
```

    16 rows affected.





<table>
    <tr>
        <th>subcategory_name</th>
    </tr>
    <tr>
        <td>Communication</td>
    </tr>
    <tr>
        <td>Cunning</td>
    </tr>
    <tr>
        <td>Empathy</td>
    </tr>
    <tr>
        <td>Expression Game</td>
    </tr>
    <tr>
        <td>Impossible Task</td>
    </tr>
    <tr>
        <td>Laterality</td>
    </tr>
    <tr>
        <td>Memory</td>
    </tr>
    <tr>
        <td>Numerosity</td>
    </tr>
    <tr>
        <td>Perspective Game</td>
    </tr>
    <tr>
        <td>Reasoning</td>
    </tr>
    <tr>
        <td>Self Control Game</td>
    </tr>
    <tr>
        <td>Shaker Game</td>
    </tr>
    <tr>
        <td>Shell Game</td>
    </tr>
    <tr>
        <td>Smell Game</td>
    </tr>
    <tr>
        <td>Social Bias</td>
    </tr>
    <tr>
        <td>Spatial Navigation</td>
    </tr>
</table>



**Question 5: How would you create a text file with a list of all the non-United States countries of Dognition customers with no country listed more than once?**


```python
not_us = %sql SELECT DISTINCT country FROM users WHERE country!="US"
not_us.csv("not_us.csv")
```

    68 rows affected.





<a href="./files/not_us.csv">CSV results</a>



**Question 6: How would you find the User ID, Dog ID, and test name of the first 10 tests to ever be completed in the Dognition database?**


```python
%sql SELECT user_guid, dog_guid, test_name FROM complete_tests ORDER BY created_at LIMIT 10
```

    10 rows affected.





<table>
    <tr>
        <th>user_guid</th>
        <th>dog_guid</th>
        <th>test_name</th>
    </tr>
    <tr>
        <td>None</td>
        <td>fd27b86c-7144-11e5-ba71-058fbc01cf0b</td>
        <td>Yawn Warm-up</td>
    </tr>
    <tr>
        <td>None</td>
        <td>fd27b86c-7144-11e5-ba71-058fbc01cf0b</td>
        <td>Yawn Game</td>
    </tr>
    <tr>
        <td>None</td>
        <td>fd27b86c-7144-11e5-ba71-058fbc01cf0b</td>
        <td>Eye Contact Warm-up</td>
    </tr>
    <tr>
        <td>None</td>
        <td>fd27b86c-7144-11e5-ba71-058fbc01cf0b</td>
        <td>Eye Contact Game</td>
    </tr>
    <tr>
        <td>None</td>
        <td>fd27b86c-7144-11e5-ba71-058fbc01cf0b</td>
        <td>Treat Warm-up</td>
    </tr>
    <tr>
        <td>None</td>
        <td>fd27b86c-7144-11e5-ba71-058fbc01cf0b</td>
        <td>Arm Pointing</td>
    </tr>
    <tr>
        <td>None</td>
        <td>fd27b86c-7144-11e5-ba71-058fbc01cf0b</td>
        <td>Foot Pointing</td>
    </tr>
    <tr>
        <td>None</td>
        <td>fd27b86c-7144-11e5-ba71-058fbc01cf0b</td>
        <td>Watching</td>
    </tr>
    <tr>
        <td>None</td>
        <td>fd27b86c-7144-11e5-ba71-058fbc01cf0b</td>
        <td>Turn Your Back</td>
    </tr>
    <tr>
        <td>None</td>
        <td>fd27b86c-7144-11e5-ba71-058fbc01cf0b</td>
        <td>Cover Your Eyes</td>
    </tr>
</table>



**Question 7: How would create a text file with a list of all the customers with yearly memberships who live in the state of North Carolina (USA) and joined Dognition after March 1, 2014, sorted so that the most recent member is at the top of the list?**


```python
nc_users = %sql SELECT DISTINCT user_guid, state, created_at FROM users WHERE membership_type=2 AND state="NC" AND created_at > "2014-03-01" ORDER BY created_at DESC
```

    68 rows affected.



```python
nc_users.csv("nc_users.csv")
```




<a href="./files/nc_users.csv">CSV results</a>



**Question 8: See if you can find an SQL function from the list provided at:**

http://www.w3resource.com/mysql/mysql-functions-and-operators.php

**that would allow you to output all of the distinct breed names in UPPER case.  Create a query that would output a list of these names in upper case, sorted in alphabetical order.**


```python
%%sql
SELECT DISTINCT UPPER(breed)
FROM dogs
ORDER BY breed LIMIT 10
```

    10 rows affected.





<table>
    <tr>
        <th>UPPER(breed)</th>
    </tr>
    <tr>
        <td>-AMERICAN ESKIMO DOG MIX</td>
    </tr>
    <tr>
        <td>-AMERICAN PIT BULL TERRIER MIX</td>
    </tr>
    <tr>
        <td>-ANATOLIAN SHEPHERD DOG MIX</td>
    </tr>
    <tr>
        <td>-AUSTRALIAN CATTLE DOG MIX</td>
    </tr>
    <tr>
        <td>-AUSTRALIAN SHEPHERD MIX</td>
    </tr>
    <tr>
        <td>-BEAGLE MIX</td>
    </tr>
    <tr>
        <td>-BICHON FRISE MIX</td>
    </tr>
    <tr>
        <td>-BLUETICK COONHOUND MIX</td>
    </tr>
    <tr>
        <td>-BORDER COLLIE MIX</td>
    </tr>
    <tr>
        <td>-BOXER MIX</td>
    </tr>
</table>



**Practice any other queries you want to try below!**


```python

```
