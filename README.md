<h1>Loading JSON Data</h1>

<p>JSON, or Javascript Object Notation, is a format for structuring data.  It lets you send data to and retrive data from a web application.  In this tutorial, we will learn how to retrieve weather data from any location around the world.</p>




<img src="images1/log.png" alt="api">


<p>First, go to <a>https://openweathermap.org/api</a>.  There are several options you can choose from, including 5 day forecasts, but today we'll find current weather data.  Click on "API doc"</p>
<img src="images1/openweather.png" alt="api">
<p>Copy the API call </p>
api.openweathermap.org/data/2.5/weather?q={city name},{country code}
<p></p>

<img src="images1/apicall.png" alt="api">

<p>Next, create an account with OpenWeatherMap to create your own API key.  After you've created an account, go to the "API keys" tab and find your API Key. </p>

<p><img src="images1/apikey.png" alt="api"></p>

<p>Next, create a new project.   I named my project "JSONDemo".  Open up MainActivity.java and enter the following code:</p>

```
public class DownloadTask extends AsyncTask<String, Void, String> {

        @Override
        protected String doInBackground(String... urls) {

            String result = "";
            URL url;
            HttpURLConnection urlConnection = null;

            try {
                url = new URL(urls[0]);

                urlConnection = (HttpURLConnection) url.openConnection();

                InputStream in = urlConnection.getInputStream();

                InputStreamReader reader = new InputStreamReader(in);

                int data = reader.read();

                while (data != -1) {

                    char current = (char) data;

                    result += current;

                    data = reader.read();

                }

                return result;

            } catch (MalformedURLException e) {
                e.printStackTrace();
            } catch (IOException e) {
                e.printStackTrace();
            }

            return null;
        }
```

<p></p>

```

```

```

```

```
@Override
        protected void onPostExecute(String result) {
            super.onPostExecute(result);

            try {

                JSONObject jsonObject = new JSONObject(result);

                String weatherInfo = jsonObject.getString("weather");

                Log.i("Weather content", weatherInfo);

                JSONArray arr = new JSONArray(weatherInfo);

                for (int i = 0; i < arr.length(); i++) {

                    JSONObject jsonPart = arr.getJSONObject(i);

                    Log.i("main", jsonPart.getString("main"));
                    Log.i("description", jsonPart.getString("description"));

                }


            } catch (JSONException e) {
                e.printStackTrace();
            }



        }
```

<p>This is where you'll be using the url and API Key you found earlier. Fill in the url below with the city and country code of the place you want to find weather info for and input your API Key after.
"http://api.openweathermap.org/data/2.5/weather?q=[CITY],[COUNTRY CODE]&&APPID=[YOUR API KEY] </p>

```
@Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        DownloadTask task = new DownloadTask();
        task.execute("http://api.openweathermap.org/data/2.5/weather?q=London,uk&&APPID=9975cd98166c97ff310adbab55fc70c8");

    }
```

<p>Next, in your AndroidManifest.xml file, be sure to include a line that gives you permission to access the internet in your app. </p>

```
 <uses-permission android:name="android.permission.INTERNET"
```

<p></p>

```

```

<p>Complete code for MainActivity.class is below:</p>

```
package com.example.zoe.jsondemo;

import android.app.Activity;
import android.os.AsyncTask;
import android.os.Bundle;
import android.util.Log;

import org.json.JSONArray;
import org.json.JSONException;
import org.json.JSONObject;

import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.net.HttpURLConnection;
import java.net.MalformedURLException;
import java.net.URL;


public class MainActivity extends Activity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        DownloadTask task = new DownloadTask();
        task.execute("http://api.openweathermap.org/data/2.5/weather?q=London,uk&&APPID=9975cd98166c97ff310adbab55fc70c8");

    }

    public class DownloadTask extends AsyncTask<String, Void, String> {

        @Override
        protected String doInBackground(String... urls) {

            String result = "";
            URL url;
            HttpURLConnection urlConnection = null;

            try {
                url = new URL(urls[0]);

                urlConnection = (HttpURLConnection) url.openConnection();

                InputStream in = urlConnection.getInputStream();

                InputStreamReader reader = new InputStreamReader(in);

                int data = reader.read();

                while (data != -1) {

                    char current = (char) data;

                    result += current;

                    data = reader.read();

                }

                return result;

            } catch (MalformedURLException e) {
                e.printStackTrace();
            } catch (IOException e) {
                e.printStackTrace();
            }

            return null;
        }

        @Override
        protected void onPostExecute(String result) {
            super.onPostExecute(result);

            try {

                JSONObject jsonObject = new JSONObject(result);

                String weatherInfo = jsonObject.getString("weather");

                Log.i("Weather content", weatherInfo);

                JSONArray arr = new JSONArray(weatherInfo);

                for (int i = 0; i < arr.length(); i++) {

                    JSONObject jsonPart = arr.getJSONObject(i);

                    Log.i("main", jsonPart.getString("main"));
                    Log.i("description", jsonPart.getString("description"));

                }


            } catch (JSONException e) {
                e.printStackTrace();
            }



        }
    }


}

```

<p></p>


<img src="images1/thumbsup.png" width="400" alt="api">
