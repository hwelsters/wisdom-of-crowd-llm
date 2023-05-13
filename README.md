## **Wisdom of the Crowd on LLM**

It is possible that the aggregate opinions of a group of LLMs can outperform the opinions of any constituent individual.  

This repo details my experiments in figuring out whether or not this is the case.  

I have done this experiment on three datasets, and it appears to be the case so far.  
The datasets in which I have tried this on are:
- DRAW-1K
- ALG-514  
- NLU-ASDIV

### **Stats**
The important columns are:  
- `sample_[num]` - Results of the array from sample [num]
- `majority` - This is the results of the arrays that are created by selecting values that appear a majority amount of times.  
- `levenshtein_correct` - This is the results of the arrays that are created by electing the array with the shortest levenshtein distance to the majority among the samples.  
- `most_majority_correct` - This is the results of the arrays that are created by electing the array with the most elements from the majority array among the samples.
- `max_size_correct` - This is the results of the arrays that are created by electing the array with the maximum size among the samples.
- `sample_average` - The average across all samples.
### **Results**:
#### **DRAW-1K**
![image](https://github.com/hwelsters/wisdom-of-crowd-llm/assets/84760072/d9176a89-8d49-4026-bbe4-0c02ff43a68e)
  
#### **ALG-514**
![image](https://github.com/hwelsters/wisdom-of-crowd-llm/assets/84760072/071d6c33-c1b5-41b5-bf21-835c4953b683)
  
#### **NLU-ASDIV**
![image](https://github.com/hwelsters/wisdom-of-crowd-llm/assets/84760072/82c83e73-c247-4234-a082-2db591c1f60c)
