# Microsoft GraphRAG + ChatGPT 4 Turbo
## Using Microsoft GraphRAG + ChatGPT, we index the book "A Christmas Carol" and ask complex relationship questions about the text  
As you can see in the responses below, the AI system is able to make surprisingly insightful responses, worthy of a book critic, including citations. However, it still struggles with the "Why?" questions.  It can even make deductions that are never explicitly spelled out in the text, such as knowing that Tiny Tim's last name must be Cratchit.  
The instructions on the Microsoft site are **mostly** complete, as is tradition :-), so I've included the the corrections below that actually make it work.  
NOTE: ChatGPT 4 Turbo is pricey, and indexing this book cost about $5-$10 and took about 10-15 minutes.  
I've already indexed the book in this repo, so you should be able to start asking questions straight away and save some time/money.  

## Index the book  
### Step 1: Create a new folder ```MicrosoftGraphRAG``` and a folder in that called ```input```  

### Step 2: Save your book in UTF-8 encoded in your input folder
I downloaded the 'A Christmas Carol' book in my web browser from https://www.gutenberg.org/cache/epub/24022/pg24022.txt and copy/pasted into Notepad and saved as UTF-8 encoding.  The instructions on the Microsoft just saved the 200 OK output of the call rather than the file itself.  
NOTE: Notepad++ did not save the txt file as UTF-8 that the indexer recognized, and running the indexer would get this error:  
```
File "<frozen codecs>", line 322, in decode
UnicodeDecodeError: 'utf-8' codec can't decode byte 0xff in position 0: invalid start byte
⠋ GraphRAG Indexer
```
The quick solution was to use Notepad to save the book and ensure it was saving the txt file as UTF-8.  

### Step 3: Initialize the indexing pipeline  
In the MicrosoftGraphRAG dir in your terminal run ```python -m graphrag.index --init --root ./```  
This automatically generates the following folders and files:  
![image](https://github.com/rcorvus/MicrosoftGraphRAG/assets/5025458/62a8f621-e89a-435b-a26a-b79f08070da6)  

In ```.env``` change ```GRAPHRAG_API_KEY=<API_KEY>``` to your OpenAI key.  

### Step 4: Run the indexing pipeline
Run the indexing pipeline, "A Christmas Carol" took about 10-15 minutes on my laptop with the default settings (compared to "Dune" which took about an hour):   
``` python -m graphrag.index --root ./ ```

You get a very long output explaining in detail what occurred.

## Ask questions
Now that you've indexed the document into the graph, you can ask it questions through ChatGpt.  
Using the graphrag.query the first time generates a lancedb stored in the directory which is uses for any further questions.  

### Global questions

Use a global search for high-level questions and it can provide incredibly insightful responses worthy of a book critic, including citations:  
```
python -m graphrag.query --root ./ --method global "What are the top themes in this story?"
```
Response:  
```
### Top Themes in the Story

The story we're examining is rich with themes that delve into human nature, societal values, and the potential for personal transformation. Below, we explore the most significant themes that stand out in the narrative.

#### Transformation and Redemption

At the heart of the story is the profound transformation and redemption of Ebenezer Scrooge. Initially portrayed as a miserly and isolated figure, Scrooge undergoes a remarkable journey to become a generous and compassionate individual. This change is not superficial but touches the core of his being, affecting how he views the world and interacts with those around him. The narrative meticulously charts Scrooge's evolution, making it a central pillar of the story [Data: Reports (24, 25, 23, 21, 14, +more)].

#### The Importance of Family and Social Connections

The narrative places a strong emphasis on the value of family and social connections, particularly through Scrooge's interactions with characters like Bob Cratchit and his family. These interactions are pivotal, showcasing a shift in Scrooge's attitude from indifference to a deep sense of care and responsibility towards others. The transformation in his relationship with the Cratchit family serves as a microcosm of his overall change, highlighting the theme's importance in the story [Data: Reports (10, 9, 14, 16, +more)].

#### The Spirit of Christmas as a Catalyst

The spirit of Christmas acts as a powerful catalyst for change, both for Scrooge and the broader community. The holiday's emphasis on generosity, compassion, and community celebration is woven throughout the narrative, serving as a backdrop for Scrooge's personal transformation. This theme underscores the potential of the Christmas spirit to inspire change and foster a sense of unity and goodwill among people [Data: Reports (18, 21, 23, 25, +more)].

#### Impact of Personal Choices

The story explores the impact of personal choices on both the individual and those around them. Through Scrooge's reflections on his past, present, and future, guided by the spirits, the narrative delves into how one's actions can profoundly affect others. This theme is a critical examination of responsibility and the interconnectedness of human lives, emphasizing the importance of making choices that are beneficial not just for oneself but for the community at large [Data: Reports (24, 25, 7, 13, +more)].

#### Generosity and Compassion

Finally, the virtues of generosity and compassion are highlighted as essential qualities for a fulfilling life. Scrooge's transformation is marked by his newfound willingness to support and improve the lives of others, exemplified by his actions towards the Cratchit family. This theme is a testament to the story's belief in the inherent goodness of people and the idea that positive change is always possible [Data: Reports (24, 10, 14, 21, +more)].

In conclusion, the story weaves together these themes to offer a rich tapestry of lessons on human nature, the power of change, and the importance of compassion and community. Through the character of Ebenezer Scrooge, it presents a hopeful narrative that suggests no one is beyond redemption, and that the spirit of Christmas can be a powerful force for good in the world.
```
![image](https://github.com/rcorvus/MicrosoftGraphRAG/assets/5025458/92b0cad4-81a2-476c-840d-2047ab7c999a)


However, the AI still struggles with "Why?" questions:  
```
python -m graphrag.query --root ./ --method global "Why did Scrooge buy the Cratchit family a goose?"
```
Response:  
```
SUCCESS: Global Search Response: I am sorry but I am unable to answer this question given the provided data.
```
![image](https://github.com/rcorvus/MicrosoftGraphRAG/assets/5025458/7b087e80-1084-4120-bd52-cbcb7b396f6d)  

### Local questions  
Use a local search for more specific questions about a specific character.

For example, the AI is able to deduce that Tiny Tim's last name must be Cratchit, making him "Tiny Tim Cratchit":  
```
python -m graphrag.query --root ./ --method local "Who is Tiny Tim, what is his last name, and what are his main relationships?"
```

```
Tiny Tim is a pivotal character in the classic narrative, known for his optimistic outlook and the famous line he utters, symbolizing hope and goodwill.  

He is a member of the Cratchit family, which provides him with his last name, making him Tiny Tim Cratchit.  

His character is central to the story's themes of redemption, the importance of family, and kindness.
```  
 ![image](https://github.com/rcorvus/MicrosoftGraphRAG/assets/5025458/0f1c0f1a-41ce-4472-aaa2-69e1d15f02c6)

