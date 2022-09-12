# SQL-Dictionary
This project showcases my ability to create complex queries using the English dictionary. The dictionary was downloaded from http://www.mieliestronk.com/corncob_lowercase.txt (saved as 'words' in repository). This work does not validate the completeness of the dictionary. Rather, it is used as a vehicle to demonstrate complex querying on dictionary data.

## Table of Contents
- Starting Tables
- Created Tables
- Distributions
  - First Letter, Frequency and Relative Frequency 
  - Word Length, Frequency and Relative Frequency
  - Word Length, Mean, Median, Mode
  - First and Last Letter Combination, Frequency and Relative Frequency
- Anagrams (of equal length or fewer)
- what words have two of the same letter next to each other
- Highest Scoring Scrabble Words

## Starting Tables
Two tables are taken as the initial inputs, words and scrabble, both available in the repository. Words is a list of words in the English dictionary. Scrabble is a list of the letters of the alphabet and the points associated with them.

Words
  - IndexCol
  - Word
 
Scrabble
  - ScrabbleIndex
  - Letter
  - Points

## Created Tables
Three tables were created to aid in analysis, letter_vector, letter_count_vector, scrabble_words. 

### letter_vector
letter_vector decomposes a word into 24 columns with one letter in each column. This is greater than 22, the longest word in this dictionary. When a word has fewer letters than the 24 columns, the remaining columns are populated with null values.
```SQL
create table letter_vector
  (select
	word,
	substring(word, 1,1) letter_1,
	substring(word, 2,1) letter_2,
	substring(word, 3,1) letter_3,
	substring(word, 4,1) letter_4,
	substring(word, 5,1) letter_5,
	substring(word, 6,1) letter_6,
	substring(word, 7,1) letter_7,
	substring(word, 8,1) letter_8,
	substring(word, 9,1) letter_9,
	substring(word, 10,1) letter_10,
	substring(word, 11,1) letter_11,
	substring(word, 12,1) letter_12,
	substring(word, 13,1) letter_13,
	substring(word, 14,1) letter_14,
	substring(word, 15,1) letter_15,
	substring(word, 16,1) letter_16,
	substring(word, 17,1) letter_17,
	substring(word, 18,1) letter_18,
	substring(word, 19,1) letter_19,
	substring(word, 20,1) letter_20,
	substring(word, 21,1) letter_21,
	substring(word, 22,1) letter_22,
	substring(word, 23,1) letter_23,
	substring(word, 24,1) letter_24
  from words)
```

### letter_count_vector
The columns of letter_count_vector are the letters of the alphabet, and each row records the number of times that letter appears in a given word.
```SQL
create table letter_count_vector
(
select 
	  word,
	  length(word) - length(replace(word, 'a', '')) a,
    length(word) - length(replace(word, 'b', '')) b,
    length(word) - length(replace(word, 'c', '')) c,
    length(word) - length(replace(word, 'd', '')) d,
    length(word) - length(replace(word, 'e', '')) e,
    length(word) - length(replace(word, 'f', '')) f,
    length(word) - length(replace(word, 'g', '')) g,
    length(word) - length(replace(word, 'h', '')) h,
    length(word) - length(replace(word, 'i', '')) i,
    length(word) - length(replace(word, 'j', '')) j,
    length(word) - length(replace(word, 'k', '')) k,
    length(word) - length(replace(word, 'l', '')) l,
    length(word) - length(replace(word, 'm', '')) m,
    length(word) - length(replace(word, 'n', '')) n,
    length(word) - length(replace(word, 'o', '')) o,
    length(word) - length(replace(word, 'p', '')) p,
    length(word) - length(replace(word, 'q', '')) q,
    length(word) - length(replace(word, 'r', '')) r,
    length(word) - length(replace(word, 's', '')) s,
    length(word) - length(replace(word, 't', '')) t,
    length(word) - length(replace(word, 'u', '')) u,
    length(word) - length(replace(word, 'v', '')) v,
    length(word) - length(replace(word, 'w', '')) w,
    length(word) - length(replace(word, 'x', '')) x,
    length(word) - length(replace(word, 'y', '')) y,
    length(word) - length(replace(word, 'z', '')) z
    
from words
)
```

