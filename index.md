## Zzish Developer Docs

### Getting Started

Head to https://developers.zzish.com and sign up. Here you will be able to add your app to your account and generate an **X-Api-Key**.

### Client-Side Authentication
When a student launches your app from Zzish, the launch URL of your app will include a token in this form:
```
https://yourlaunchurl.com/?token=${tokenId}
```
You can use this token to authenticate the app to be able to send events to Zzish from it.

Do a GET request to this endpoint to get the Authorization token.

```
${API_URL}/api/auth/${tokenid}
```
>**Note**: API_URL is *test-api-v2.zzish.com* for test purposes

In this GET request you must set your X-Api-Key header to the key provided in the settings section of the developer dashboard as explained in the **Getting Started** section above.

If your Api Key and Token are valid you will receive a successful response in the following format:
```
{
	"status": 200,
	"payload": {
		"Authorization": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
	},
	"message": null,
	"now": 1490030158728
}
```
The **Authorisation token** is what allows you to call the Zzish Api for this user and this app.

You can generate tokens on the developer dashboard for testing purposes as shown below:

You can then post actions to it as explained below and see the results on the actions page.

### Sending Actions

For each action your user does which you want to provide to Zzish to be used in the dashboards you must post it to the following endpoint, making sure you set the Authorisation token and X-Api Key in your request headers like so:
```
	X-Api-Key 5hfnrqrVjm5L145cDKWiY1UtCWej9tgp2yR8EXUL
	Authorization eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

You can send an array of actions of minimum length 1 in this posts' body. This is the action format we expect:

```
[{
	"actionId": 056c20a8-ba95-4747-bdc5-69f6a0e98f86,
	"name": "What is the Capital of France",
	"correct": true,
	"response": "Paris",
	"timestamp": 1490030158728,
	"duration": 4000,
	"score": 0.75
}]
```

#### Action Schema

Actions must conform to the following schema:
```
	actionId: string;
	name: string;
	correct: ?boolean;
	response: ?string;
	timestamp: ?integer;
	duration: ?integer;
	score: ?float;
```

Field | Type | Description
---: | --- | ---
**actionId** `required` | string |  a **unique identifier** for this action defined by you =.  It must be in **UUID format**. This id allows us to map actions done by different users to one another. Therefore, for the same action in your application (for example, the same question or the same click) you should attach the same actionId. This is fundamental to how the Zzish dashboards work and how we can provide teachers with insight from your app.
**name** `required` | string |  name of the action. This is anything that a **teacher can read and understand**. For example, the text of the question answered or the name of the action completed.
**correct** `optional` | boolean | only applies to actions which can have both a positive and negative result, such as answering a question.
**response** `optional` | string | the student's response to this action. For example the text entered by the student to complete this action.
**timestamp** `optional` | integer | UNIX timestamp
**duration** `optional` | integer |  time in **milliseconds**. How long the user has taken to complete this action. If possible, it is **highly recommended** to include it as this allows Zzish to guess how confident a student was when completing this action.
**score** `optional` | float | number between **0 and 1**. Allows you to indicate to Zzish how the user did for this action, where 1 is the best possible user response, and 0 the worst.

#### Action Definition

When you first send an action to the Zzish Api, it creates an **Action Definition** for that actionId. This is to ensure that the data from the students is consistent and allows us to show teachers insightful information about your app. For example, we can compare students to one another.

When the first action is sent, we look at which optional fields you have provided us. To ensure consistency across our dashboards, any new action tied to the same actionId must include at least those optional fields. Otherwise it will be rejected.
