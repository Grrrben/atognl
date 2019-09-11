---
title: A BroadcastReceiver in your Activity
author: grrrben
type: post
date: 2015-11-11T22:30:25+00:00
url: /blog/a-broadcastreceiver-in-your-androidactivity/
categories:
  - android
tags:
  - android
  - asynctask
  - broadcastreceiver
  - java

---
In developing an application which content is all based on a RESTfull api, I needed a solid listener for the network connection of the used device. No internet equals no content, so the app had to tell the user about that and more importantly &#8211; react if the users decides to activate the connection.

It&#8217;s done in a process that took a while to grasp and build in my app, so let&#8217;s get into a bit more detail. Network connectivity is a system wide event, managed outside your app itself, therefore, if you want to react to it you need to implement a BroadcastReceiver. A BroadcastReceiver is a class that receives calls from Android system, system-to-app communication if you like. The BroadcastReceiver is often placed in the manifest, however, I needed a more hands-on approach for my app functionality. Basically, the BroadcastReceiver has to call a method in my AndroidActivity class as soon as a network connection is available.

In most of the applications Activity-classes an AsyncTask is used to communicatie with the API. If you write it down, it&#8217;s really quite simpel:

Is there a network connection?
  
&#8211; Yes; talk with the API.
  
&#8211; No; inform the user and wait for the connection. Then talk to the API.

Communication with the API is done by an [AsyncTask][1] inner class which is housed in an abstract baseclass as I need it often in the activities.

In a bit more detail:

<pre><code class="java">
protected class CallAPI extends AsyncTask&lt;String, Void, Void&gt; {
	// see http://developer.android.com/reference/android/os/AsyncTask.html
}
&lt;/pre>
&lt;p></code><br />
Which is called by method startAsyncTask():</p>


<pre><code class="java">
protected final void startAsyncTask() {
	new CallAPI(this).execute(urlString); // urlString is a protected String
}
</code></pre>


<p>
  Now let's talk about the 'No. No internet' option. In order to call the startAsyncTask method as soon as an internet connection becomes available you need the define a BroadcastReceiver in the Activity class itself:
</p>


<pre><code class="java">
protected final BroadcastReceiver networkStateReceiver = new BroadcastReceiver() {
	// protected as it's defined in the base class.
	@Override
	public void onReceive(Context context, Intent intent) {
		if (intent.getExtras() != null) {

			final ConnectivityManager connectivityManager = (ConnectivityManager)context.getSystemService(Context.CONNECTIVITY_SERVICE);
			final NetworkInfo networkInfo = connectivityManager.getActiveNetworkInfo();

			if (networkInfo != null && networkInfo.isConnectedOrConnecting()) {
				Log.i("BroadcastReceiver", "Network " + networkInfo.getTypeName() + " available!");
				startAsyncTask();
			}
		}
	}
};
</code></pre>


<p>
  And finally, simpel as it is, check the network connection in your onCreate (or other) method. If it's available, just start up your AsyncTask. If not, register your BroadcastReceiver (which we defined as networkStateReceiver) and forget about it for the moment. Let you app handle it as soon as a network is available.
</p>


<pre><code class="java">
if(isConnected) {
	// Connection available
	// just start the download
	startAsyncTask();

} else {
	// No connection yet
	// inform you user with a Toast or the preferred Snackbar message
	// Register the BroadcastReceiver to fetch your data later
	registerReceiver(networkStateReceiver, new IntentFilter(ConnectivityManager.CONNECTIVITY_ACTION));
}
</code></pre>


<p>
  A last tip. A BroadcastReceiver uses resources of the users device. Karma tell's you to unregister it when it's not needed (i.e. onPause).
</p>


<pre><code class="java">unregisterReceiver(networkStateReceiver);</code></pre>

 [1]: http://developer.android.com/reference/android/os/AsyncTask.html