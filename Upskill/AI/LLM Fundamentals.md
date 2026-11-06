### LLM Model
A model is basically made up of a weights/parameter file and a small model runner file. 
Imagine a massive spreadsheet of knobs, each set to some specific number, tuned during training so that turning all the knobs together and feeding in text produces sensible next-word predictions. 
That spreadsheet of numbers is the model. Nothing mystical — just numbers.

### Tokens
[Refer Matt Pocock](https://www.youtube.com/watch?v=nKSk_TiR8YA)

 We know that models are charged by tokens:
 
 ![[Pasted image 20260920160441.png|550]]

Input tokens are split into tokens, then each token is encoded into numbers and LLM work on those numbers. Once it is done, it produces tokens back and decodes the output tokens and combines them into the output.

Token conversion depends on the model and different models split the text into tokens differently. It depends on the data the model is trained on.

This happens during tokenization training, where the tokenizer is fed the text and then it groups commonly occuring characters together and store them as tokens.

### Context Window
A context window is the number of token used in a conversation adding input and output tokens.

A bloated context window impacts performance and consumes more memory.

![[Pasted image 20260920160812.png|442]]

There is something called lost in the middle problem, that states that performance is bad in the middle of the context window. 

Also, some mcp servers, large rules can also bloat the context window.
It is better to keep on clearing or compacting the context window for a better performance.

### Temperature
a randomness dial. 

Low = model always picks the most likely next token (boring but consistent); 
high = it sometimes picks a less likely one (more creative, less predictable).

### Pre-training model
So the first phase is to create the pretraining model, which helps us predicting the next word. This is an expensive process and takes a lot of money.

When we download a model, this is what we get. 
It contains a file which has a lot of parameters. File size could be in GBs.

For example, we use internet data for training this.

### Fine tuning
We take the above model and tune it according to our use case. 

This is also a training and we can use more specialised data for this. This is less expensive and we can run it more frequently.

Fine tuning, can also provide multiple outputs to the user and then they can select the most suitable one. This was it also gets feedback.
Note that, fine tuning changes the weights in the original parameter file.

### Self improvement model
For a limited context, we can train the model on the its own data. For example, model for playing Go. 
We can use all the games played by humans till now to train the model. In that case our model will be limited to the best human player.

But in case we let the system play its own game and keep on taking the feedback from that, we can create a model which is not limited by the human skills.

### RAG
Retrieval augmented generation. [Link: YT](https://www.youtube.com/watch?v=_HQ2H_0Ayy0)

So, you embed extra info in your prompt so your model can answer using this data. This model is called RAG.

- Ingest: Ingest the data that you want your model to work on, like documents, runbooks, logs etc. Data is stored in vector dbs. This is where chunking takes place. We need to carefully select a chunking strategy here.
- Retrieve: We retrieve chunks from the vector db that was created in then ingest step. We create a retrieval strategy here, which gets the chunks with the similarity values. We also enable semantic search here in the db.
- Augment: Augment the embeddings in the user prompt.
- Generate: LLM generates response.

Using RAGs we do not need to change the base model. RAGs are also faster to create than fine-tuning.

#### a bit about vector db
query:
- Find me the chunks which are similar to this query (nearst neighbours search)

### Chunking Strategies

Chunks too small we can lose the context, too large, we will use too many tokens.

Lost in the middle problem with using too much context.

Chunking helps with
1. good retrieval quality
2. helps reduce hallucination
3. helps manage context window
4. Helps with cost

| Chunking Strategy         | How It Works                                                                                                 | Complexity  | Best For                                                                                      | Examples                                                           |
| ------------------------- | ------------------------------------------------------------------------------------------------------------ | ----------- | --------------------------------------------------------------------------------------------- | ------------------------------------------------------------------ |
| **Fixed-Size (or Token)** | Splits by token or character count.                                                                          | Low         | Small or simple docs, or when speed matters most                                              | Meeting notes, short blog posts, emails, simple FAQs               |
| **Recursive**             | Splits text by repeatedly dividing it until it fits the desired chunk size, often preserving some structure. | Medium      | Documents where some structure should be maintained but speed is still important              | Research articles, product guides, short reports                   |
| **Document-Based**        | Treats each document as a single chunk or splits only at document boundaries.                                | Low         | Collections of short, standalone documents                                                    | News articles, customer support tickets, short contracts           |
| **Semantic**              | Splits text at natural meaning boundaries (topics, ideas).                                                   | Medium-High | Technical, academic, or narrative documents                                                   | Scientific papers, textbooks, novels, whitepapers                  |
| **LLM-Based**             | Uses a language model to decide chunk boundaries based on context, meaning, or task needs.                   | High        | Complex text where meaning-aware chunking improves downstream tasks like summarization or Q&A | Long reports, legal opinions, medical records                      |
| **Agentic**               | Lets an AI agent decide how to split based on meaning and structure.                                         | Very High   | Complex, nuanced documents that require custom strategies                                     | Regulatory filings, multi-section contracts, corporate policies    |
| **Late**                  | Embeds the whole document first, then derives chunk embeddings from it.                                      | High        | Use cases where chunks need awareness of full document context                                | Case studies, comprehensive manuals, long-form analysis reports    |
| **Hierarchical**          | Breaks text into multiple levels (sections → paragraphs → sentences). Keeps structure intact.                | Medium      | Large, structured docs like manuals, reports, or contracts                                    | Employee handbooks, government regulations, software documentation |
| **Adaptive**              | Adjusts chunk size and overlap dynamically with ML or heuristics.                                            | High        | Mixed datasets with varying structures and lengths                                            | Data from multiple sources: blogs, PDFs, emails, technical docs    |
| **Code**                  | Splits by logical code blocks (functions, classes, modules) while preserving syntax.                         | Medium      | Source code, scripts, or programming documentation                                            | Python modules, JavaScript projects, API docs, Jupyter notebooks   |

