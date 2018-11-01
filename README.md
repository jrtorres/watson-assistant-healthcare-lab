# Introduction to Watson Assistant

## Overview

Watson Assistant service combines machine learning, natural language understanding, and integrated dialog tools to create conversation flows between your apps and your users. In this lab, you will create a workspace and understand the terminology of creating a chatbot.

This application is based on the code pattern published here - https://developer.ibm.com/patterns/assemble-a-pizza-ordering-chatbot-dialog/

### Flow

1. User sends messages to the application (running locally or on IBM Cloud).
2. The application sends the user message to IBM Watson Assistant service, and displays the ongoing chat in a web page.

   ![](doc/source/images/architecture.png)

### Featured Technologies

* [IBM Watson Assistant](https://www.ibm.com/watson/developercloud/conversation.html): Build, test and deploy a bot or virtual agent across mobile devices, messaging platforms, or even on a physical robot.
* [Node.js](https://nodejs.org/): An asynchronous event driven JavaScript runtime, designed to build scalable applications.


## Part 1: Building a bot from scratch

### Step 1: Designing Your Bot

Building a chatbot with Watson Assistant is so easy, some developers choose to dive right into the tooling. However, with a well-thought out, well-planned chatbot, the interaction with the user can lead to a much better experience that can handle edge cases. In this section, we will design the interaction between a user, Dave, and a chatbot named HungerBot.

A good question to ask yourself is, "Who is my user and what problem do they have?" Expand on the user's profile by determining what the user needs from this chatbot. Does the user have a need to book a reservation at a restaurant? Or an answer to a common question like "Where's the bathroom?" at a conference. Maybe a chatbot that handles tasks like turning on lights or other equipment. It might help to think of the chatbot as an automated version of an existing agent, such as a customer service agent. Look at existing processes that include repeated manual processes, which can sometimes be augmented with chatbots.

Training a chatbot is like training a human agent. You will train the chatbot with the knowledge of certain tasks (intents) and things that these tasks interact with (entities). These components are then combined to create a dialog tree that can take one or more paths to respond to the user's request.

In the following steps, we have provided an insurance chatbot that handles simple questions around plans and claims. 

1. Envision the user that interacts with the bot.

  | Examples                                         |
  | ----------------------------------------------- |
  | A user needs to get information about available plans  |

2. Now, let's describe the overall function of the chatbot.

  | Example                                                   |
  | --------------------------------------------------------- |
  | The chatbot, helps users with basic plan and claim inquiries. |

3. Let's start with the action the user wants to do, which is referred to as an intent. Frist we write a human-friendly description of the action the user is wanting to perform. Then we list a couple of ways the user might phrase this request. Lastly, add a label, like a variable name in code (alpha-numeric, underscores, etc.), that can be used later as a reference.

  | Intent  | Examples  | Label |
  | ------- | ------- | ------- |
  | General plan information  | Tell me about HSA , What plans do you offer ?, What is HIA ?  | #General_Plan_Info |
  | Contribution information  | Whats the max contribution, How is HSA funded ?  | #Plan_Funding_Contributions  |
  | Initiate a payment | Pay my premium. |  #Payment  |


  If you find that you don't have many variations, invite a colleague, a friend (or a real user!) to suggest how they would ask request the these actions. In the real world, you would use real world customer interactions as a basis for training.

4. Another component to training a chatbot is recognizing objects, which are referred to as entities. This example plan bot can differentiate different types of plans.

  | Entity  | Values  | Label |
  | ------- |  ------- | ------- | 
  | Plan Type  | HSA, HIA | Label: @plan | 

  
 We could add time, number and currency entities, however, there are some built-in system entities provided by IBM, that the bot will use. 

In the Dialog editor of Watson Assistant, we can now setup logic to step the user through the conversation. In the next section, we will use this design to train the Watson Assistant service.


### Step 2: Train Watson Assistant Service

Now that we have designed the first dialogue between the chatbot and the user, we can train the Watson Assistant service. Sign up for an IBM Cloud account at https://console.cloud.ibm.com. If you already have an account, sign into your account.

1. **Click on the Catalog*** link in the top-left corner of the IBM Cloud dashboard.

2. **Select the AI category** on the left, under 'All Categories'.

3. **Select the Watson Assistant** service tile.

   ![Assistant Service](doc/source/images/WA_Tile.png)

4. Click **Create** (*Leave default options for Lite plan and service name*)

5. Click on the  **Launch tool** button to launch into the Watson Assistant tooling.

   ![Launch](doc/source/images/WA_LaunchTool.png)

6. This is the Watson Assistant tooling where you can create workspaces and setup different chatbots dialogues and applications. There is an example Customer Service sample workspace where you can see a more evolved training. However, we'll create a new workspace for our bot to use. Click on **Workspaces** and then on  **Create**   in the box labeled **Create a new workspace**.

   ![New workspace](doc/source/images/WA_CreateWS.png)

7. Enter a name for the chatbot and optionally a description, then click **Create**

   ![create](doc/source/images/WA_CreateWS_Complete.png)

8. You will be redirected into a page with four tabs, Intents, Entities, Dialog, and Content Catalog. Before we create our custom intents. Lets make use of some of the content provided by the Watson Assistant Service. Click on the **Content Catalog**

9. The Content Catalog provides some pre-trained intents that you can use in your chatbot. Take a look at the various categories of intents. Click on the **General** category. 

10. There are several common interactions the chatbot can take advantage of from this category. Click on the **Add to workspace** link on the top right corner of the page. You will receive a message that the intent was added to your workspace. Click on the **Arrow** next to the category name 'General'.

   ![Add to Workspace](doc/source/images/WA_ContentCatalog_General.png)

11. Click the **Intents** tab, click on **Add intent** to create the first custom intent.

12. Name the intent *plan_general_information* and click **Create intent**

   ![Create Intent 1](doc/source/images/WA_CreateIntent.png)

13. Add the example utterances shown in the screenshot below, clicking on **Add example** after entering each one 

   ![Intent Examples 1](doc/source/images/WA_CreateIntent_Examples1.png)

14. Click on the return icon to go back to the main menu screen

   ![Return to Intent Page](doc/source/images/WA_CreateIntent_Back.png)

15. Repeat Steps 10 - 13, to create two additional intents as shown below.

   ![Second Intent](doc/source/images/WA_CreateIntent_Examples2.png)

   ![Third Intent](doc/source/images/WA_CreateIntent_Examples3.png)

16. Go ahead and test the chatbot as it is (You will be testing the intent classification). Click on the **Try it** button on the top right of the page. *If there is a purple banner on the Try It Out panel, you will have to wait until training is completed*.  Enter some text in the panel to have Watson Assistant identify the intent.

   ![Intent Test](doc/source/images/WA_IntentTesting.png)

17. Close the 'Try it out' panel by clicking the X on the top right corner. Then Click on the **Entities** tab in the top menu bar. This is where you can add the "objects" that will be extracted from the user input. Click **Add entity**

18. Name the entity *Plan_Type* and add the following values clicking **Add value** after entering each one

   ![Entity Values](doc/source/images/WA_CreateEntity_Example1.png)

19. Click in the return icon to go back to the main menu screen

20. The Watson Assistant has a handful of common entities created by IBM that can be used across any use case. These entities include: date, time, currency, percentage, and numbers. Click on **System entities** and enable @sys-currency, @sys-date, @sys-number and @sys_person

   ![System Entities](doc/source/images/WA_EnableSystemEntities.png)

21. ***[Optional]*** Feel free to test out the entity extraction using the 'Try it out' panel, as you did in step 16.

22. Click on the Dialog tab in the top menu bar. Click **Create**. There are two nodes added by default. The welcome condition is triggered when the chatbot is initially started. This is a good place to introduce the bot and suggest actions the user can ask of this chatbot. Select the Welcome node and change the response as shown below:
   ![Welcome Response](doc/source/images/WA_Dialog_Welcome.png)

23. The second node checks for the condition anything_else. In the event the user enters something that wasn't expected, the service will return this response. Ideally, it should convey a way for the user to recover, such as example phrases.

   ![Anything else](doc/source/images/WA_Dialog_Anythingelse.png)

24. Lets add a dialog node to handle some of our pre-built intents. Select the Welcome node again and click **Add node**. In the dialog node editor, enter a node name. For the input triggers, have it set to when the intents **#General_Agent_Capabilities** or **#General_Greetings** are identified (ensure the trigger is an OR of the two conditions). In the response, add a text response as shown below.

   ![Dialog Node 1](doc/source/images/WA_Dialog_Node1.png)

25. To handle the general plan information queries, we will add a couple of nodes to the dialog tree. Select the node created above and click **Add node** to create a new node. In the dialog node editor, enter a node name (i.e. 'Plan Information'). For the input triggers, have it set to the intent **#plan_general_information** . There is no need to add any response text.

   ![Dialog Node 2](doc/source/images/WA_Dialog_Node2.png)

26. Select the newly created 'Plan Information' node, then click the **Add child node** button.  In the dialog node editor, enter a node name (i.e. 'Current Plan Responses'). For the input triggers, have it set to the entity type **@Plan_Type** . Then click the **Customize** link next to the name and enable Multipe responses for the node and click the **Apply** button. Complete this node by adding three responses for the three different plans ()

   ![Dialog Node 3](doc/source/images/WA_Dialog_Node3.png)

   ![Dialog Node 3 MR](doc/source/images/WA_Dialog_Node3_MR.png)

   ![Dialog Node 3 Responses](doc/source/images/WA_Dialog_Node3_Responses.png)

27. Select the 'Plan Information' node, then click the **Add child node** button.  In the dialog node editor, enter a node name (i.e. 'Prompt for Plan'). For the input triggers, set it to **true** . Add the responses as shown in the screen shot and in the 'And finally' section, set the action to **Jump to**, selecting the 'Current Plan Response' node and the 'Wait for user input' option

   ![Dialog Node 4](doc/source/images/WA_Dialog_Node4.png)

28. Select the 'Plan Information' node. In the 'And finally' section of the node editor, set the action to **Jump to**, selecting the 'Current Plan Response' node and the 'If bot recognizes (condition)' option

   ![Dialog Node 2 Jump-to](doc/source/images/WA_Dialog_Node2_Jumpto.png)

29. Your dialog tree should now look as shown below. ***[Optional]*** Feel free to test out the dialog using the 'Try it out' panel,

   ![Dialog Plan Information Nodes](doc/source/images/WA_DialogPart1.png)

30. ***[Optional]*** Following a similar process as steps 25 - 29, add dialog nodes to address the plan contribution intent.

   ![Dialog Plan Contribution Nodes](doc/source/images/WA_DialogPart1b.png)

31. We will now add another dialog node to handle payments. We will use the slots feature of Watson Assistant, which will simplify the process of gathering the information necessary for a payment. In the dialog tree, add a node at the root / top level. Name the node *Payment* and select the #payment_request intent as the trigger where it says **If bot recognizes**. Then click on **Customize** in the top right corner and enable Slots. Click **Apply**

   ![Dialog Node 5 Enable Slots](doc/source/images/WA_Dialog_Node5_EnableSlots.png)

32. Add three slots as follows (clicking **Add slot** to add each slot section)

   | Check for  | Save it as  | If not present, ask |
   | ------- | ------- | ------- |
   | @sys-date  | $payment_date  | What is the payment date ? |
   | @sys-currency | $payment_amount  | How much would you like to pay ?  |
   | @sys-person | $policy_holder_name |  What is the policy holders name ? |

   ![Dialog Node 5 Slots](doc/source/images/WA_Dialog_Node5_Slots.png)

33. Have the bot respond with the details of the payment. The syntax uses the values stored in the context and injects the values into the response. The full text should read:
`I'll schedule a payment of <? $payment_amount ?> on <? $payment_date ?> for policy holder <? $policy_holder_name ?>`

   ![Dialog Node 5 Response](doc/source/images/WA_Dialog_Node5_Response.png)

34. Use the 'Try it out' panel to test your Watson Assistant chat bot. Click on the **Try it** icon in the top-right corner of the tooling.

   ![Try it out](doc/source/images/WA_DialogPart1_Test.png)


## Part 2: Run the Application

### Run in container

Run in a container on IBM Cloud, using [these instructions](doc/source/Container.md).

 **OR**

### Run locally
 Perform steps 1-5:

1. [Clone the repo](#1-clone-the-repo)
2. [Configure Watson Assistant](#2-configure-watson-assistant)
3. [Add IBM Cloud credentials and add to .env](#3-add-ibm-cloud-services-credentials-and-add-to-env-file)
5. [Run the application](#5-run-the-application)

#### 1. Clone the repo

Clone the `watson-assistant-functions-lab` repository locally. In a terminal, run:

  `$ git clone https://github.com/jrtorres/watson-assistant-functions-lab.git`


### 2. Configure Watson Assistant

Complete the steps in Part 1 above to build the chatbot from scratch. As an alternative, you can Launch the **Watson Assistant** tool. Use the `import` icon button on the right

<p align="center">
  <img width="50%" height="50%" src="doc/source/images/import_conversation_workspace.png">
</p>

Find the local version of [`data/plan-bot-part1.json`](data/plan-bot-part1.json) and select
`Import`. 

#### 3. Add IBM Cloud service credentials and Workspace ID to .env file

As you create the IBM Cloud services, you'll need to create service credentials. You might get either IAM or username/password based credentials based on the region.

First of all, copy the `env.example` file to ``.env``.

* If the service credentials from IBM Watson Assistant is username/password based as below populate the username, password and workspace_id and comment out the IAM credentials part.

![](doc/source/images/WatsonCred1.png)


```
WORKSPACE_ID=<put workspace id here>

# Watson Assistant authentication using username/password authentication
CONVERSATION_USERNAME=<put assistant username here>
CONVERSATION_PASSWORD=<put assistant password here>

# Watson Assistant Authentication using IAM
#CONVERSATION_IAM_APIKEY=<put assistant IAM apikey here>
#CONVERSATION_URL=<put assistant url here>
```

* If the service credentials from IBM Watson Assistant is IAM based as below, populate the IAM apikey, url, and workspace_id and comment out the username/password part

![](https://github.com/IBM/pattern-images/raw/master/watson-assistant/watson_assistant_api_key.png)


```
WORKSPACE_ID=<put workspace id here>

# Watson Assistant authentication using username/password authentication
#CONVERSATION_USERNAME=<put assistant username here>
#CONVERSATION_PASSWORD=<put assistant password here>

# Watson Assistant Authentication using IAM
CONVERSATION_IAM_APIKEY=<put assistant IAM apikey here>
CONVERSATION_URL=<put assistant url here>
```

You can find the credentials and workspace information from the deploy tab in the tooling. 

<p align="center">
  <img width="50%" height="50%" src="doc/source/images/WA_Deploy_Credentials.png">
</p>


### 5. Run the application

From a terminal, in the directory where you cloned the GIT repository, run the following commands:

```
$ npm install
$ npm start
```

# Troubleshooting

* Deploy using Cloud Foundry `cf push` gives:

``FAILED
Could not find service <Watson_service> to bind to <IBM_Cloud_application>``

If you name your service `wcsi-conversation-service`, this should work.
When you use `cf push`, it is trying to bind to the services listed in the `manifest.yml`.

So, there are 2 ways you can get this to work:

* Change the names of your IBM Cloud services to match the names in the manifest.
* Change the names in the manifest to match the names of your IBM Cloud services.



# License

[Apache 2.0](LICENSE)

# Links

* [IBM Watson Assistant Docs](https://console.bluemix.net/docs/services/conversation/dialog-build.html#dialog-build)
* [Blog for IBM Watson Assistant Slots Code Pattern](https://developer.ibm.com/code/2017/09/19/managing-resources-efficiently-watson-conversation-slots/)

# Learn more

* **Artificial Intelligence Code Patterns**: Enjoyed this Code Pattern? Check out our other [AI Code Patterns](https://developer.ibm.com/code/technologies/artificial-intelligence/).
* **AI and Data Code Pattern Playlist**: Bookmark our [playlist](https://www.youtube.com/playlist?list=PLzUbsvIyrNfknNewObx5N7uGZ5FKH0Fde) with all of our Code Pattern videos
* **With Watson**: Want to take your Watson app to the next level? Looking to utilize Watson Brand assets? [Join the With Watson program](https://www.ibm.com/watson/with-watson/) to leverage exclusive brand, marketing, and tech resources to amplify and accelerate your Watson embedded commercial solution.
* **Kubernetes on IBM Cloud**: Deliver your apps with the combined the power of [Kubernetes and Docker on IBM Cloud](https://www.ibm.com/cloud-computing/bluemix/containers)
