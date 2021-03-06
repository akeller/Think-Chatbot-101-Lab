# 101 Lab: Build Your Own Chatbot in 15 Minutes or Less

You should be able to complete this lab in 10-15 minutes! If you'd like to continue working on your chatbot after the lab, there will be additional steps to include more advanced techniques and even deploy.

This lab will walk you through creating a chatbot to guide the user through creating a support ticket. Instead of a traditional form or wizard, this chatbot can offer a more personalized approach, soliciting more quality information from the user to build the most informative support ticket.

In IBM Cloud there are often multiple ways to accomplish the same steps. In the steps outlined below, you should consider this one way to accomplish a given task.

## Create an instance of the Watson Assistant service

Navigate to the IBM Cloud dashboard: https://ibm.biz/Bd2Sfe

Click ```Catalog``` in the upper menu bar.

![IBM Cloud Dashboard with an annotation pointing to Catalog.][dashboardCatalog]

This will load a full list of all available services in the Catalog. Click ```AI```.

![Catalog in IBM Cloud with an annotation pointing to the AI category.][catalogAI]

You should see all the AI services in IBM Cloud, including the Watson API Services and data science tools and platforms. Click ```Watson Assistant```.

![AI services in IBM Cloud with an annotation pointing to the Watson Assistant service.][AIassistant]

This starts the process of creating your Watson Assistant service. Make sure you give your service a good, descriptive name - ```Think 2019``` or just keep the autogenerated one. 

You can leave all the remaining feels with the defaults, including the Pricing Plan. Click ```Create```.

![Provisioning screen of Watson Assistant.][createScreen]

Depending on the connection, this next step may spin for a bit as your Watson Assistant service is being created. When the screen loads, notice your API key and URL. You'll need this information if you decide to deploy your bot after the lab. Click ```Launch tool```.

![Watson Assistant service screen with annotation for Launch tool button.][launchTool]

You are now ready to start building your bot!

## Create a Skill

Your Watson Assistant instance should load and take you to a page where you can see information about the steps to build your assistants. Click ```Create a Skill``` to start building.

![Watson Assistant Home screen with an annotation for the Create a skill button.][createSkillButton]

If you had skills already created, you would see them below. Click ```Create new```.

![Watson Assistant Skills page with an annotation for the Create new button.][createNewSkill]

Name your skill something useful - ```Support Ticket```. If you or your users speak a language other than English, take note of the available options in the language dropdown.

![Add Dialog Skill screen with form to create new skill.][addDialogSkill]

Your skill should look something like this. Click ```Create```.

![Add Dialog Skill screen with information in form to create new skill with annotation pointing to Create button.][skillFilled]

## Build Intents

Once you've created a skill, you can add your intents. An intent is a goal or purpose of the uer's input. Think about what you may want to help your user complete when working through the creation of a support ticket.

Click ```Add intent```.

![Initial Intents screen with definition of Intents and annotation pointing to Add intent.][addIntent]

Notice the ```Intent name``` field is denoted with a hash or # sign. 

Name your intent ```createTicket``` as this is the best way to describe the user's goal.

![Blank create new intent screen with no user examples.][createNewIntent]

Provide user examples and hit Enter to see them appear in the list below. Think through how you might prompt someone in a conversation to indicate to them that you need to create a support ticket. 

When finished, hit the back arrow next to the intent name at the top.

![Create intent screen with a name and user examples per the lab.][userExamplesIntent]

You now see your intent listed below. Let's move on to entities.

## Build Entities

With the intent you just created, we now know the user is looking to create a support ticket. Simply creating a ticket will not be enough, we need more information.

Click ```Entities```.

![Intents screen with one intent and an annotation pointing to the Entities tab.][entities]

We need to build an entity. An entity is a portion of the user's input that you can use to provide a different response to a particular intent. 

For example, a user who submits a bug will need to provide different information than a user who submits a featue request. Let's create an entity to handle that.

Click ```Add entity```.

![Initial my entities screen with annotation for the Add entity button.][addEntity]

Notice the ```Entity name``` field is denoted with an at-symbol or @ sign. 

Name your entity ```problem_type```.

![Blank create new entity screen with no values.][createNewEntity]

Let's provide 3 values for entities - ```bug```, ```feature request```, and ```how do I```. If you can think of synonyms feel free to provide those as well. 

Click ```Add value``` to add an entity value to the list below.

![Entity screen with values per the lab.][entityValues]

You have now created an entity! Let's pull our intent and entity together with dialog.

## Build Dialog

Working with our intent and entity, we can start diagramming out our dialog. 

Click ```Dialog```.

![My entities screen with one entity with an annotation pointing to Dialog tab.][dialog]

If a dialog tree has already been started, we would see it below. Let's create one.

Click ```Create```. (Additionally you can click 'x' if the chat popups in the right hand corner like you see in the screenshot)

![Initial dialog screen with an annotation pointing to the create button.][createDialog]

Notice there are two nodes created automatically. When adding new nodes, remember to put them between the "Welcome" node and the "Anything else" node. Nodes that are listed after the "Anything else" node will not be evaluated. Evaluation order may matter in your dialog tree.

