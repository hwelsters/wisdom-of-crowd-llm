# **Wisdom of the Crowd on LLMs**
LLM used in this experiment: `ChatGPT gpt-0301-01`  

## **Abstract**
We analyze the performance of a group of LLMs as compared to the performance of its constituent individuals.  
We do this by performing a majority vote on the numbers that appear in the final answer.  
For example, given the following solutions
| Solution 1 | Solution 2 | Solution 3 |
| --- | --- | --- |
| 1 | 1 | 2 |
| 3 | 4 | 5 |
| 6 | 0 | 6 |

The majority solution will be 
| Solution |
| --- |
| 1 | 
| 6 |

Since both 1 and 6 appear 2 times where 2 ≥ 3 / 2 where 3 is the number of sample solutions.   

This method only the numbers that appear the majority of the time. (majority = greater than half the time. ) 
However, this method lacks real-world use-case especially in cases where an explanation is needed.  

Later, we propose a method whereby we instead elect a response using various methods to do so.  
This method will provide users with an explanation and is much more useful for real-world use cases.  

This repo details my experiments in figuring out whether or not this is the case.  

I have done this experiment on three datasets, and it appears to be the case so far.  
The datasets in which I have tried this on are:
- DRAW-1K
- ALG-514  
- NLU-ASDIV

## **Gotchas**
- ChatGPT's responses and the actual solution are all rounded to 3 decimals.  

## **Election methods**
We employ various methods for selecting a representative sample.  
- `Levenshtein distance` - We look at the levenshtein distance between the sample solution and the majority solution. We select the one with the smallest levenshtein distance.   
- `Most majority` - We select the sample solution where the intersection between the sample solution and the majority solution has the greatest length.  
- `Longest` - We select the sample solution with the most numbers.  


## **Stats**
The important columns are:  
- `sample_[num]` - Results of the array from sample [num]
- `majority` - This is the results of the arrays that are created by selecting values that appear a majority amount of times.  
- `levenshtein_correct` - This is the results of the arrays that are created by electing the array with the shortest levenshtein distance to the majority among the samples.  
- `most_majority_correct` - This is the results of the arrays that are created by electing the array with the most elements from the majority array among the samples.
- `max_size_correct` - This is the results of the arrays that are created by electing the array with the maximum size among the samples.
- `sample_average` - The average across all samples.
## **Results**:
The distribution of ChatGPT's response correctness.  
- `all` - This means ChatGPT's solution contains all the solution in the response.  
- `some` - This means ChatGPT's solution contains some of the solution in the response but not all of it.  
- 
### **DRAW-1K**
![image](https://github.com/hwelsters/wisdom-of-crowd-llm/assets/84760072/a33e8052-7931-4acc-b04a-1a236e0802bb)
  
### **ALG-514**
![image](https://github.com/hwelsters/wisdom-of-crowd-llm/assets/84760072/219cc55c-7a24-425f-9f55-c7ff5239dab1)
  
### **NLU-ASDIV**
![image](https://github.com/hwelsters/wisdom-of-crowd-llm/assets/84760072/3d5b2ef0-7c94-40cf-890c-f52941b7f506)
