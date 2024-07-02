# MicrosoftGraphRAG
## Using Microsoft GraphRAG + ChatGPT, we index the book "A Christmas Carol" and ask complex relationship questions about the text  
The instructions on the Microsoft site are mostly complete, as is tradition, so here are the corrections that actually make it work.  
NOTE: ChatGPT 4 Turbo is pricey, and indexing this book cost about $5-$10 and took about 10-15 minutes.  
I've already indexed the book in this repo, so you should be able to start asking questions straight away and save some time/money.  

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

### Step 5: Ask questions
Now that you've indexed the document into the graph, you can ask it questions through ChatGpt.  
Using the graphrag.query the first time generates a lancedb stored in the directory which is uses for any further questions.  

#### Global questions

Use a global search for high-level questions:  
```
python -m graphrag.query --root ./ --method global "What are the top themes in this story?"
```

The AI still struggles with "Why?" questions:  
```
python -m graphrag.query --root ./ --method global "Why did Scrooge buy the Cratchit family a goose?"
```
Response:  
```
SUCCESS: Global Search Response: I am sorry but I am unable to answer this question given the provided data.
```
![image](https://github.com/rcorvus/MicrosoftGraphRAG/assets/5025458/7b087e80-1084-4120-bd52-cbcb7b396f6d)  

#### Local questions  
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

