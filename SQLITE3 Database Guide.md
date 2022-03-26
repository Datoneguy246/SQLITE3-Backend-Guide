# SQLITE3 Database Guide

## Step 1: Creating/formating a database:
You'll need an actual file to act as your database (unless you have MySQL configured with your hosting service). You can download *DB Browser for SQLite* at: https://sqlitebrowser.org/.

Once in the app, you can create a new database by clicking "New Database" at the top:
![Demo](/img/1.png "Nothing to see here")

You can then create a table to store your data by clicking "Create Table" and populating it with your required fields:
![Demo](/img/2.png "Nothing to see here")

Once that is done, click "Write Changes" to save your database.

## Step 2: Adding web-request functionallity to your project:
Here's a java function I wrote to send web-requests.
```
import java.io.IOException;
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.net.HttpURLConnection;
import java.net.URL;

public class WebRequest {
    public static String GET(String url) throws IOException {
        URL fUrl = new URL(url);
        HttpURLConnection connection = (HttpURLConnection) fUrl.openConnection();
        connection.setRequestMethod("GET");
        BufferedReader in = new BufferedReader(
                new InputStreamReader(connection.getInputStream()));
        String inputLine;
        StringBuilder webContents = new StringBuilder();
        while ((inputLine = in.readLine()) != null) {
            webContents.append(inputLine);
        }
        in.close();
        connection.disconnect();
        return webContents.toString();
    }
}
```
With this, you can send web-requests to different websites/urls, which you can use on your web-server to trigger PHP scripts and interact with the database.

## Step 3: Writing PHP scripts to interact with your database.
You can place PHP scripts on the web-server which can be triggered by sending a web-request to the URL that the script is located. In these scripts, you can pass parameters into your code within the URL. For example, if your script is located at 
```
http://localhost:3000/script.php
```

You can pass a vairable called "test" into it by calling the url:
```
http://localhost:3000/script.php?test=VALUE
```
And you can get that value in your script by writing:
```
$variable = $_GET["test"];
```

You get a refrence to your database by writing:
```
$db = new SQLite3('PATH_TO_YOUR_DATABASE');
```
And you can query your database by writing:
```
$result = $db->query("YOUR_SQL_QUERY_HERE")->fetchArray();
```

If you need to send data from your database to your client, you can *echo* the data you need to send, and the web-request will recieve that data as a string variable.