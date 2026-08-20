LangChain is an open source orchestration framework for application development using [large language models (LLMs)](https://www.ibm.com/think/topics/open-source-llms). Available in both Python- and Javascript-based libraries, LangChain’s tools and [APIs](https://www.ibm.com/think/topics/api) simplify the process of building LLM-driven applications like [chatbots](https://www.ibm.com/think/topics/chatbots) and [AI agents](https://www.ibm.com/think/topics/ai-agents). 

LangChain serves as a generic interface for nearly any LLM, providing a centralized development environment to build LLM applications and integrate them with external data sources and software workflows. LangChain’s module-based approach allows developers and data scientists to dynamically compare different prompts and even different [foundation models](https://research.ibm.com/blog/what-are-foundation-models) with minimal need to rewrite code. This modular environment also allows for programs that use multiple LLMs: for example, an application that uses one LLM to interpret user queries and another LLM to author a response.

Launched by Harrison Chase in October 2022, LangChain enjoyed a meteoric rise to prominence: as of June 2023, it was the single fastest-growing [open source](https://www.ibm.com/think/topics/open-source) project on Github. 

Coinciding with the momentous launch of OpenAI’s [ChatGPT](https://www.ibm.com/think/topics/chatgpt) the following month, LangChain has played a significant role in making [generative AI](https://www.ibm.com/think/insights/generative-ai-benefits) (genAI) more accessible to enthusiasts and startups in the wake of its widespread popularity. Advancements in accessibility for agentic AI are currently enabling a revolution in [automation](https://www.ibm.com/think/topics/workflow-automation).

LangChain can facilitate most use cases for LLMs and [natural language processing (NLP)](https://www.ibm.com/solutions/natural-language-processing), like chatbots, [intelligent search](https://www.ibm.com/think/topics/intelligent-search), [question-answering](https://www.ibm.com/think/topics/question-answering), [summarization](https://www.ibm.com/think/topics/text-summarization) services or even [AI agents](https://www.ibm.com/think/topics/ai-agents) capable of [robotic process automation](https://www.ibm.com/think/topics/rpa).
### Integrations with LLMs

LLMs are not standalone applications: they are pre-trained statistical models that must be paired with an application (and, in some cases, specific data sources) in order to meet their purpose.

For example, Chat-GPT is not an LLM: it is a chatbot application that, depending on the version you’ve chosen, uses the GPT-3.5 or GPT-4 language model. While it’s the GPT model that interprets the user’s input and composes a natural language response, it’s the application that (among other things) provides an interface for the user to type and read and a UX design that governs the chatbot experience. Even at the enterprise level, Chat-GPT is not the only application using the GPT model: Microsoft uses GPT-4 to power Bing Chat.

Furthermore, though foundation models (like those powering LLMs) are pre-trained on massive [datasets](https://www.ibm.com/think/topics/dataset), they are not omniscient. If a particular task requires access to specific contextual information, like internal documentation or domain expertise, LLMs must be connected to those external data sources. Even if you simply want your model to reflect real-time awareness of current events, it requires external information: a model’s internal data is only up-to-date through the time period during which it was pre-trained.

Likewise, if a given generative AI task requires access to external software workflows—for example, if you wanted your virtual agent to [integrate with Slack](https://www.ibm.com/docs/en/watsonx/watson-orchestrate/base)—then you will need a way to integrate the LLM with the [API](https://www.ibm.com/think/topics/api) for that software.

While these integrations can generally be achieved with fully manual code, [orchestration](https://www.ibm.com/think/topics/llm-orchestration) frameworks such as LangChain and the [IBM watsonx](https://www.ibm.com/products/watsonx) portfolio of artificial intelligence products greatly simplify the process. They also make it much easier to experiment with different LLMs to compare results, as different models can be swapped in and out with minimal changes to code.

## How does LangChain work?

At LangChain’s core is a development environment that streamlines the programming of LLM applications through the use of _abstraction_: the simplification of code by representing one or more complex processes as a named component that encapsulates all of its constituent steps.

Abstractions are a common element of everyday life and language. For example, “_π_” allows us to represent the ratio of the length of a circle’s circumference to that of its diameter without having to write out its infinite digits. Similarly, a thermostat allows us to control the temperature in our home without needing to understand the complex circuitry this entails—we only need to know how different thermostat settings translate to different temperatures.

LangChain is essentially a library of abstractions for Python and Javascript, representing common steps and concepts necessary to work with language models. These modular components—like functions and object classes—serve as the building blocks of generative AI programs. They can be “_chained_” together to create applications, minimizing the amount of code and fine understanding required to execute complex NLP tasks. Though LangChain’s abstracted approach may limit the extent to which an expert programmer can finely customize an application, it empowers specialists and newcomers alike to quickly experiment and prototype.

### Importing language models

Nearly any LLM can be used in LangChain. Importing language models into LangChain is easy, provided you have an API key. The _LLM_ class is designed to provide a standard interface for all models.

Most LLM providers will require you to create an account in order to receive an API key. Some of these APIs—particularly those for proprietary closed-source models, like those offered by OpenAI or Anthropic—may have associated costs.

Many open source models, like Meta AI’s LLaMa, Deepseek's Deepseek-LLM, IBM's [Granite](https://www.ibm.com/granite) and Google’s Flan-T5, can be accessed through [Hugging Face](https://huggingface.co/models). [IBM watsonx](https://www.ibm.com/products/watsonx), through its [partnership with Hugging Face](https://developer.ibm.com/blogs/awb-hugging-face-and-ibm-working-together-in-open-source/), also offers a curated suite of open source models. Creating an account with either service will allow you to generate an API key for any of the models offered by that provider.

LangChain is not limited to out-of-the-box foundation models: the [_CustomLLM_ class](https://python.langchain.com/v0.1/docs/modules/model_io/llms/custom_llm/) allows for custom LLM wrappers. Likewise, you can use the [IBM watsonx APIs and Python SDK](https://dataplatform.cloud.ibm.com/docs/content/wsj/getting-started/wdp-apis.html?context=wx), which includes a LangChain integration, to build applications in LangChain with models that you’ve already trained or fine-tuned for your specific needs using the _WatsonxLLM_ class (and that model’s specific [project ID](https://www.ibm.com/docs/en/watsonx/saas)).

### Prompt templates

Prompts are the instructions given to an LLM. The “art” of composing prompts that effectively provide the context necessary for the LLM to interpret input and structure output in the way most useful to you is often called [prompt engineering](https://www.ibm.com/think/topics/prompt-engineering).

The _PromptTemplate_ class in LangChain formalizes the composition of prompts without the need to manually hard code context and queries. Important elements of a prompt are likewise entered as formal classes, like _input_variables_. A prompt template can thus contain and reproduce context, instructions (like “do not use technical terms”), a set of examples to guide its responses (in what is called “few-shot prompting”), a specified output format or a standardized question to be answered. You can save and name an effectively structured prompt template and easily reuse it as needed.

Though these elements can all be manually coded, _PromptTemplate_ modules empower smooth integration with other LangChain features, like the eponymous _chains._

### Chains

As its name implies, _chains_ are the core of LangChain’s workflows. They combine LLMs with other components, creating applications by executing a sequence of functions.

The most basic chain is LLMChain. It simply calls a model and prompt template for that model. For example, imagine you saved a prompt as “ExamplePrompt” and wanted to run it against Flan-T5. You can import LLMChain from langchain.chains, then define chain_example = LLMChain(llm = flan-t5, prompt = ExamplePrompt). To run the chain for a given input, you simply call chain_example.run(“input”).

To use the output of one function as the input for the next function, you can use _SimpleSequentialChain_. Each function could utilize different prompts, different tools, different parameters or even different models, depending on your specific needs.

### Indexes

To achieve certain tasks, LLMs will need access to specific external data sources not included in its training dataset, such as internal documents, emails or datasets. LangChain collectively refers to such external documentation as “indexes”.

#### Document loaders

LangChain offers a wide variety of [document loaders](https://python.langchain.com/v0.2/docs/integrations/document_loaders/) for third party applications. This allows for easy importation of data from sources like file storage services (like Dropbox, Google Drive and Microsoft OneDrive), web content (like YouTube, PubMed or specific URLs), collaboration tools (like Airtable, Trello, Figma and Notion), databases (like Pandas, MongoDB and Microsoft), among many others.

#### Vector databases

Unlike “traditional” structured databases, [vector databases](https://ibm.com/think/topics/vector-database) represent data points by converting them into _vector embeddings_: numerical representations in the form of vectors with a fixed number of dimensions, often clustering related data points using [unsupervised learning methods](https://www.ibm.com/think/topics/unsupervised-learning). This enables low latency queries, even for massive datasets, which greatly increases efficiency. [Vector embeddings](https://www.ibm.com/think/topics/vector-embedding) also store each vector’s metadata, further enhancing search possibilities.

LangChain provides integrations for over 25 different embedding methods, as well as for over 50 different vector stores (both cloud-hosted and local).

#### Text splitters 

To increase speed and reduce computational demands, it’s often wise to split large text documents into smaller pieces. LangChain’s _TextSplitters_ split text up into small, semantically meaningful chunks that can then be combined using methods and parameters of your choosing.

#### Retrieval

Once external sources of knowledge have been connected, the model must be able to quickly retrieve and integrate relevant information as needed. Like watsonx, LangChain offers [retrieval augmented generation (RAG)](https://www.ibm.com/think/topics/retrieval-augmented-generation): its _retriever_ modules accept a string query as an input and return a list of _Document_’s as output.

With LangChain, we can also [build agentic RAG systems](https://www.ibm.com/think/tutorials/agentic-rag). In traditional RAG applications, the LLM is provided with a vector database to reference when forming its responses. In contrast, agentic AI applications are not restricted to only data retrieval. [Agenic RAG](https://www.ibm.com/think/topics/agentic-rag) can also encompass tools for tasks such as solving mathematical calculations, writing emails, performing data analysis and more.

### Memory

LLMs, by default, do not have any long-term memory of previous interactions (unless that chat history is used as input for a query). LangChain solves this problem with simple utilities for adding memory to a system, with options ranging from retaining the entirety of all conversations to retaining a summarization of the conversation thus far to retaining the _n_ most recent exchanges.

### Tools

Despite their heralded power and versatility, LLMs have important limitations: namely, a lack of up-to-date information, a lack of domain-specific expertise and a general difficulty with math.

[LangChain _tools_](https://python.langchain.com/v0.2/docs/integrations/tools/) are a set of functions that empower LangChain agents to interact with real-world information in order to expand or improve the services it can provide. Examples of prominent pre-built LangChain tools include:  

- **Wolfram Alpha:** provides access to powerful computational and data visualization functions, enabling sophisticated mathematical capabilities.
    
- **Google Search:** provides access to Google Search, equipping applications and agents with real-time information.
    
- **OpenWeatherMap:** fetches weather information.
    
- **Wikipedia**: provides efficient access to information from Wikipedia articles.
    

## The latest tech news, backed by expert insights

Stay up to date on the most important—and intriguing—industry trends on AI, automation, data and beyond with the Think Newsletter, delivered twice weekly. 

First name*

Last name*

Business email*

Your subscription will be delivered in English. You will find an unsubscribe link in every newsletter. Refer to our [IBM Privacy Statement](https://www.ibm.com/us-en/privacy) for more information.

Submit

## LangChain agents

We can build an [agent](https://www.ibm.com/think/topics/ai-agents) with the LangChain framework to give an LLM the ability to make decisions, use tools and complete complex tasks step-by-step, rather than just generating a single text response. Unlike a simple prompt-response interaction with just an LLM, an agent powered by LangChain can [think](https://www.ibm.com/think/topics/agentic-reasoning), [plan](https://www.ibm.com/think/topics/ai-agent-planning), execute a sequence of actions, [learn](https://www.ibm.com/think/topics/ai-agent-learning) and adapt.

LangChain provides a streamlined user experience with a ready-made, extensible framework for creating AI agents, so there’s no need to build new tool selection logic, reasoning loops (such as for [ReAct agents](https://www.ibm.com/think/topics/react-agent)), observation/action tracking or prompt orchestration and formatting.

The specific LangChain packages, classes and methods vary depending on the AI platform you intend to use. Some key components of the _[WatsonxLLM](https://python.langchain.com/docs/integrations/llms/ibm_watsonx/)_ class that allow for communication with watsonx.ai models using LangChain include:

- _**langchain_ibm**_: The package responsible for the LangChain IBM integration. It is necessary to install this package to use any of the following classes and methods.
- _**ibm_watsonx_ai**_: The library that allows connection to watsonx.ai services like IBM Cloud and IBM Cloud Pak for Data.
- _**APIClient**_: The main class of the _ibm_watsonx_ai_ library that manages the API service resources. The parameters include the API credentials and endpoint.
- _**WatsonxLLM**_: The wrapper for IBM watsonx.ai foundation models. This wrapper provides chain integration and is necessary to import. The parameters include the model ID, watsonx.ai API key, URL endpoint, project ID as well as any LLM parameters.
- _**ModelInference**_: The class that instantiates the model interface. The parameters include the model ID, watsonx.ai credentials, project ID, model parameters and more. Once instantiated, the model can then be passed into the class.
- _**invoke**_: The method that calls the model directly with a single prompt of string type. 
- _**generate**_: The method that calls the model with multiple prompts of string type in a list.

Another LangChain class for building AI agents with the integration of tool calling and chaining with watsonx.ai models is _[ChatWatsonx](https://python.langchain.com/docs/integrations/chat/ibm_watsonx/)_. This class, which is leveraged in many of our [tutorials](https://www.ibm.com/think/tutorials/using-langchain-tools-to-build-an-ai-agent), uses the _bind_tools_ method to pass a list of tools to the LLM upon each iteration. These can include both custom and pre-built tools. To retrieve the AI agent response, the _invoke_ method can be used. Once the agent is invoked, the _tool_calls_ attribute of the response displays the name, arguments, id and type of each tool call made, if any.

### LangGraph

[LangGraph](https://www.ibm.com/think/topics/langgraph), created by LangChain, is an open source AI agent framework that supports multi-agent orchestration and enables developers to build [agentic workflows](https://www.ibm.com/think/topics/agentic-workflows) where different agents interact, specialize and collaborate. 

At its core, LangGraph uses the power of graph-based architectures to model and manage the intricate relationships between various components of an [AI agent workflow](https://www.ibm.com/think/topics/ai-agents). Combined with the human-in-the-loop monitoring mechanism and a set of API and tool integrations, LangGraph provides users with a versatile platform for developing AI solutions and workflows including [chatbots](https://www.ibm.com/think/topics/chatbots), state graphs and [other agent-based systems](https://www.ibm.com/think/topics/multiagent-system). 

With the _langchain-mcp-adapters_ library, LangGraph agents can also use tools defined on model context protocol (MCP) servers. The _mcp_ library allows users to build custom MCP servers as well. Essentially, MCP enables a secure connection between an AI system, such as an AI agent, and external tools. Thus, various LLMs can connect to the same tools and data sources given the standard MCP. 

## LangSmith

Released in the fall of 2023, LangSmith aims to bridge the gap between the accessible prototyping capabilities that brought LangChain to prominence and building production-quality LLM applications. 

LangSmith provides tools to monitor, evaluate and [debug](https://www.ibm.com/think/topics/debugging) applications, including the ability to automatically trace all model calls to spot errors and test performance under different model configurations. The use of LangSmith is not limited to applications built using the LangChain ecosystem. The [evaluation](https://www.ibm.com/think/topics/ai-agent-evaluation) of agent performance is done using LLM-as-a-judge evaluators. This [observability](https://www.ibm.com/think/topics/llm-observability) and these key metrics aim to optimize more robust, cost-efficient applications.   

## Getting started with LangChain

LangChain is open source and free to use: source code is [available for download on Github](https://github.com/langchain-ai/langchain).

LangChain can also be installed on Python with a simple pip command: _pip install langchain_. To install all LangChain dependencies (rather than only those you find necessary), you can run the command _pip install langchain[all]_.

Many step-by-step tutorials are provided by IBM including [LangChain tool calling](https://www.ibm.com/think/tutorials/using-langchain-tools-to-build-an-ai-agent), [agentic RAG](https://www.ibm.com/think/tutorials/agentic-rag), [LLM agent orchestration](https://www.ibm.com/think/tutorials/llm-agent-orchestration-with-langchain-and-granite), [agentic chunking](https://www.ibm.com/think/tutorials/use-agentic-chunking-to-optimize-llm-inputs-with-langchain-watsonx-ai) and [more](https://developer.ibm.com/tutorials/awb-create-langchain-ai-agent-python-watsonx/).

## LangChain use cases

AI Applications made with LangChain provide great utility for a variety of use cases, from straightforward question-answering and text generation tasks to more complex solutions that use an LLM as a “reasoning engine.”

### Chatbots

Chatbots are among the most intuitive uses of LLMs. LangChain can be used to provide proper context for the specific use of a chatbot, and to integrate chatbots into existing communication channels and workflows with their own APIs.

### Summarization

Language models can be tasked with summarizing many types of text, from breaking down complex academic articles and transcripts to providing a digest of incoming emails.

### Question answering

Using specific documents or specialized knowledge bases (like Wolfram, arXiv or PubMed), LLMs can retrieve relevant information from storage and articulate helpful answers). If fine-tuned or properly prompted, some LLMs can answer many questions even without external information.

### Data augmentation

LLMs can be used to generate [synthetic data](https://www.ibm.com/think/topics/synthetic-data) for use in [machine learning](https://www.ibm.com/think/topics/machine-learning). For example, an LLM can be trained to generate additional data samples that closely resemble the data points in a training dataset.

### Virtual agents

Integrated with the right workflows, LangChain’s Agent modules can use an LLM to autonomously determine next steps and take action using robotic process automation (RPA).