### scrabble_words
The scrabble_words table has two columns, words and word_points. Word points is the sum of the points that each letter in a word has. No bonus tiles were included (double word score, triple letter score, etc.). Also, only words of length 1 through 7 were included. This is due to players only having 7 letters at a time in their shelf. Constraints were imposed to exclude words that use more letters than exist in the game (i.e., jazz is excluded because Scrabble only has one z tile). 
```SQL
create table scrabble_words
(
select 
	word,
	(ifnull(one.points,0) + ifnull(two.points,0) + ifnull(three.points,0) + ifnull(four.points,0) + ifnull(five.points,0) + ifnull(six.points,0) + ifnull(seven.points,0) + ifnull(eight.points,0) + ifnull(nine.points,0) + ifnull(ten.points,0) + ifnull(eleven.points,0) + ifnull(twelve.points,0) + ifnull(thirteen.points,0) + ifnull(fourteen.points,0) + ifnull(fifteen.points,0) + ifnull(sixteen.points,0) + ifnull(seventeen.points,0) + ifnull(eighteen.points,0) + ifnull(nineteen.points,0) + ifnull(twenty.points,0) + ifnull(twentyone.points,0) + ifnull(twentytwo.points,0) + ifnull(twentythree.points,0) + ifnull(twentyfour.points,0)) word_points
from letter_vector left join scrabble one on
	letter_vector.letter_1 = one.Letter
    left join scrabble two on 
    letter_vector.letter_2 = two.Letter
    left join scrabble three on 
    letter_vector.letter_3 = three.Letter
    left join scrabble four on 
    letter_vector.letter_4 = four.Letter
    left join scrabble five on 
    letter_vector.letter_5 = five.Letter
    left join scrabble six on 
    letter_vector.letter_6 = six.Letter
    left join scrabble seven on 
    letter_vector.letter_7 = seven.Letter
    left join scrabble eight on 
    letter_vector.letter_8 = eight.Letter
    left join scrabble nine on 
    letter_vector.letter_9 = nine.Letter
    left join scrabble ten on 
    letter_vector.letter_10 = ten.Letter
    left join scrabble eleven on 
    letter_vector.letter_11 = eleven.Letter
    left join scrabble twelve on 
    letter_vector.letter_12 = twelve.Letter
    left join scrabble thirteen on 
    letter_vector.letter_13 = thirteen.Letter
    left join scrabble fourteen on 
    letter_vector.letter_14 = fourteen.Letter
    left join scrabble fifteen on 
    letter_vector.letter_15 = fifteen.Letter
    left join scrabble sixteen on 
    letter_vector.letter_16 = sixteen.Letter
    left join scrabble seventeen on 
    letter_vector.letter_17 = seventeen.Letter
    left join scrabble eighteen on 
    letter_vector.letter_18 = eighteen.Letter
    left join scrabble nineteen on 
    letter_vector.letter_19 = nineteen.Letter
    left join scrabble twenty on 
    letter_vector.letter_20 = twenty.Letter
    left join scrabble twentyone on 
    letter_vector.letter_21 = twentyone.Letter
    left join scrabble twentytwo on 
    letter_vector.letter_22 = twentytwo.Letter
    left join scrabble twentythree on 
    letter_vector.letter_23 = twentythree.Letter
    left join scrabble twentyfour on 
    letter_vector.letter_24 = twentyfour.Letter
where
	-- only seven tiles per person
	length(word) <= 7
	-- constraints on number of available tiles
	and ((length(word) - length(replace(word,'a','')) <= 9))
	and ((length(word) - length(replace(word,'b','')) <= 2))
	and ((length(word) - length(replace(word,'c','')) <= 2))
	and ((length(word) - length(replace(word,'d','')) <= 4))
	and ((length(word) - length(replace(word,'e','')) <= 12))
	and ((length(word) - length(replace(word,'f','')) <= 2))
	and ((length(word) - length(replace(word,'g','')) <= 3))
	and ((length(word) - length(replace(word,'h','')) <= 2))
	and ((length(word) - length(replace(word,'i','')) <= 9))
	and ((length(word) - length(replace(word,'j','')) <= 1))
	and ((length(word) - length(replace(word,'k','')) <= 1))
	and ((length(word) - length(replace(word,'l','')) <= 4))
	and ((length(word) - length(replace(word,'m','')) <= 2))
	and ((length(word) - length(replace(word,'n','')) <= 6))
	and ((length(word) - length(replace(word,'o','')) <= 8))
	and ((length(word) - length(replace(word,'p','')) <= 2))
	and ((length(word) - length(replace(word,'q','')) <= 1))
	and ((length(word) - length(replace(word,'r','')) <= 6))
	and ((length(word) - length(replace(word,'s','')) <= 4))
	and ((length(word) - length(replace(word,'t','')) <= 6))
	and ((length(word) - length(replace(word,'u','')) <= 4))
	and ((length(word) - length(replace(word,'v','')) <= 2))
	and ((length(word) - length(replace(word,'w','')) <= 2))
	and ((length(word) - length(replace(word,'x','')) <= 1))
	and ((length(word) - length(replace(word,'y','')) <= 2))
	and ((length(word) - length(replace(word,'z','')) <= 1))
)
```

