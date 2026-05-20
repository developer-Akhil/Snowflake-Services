**CLASSIFY_TEXT()**

CLASSIFY_TEXT() is an AI function in Snowflake Cortex AI used to automatically classify or categorize text into predefined labels using Large Language Models (LLMs).
Serverless SQL function in Snowflake Cortex that uses large language models (LLMs) to automatically categorize text into predefined categories.

It helps identify:\
•	sentiment \
•	intent \
•	topic \
•	category \
•	priority \
•	spam/non-spam \
•	business classification \
without building a custom ML model.

CLASSIFY_TEXT() takes:
1.	Input text 
2.	A list of categories 
and returns the most appropriate category.

**Does it require model training?**
Answer:
•	No. 
•	Uses zero-shot classification.

**Syntax**
```
SNOWFLAKE.CORTEX.CLASSIFY_TEXT(
    '<text>',
    ARRAY_CONSTRUCT('<label1>', '<label2>', ...)
)
```

**Example 1 — Customer Support Ticket Classification**

```
SELECT SNOWFLAKE.CORTEX.CLASSIFY_TEXT(
    'My payment failed during checkout',
    ARRAY_CONSTRUCT(
        'Billing',
        'Technical Support',
        'Account Issue'
    )
);
```

**Output**\
Billing

**Example 2 — Email Intent Detection**

```
SELECT SNOWFLAKE.CORTEX.CLASSIFY_TEXT(
    'I want to cancel my subscription',
    ARRAY_CONSTRUCT(
        'Cancellation',
        'Upgrade',
        'Complaint',
        'Inquiry'
    )
);
```

**Output:**\
Cancellation
