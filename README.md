# **Wisdom of the Crowd on LLMs**

***

# **I. Abstract**
In various problem domains, it has been observed that the aggregate opinions of a group can outperform the results of any one individual in the group. We study the aggregate performance of a commercially available LLM known as ChatGPT across 10 nondeterministic runs through the math word problem question sets DRAW-1K, ALG-514 and NLU-ASDIV. We find the aggregate using majority voting performs better in terms of providing more accurate responses and fewer wrong responses compared to any individual run. Later on, we propose various election methods for selecting the best response from the various sample solutions which appears to bring about even greater improvements in performance. The dataset of ChatGPT’s responses to the various question sets are also provided in this repo.

# **II. Introduction**
Large-language models have gained popularity in recent years. At the moment, many consider OpenAI’s ChatGPT as state-of-the-art. However, it notably fails to perform well on tasks requiring precise calculations and reasoning. In this repo, we attempt to improve ChatGPT’s performance in math-word problems by applying the wisdom of the crowds theory.  

The wisdom of the crowds theory refers to the concept that the aggregate judgments of groups are often more accurate than those of the individual. In this repo, we apply this theory to OpenAI’s ChatGPT 3.5 on math-word problems by using majority voting to select numbers that appear the most often. Furthermore, we later also propose various “election” methods by which representative samples are selected from the pool of candidates as the solution instead of majority voting.

# **III. Materials and Methods**
**MWP Datasets.** We employed the following math-word problem question sets: DRAW-1K, ALG-514, NLU-ASDIV. These question sets contain not only the questions and answers but also the system of equations that may be used to solve the problem. An example is the following MWP:
```
Juniors boat will go 15 miles per hour in still water . 
If he can go 12 miles downstream in the same amount of time as it takes to go 9 miles upstream , 
then what is the speed of the current .
```
**Collecting ChatGPT's responses at scale.** I made use of the following tool to collect ChatGPT's responses: https://github.com/aardoh/sleepyask. sleepyask is a tool I made which allows me to collect responses from ChatGPT faster since it applies the producer-consumer problem in order to ask multiple questions in parallel. Before each question asked to ChatGPT, I added the following piece of text: 
```
Solve the following math problem: 
```
For each question set, we collected 10 sample responses for each question in the dataset.  
  
### **Majority voting**
In this repo, majority voting has the following definition. 
```
A particular number appears in the majority solution iff it appears more than or equal to n / 2 times where n is the number of samples collected for the question.
```

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

Since both 1 and 6 appear 2 times where `2 > 3 / 2 = 1.5`

## **Election methods**
Instead of getting the majority vote response, we also use various election methods for selecting a single representative sample among the various solutions. Election methods also have an advantage over majority voting in that since we are selecting a sample from the candidates, we get the explanations that come with it.  
The various methods are as follows:  
- `Levenshtein distance` - We look at the levenshtein distance between the sample solution and the majority solution. We select the one with the smallest levenshtein distance.   
- `Most majority` - We select the sample solution where the intersection between the sample solution and the majority solution has the greatest size.  
- `Longest` - We select the sample solution with greatest set size.  

## **Extracting answers**
We extract all decimals from ChatGPT's entire response. For example, given the text `1 + 1 = 2`, the solution will be `[1, 2]`. Of course, this isn't 100% accurate. As such, this will only be an estimate of ChatGPT's performance. However, based on what we have seen of ChatGPT's responses, it is a fairly reasonable estimate. If there is a more accurate to do this programmatically, we are open to suggestions.  

# **Results**
The important columns are:  
- `sample_[num]` - Results of the solutions from sample [num]
- `majority` - This is the results of the solutions that are created by selecting values that appear a majority amount of times.  
- `levenshtein_correct` - This is the results of the solutions that are created by electing the array with the shortest levenshtein distance to the majority among the samples.  
- `most_majority_correct` - This is the results of the solutions that are created by electing the array with the most elements from the majority array among the samples.
- `max_size_correct` - This is the results of the solutions that are created by electing the array with the maximum size among the samples.
- `sample_average` - The average across all samples.


The distribution of ChatGPT's response correctness.  
- `all` - This means ChatGPT's solution contains all the solution in the response.  
- `none` - This means ChatGPT's solution did not contain the solution in its response at all.  
- `some` - This means ChatGPT's solution contains some of the solution in the response but not all of it.  
### **DRAW-1K**
![image](https://github.com/hwelsters/wisdom-of-crowd-llm/assets/84760072/41d091b5-c810-4b77-9077-1c62b95bc83b)
  
### **ALG-514**
![image](https://github.com/hwelsters/wisdom-of-crowd-llm/assets/84760072/bd646682-c7ff-4f62-965d-9958c9ad6559)
  
### **NLU-ASDIV**
![image](https://github.com/hwelsters/wisdom-of-crowd-llm/assets/84760072/a4114d94-0301-4385-b6f8-f93a4be2f92d)

## **Potential issues**
- ChatGPT's responses and the actual solution are all rounded to 3 decimals since ChatGPT likes to round irrational numbers. This could possibly create some problems. However, it seems to be pretty reasonable so far.  
- In the experiment, we check the entire response for decimals and include it in the solution. If there is a more accurate to do this, I am open to suggestions. As such, one possible argument against this method is that the majority array might simply be larger and ChatGPT simply "casts a wider net" in which it may be lucky. In this experiment, we attempt to show that on average, the majority array is often similarly sized, if not smaller, than its individual sample solutions. Furthermore, we use `Longest` as an election method and it doesn't perform as well, attempting to counteract this argument.

## **Things left to do**
- Give a reason as to WHY majority vote performs better. Try to explain why wisdom of the crowd works on LLMs.
- Give a reason as to WHY election methods work. For example, I am quite confused as to why most majority would work better than majority. Are there numbers that appear very often that are important that somehow don't appear often enough to appear in the majority solution?  
- Ways to apply this to other problem domains. Math word problems are awfully limited and might not have a whole lot of use. 


