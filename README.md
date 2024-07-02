# MicrosoftGraphRAG
## Using Microsoft GraphRAG + ChatGPT, we index the book "A Christmas Carol" and ask complex relationship questions about the text  
The instructions on the Microsoft site are mostly complete, as is tradition, so here are the corrections that actually make it work.
NOTE: ChatGPT 4 Turbo is pricey, and indexing this book cost about $5-$10 and took about 20 minutes.

### Step 1: Create a new folder ```MicrosoftGraphRAG``` and a folder in that called ```input```  

### Step 2: Save your book in UTF-8 encoded in your input folder
I downloaded the 'A Christmas Carol' book in my web browser from https://www.gutenberg.org/cache/epub/24022/pg24022.txt and copy/pasted into Notepad and saved as UTF-8 encoding.  The instructions on the Microsoft just saved the 200 OK output of the call rather than the file itself.
NOTE: Notepad++ did not save the txt file as UTF-8 that the indexer recognized, and running the indexer would get this error 
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


 