## Distributions
### First Letter, Frequency and Relative Frequency
```SQL
select
	left(word, 1) first_letter,
    count(left(word,1))/(select count(*) from words) relative_frequency,
    count(left(word,1)) frequency
from words
where left(word, 1) is not null
group by left(word, 1)
```

### Word Length, Frequency and Relative Frequency
```SQL
select 
	length(word) as word_length, 
    count(word) as word_length_frequency,
    count(word)/( select count(*) from words )
from words
group by length(word)
order by count(word) desc
```

### Word Length, Mean, Median, and Mode
```SQL
select
    "word length",
    (select avg(length(word)) from words) mean,
    -- median calculation below
    if((select count(*) from words) % 2 = 0, 
			(
            -- case when even
            select avg(word_length)
			from (
				select 
				length(word) word_length,
				row_number() over() id
				from words
				where length(word) is not null
				order by length(word)
				) as ordered_word_length
			where id = (select ((select count(*) from words) -1) div 2) or id = (select ((select count(*) from words) -1) div 2 + 1)
            )
		, 
			(
			-- case when odd
			select word_length
			from (
				select 
				length(word) word_length,
				row_number() over() id
				from words
				where length(word) is not null
				order by length(word)
				) as ordered_word_length
			where id = (select ((select count(*) from words) -1) div 2 + 1)
			)
        
        ) median,
        
	-- mode calculation below
    (
		select word_length
		from (
			select word_length, max(c)
			from (
			select
				length(word) word_length,
				count('') c
			from words
			group by length(word) 
			)hist
		) max
    )mode
```

### First and Last Letter Pairing, Frequency and Relative Frequency
```SQL
select
	concat(left(word,1),right(word,1)) first_last_letter,
    count(word) frequency,
    count(word)/( select count(*) from words ) relative_frequency
from words
where concat(left(word,1),right(word,1)) is not null
group by first_last_letter
order by frequency desc
```
## Anagrams (of equal length or fewer)
Anagrams have two commonly used definitions. They are either the rearrangement of the letters of one word to form other words of equal length. Or they are the words that can be made from using a subset of letters from another word. Computationally, the code is quite similar. However, the latter definition is used as it is more robust. The following code generates a list of anagrams for each word and the count of how many there are.  
```SQL
select origin.word word, group_concat(anagram.word) component_words, count('') num_of_component_words
from letter_count_vector as origin inner join letter_count_vector as anagram on
	origin.a <= anagram.a
    and origin.b >= anagram.b
    and origin.c >= anagram.c
    and origin.d >= anagram.d
    and origin.e >= anagram.e
    and origin.f >= anagram.f
    and origin.g >= anagram.g
    and origin.h >= anagram.h
    and origin.i >= anagram.i
    and origin.j >= anagram.j
    and origin.k >= anagram.k
    and origin.l >= anagram.l
    and origin.m >= anagram.m
    and origin.n >= anagram.n
    and origin.o >= anagram.o
    and origin.p >= anagram.p
    and origin.q >= anagram.q
    and origin.r >= anagram.r
    and origin.s >= anagram.s
    and origin.t >= anagram.t
    and origin.u >= anagram.u
    and origin.v >= anagram.v
    and origin.w >= anagram.w
    and origin.x >= anagram.x
    and origin.y >= anagram.y
    and origin.z >= anagram.z
where origin.word <> anagram.word
group by origin.word
order by count('') desc
```

## Highest Scoring Scrabble Words
The following code reports the highest scoring word that is possible to have in a shelf of Scrabble.
```SQL
select length(word), word, word_points
from scrabble_words
where
(word_points = (select max(word_points) from scrabble_words where length(word) = 1) and length(word) = 1) or
(word_points = (select max(word_points) from scrabble_words where length(word) = 2) and length(word) = 2) or
(word_points = (select max(word_points) from scrabble_words where length(word) = 3) and length(word) = 3) or 
(word_points = (select max(word_points) from scrabble_words where length(word) = 4) and length(word) = 4) or 
(word_points = (select max(word_points) from scrabble_words where length(word) = 5) and length(word) = 5) or 
(word_points = (select max(word_points) from scrabble_words where length(word) = 6) and length(word) = 6) or
(word_points = (select max(word_points) from scrabble_words where length(word) = 7) and length(word) = 7)
group by length(word)
order by length(word)
```
