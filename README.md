# Langflow-Chatbot
Combine MULTIPLE LLMs to build an AI API! (super simple!!!) Langflow | LangChain | Groq | OpenAI

Tutorial from Ania Kubow
https://www.youtube.com/watch?v=5lFChDglhmI&t=135s

- Create DB(Serverless Vector) at https://astra.datastax.com/ with name: "multillm"

In VSCode:
- Create >Python3.10 Envirenment
- python -m pip install langflow -U
- python -m langflow run

Create Langflow -> Field##Groups:         -> Connections <>: 
search for -:
##DB Ingestion (Create and store vector embeddings from files)
- file (Database):  -> File         -> Upload .csv
- split:            -> Split Text   -> File <> DataInputs(*Split text)   -> Split Text Chunks <> IngestData(*AstraDB)
- llm(openai) embedd-> LLM Embedd.  -> Embeddings <> Embedding or Astra Vectorize
- db(astra):        -> AstraDB
  *(milvus)         -> *(Milvus = OpenSource/forFREE)
##Main-Chain (Main Proccess from Input to Output)
- chat input:       -> Chat Input   -> Message <> SearchInput(*2xAstraDB)
- 2x(llm) embedding:-> 2xLLM Embedd.-> Embeddings <> 2x Embedding or Astra Vectorize
- parse data:       -> Parse Data   -> Search Results(*2xAstraDB) <> Data(*Parse Data) 
- prompt:           -> Prompt       -> Edit:    
                                                Below is the context: {context}

                                                ---

                                                Given the context above answer the users question.
                                                Question: {question}
                                                Answer:
- openai:           -> OpenAI       -> PromptMessage(*prompt) <> Input
-> Message(*Chat Input) <> Question(*Prompt)
##URL Scraping (Search whole websites for results)
- url:              -> URL
- 2x parse data:    -> 2x Parse Data-> Data(*URL) <> Data(*2x Parse Data)   -> Text(*2x Parse Data) <> context(*Prompt)
                                                                            -> Edit Prompt: Below is the website data about the course: {website}
                                                                            ---

#Playground Output (Chat the process in the Playground of Langflow)
- chat output:      -> Chat Output  -> Text(*OpenAI) <> Text(*Chat Output)
#Chat Memory (Store the chat history)
- memory:           -> Memory       -> Edit Prompt: ---
                                                    {memory}
                                    -> Messages <> memory(*Prompt)
#Groq (Groq API instead LLM)
- groq:             -> Groq         -> Groq API-Key at: https://console.groq.com/keys
                                    -> PrompMessage(*Prompt) <> Text(*Groq)&&Text <> Text(*Chat Output)
