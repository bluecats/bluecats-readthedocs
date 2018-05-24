---
layout: documentation
title: Testing Code with Your API Keys 
category: Tutorials 
order: 12
---

This guide will show you how to create a new API application and add the keys to your developer.bluecats.com account for interactive code testing.

First, login to the BlueCats beacon management platform at https://app.bluecats.com and click Apps.
Client IDs and Secrets are obtained in the [app](https://app.bluecats.com/apps) section of the web app using.

### Creating an API Client
Navigate to the **Apps** section of the web app.
![Apps](https://d16co4vs2i1241.cloudfront.net/uploads/tutorial_image/file/480045892460611400/1e9741ddaf04599001c9af5c099be6f4d84b1aa6e8e13a2b82519bbce1ab08d2/column_sized_apps.png)

Select **Create New App**
![Create App](https://d16co4vs2i1241.cloudfront.net/uploads/tutorial_image/file/480045987084109641/1c1a28be9df7639aa533cfe511a210093f53502fdd91a697fb6fb06a4357db90/column_sized_createApp.png)

Name your app and select **API Client**.
![Api Client](https://d16co4vs2i1241.cloudfront.net/uploads/tutorial_image/file/480045993081964362/66005e306106f37d4ae7cb434e5edcd832ad18d90a97937ef7cb315d1ffc9e6c/column_sized_apiClient.png)

Lastly, click **Create App** to create your app. View your app's client ID and secret by pressing **Show App Token/Secret** from your app's information.

### What's next?
Now that you have your App Token/Secret, visit the [BlueCats Developer Portal](https://developer.bluecats.com) dashboard and click "Go to your Dashboard".
![Go to Dashboard](https://d16co4vs2i1241.cloudfront.net/uploads/tutorial_image/file/475671700130760637/8591d16b9b0641df48aecfee996935968a82c34af3d6731432fc0c67b3b49be9/column_sized_Screen_Shot_2016-04-13_at_10.47.43_AM.png)

Click "+ Add Access Token" to show the add key dialog. (It's okay if you already have keys in there for other apps - just make sure you use the right one for your tests in the API Explorer.)
![Add Access Tokens](https://d16co4vs2i1241.cloudfront.net/uploads/tutorial_image/file/475669125650188205/2a18f735b4fbde5a2011cd6dba9a815ba8a271fa2fc90002533505e5eb3b8c27/column_sized_Screen_Shot_2016-04-12_at_4.09.03_PM.png)

Paste your Client ID and Secret into the fields, and give your key a meaningful name.
![Give your key a name](https://d16co4vs2i1241.cloudfront.net/uploads/tutorial_image/file/475672558805452776/758c6e80e9f7609c74568ebd3f2f0fc8abd15d3f3b6df84cea4ba32a52119f59/column_sized_Screen_Shot_2016-04-12_at_4.10.10_PM.png)

Once you click "Save Access Token", your new key should be visible (obfuscated until you click reveal).
![Save Access Token](https://d16co4vs2i1241.cloudfront.net/uploads/tutorial_image/file/475673179772159978/3d87cb251ab49f74259aaf5155960b70fdab5d5441cfe9f2ccb2584499e188eb/column_sized_Screen_Shot_2016-04-12_at_4.10.27_PM.png)

Click the "EXPLORER" link on the portal menu bar to test out your new key.
![Click EXPLORER](https://d16co4vs2i1241.cloudfront.net/uploads/tutorial_image/file/475677719493870661/ec7cb02340f33156e6c753fa089114fa6c46fefab0fecf275fe9b3a9a95da1be/column_sized_Screen_Shot_2016-04-12_at_4.10.45_PM.png)

Test out your new API Key by doing a GET request on the Beacons Resource. Click "Send Request" and you should get a JSON packet back containing data on your beacons.
![Test things out](https://d16co4vs2i1241.cloudfront.net/uploads/tutorial_image/file/475682499129771169/07f18ff1ec72714711580d3f89661faf78d136633d5dd5c5c0e6cb09d45dc605/column_sized_Screen_Shot_2016-04-12_at_4.11.05_PM.png)
