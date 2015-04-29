# Homework 2 Part 1
**CS 461** 

### Problem 1
1. Write **two equivalent** relational algebra queries that list the names of houses other than house Targaryen
	1. $$\pi_{name}(\sigma_{house != "Targaryen"}(Characters))$$ 
	2. $$\pi_{name}((Characters)-\sigma_{house == "Targaryen"}(Characters))$$

			SELECT DISTINCT name 
			FROM Characters 
			WHERE house <> "Targaryen";

2. Write a relational algebra query that computes the names of characters that appeared in episodes with more than 3M viewers
	1. **$$\pi_{Appearances.name}(\sigma_{Episodes.viewers>3.3M}(Episodes\Join_{season, num}Appearances$$**

			SELECT DISTINCT A.name 
			FROM Appearances A
			JOIN Episodes E on E.season=A.season 
			AND E.num = A.num
			WHERE E.Viewers > 3.3M 
			
3. Write **two equivalent** relational algebra queries that compute the name and the house of all characters that appeared in any episode for which it's title starts with "The". For each expression, draw a query execution tree and show how many tuples are produced at each step for the instances on the previous page. 
	1. $$\pi_{Characters.name, Characters.house}(\sigma_{Episodes.title = "The\%"}((Episodes\Join_{season, num}Appearances)\Join_{name}Characters))$$
	2. $$\pi_{Characters.name, Characters.house}(\sigma_{Episodes.title = "The\%"}((Episodes\times_{season, num}Appearances)\times_{name}(Characters)))$$

			SELECT DISTINCT C.name, C.house 
			FROM Characters C
			JOIN Appearances A, Episodes E
			ON C.name = A.name
			AND E.season=A.season 
			AND E.num = A.num
			WHERE  E.title LIKE "The%"

4. Write a relational algebra query that computes names and houses of **pairs of characters** who appeared together in some episode in season 1. For example, this expression will output (Eddard, Stark, Tyrion, Lannister) because both characters appeared in episode 1 of season 1.
	1. $$\pi_{Characters.name, Characters.house}(\sigma_{Appearances.name=Appearances.name}(Characters\Join_{name=name}Appearances))$$

			SELECT DISTINCT C.name, C.house 
			FROM Characters C
			JOIN Appearances A 
			ON C.name = A.name
			WHERE A.season = 1
			AND C.name = A.name
		
* How many tuples will be in the result of this query for the given instances? Briefly justify your answer.
		* **10 Because every person who appeared in each episode will be permuted with every other person who appeared in the episode**
5. Suppose that you execute the query in (d) on some instance in which there are c tuples in relation Characters, *e* tuples in relation Episodes, and *a* tuples in relation Appearances. What is the minimum and maximum number of tuples in the result of the query in (d)? Briefly justify your answer. 
	* **The maximum will be c*e*a since the cross product of the three will have a maximum of the product of all 3. The minimum will be 0 if the query eliminates all of the possible results.**
6. Write a relational algebra expression that lists names of charachters that did not appear in any episode with over 3M viewers. 
	1.  $$\pi_{Appearances.name}(Appearances\Join_{season, num}Episodes)-\sigma_{viewers>3.3M}(Appearances\Join_{season, num}Episodes))$$

			SELECT DISTINCT A.name 
			FROM Appearances A
			JOIN Episodes E 
			ON E.season = A.season
			AND E.num = A.num
			WHERE E.Viewers < 3.3M 

### Problem 2
Consider SQL queries below. For each query, write two different relational algebra expressions to which this query can be re-written. There may be more than 2 ways to rewrite the SQL query into relational algebra. You will only be graded on the first two relational algebra expressions you give. Do not use the renaming operator in your relational algebra expressions.

Note that most relational algebra operations are symmetric, that is A op B = B op A. Writing operands of a symmetric operation in a different order does not count as a different relational algebra expression. 

A. 
		SELECT DISTINCT viewers
		FROM Episodes
		WHERE viewers < 3M;

1. $$\pi_{viewers}(\sigma_{viewers < 3M}(Episodes))$$
2. $$\pi_{viewers}(Episodes - \sigma_{viewers > 3M}(Episodes))$$

B. 
		SELECT DISTINCT C.house
		FROM Characters C, Episodes E, Appearances A
		WHERE E.season = A.season
		AND E.num = A.num
		AND A.name = C.name
		AND A.num = 2;

1. $$\pi_{Characters.house}(\sigma_{Appearances.num = 2, Episode.num=2}((Characters\Join_{name}Appearances)\Join_{num}Episodes))$$
2. $$\pi_{Characters.house}(\sigma_{Appearances.num = 2}(\sigma_{Appearances.season=Episode.season}(Episodes\times Appearances\times Character)))$$

	


