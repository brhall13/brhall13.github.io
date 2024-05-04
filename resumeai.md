## App 
- Talk about some functionality and why I chose it 
- Talk briefly about non AI features 
- Transition into talking about GPT Technologies Used - Azure Blob Storage: for handling PDF resumes. 
- Azure Functions: for triggering Python functions. - GPT-3.5 Turbo: for summarizing resume content and exporting to JSON. 
- Text-embedding-small-003: for text vectorization 
- Pinecone: for storing and searching vectorized resume data. 

- How the app works 

1. The PDF resume is uploaded to Azure Blob Storage.
2. An Azure Function is triggered by the file upload, extracts the text from the PDF using PyPDF2, and passes the text to ChatGPT.
3. GPT processes the text following the examples given in the prompt, summarizes it, and converts it to the defined JSON format.  
4. The JSON document is vectorized using OpenAI's text-embedding-small-003 and stored in the Pinecone database. 

(insert architecture image here)
## Prompt


 ## GPT 
- Ive made multiple chat bots at this point 
- Demonstrate knowledge of building and calling API From helping me write code to advising tone in this blog, OpenAI's GPT has been an invaluable resource, especially after I discovered the wonders of JSON outputs. I also refined my prompts throughout this process and have learned some helpful insights. 
- Few shots for consistent output (highlight importance for an app like this) - JSON output allowing for outputs that are easily and consistently accessible