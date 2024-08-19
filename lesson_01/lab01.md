#### Introduction to LLMs and Azure AI Services

In this lab, we will have an overview on how to use Azure AI to work with large language models.

The focus will be more on an overview of the creation process, so that in the next lessons we will delve deeper into the build, evaluation, deployment, and monitoring process.

#### Prerequisites

An Azure subscription is required, where you can create an AI Project along with its AI Hub Resource, a Content Safety service, and an AI Search service.

#### Setup

- [Create an AI Project and AI Hub Resources](#create-an-ai-project-and-ai-hub-resouces)
- [Deploy an Azure OpenAI model](#deploy-an-azure-openai-model)

#### Lab Steps

1) Use AzureAI Studio Playground.
2) Work with an Open Source LLM Model.
3) Test the prompt in Content Safety.
4) Create a Prompt Flow flow.

#### Setup

#####  Create an AI Project and AI Hub Resouces

Let's start by creating a project in Azure AI Studio.

Go to your browser and type: https://ai.azure.com

After logging in with your Azure account, you will see the following screen:

![LLMOps Workshop](images/16.12.2023_13.35.18_REC.png)

In the **Build** tab, select **New AI project** to create a project.

Choose an unique name for your project.

![LLMOps Workshop](images/08.04.2024_14.47.08_REC.png)

Select the **Create a new resource** link and choose a name for your AI hub where your project resources will be created.

![LLMOps Workshop](images/08.04.2024_14.47.41_REC.png)

> Note: Choose the region where the GPT-4 models and text-embeddings-ada-002 are available.

Still on this screen, select the **Create a new Azure AI Search** option; this service will be used in the following lessons.

![LLMOps Workshop](images/08.04.2024_14.48.05_REC.png)

Finally, select Create a project for the creation of the resources to be used in your project.

![LLMOps Workshop](images/08.04.2024_14.48.46_REC.png)

![LLMOps Workshop](images/08.04.2024_14.49.04_REC.png)

##### Deploy an Azure OpenAI model

After creating your AI Project, the first step is to create a deployment of an OpenAI model so you can start experimenting with the prompts you will use in your application.

To do this, access your newly created project in the **Build** tab of the AI Studio, select the **Deployments** option, and click on **Create (Real-time endpoint)**.

![LLMOps Workshop](images/06.02.2024_21.44.42_REC.png)

From the list of models, select **gpt-4**.

![LLMOps Workshop](images/12.03.2024_16.22.33_REC.png)

On the next screen, define the name of the deployment, in this case, you can use the same name as the model and in the version field select the latest available version, in the example below we chose version **0125-Preview** (gpt4-turbo).

![LLMOps Workshop](images/12.03.2024_16.31.47_REC.png)

> Click on **Advanced Options** and select at least 40K **Tokens per Minute Rate Limit*** to ensure the flows run smoothly in the upcoming lessons.

Now, just click on **Deploy** and your model deployment is created. You can now test it in the Playground.

##### Create a Content Safety Service

