## Prompt for Genrate Q&A pairs

```
For both the teacher and student models, we used the same prompt instructing them to generate up to three Q&A pairs, with variable parameters in the prompt assigned in part by the results of the chunk classification. The prompt is shown below:

根据给定文本，创建问答对，问题要尽可能专业具体，结合你自身拥有的知识和给出文本中的原文将答案加工，在保持文本原意的情况下以提高答案细节清晰度和多样化分析过程。答案的内容要丰富，需要有组织、条理性地阐述，以层次结构展现，答案长度要尽可能的长、详细。

给定文本属于{chunk\_type}类别，考虑从[{labels\_dict[chunk\_type]}]几个方面生成问题。

注意：

1.给定的文本有可能不适合生成问答对，如果不适合生成问答对请直接返回“基于给定文本不适合生成问答对”。

2.务必不要生成类似“本书的主要内容是”、“根据文本内容”、“书中的XXX”、“XX机构是怎么做的”类似的问题，即保证生成的问答对是可以脱离给定文本独立存在的具体知识性问题，如果不能请直接返回“基于给定文本不适合生成问答对”。

3.每次最多生成三个不相同的问答对，如果给定文本的内容不足以生成三个多样性强的问答对就停止，不要强行生成。

4.问题应该从老人或照护者的角度发起。

格式如下：

问题：...答案：...

你要处理的文本是：
```



***

```
Translation:

Based on the given text, create a Q&A pair with questions that are as specialized as possible, combining your knowledge and the original text in the given text to process the answers to improve the clarity of details and diversify the analysis process while maintaining the original meaning of the text. The answer should be rich in content, organized and structured, presented in a hierarchical structure, and as long and detailed as possible.

The given text belongs to the category {chunk_type}, consider generating questions from several aspects of [{labels_dict[chunk_type]}].
Note:

1.The given text may not be suitable for generating Q&A pairs, if it is not suitable for generating Q&A pairs, please return directly to "Not suitable for generating Q&A pairs based on the given text".


2.It is important not to generate questions like "The main content of this book is", "According to the content of the text", "XXX in the book", or "XX organizations are How to do" similar questions, that is, to ensure that the generated Q&A pairs can be detached from the given text independent of the existence of specific knowledge questions, if not, please return directly to "Not suitable for generating Q&A pairs based on the given text".

3.Generate up to three different pairs at a time, and stop if the content of the given text is insufficient to generate more diverse pairs, do not force the generation.

4.Questions should be initiated from the perspective of an older person or caregiver.

The format is as follows:

Question: ... Answer: ...

The chunk is:
```

***

## Prompt for GPT-4o Evaluation for Generating Q&A Pairs

```
The prompt for GPT-4o Evaluation for generating Q&A pairs is shown below:

现有两个assistant负责针对给定文段生成养老照护相关的问答对(老年医学、老年护理、老年养生、老年心理学、老年饮食等)，这些问答对将被用作训练专业的养老照护大语言模型，使得其具备更专业的养老照护知识以供老年人及照护者使用。

评估要求及注意事项：

1.生成的问题及答案行文流畅，且问题和答案必须能够脱离文段独立存在（即不能出现针对具体文段的内容的问答，如特定的人发生了什么、特定的机构、特定的地点发生了什么、依据或根据给定文段内容等），问答必须与老年照护相关，具有实际意义。

2.打分评估两个assistant生成结果的质量，满分为10分，从问题是否独立、语言是否流畅无语病、答案是否准确易懂等角度考虑，务必客观评价，打分要有区分度，如果assistant的生成结果不佳请直接打低分，谨慎给出较高的分数。

3.注意，模型有可能会生成“基于给定文本不适合生成问答对”这一结果，这并不意味着模型能力较差，需要结合具体文段考虑是否确实不适合生成满足要求的养老照护问答对。

4.每个assistant最多会针对每个文段生成三个问答对，评分要综合考虑所有问答对的质量。

使用如下的JSON格式回复： [{"name": "assistant1", "assessment": assessment, "score": score},{"name": "assistant2", "assessment": assessment, "score": score}]

给定问答对如下：

文段:{chunk}

assistant1生成的内容:{assistant1}

assistant2生成的内容:{assistant2}

你的评分是:
```

***

```
Translation:

Two assistants are responsible for generating elderly healthcare-related Q&A pairs (geriatrics diseases, elderly nursing, elderly mental health, elderly wellness, nutrition for the elderly, etc.) for a given passage, which will be used to train a professional elderly healthcare large language model with more specialized knowledge of elderly healthcare for the use of older adults and caregivers.

Assessment Requirements and Notes:

1. The questions and answers must be fluent and independent of the passage (i.e., there can be no passage-specific questions and answers, such as what happened to a specific person, a specific organization, what happened at a specific location, based on or by the content of a given passage, etc.), and the questions and answers must be relevant to elderly healthcare and have practical significance.

2. Scoring to assess the quality of the two assistants' generation, full of 10 points, from the point of view of whether the question is independent, whether the language is fluent without speech defects, whether the answer is accurate and easy to understand, etc., be sure to objectively evaluate, scoring to be differentiated, if the assistants' generation of results is not good, please directly hit the low score, be careful to give a higher score.

3. Note that the model may generate "Not suitable for generating Q&A pairs based on given text" This result, which does not mean that the model's ability is poor, needs to be considered in conjunction with the specific paragraph whether it is indeed not suitable for the generation of elderly healthcare to meet the requirements of the Q&A pairs.

4. Each assistant will generate a maximum of three pairs of questions and answers for each paragraph, the score should take into account the quality of all pairs.


Reply using the following JSON format: [{"name": "assistant1", "assessment": assessment, "score": score},{"name": "assistant2", "assessment": assessment, "score": score}]

The given Q&A pairs are as follows:

text:{chunk}

assistant1 generated content:{assistant1}

assistant2 generated content:{assistant2}

Your rating is:
```

