# Firebase • Realtime Database • Cheat Sheet

## Inter Font
Include this stylesheet reference in your `<HEAD>` 

`<link rel="stylesheet" href="https://rsms.me/inter/inter.css">`

# URL Parameters
## Pretty Print

**Usage Example:** `https://[PROJECT_ID].firebaseio.com/[PROJECT_ID]/api/v1/apps.json?print=pretty`

**Response:**
```js
[ {
  "downloadUrl" : "https://s3.wasabisys.com/[PROJECT_ID]/scripts/InstallOrUpdateAdobePDFReader.ps1",
  "id" : "eej3ra4UCqylrRzfI7sQ",
  "logo" : "https://s3.wasabisys.com/[PROJECT_ID]/assets/img/logos/adobe-pdf-reader-128px.png",
  "name" : "Adobe PDF Reader DC",
  "version" : "20.013.20074"
}, {
  "downloadUrl" : "https://s3.wasabisys.com/[PROJECT_ID]/scripts/InstallOrUpdateGoogleChrome.ps1",
  "id" : "l7rGcmuCoRVZutC1HblH",
  "logo" : "https://s3.wasabisys.com/[PROJECT_ID]/assets/img/logos/google-chrome-128px.png",
  "name" : "Google Chrome",
  "version" : "87.0.4280.88"
}, {
  "downloadUrl" : "https://s3.wasabisys.com/[PROJECT_ID]/scripts/InstallOrUpdateMozillaFirefox.ps1",
  "id" : "oI9va9BrKhgKgNnbDOPb",
  "logo" : "https://s3.wasabisys.com/[PROJECT_ID]/assets/img/logos/mozilla-firefox-128px.png",
  "name" : "Mozilla Firefox",
  "version" : "84.0"
}, {
  "downloadUrl" : "https://s3.wasabisys.com/[PROJECT_ID]/scripts/InstallOrUpdateSpotify.ps1",
  "id" : "ZhCntZZqblpISkH5uokt",
  "logo" : "https://s3.wasabisys.com/[PROJECT_ID]/assets/img/logos/spotify-128px.png",
  "name" : "Spotify",
  "version" : "1.1.48.625"
} ]
```

---

## Download
Adding `download={{filename}}` query parameter will tell the browser to return the response as a file to download.

**Usage Example:** `https://[PROJECT_ID].firebaseio.com/[PROJECT_ID]/api/v1/apps.json?print=pretty&download=myapps.txt`

---

## Callback
Supported by `GET` only. To make REST calls from a web browser across domains, you can use `JSONP` to wrap the response in a JavaScript callback function. Add `callback=` to have the REST API wrap the returned data in the callback function you specify.

```js
<script>
  function gotData(data) {
    console.log(data);
  }
</script>
<script src="https://[PROJECT_ID].firebaseio.com/.json?callback=gotData"></script>
```

---

## Writing Server Values
Server values can be written at a location using a placeholder value, which is an object with a single ".sv" key. The value for that key is the type of server value we wish to set. For example, to set a timestamp when a user is created we could do the following:

```js
curl -X PUT -d '{".sv": "timestamp"}' 'https://docs-examples.firebaseio.com/alanisawesome/createdAt.json'
```

The only supported server value is `timestamp` and is the time since the UNIX epoch in milliseconds.

---

## Improving Write Performance
Improving Write Performance
If we're writing large amounts of data to the database, we can use the print=silent parameter to improve our write performance and decrease bandwidth usage. In the normal write behavior, the server responds with the JSON data that was written. When print=silent is specified, the server immediately closes the connection once the data is received, reducing bandwidth usage.

In cases where we're making many requests to the database, we can re-use the HTTPS connection by sending a Keep-Alive request in the HTTP header.