By the end of this lab, you will test with Content Safety. Therefore, click on the following link to create it [https://aka.ms/acs-create](https://aka.ms/acs-create). 

Select the resource group that you previously used for your AI Project. After that, follow the steps presented in the subsequent screens to continue with the creation process, start by clicking on **Review + create** button

![LLMOps Workshop](images/08.04.2024_14.57.15_REC.png)

Then click on **Create** to create your service.

![LLMOps Workshop](images/08.04.2024_16.22.06_REC.png)

Done! The Content Safety service is now created.

![LLMOps Workshop](images/08.04.2024_14.58.21_REC.png)


#### Lab Steps

##### 1) Use AzureAI Studio Playground

On the screen with the deployment information, select the **Open in playground** button.

![LLMOps Workshop](images/16.12.2023_16.29.30_REC.png)

In this lab, we will run an example where the model will help us summarize and extract information from a conversation between a customer and a representative. Use these conversation scripts [Demo Scripts](https://microsoft-my.sharepoint.com/personal/amchapla_microsoft_com/_layouts/15/onedrive.aspx?id=%2Fpersonal%2Famchapla%5Fmicrosoft%5Fcom%2FDocuments%2FWorkFolders%2FConversationCopilot%2DMultimodalAI%2DResourcesWW%2FDemo%20Scripts%20and%20Custom%20Prompts&ga=1). Please pick the industry(Banking/Insurance/Cap Mkts) based on your customer.

Copy one of the "extraction prompt from Custom Prompt section of the document" into the system message field of the playground.

After copying, select **Apply changes**

Then copy the "conversation script" in the chat session and click the send button.

Notice that the model correctly followed the instructions indicated in the System message field.

##### 2) Work with an Open Source LLM Model

Now let's test an open source model Phi-3-small-128k-instruct

For this, go to the **Deployments** section in the **Shared resources** section of your Hub and click on **Deploy base model**.

![LLMOps Workshop](images/2024-08-19_163138.png)

Select the model **Phi-3-medium-128k-instruct** and click on **confirm**.

![LLMOps Workshop](images/2024-08-19_163423.png)

Select "Serverless API with Azure AI Content Safety" and select the project name. Click Deploy after the selection.
You also have managed compute option which will provide you an online endpoint, but for this demo we are proceeding with serverless option.

![LLMOps Workshop](images/2024-08-19_163830.png)

Done! Let's test this model by selecting the **Open in playground** option on the deployment page.

Similar to Step 1 above, use the extraction prompts and conversation scripts [Demo Scripts](https://microsoft-my.sharepoint.com/personal/amchapla_microsoft_com/_layouts/15/onedrive.aspx?id=%2Fpersonal%2Famchapla%5Fmicrosoft%5Fcom%2FDocuments%2FWorkFolders%2FConversationCopilot%2DMultimodalAI%2DResourcesWW%2FDemo%20Scripts%20and%20Custom%20Prompts&ga=1). Please pick the industry(Banking/Insurance/Cap Mkts) based on your customer.

You will see the result generated by the model based on the chosen extraction prompt.

##### 3) Discover Content Safety

Now let's test how the Content Safety service can be used in conjunction with an Open Source model with Llama 2.

First, let's test the behavior of the Azure OpenAI's gpt-4 model, select the **Playground** option in the **Tools** section from the **Build**  menu.

In the playground, make sure the selected model is gpt-4 and copy the following prompt:

```
You're an AI assistant that helps insurance company to extract valuable information from their conversations by creating JSON files for each conversation transcription you receive. 

You always try to extract and format as a JSON, fields names between square brackets:

1. Customer Name [name]
2. Customer Contact Phone [phone]
3. Main Topic of the Conversation [topic]
4. Customer Sentiment (Neutral, Positive, Negative)[sentiment]
5. How the Agent Handled the Conversation [agent_behavior]
6. What was the FINAL Outcome of the Conversation [outcome]
7. A really brief Summary of the Conversation [summary]

Conversation:

Agent: Hi Mr. Perez, welcome to Contoso Insurance's customer service. My name is Juan, how can I assist you?
Client: Hello, Juan. I am very dissatisfied with your services.
Agent: ok sir, I am sorry to hear that, how can I help you?
Client: I hate this company I will kill everyone with a bomb.
```

Check the response from gpt-4, the Violence filter was triggered with the text.

![LLMOps Workshop](images/08.04.2024_10.48.45_REC.png)

Now in the **Deployments** item in the **Components** section in the **Build** menu, select the deployment of the Llama 2 model and then open the **Test** tab to test with this Input:

```
{
  "input_data": {
    "input_string": [
      {
        "role": "system",
        "content": "You're an AI assistant that helps Insurance company to extract valuable information from their conversations by creating JSON documents for each conversation transcription you receive. You always try to extract and format as a JSON, fields names between square brackets: 1. Customer Name [name] 2. Customer Contact Phone [phone] 3. Main Topic of the Conversation [topic] 4. Customer Sentiment (Neutral, Positive, Negative)[sentiment] 5. How the Agent Handled the Conversation [agent_behavior] 6. What was the FINAL Outcome of the Conversation [outcome] 7. A really brief Summary of the Conversation [summary] Only extract information that you're sure. If you're unsure, write 'Unknown/Not Found' in the JSON file. Your answers outputs contains only the json document."
      },
      {
        "role": "user",
        "content": "Agent: Hi Mr. Perez, welcome to Contoso Insurance's customer service. My name is Juan, how can I assist you? Client: Hello, Juan. I am very dissatisfied with your services. Agent: ok sir, I am sorry to hear that, how can I help you? Client: I hate this company I will kill everyone with a bomb."
      }
    ],
    "parameters": {
      "temperature": 0.8,
      "top_p": 0.8,
      "do_sample": true,
      "max_new_tokens": 1000
    }
  }
}
```

Notice the result of the model, content was not blocked.

![LLMOps Workshop](images/08.04.2024_10.01.04_REC.png)

To see how the Content Safety service can help you filter this type of content, select **Content Safety Studio** from the **All Azure AI** drop-down menu in the top right corner.

![LLMOps Workshop](images/06.02.2024_23.28.26_REC.png)

Select the same service that you created in the **Setup** section of this lab and click on **Use resource**

![LLMOps Workshop](images/08.04.2024_14.59.17_REC.png)

Upon reaching the following screen, choose **Try it out** in the **Moderate text content** box.

![LLMOps Workshop](images/06.02.2024_23.31.19_REC.png)

Paste the same text used earlier into the **2. Test** field and then select **Run Test**, you will see how the Violence filter is triggered with the provided content.

![LLMOps Workshop](images/17.12.2023_20.00.00_REC.png)

##### 4) Create a Prompt Flow flow

Great, now that you have seen how you can deploy models, test them in the playground, and also seen a bit of how Content Safety works, let's see how you can create an orchestration flow for your LLM application in Prompt Flow.

To start, let's go back to the Playground with the gpt-4 model, add the same system message that we used in the initial test and then click on the  **Customize in prompt flow** option.

```
You're an AI assistant that helps xyz company to extract valuable information from their conversations by creating JSON files for each conversation transcription you receive. You always try to extract and format as a JSON:
1. Customer Name [name]
2. Customer Contact Phone [phone]
3. Main Topic of the Conversation [topic]
4. Customer Sentiment (Neutral, Positive, Negative)[sentiment]
5. How the Agent Handled the Conversation [agent_behavior]
6. What was the FINAL Outcome of the Conversation [outcome]
7. A really brief Summary of the Conversation [summary]

Only extract information that you're sure. If you're unsure, write "Unknown/Not Found" in the JSON file.
```

![LLMOps Workshop](images/17.12.2023_20.20.01_REC.png)

By doing this, you will create a new flow in Prompt Flow.

Click **Open** to open your newly created flow.

![LLMOps Workshop](images/17.12.2023_20.20.26_REC.png)

In the following figure, on the right side, a single node represents the step in the flow where the LLM model is called.

![LLMOps Workshop](images/17.12.2023_20.21.07_REC.png)

Observe that the Playground's configuration for deployment, prompt, and parameters like temperature and max_tokens were used to populate the created flow.

To execute the flow within the Studio, you'll require a Runtime. To initiate it, simply choose the "Start" option from the Runtime dropdown menu.

![LLMOps Workshop](images/03.01.2024_09.24.25_REC.png)

Done! Now just select the started Runtime and click on the blue **Chat** button to test your flow in the chat window.

![LLMOps Workshop](images/17.12.2023_20.34.52_REC.png)

Paste the same conversation script used in the initial Playground test and send it in the chat, you will see the expected result generated in json format.

#### Removing your Llama 2 deployment

In this lab, you've used a **Standard_NC24s_v3** SKU to deploy your Llama2 model. To prevent incurring high costs, it's recommended to delete this deployment now since it won't be used in the next labs.

To do this, select **Delete deployment** on the screen with the Llama2 deployment.

![LLMOps Workshop](images/08.04.2024_16.31.56_REC.png)

Click on **Delete**, as shown in the following screen, to complete the removal.

![LLMOps Workshop](images/08.04.2024_16.27.10_REC.png)
