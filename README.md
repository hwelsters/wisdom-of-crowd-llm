# **Wisdom of the Crowd on LLMs**

***

# **I. Abstract**
In various problem domains, it has been observed that the aggregate opinions of a group often outperform the results of any one individual in the group. We study the aggregate performance of a commercially available LLM known as ChatGPT across 10 nondeterministic runs through the question sets DRAW-1K, ALG-514 and NLU-ASDIV. We find the aggregate using majority voting performs better in terms of providing more accurate responses and fewer wrong responses compared to any individual run. Later on, we propose various election methods for selecting the best response from the various sample solutions which appears to bring about even greater improvements in performance. The dataset of ChatGPT’s responses to the various question sets are also released.

# **II. Introduction**
The "wisdom of the crowd" refers to theory where independent judgements are combined in order to achieve a final judgement with the greatest accuracy. Due to the non-determinism of LLMs such as ChatGPT, we treat each sample solution provided by ChatGPT as an independent judgement in this experiment.  

# **III. Materials and Methods**
I made use of the following tool to collect ChatGPT's responses: https://github.com/aardoh/sleepyask. Sleepyask is a tool I made which makes collecting data with ChatGPT significantly faster since it applies the producer-consumer problem in order to ask multiple questions in parallel.  
  
The question sets used in this experiment are:
- DRAW-1K
- ALG-514
- NLU-ASDIV
  
### **Majority voting**
In this repo, majority voting has the following definition. We select only numbers that appear greater than `[num of sample solutions] / 2` times
For example, given the following solutions
| Solution 1 | Solution 2 | Solution 3 |
| --- | --- | --- |
| 1 | 1 | 2 |
| 3 | 4 | 5 |
| 6 | 0 | 6 |

The following majority solution will be created:
| Solution |
| --- |
| 1 | 
| 6 |

Since both 1 and 6 appear 2 times where `2 times` and `2 > 3 / 2 = 1.5`

However, this method lacks real-world use-case especially in cases where an explanation is needed.  

As such, we later propose a method whereby we instead elect a response using various methods to do so.  
This method will provide users with an explanation and is much more useful for real-world use cases.  

This repo details my experiments in figuring out whether or not this is the case.  

I have done this experiment on three datasets, and it appears to be the case so far.  
The datasets in which I have tried this on are:
- DRAW-1K
- ALG-514  
- NLU-ASDIV

## **Possible Gotchas**
- ChatGPT's responses and the actual solution are all rounded to 3 decimals since ChatGPT likes to round irrational numbers. This could possibly create some problems. However, none have been observed so far but it is definitely possible.
- In the experiment, we check the entire response for decimals and include it in the solution. If there is a more accurate to do this, I am open to suggestions. As such, one possible argument against this method is that the majority array might simply be larger and ChatGPT simply "casts a wider net" in which it may be lucky. In this experiment, we attempt to show that on average, the majority array is often similarly sized, if not smaller, than its individual sample solutions.  

## **Election methods**
We employ various methods for selecting a representative sample.  
- `Levenshtein distance` - We look at the levenshtein distance between the sample solution and the majority solution. We select the one with the smallest levenshtein distance.   
- `Most majority` - We select the sample solution where the intersection between the sample solution and the majority solution has the greatest length.  
- `Longest` - We select the sample solution with the most numbers.  


## **Stats**
The important columns are:  
- `sample_[num]` - Results of the solutions from sample [num]
- `majority` - This is the results of the solutions that are created by selecting values that appear a majority amount of times.  
- `levenshtein_correct` - This is the results of the solutions that are created by electing the array with the shortest levenshtein distance to the majority among the samples.  
- `most_majority_correct` - This is the results of the solutions that are created by electing the array with the most elements from the majority array among the samples.
- `max_size_correct` - This is the results of the solutions that are created by electing the array with the maximum size among the samples.
- `sample_average` - The average across all samples.
## **Results**:
The distribution of ChatGPT's response correctness.  
- `all` - This means ChatGPT's solution contains all the solution in the response.  
- `none` - This means ChatGPT's solution did not contain the solution in its response at all.  
- `some` - This means ChatGPT's solution contains some of the solution in the response but not all of it.  
### **DRAW-1K**
![image](https://github.com/hwelsters/wisdom-of-crowd-llm/assets/84760072/a33e8052-7931-4acc-b04a-1a236e0802bb)
  
### **ALG-514**
![image](https://github.com/hwelsters/wisdom-of-crowd-llm/assets/84760072/219cc55c-7a24-425f-9f55-c7ff5239dab1)
  
### **NLU-ASDIV**
![image](https://github.com/hwelsters/wisdom-of-crowd-llm/assets/84760072/3d5b2ef0-7c94-40cf-890c-f52941b7f506)

## **Things left to do**
- Reason WHY majority vote performs better. Try to explain why wisdom of the crowd works on LLMs?  
- Reason WHY election methods work?  
- Draw a few conclusions from this.
