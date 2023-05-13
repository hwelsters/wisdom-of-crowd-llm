# **Wisdom of the Crowd on LLMs**
LLM used in this experiment: `ChatGPT gpt-0301-01`  

## **Abstract**
We analyze the performance of a group of LLMs as compared to the performance of its constituent individuals.  
We do this by performing a majority vote on the numbers that appear in the final answer.  

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

## **Stats**
The important columns are:  
- `sample_[num]` - Results of the array from sample [num]
- `majority` - This is the results of the arrays that are created by selecting values that appear a majority amount of times.  
- `levenshtein_correct` - This is the results of the arrays that are created by electing the array with the shortest levenshtein distance to the majority among the samples.  
- `most_majority_correct` - This is the results of the arrays that are created by electing the array with the most elements from the majority array among the samples.
- `max_size_correct` - This is the results of the arrays that are created by electing the array with the maximum size among the samples.
- `sample_average` - The average across all samples.
## **Results**:
### **DRAW-1K**
![image](https://github.com/hwelsters/wisdom-of-crowd-llm/assets/84760072/a33e8052-7931-4acc-b04a-1a236e0802bb)
  
### **ALG-514**
![image](https://github.com/hwelsters/wisdom-of-crowd-llm/assets/84760072/219cc55c-7a24-425f-9f55-c7ff5239dab1)
  
### **NLU-ASDIV**
![image](https://github.com/hwelsters/wisdom-of-crowd-llm/assets/84760072/3d5b2ef0-7c94-40cf-890c-f52941b7f506)