Click ```Add node```. This will add a node between the two existing nodes.

![Dialog screen with 2 initial nodes and an annotation pointing to Add node.][addNode]

Your node has been placed in the dialog tree and is ready to be constructed.

![New node in dialog tree with information form open.][newNode]

Let's start by adding our intent to the ```If assistant recognizes: ``` field. Click the field and notice you can filter by # or intents. Find the ```#createTicket``` intent you created in an earlier step.

In the dialog tree you can see the intent name is listed in the node. Feel free to name the node as well.

![New node with intent added for Watson to recognize.][dialogAddIntent]

If you name the node, you will see both the name and what the assistant recognizes in the top level information of the node.

![New node adding text response per lab.][dialogAddText]

Let's add some response text and test it out in the console. Type a response like ```I can help with that, but I need some more information``` so your user knows that Watson understands the #createTicket intent. 

Click the ```Try It``` button in the upper left corner to open up the ```Try it out``` panel.

Type in one of your user examples in your intent like ```I need help```. Does Watson respond?

![Dialog screen showing dialog node tree and node with try it out panel opened for testing Watson.][dialogTryText]

Let's make this a little more useful. Hit "Customize" to the right of the Create Ticket name field. A modal will appear.

We are going to be using slots, so turn on the toggle and check the ```Prompt for everything``` box.

Hit ```Apply```.

![Model of Customizing Create Ticket node with slots enabled and prompt for everything checked.][dialogCustomizeSlot]

Your node has changed. ```Then check for:``` provides a new set of fields. 

In the ```Check for``` column find your ```@problem_type``` entity. The context variable (denoted by the $) will autopopulate, keep it. 

In the ```If not present, ask``` column type ```What kind of ticket would you like to submit?```

This allows us to look for the information if its already provided, or ask for it since we will need it to complete this intent.

![Node with slots enabled and first slot information filled in per lab.][dialogSlot]

Let's test it! You might want to hit ```Clear``` in the ```Try it out``` panel. 

![Try it out panel open to test Watson with node and slot functionality.][dialogSlotTest] 

Watson should have correctly identified the intent, and prompted you with the slot question. If you answer with one of the entity values, you should see it in bold, followed by the response. 


## Congratulations! 

You have officially completed your lab. You made your first chatbot! Now what?

# Next Steps

You can continue accessing this lab material and your IBM Cloud account even after Think. 

This is only part of a chatbot solution; you completed the backend. A typical, fully implemented chatbot solution may look something like this:

![alt text][chatstack]

Take a look at the following patterns and tutorials to make your chatbot more robust and ready to be deployed!

## Patterns

* https://developer.ibm.com/patterns/create-cognitive-retail-chatbot/
* https://developer.ibm.com/patterns/compose-bots-using-an-agent-bot/
* https://developer.ibm.com/patterns/create-cognitive-banking-chatbot/
* https://developer.ibm.com/patterns/create-an-agent-for-rental-car-reservations/
* https://developer.ibm.com/patterns/create-a-virtual-reality-speech-sandbox/
* https://developer.ibm.com/patterns/build-a-virtual-assistant-for-ios-with-watson/
* https://developer.ibm.com/patterns/build-an-ai-powered-ar-character-in-unity-with-arkit/
* https://developer.ibm.com/patterns/watson-conversation-with-ibm-linuxone-systems/

## Tutorials

* https://developer.ibm.com/tutorials/cc-build-chatbot-ibm-cloud/
* https://developer.ibm.com/tutorials/set-up-your-own-instance-of-a-chatbot-and-deploy-it-to-the-kubernetes-environment-on-ibm-cloud/
* https://developer.ibm.com/tutorials/learn-how-to-export-import-a-watson-assistant-workspace/

## Additional Resources

* https://developer.ibm.com/technologies/conversation/ 
* https://developer.ibm.com/code/exchanges/bots/





[dashboardCatalog]: /images/dashboardCatalog.png
[AIassistant]: /images/AIassistant.png
[createScreen]: /images/createScreen.png
[launchTool]: /images/launchTool.png
[createSkillButton]: /images/createSkillButton.png
[createNewSkill]: /images/createNewSkill.png
[addDialogSkill]: /images/addDialogSkill.png
[skillFilled]: /images/skillFilled.png
[addIntent]: /images/addIntent.png
[createNewIntent]: /images/createNewIntent.png
[catalogAI]: /images/catalogAI.png
[userExamplesIntent]: /images/userExamplesIntent.png
[entities]: /images/entities.png
[addEntity]: /images/addEntity.png
[createNewEntity]: /images/createNewEntity.png
[entityValues]: /images/entityValues.png
[dialog]: /images/dialog.png
[createDialog]: /images/createDialog.png
[addNode]: /images/addNode.png
[newNode]: /images/newNode.png
[dialogAddIntent]: /images/dialogAddIntent.png
[dialogAddText]: /images/dialogAddText.png
[dialogTryText]: /images/dialogTryText.png
[dialogCustomizeSlot]: /images/dialogCustomizeSlot.png
[dialogSlot]: /images/dialogSlot.png
[dialogSlotTest]: /images/dialogSlotTest.png
[chatstack]: /images/chatstack.png
