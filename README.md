# 来源
[https://inst.eecs.berkeley.edu//~cs61a/fa13/proj/trends/trends.html](https://inst.eecs.berkeley.edu//~cs61a/fa13/proj/trends/trends.html)

## Introduction
- In this project, you will develop a geographic visualization of Twitter data across the USA. 
- You will need to use dictionaries, lists, and data abstraction techniques to create a modular program. 
- This project uses ideas from Sections 2.1, 2.2, 2.3, and 2.4, of the Composing Programs online textbook.
- The map displayed above depicts how the people in different states feel about Texas. 

This image is generated by:
- Collecting public Twitter posts (tweets) that have been tagged with geographic locations and filtering for those that contain the "texas" query term,
- Assigning a sentiment (positive or negative) to each tweet, based on all of the words it contains,
- Aggregating tweets by the state with the closest geographic center, and finally
- Coloring each state according to the aggregate sentiment of its tweets. Red means positive sentiment; blue means negative.
- The details of how to conduct each of these steps is contained within the project description. By the end of this project, you will be able to map the sentiment of any word or phrase. The trends.zip archive contains all the starter code and all data (81 MB).

## Tips
### The Autograder
We've included an autograder which includes tests for each question. You can invoke it for a particular question number as follows:
```
python3 trends_grader.py -q <question number>
```
You can also invoke the autograder for all problems at once using:
```
python3 trends_grader.py
```

### Debugging
- You can use the functions `trace`, `interact`, and `log_current_line` defined in ucb.py to inspect a running program.
- You can load your entire implementation and then interact with the current environment using the command: `python3 -i trends.py`
- Add print calls to your functions, but remove them before submitting your final version.


## Phase 1: The Feelings in Tweets
In this phase, you will create an abstract data type for tweets, split the text of a tweet into words, and calculate the amount of positive or negative feeling in a tweet.

- Tweets
First, we will define an abstract data type for tweets. To ensure that we do not violate abstraction barriers later in the project, we will create two different representations:
```text
(A) The constructor make_tweet returns a Python dictionary with the following entries:

  'text':      a string, the text of the tweet, all in lowercase
  'time':      a datetime object, when the tweet was posted
  'latitude':  a floating-point number, the latitude of the tweet's location
  'longitude': a floating-point number, the longitude of the tweet's location
(B) The alternate constructor make_tweet_fn returns a function that takes a string argument that is one of the keys above and returns the corresponding value.
```
### Problem 1 (1 pt). 
Implement the missing selector and constructor functions for these two representations: tweet_text, tweet_time, tweet_location correspond to representation (A); make_tweet_fn corresponds to representation(B). For tweet_location you should return a position object. The constructors and selectors for this abstract data type can be found in geo.py. Remember to preserve data abstraction!

The two representations created by make_tweet and make_tweet_fn do not need to work together, but each constructor should work with its corresponding selectors. The doctests for make_tweet and make_tweet_fn ensure that this is the case. They can be run along with other tests using:
```
python trends_grader.py -q 1
```
Next, we will retrieve the words from a tweet and compute their sentiment.

### Problem 2 (2 pt). 
Improve the extract_words function as follows: 
- Assume that a word is any consecutive substring of text that consists only of ASCII letters. 
- The string ascii_letters in the string module contains all letters in the ASCII character set. 
- The `extract_words` function should list all such words in order and nothing else.

When you complete this problem, tests for question 2 should pass:
```
python trends_grader.py -q 2
```

### Problem 3 (1 pt). 
Implement the sentiment abstract data type, which represents a sentiment value that may or may not exist. 
- The constructor `make_sentiment` takes either a numeric value within the interval -1 to 1, or None to indicate that the value does not exist. 

Implement the selectors `has_sentiment` and `sentiment_value` as well. 
- You may use any representation you choose, but the rest of your program should not depend on this representation.

When you complete this problem, the question 3 tests should pass:
```
python trends_grader.py -q 3
```

You can also call the print_sentiment function to print the sentiment values of all sentiment-carrying words in a line of text.
```
python trends.py -p computer science is my favorite!
python trends.py -p life without lambda: awful or awesome?
```

### Problem 4 (1 pt). 
Implement analyze_tweet_sentiment, which takes a tweet (of the abstract data type) and returns a sentiment.
Read the docstrings for `get_word_sentiment` and `analyze_tweet_sentiment` in `trends.py` to understand how the two functions interact. 
Your implementation should not depend on the representation of a sentiment!.

The tweet_words function should prove useful here: it combines the tweet_text selector and extract_words function from the previous questions to return a list of words in a tweet.

When you complete this problem, the question 4 tests should pass:
```
python trends_grader.py -q 4
```