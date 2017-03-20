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

In this GET request you must set your X-Api-Key header to the key provided in the settings section of the developer dashboard for you app as explained in the **Getting Started** section above.

If your Api Key and Token are valid you will receive a successful response in the following format:
```
{
	status: 200,
	payload: {
		Authorization: "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
	},
	message: null,
	now: 1490030158728
}
```
The **Authorisation token** is what allows you to call the Zzish Api for this user and this app. 

### Sending Actions

For each action your user does which you want to provide to Zzish to be used in the dashboards you must post it to the following endpoint, making sure you set the Authorisation token and X-Api Key in your request headers like so:
```
	X-Api-Key "5hfnrqrVjm5L145cDKWiY1UtCWej9tgp2yR8EXUL"
	Authorization "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
```

