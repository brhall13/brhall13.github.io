---
layout: default
title:  "Streamlining Transcription with Azure and OpenAI"
date:   2024-10-15 15:07:10 -0400
categories: ChatGPT Parser
excerpt: One of the key areas where technology...
tag: LLM
---

# Streamlining Transcription with Azure and OpenAI
## Introduction
One of the key areas where technology can offer significant assistance is in automating the transcription process, whether in something as large as call centers or the smallest of businesses. I am developing a program using Azure and OpenAI to receive, summarize, and categorize transcriptions from an Azure Queue, and store them in Azure Table Storage. In a follow up post, I will break down how to use OpenAI's Whisper model to transcribe audio into text that can be slotted into this program's workflow.

## Receiving Messages from Azure Queue
The function receive_queue connects to the Azure Queue and processes messages one at a time. Each message is decoded and then summarized using an OpenAI model before being written to Azure Table Storage. Using a queue you can control the rate and amount of data you are sending to OpenAI at once. Additionally, queues handle failures well, returning any failed messages back to the queue.

## Defining Enums and Models
Previously, ensuring that OpenAI returned expected data types required JSON formatting and strict prompting. Now, with [structured output](https://platform.openai.com/docs/guides/structured-outputs/introduction), you can simply point the model towards a response format. In this case I created a class to ensure an output containing a summary (string) and a predefined category (enum). Because the model will adhere to the given format, you can insert into a database with much higher confidence that the data type is correct.

```
class CategoryEnum(Enum):
    NETWORKING= "Networking"
    FOLLOW_UP= "Follow-up"
    SOFTWARE= "Software"
    LOGIN= "Login"
    ACCESS= "Access"
    BILLING= "Billing"
    OTHER= "Other"

class TranscriptSummary(BaseModel):
    summary : str
    category : CategoryEnum
```

## Generating a Summary
Because of structured output handling the categorization and formatting of the outputs, the system prompt needed is quite short. I opted to instruct it to keep summaries short and make sure that key points are covered. The rest can be handled by setting the category Enums to be inline with expected/needed data. Let's take a short example transcript and return a summary.

### Example Input
<span style="color:blue">"Hi, I'm having trouble accessing my account. I tried resetting my password, but the link in the email doesn't work. Can you help me reset it?</span>

<span style="color:green">Absolutely, I can help with that. Could you please provide me with your email address and the last four digits of your phone number for verification?</span>

<span style="color:blue">It's johndoe@example.com, and the last four digits are 1234.</span>

<span style="color:green">Great, I’ve verified your information. I’m sending you a new password reset link now. Please check your email and let me know if you receive it.</span>

<span style="color:blue">Got it! Opening the email now... The link worked this time! I’m resetting my password. Thanks a lot for your help!</span>

<span style="color:green">You’re welcome! Is there anything else I can assist you with today?</span>

<span style="color:blue">No, that’s all. Thanks again!"</span>

### Example Output
Summary: "The user faced issues accessing their account and requested help resetting their password. After providing their email and phone verification, the support representative sent a new password reset link, which worked successfully for the user."

Category: "Access"


## Writing to Azure Table Storage
Once you have a summary returned from the API its straightforward to store it. I chose Azure Table Storage as it is immensely scalable and cost effective. Since the API also returns a categorization, you can index and sort as needed as well as add any future categories to the category class. That being said, I made sure to add a uuid to each summary to act as a row key in the table. 
```
def write_to_table(summary):
    """
    Writes a summary entity to an Azure Table Storage.
    """
    ROW_KEY = str(uuid.uuid4())
    PARTITION_KEY = 'Summary'

    my_entity = {
        'PartitionKey': PARTITION_KEY,
        'RowKey': ROW_KEY,
        'Summary': summary.summary,
        'Category': summary.category.value,  # .value to get the text of the Enum
    }

    # Values from .env
    table_service_client = TableServiceClient.from_connection_string(conn_str="DefaultEndpointsProtocol=https;AccountName=example;cAccountKey=examplekey==;EndpointSuffix=core.windows.net")
    table_client = table_service_client.get_table_client(table_name="EXAMPLE")

    entity = table_client.create_entity(entity=my_entity)

    print(f"Entity created: {entity}")
``` 