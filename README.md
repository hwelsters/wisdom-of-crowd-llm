# **The Wisdom of Crowds of LLMs**

***

# **I. Abstract**
In various problem domains, it has been observed that the aggregate opinions of a group can outperform the results of any one individual in the group. We study the aggregate performance of a commercially available LLM known as ChatGPT's GPT-3.5 across 10 independent runs through the question sets DRAW-1K, ALG-514 and NLU-ASDIV. We find the aggregate using majority voting performs better in terms of providing more accurate responses and fewer wrong responses compared to any individual run. Later on, we propose various methods for selecting the best response from the various candidate solutions. The dataset of ChatGPT’s responses to the various question sets are also released.

# **II. Introduction**
The wisdom of the crowds theory refers to the concept that the aggregate judgments of groups are often more accurate than those of the individual. In this repo, we apply this theory to OpenAI’s ChatGPT 3.5 on math-word problems by using majority voting to select numbers that appear the most often. Furthermore, we later also propose various “election methods” by which representative samples are selected from the pool of candidates as the solution instead of using majority voting.


# **III. Materials and Methods**
**MWP Datasets.** In our experiment, we employed the following math-word problem question sets: DRAW-1K, ALG-514, NLU-ASDIV. These question sets contain not only the questions and the associated answers but also the representative system of equations that can be used to solve the problem. As an example, consider the following math word problem.

```
My mothers age is three times my age . The sum of our ages is 40 . How old am I ? How old is my mother ?
```
In addition to the correct answers, which are `10.0` and `30.0`, it also includes the system of equations needed to solve the problem `x + y = 40` and `y = 3*x`. These equations are representative of the problem.

**Collecting ChatGPT's responses at scale.** ChatGPT has released an API for public use. We used a tool named sleepyask to collect GPT-3.5’s responses at scale. This tool was made by us and allows us to collect a large amount of responses from ChatGPT. It applies the producer-consumer problem to ChatGPT to ask questions in parallel, allowing us to collect ChatGPT’s responses at a rapid pace. For the experiments, we suffixed each question with the following phrase before posting it to ChatGPT as a question.
```
Solve the following math problem: 
```
For each question set, we collected 10 sample responses for each question in the dataset.  
  
### **Majority voting**
In this repo, majority voting has the following definition. 
```
Majority voting produces a majority solution. A particular number appears in the majority solution iff the number of times it appears across various samples, x, is greater than or equal to n where n is the number of samples divided by two. In other words, a particular number appears in the majority solution iff x  n / 2.
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
In addition to using majority voting, we also use ‘election methods’ to select a representative sample from a set of candidate solutions. In this paper, we further outline the various methods that are used such as computing levenshtein distance, intersection size and picking the sample with the largest size. 
- **Levenshtein distance.** This is the string metric considered by the Soviet mathematician Vladimir Levenshtein. In this paper, we use the levenshtein as an election method whereby the solution array with the smallest levenshtein distance from the majority solution is selected. The result of the levenshtein distance between two arrays a and b are given by lev(a,b) where:  ![image](https://github.com/hwelsters/wisdom-of-crowd-llm/assets/84760072/d551f0fb-5c45-45f7-bf35-7ba522f8aec7)
- **Intersection size.** The intersection size of two arrays a and b are given by `sect(a, b)` where `sect(a, b)=|a ∩ b|`. We pick the candidate solution with the largest intersection size.  
- **Picking the sample with the largest size.** Given a set of candidate solutions S, we pick the first candidate solution x such that a  `S(|x| ≥ |a|)`.

## **Extracting answers**
We extract all decimals from ChatGPT's entire response. For example, given the text `1 + 1 = 2`, the solution will be `[1, 2]`. Of course, this isn't 100% accurate. As such, this will only be an estimate of ChatGPT's performance. However, based on what we have seen of ChatGPT's responses, it is a fairly reasonable estimate. If there is a more accurate to do this programmatically, we are open to suggestions.  

# **Results**
The important columns are:  
- `sample_[num]` - Results of the solutions from sample [num]
- `majority` - This is the results of the solutions that are created by selecting values that appear a majority amount of times.  
- `levenshtein_correct` - This is the results of the solutions that are created by electing the array with the shortest levenshtein distance to the majority among the samples.  
- `most_majority_correct` - This is the results of the solutions that are created by electing the array with the largest intersection size.
- `max_size_correct` - This is the results of the solutions that are created by electing the array with the maximum size among the samples.
- `sample_average` - The average across all samples.


The distribution of ChatGPT's response correctness.  
- `all` - This means ChatGPT's solution contains all the solution in the response.  
- `none` - This means ChatGPT's solution did not contain the solution in its response at all.  
- `some` - This means ChatGPT's solution contains some of the solution in the response but not all of it.  
### **DRAW-1K**
![image](https://github.com/hwelsters/wisdom-of-crowd-llm/assets/84760072/7703dff0-2b50-4c39-9a1f-97846b837d91)
  
### **ALG-514**
![image](https://github.com/hwelsters/wisdom-of-crowd-llm/assets/84760072/2b78e427-856a-4177-bc69-fa9a8ca44c29)
  
### **NLU-ASDIV**
![image](https://github.com/hwelsters/wisdom-of-crowd-llm/assets/84760072/e049a749-60e1-48f8-831a-14356a8768b9)

## **Conclusions**
It appears that majority-vote, levenshtein distance and intersection size are fairly promising methods by which ChatGPT's performance on math-word problems can be improved. Furthermore, levenshtein distance and intersection size possess the advantage over majority vote in the sense that, since it selects a representative solution, we can use the explanations present in that solution.  

## **Potential issues**
- ChatGPT's responses and the actual solution are all rounded to 3 decimals since ChatGPT likes to round irrational numbers. This could possibly create some problems. However, it seems to be pretty reasonable so far.  
- In the experiment, we check the entire response for decimals and include it in the solution. If there is a more accurate to do this, I am open to suggestions. As such, one possible argument against this method is that the majority array might simply be larger and ChatGPT simply "casts a wider net" in which it may be lucky. In this experiment, we attempt to show that on average, the majority array is often similarly sized, if not smaller, than its individual sample solutions. Furthermore, we use `Longest` as an election method and it doesn't perform as well, attempting to counteract this argument.

## **Things left to do**
- Give a reason as to WHY majority vote performs better. Try to explain why wisdom of the crowd works on LLMs.
- Give a reason as to WHY election methods work. For example, I am quite confused as to why most majority would work better than majority. Are there numbers that appear very often that are important that somehow don't appear often enough to appear in the majority solution?  
- Ways to apply this to other problem domains. Math word problems are awfully limited and might not have a whole lot of use. 