***

## Prompt for Data Reconstruction

```
The prompt for data reconstruction is shown below:

你是一个养老照护专家。充分利用以下问答对中包含的知识，创建一个更完整的老年照护相关的问题并详尽的回答。若问答对不适合创建问题，请回答“给定问答对不适合创建问题”。注意，生成新问题的时候务必尽量多的用到给定问答对中的原文，回答时要分条、清晰，注意细节。

给定问答对如下：
```

***

```
Translation:

You are an elderly healthcare expert. Utilize the knowledge contained in the following Q&A pairs to create a more complete senior care-related question and answer it thoroughly. If the question and answer pairs are not suitable for creating a question, please answer "The given Q&A pair is not suitable for creating a question". Note that it is important to use as much of the original text from the given pair as possible when generating a new question, and to answer it in a clear and organized manner, with attention to detail.

The given Q&A pair is as follows:
```

***

## Prompt for GPT-4o Evaluation for Subjective Questions

```
The prompt for GPT-4o evaluation for subjective questions is shown below:

评价两个Assistant针对养老照护问题生成的答案质量，分析哪个模型生成的答案质量更好，或质量相当。

评价时，考虑以下因素：

1.安全性（确保回答不包含可能对用户造成伤害或风险的信息。）
        
2.准确性（准确解释问题并利用专业知识提供正确和相关的答案。）
        
3.流畅性（生成连贯、清晰和流畅的回答以提升用户的整体体验。）
        
注意，两个答案的顺序与质量无关，要按照上述因素合理分析评价，尽量有区分度的去评价。
        
请严格按照以下格式回答，先分析，后给出结论，结论不要有多余内容。：
        
分析：...
        
结论：(Assistant 1 is 'better than', 'worse than', or 'equal to' Assistant 2.)

给定问答对如下：
```

***

```
Translation:

Evaluate the quality of the answers generated by the two assistants for the elderly healthcare question and analyze which model generates better or comparable quality answers.

The evaluation takes into account the following factors:


1. Safety (Ensuring that responses do not contain any information that could potentially cause harm or pose risks to the user.)
        
2. Accuracy (Accurately interpreting the questions and utilizing specialized knowledge to provide correct and relevant answers.)
        
3. Fluency (Generating responses that are cohesive, clear, and fluent, enhancing the overall user experience.)
        
Note that the order of the two answers has nothing to do with the quality, it should be analyzed and evaluated reasonably by the above factors, and try to have a differentiation to evaluate.
        
Please answer strictly by the following format, first analyze, and then give the conclusion, the conclusion should not have redundant content.
        
Analysis:...
        
Conclusion: (Assistant 1 is 'better than', 'worse than', or 'equal to' Assistant 2.)
```

***

## Human Evaluation Notes

```
Our human evaluation notes are listed below:

1. 准确专业性：Assistant 的回答是否准确理解了问题，并且清晰明了地给出答 案，答案的准确性如何。 

2. 流畅性：语义是否连贯，有无逻辑错误，态度是否友好，用词是否恰当。 

3. 重要程度：准确专业性 > 流畅性。 

注：忽略Assistant生成答案中的“**”、“-”这些内容，这些内容实际上是展示给用户界面的强调符号，与内容无关。有极小部分Assistant生成的内容可能与问题无关，或者突然“胡言乱语”，例如出现乱七八糟的英语，这是质量差的表现。

填写标准：“准确专业性”和“流畅性”两栏可简单填写理由，评估结果一栏填写最终的结果——如果 Assistant1 的 回答比较好，则填写“1”，如果 Assistant2 的回答比较好，则填写“2”。先尽量有区分的评价，如果两者的质量相似，则填写“3”。两个回答的质量与顺序无关。
```

***

```
Translation:

1. Accuracy and professionalism: Whether the Assistant's answer understands the question accurately and gives the answer clearly and concisely. 

2. Fluency: whether the semantics are coherent, whether there are logical errors, whether the attitude is friendly, and whether the wording is appropriate. 

3. Importance: accuracy professionalism > fluency. 

Note: Ignore the "**" and "-" in the answers generated by Assistant, which are emphasized symbols shown to the user interface and have nothing to do with the content. A very small portion of the content generated by the Assistant may not be related to the question, or suddenly "gibberish", such as the appearance of garbled English, which is a sign of poor quality.

Fill in the criteria for : "Accuracy and professionalism" and "fluency" can be filled in with simple reasons, and the final result can be filled in for the evaluation result - if the answer of Assistant1 is better, then the result will be the same. If the answer of Assistant1 is better, fill in "1", if the answer of Assistant2 is better, fill in "2". Evaluate as differentiated as possible first, and fill in "3" if the quality of both is similar. The quality of the two responses is irrelevant to the order. 
```

***

## Training Settings

![image-20240816000123607](C:\Users\XiaoHe\AppData\Roaming\Typora\typora-user-images\image-20240816000123607.png)

***

## Word Cloud Map (Chinese Version)

![image-20240816000202209](C:\Users\XiaoHe\AppData\Roaming\Typora\typora-user-images\image-20240816000202209.png)