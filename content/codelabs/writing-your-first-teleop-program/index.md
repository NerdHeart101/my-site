
<!doctype html>

<html>
<head>
  <meta name="viewport" content="width=device-width, minimum-scale=1.0, initial-scale=1.0, user-scalable=yes">
  <meta name="theme-color" content="#4F7DC9">
  <meta charset="UTF-8">
  <title>Writing Your First TeleOp Program</title>
  <link rel="stylesheet" href="//fonts.googleapis.com/css?family=Source+Code+Pro:400|Roboto:400,300,400italic,500,700|Roboto+Mono">
  <link rel="stylesheet" href="//fonts.googleapis.com/icon?family=Material+Icons">
  <link rel="stylesheet" href="https://storage.googleapis.com/codelab-elements/codelab-elements.css">
  <style>
    .success {
      color: #1e8e3e;
    }
    .error {
      color: red;
    }
  </style>
</head>
<body>
  <google-codelab-analytics gaid="UA-49880327-14"></google-codelab-analytics>
  <google-codelab codelab-gaid=""
                  id="writing-your-first-teleop-program"
                  title="Writing Your First TeleOp Program"
                  environment="web"
                  feedback-link="">
    
      <google-codelab-step label="Introduction" duration="0">
        <p class="image-container"><img style="width: 624.00px" src="img\\782be249282e2b7c.jpeg"></p>
<h2 is-upgraded><strong>What makes a web app, a Progressive Web App?</strong></h2>
<p>Progressive Web Apps provide an installable, app-like experience on desktop and mobile that are built and delivered directly via the web. They&#39;re web apps that are fast and reliable. And most importantly, they&#39;re web apps that work in any browser. If you&#39;re building a web app today, you&#39;re already on the path towards building a Progressive Web App.</p>
<h3 is-upgraded><strong>Fast &amp; Reliable</strong></h3>
<p>Every web experience must be fast, and this is especially true for Progressive Web Apps. Fast refers to the time it takes to get meaningful content on screen, and provide an interactive experience in less than 5 seconds.</p>
<p>And, it must be <strong>reliably fast</strong>. It&#39;s hard to stress enough how much better reliable performance is. Think of it this way: the first load of a native app is frustrating. It&#39;s gated by an app store and a huge download, but once you get to a point where the app is installed, that up-front cost is amortized across all app starts, and none of those starts have a variable delay. Each application start is as fast as the last, no variance. A Progressive Web App must deliver this reliable performance that users have come to expect from any installed experience.</p>
<h3 is-upgraded><strong>Installable</strong></h3>
<p>Progressive Web Apps can run in a browser tab, but are also installable. Bookmarking a site just adds a shortcut, but an installed Progressive Web App looks and behaves like all of the other installed apps. It launches from the same place as other apps launch. You can control the launch experience, including a customized splash screen, icons and more. It runs as an app, in an app window without an address bar or other browser UI. And like all other installed apps, it&#39;s a top level app in the task switcher.</p>
<p>Remember, it&#39;s critical that an installable PWA is fast and reliable. Users who install a PWA expect that their apps work, no matter what kind of network connection they&#39;re on. It&#39;s a baseline expectation that must be met by every installed app.</p>
<h3 is-upgraded><strong>Mobile &amp; Desktop</strong></h3>
<p>Using responsive design techniques, Progressive Web Apps work on both mobile <strong>and</strong> desktop, using a single code base between platforms. If you&#39;re considering writing a native app, take a look at the benefits that a PWA offers.</p>
<h2 is-upgraded><strong>What you&#39;ll build</strong></h2>
<p>In this codelab, you&#39;re going to build a weather web app using Progressive Web App techniques. Your app will:</p>
<ul>
<li>Use responsive design, so it works on desktop or mobile.</li>
<li>Be fast, using a service worker to precache the app resources (HTML, CSS, JavaScript, images) needed to run, and cache the weather data at runtime to improve performance.</li>
<li>Be installable, using a web app manifest and the <code>beforeinstallprompt</code> event to notify the user it&#39;s installable.</li>
</ul>
<p class="image-container"><img style="width: 624.00px" src="img\\c3f97a71ceb62d00.png"></p>
<aside class="warning"><p><strong>Note:</strong> To simplify this codelab, and explain the fundamentals of providing an offline experience, we&#39;re using vanilla JavaScript. In a production app, we strongly recommend using tools like <a href="https://developers.google.com/web/tools/workbox/" target="_blank">Workbox</a> to build your service worker. It removes many of the sharp edges and dark corners you may run into.</p>
</aside>
<h2 class="checklist" is-upgraded><strong>What you&#39;ll learn</strong></h2>
<ul class="checklist">
<li>How to create and add a web app manifest</li>
<li>How to provide a simple offline experience</li>
<li>How to provide a full offline experience</li>
<li>How to make your app installable</li>
</ul>
<p>This codelab is focused on Progressive Web Apps. Non-relevant concepts and code blocks are glossed over and are provided for you to simply copy and paste.</p>
<h2 is-upgraded><strong>What you&#39;ll need</strong></h2>
<ul>
<li>A recent version of Chrome (74 or later)<br>PWAs are just web apps, and work in all browsers, but we&#39;ll be using a few features of the Chrome DevTools to better understand what&#39;s happening at the browser level, and use it to test the install experience.</li>
<li>Knowledge of HTML, CSS, JavaScript, and <a href="https://developer.chrome.com/devtools" target="_blank">Chrome DevTools</a>.</li>
</ul>


      </google-codelab-step>
    
      <google-codelab-step label="Getting set up" duration="2">
        <h2 is-upgraded><strong>Get a key for the Dark Sky API</strong></h2>
<p>Our weather data comes from the <a href="https://darksky.net/dev" target="_blank">Dark Sky API</a>. In order to use it, you&#39;ll need to request an API key. It&#39;s easy to use, and free for non-commercial projects.</p>
<p><a href="https://darksky.net/dev/register" target="_blank"><paper-button class="colored" raised>Register for API Key</paper-button></a></p>
<aside class="special"><p><strong>Note:</strong> You can still complete this codelab without a Dark Sky API key. If our server is unable to get real data from the Dark Sky API, it will return fake data instead.</p>
</aside>
<h3 is-upgraded><strong>Verify your API key is working properly</strong></h3>
<p>To test that your API Key is working properly, make an HTTP request to the DarkSky API. Update the URL below to replace <code>DARKSKY_API_KEY</code> with your API key. If everything works, you should see the latest weather forecast for New York City.</p>
<p><code>https://api.darksky.net/forecast/DARKSKY_API_KEY/40.7720232,-73.9732319</code></p>
<aside class="warning"><p><strong>Caution:</strong> Protect you API Keys like you protect your passwords. You should <strong>never</strong> check your API Key into a source repository, post it online, or share it. If your API Key is used improperly, it may result in it being revoked, or more. </p>
</aside>
<h2 is-upgraded><strong>Get the code</strong></h2>
<p>We&#39;ve put everything you need for this project into a Git repo. To get started, you&#39;ll need to grab the code and open it in your favorite dev environment. For this codelab, we recommend using Glitch.</p>
<h3 is-upgraded><strong>Strongly Recommended: Use Glitch to import the repo</strong></h3>
<p>Using Glitch is the recommended method for working through this codelab.</p>
<ol type="1" start="1">
<li>Open a new browser tab and go to <a href="https://glitch.com" target="_blank">https://glitch.com</a>.</li>
<li>If you don&#39;t have an account, you&#39;ll need to sign up.</li>
<li>Click <strong>New Project</strong>, then <strong>Clone from Git Repo.</strong></li>
<li>Clone <strong>https://github.com/googlecodelabs/your-first-pwapp.git</strong> and click OK.</li>
<li>Once the repo has loaded, edit the <code>.env</code> file, and update it with your DarkSky API key.</li>
<li>Click the <strong>Show Live</strong> button to see the PWA in action.</li>
</ol>
<h3 is-upgraded><strong>Alternative: Download code &amp; work locally</strong></h3>
<p>If you want to download the code and work locally, you&#39;ll need to have a recent version of Node, and code editor setup and ready to go. </p>
<aside class="warning"><p><strong>Caution:</strong> If you work locally, some of the Lighthouse audits won&#39;t pass, and installation may not be available because the local server doesn&#39;t serve the content over a secure context.</p>
</aside>
<p><a href="https://github.com/googlecodelabs/your-first-pwapp/archive/master.zip" target="_blank"><paper-button class="colored" raised><iron-icon icon="file-download"></iron-icon>Download source code</paper-button></a></p>
<ol type="1" start="1">
<li>Unpack the downloaded zip file.</li>
<li>Run <code>npm install</code> to install the dependencies required to run the server.</li>
<li>Edit <code>server.js</code> and set your DarkSky API key.</li>
<li>Run <code>node server.js</code> to start the server on port 8000.</li>
<li>Open a browser tab to <a href="http://localhost:8000" target="_blank">http://localhost:8000</a>  </li>
</ol>


      </google-codelab-step>
    
      <google-codelab-step label="Establish a baseline" duration="2">
        <h2 is-upgraded><strong>What&#39;s our starting point?</strong></h2>
<p>Our starting point is a basic weather app designed for this codelab. The code has been overly simplified to show the concepts in this codelab, and it has little error handling. If you choose to reuse any of this code in a production app, make sure that you handle any errors and fully test all code.</p>
<p>Some things to try...</p>
<ol type="1" start="1">
<li>Add a new city with the blue plus button in the bottom right corner.</li>
<li>Refresh the data with the refresh button in the upper right corner.</li>
<li>Delete a city using the x on the upper right of each city card.</li>
<li>See how it works on desktop and mobile.</li>
<li>See what happens when you go offline.</li>
<li>Using Chrome&#39;s Network panel, see what happens when the network is throttled to Slow 3G.</li>
<li>Add a delay to the forecast server by changing <code>FORECAST_DELAY</code> in <code>server.js</code></li>
</ol>
<h2 is-upgraded><strong>Audit with Lighthouse</strong></h2>
<p><a href="https://developers.google.com/web/tools/lighthouse/#devtools" target="_blank">Lighthouse</a> is an easy to use tool to help improve the quality of your sites and pages. It has audits for performance, accessibility, progressive web apps, and more. Each audit has a reference doc explaining why the audit is important, as well as how to fix it.</p>
<p class="image-container"><img style="width: 624.00px" src="img\\570483e9d82fa4a4.png"></p>
<p>We&#39;ll use Lighthouse to audit our Weather app, and verify the changes we&#39;ve made.</p>
<aside class="special"><p><strong>Tip: </strong>You can run Lighthouse in Chrome DevTools, from the command line, or as a Node module. Consider <a href="https://github.com/GoogleChromeLabs/lighthousebot" target="_blank">adding Lighthouse</a> to your build process to make sure your web app doesn&#39;t regress. </p>
</aside>
<h2 is-upgraded><strong>Let&#39;s </strong><strong>run Lighthouse</strong></h2>
<ol type="1" start="1">
<li>Open your project in a new tab.</li>
<li>Open Chrome DevTools and switch to the <strong>Audits</strong> tab, DevTools shows a list of audit categories, leave them all enabled.</li>
<li>Click <strong>Run audits</strong>, after 60-90 seconds, Lighthouse gives you a report on the page.</li>
</ol>
<h2 is-upgraded><strong>The Progressive Web App Audit</strong></h2>
<p>We&#39;re going to focus on the results of the Progressive Web App audit. </p>
<p class="image-container"><img style="width: 624.00px" src="img\\3a17a6eb7d917893.png"></p>
<p>And there&#39;s a lot of red to focus on:</p>
<ul>
<li><strong>❗FAILED:</strong> Current page does not respond with a 200 when offline.</li>
<li><strong>❗FAILED: </strong><code>start_url</code> does not respond with a 200 when offline.</li>
<li><strong>❗FAILED:</strong> Does not register a service worker that controls page and <code>start_url.</code></li>
<li><strong>❗FAILED:</strong> Web app manifest does not meet the installability requirements.</li>
<li><strong>❗FAILED:</strong> Is not configured for a custom splash screen.</li>
<li><strong>❗FAILED:</strong> Does not set an address-bar theme color.</li>
</ul>
<p>Let&#39;s jump in and start fixing some of these issues!</p>


      </google-codelab-step>
    
      <google-codelab-step label="Add a web app manifest" duration="3">
        <p>By the end of this section, our weather app will pass the following audits:</p>
<ul>
<li>Web app manifest does not meet the installability requirements.</li>
<li>Is not configured for a custom splash screen.</li>
<li>Does not set an address-bar theme color.</li>
</ul>
<h2 is-upgraded><strong>Create the web app manifest</strong></h2>
<p>The <a href="https://developers.google.com/web/fundamentals/web-app-manifest" target="_blank">web app manifest</a> is a simple JSON file that gives you, the developer, the ability to control how your app appears to the user.</p>
<p>Using the web app manifest, your web app can:</p>
<ul>
<li>Tell the browser you want your app to open in a standalone window (<code>display</code>).</li>
<li>Define what page is opened when the app is first launched (<code>start_url</code>).</li>
<li>Define what the app should look like on the dock or app launcher (<code>short_name</code>, <code>icons</code>).</li>
<li>Create a splash screen (<code>name</code>, <code>icons</code>, <code>colors</code>).</li>
<li>Tell the browser to open the window in landscape, or portrait mode (<code>orientation</code>).</li>
<li>And <a href="https://developer.mozilla.org/en-US/docs/Web/Manifest#Members" target="_blank">plenty more</a>.</li>
</ul>
<p>Create a file named <code>public/manifest.json</code> in your project and copy/paste the following contents:</p>
<p><code>public/manifest.json</code></p>
<pre><code>{
  &#34;name&#34;: &#34;Weather&#34;,
  &#34;short_name&#34;: &#34;Weather&#34;,
  &#34;icons&#34;: [{
    &#34;src&#34;: &#34;/images/icons/icon-128x128.png&#34;,
      &#34;sizes&#34;: &#34;128x128&#34;,
      &#34;type&#34;: &#34;image/png&#34;
    }, {
      &#34;src&#34;: &#34;/images/icons/icon-144x144.png&#34;,
      &#34;sizes&#34;: &#34;144x144&#34;,
      &#34;type&#34;: &#34;image/png&#34;
    }, {
      &#34;src&#34;: &#34;/images/icons/icon-152x152.png&#34;,
      &#34;sizes&#34;: &#34;152x152&#34;,
      &#34;type&#34;: &#34;image/png&#34;
    }, {
      &#34;src&#34;: &#34;/images/icons/icon-192x192.png&#34;,
      &#34;sizes&#34;: &#34;192x192&#34;,
      &#34;type&#34;: &#34;image/png&#34;
    }, {
      &#34;src&#34;: &#34;/images/icons/icon-256x256.png&#34;,
      &#34;sizes&#34;: &#34;256x256&#34;,
      &#34;type&#34;: &#34;image/png&#34;
    }, {
      &#34;src&#34;: &#34;/images/icons/icon-512x512.png&#34;,
      &#34;sizes&#34;: &#34;512x512&#34;,
      &#34;type&#34;: &#34;image/png&#34;
    }],
  &#34;start_url&#34;: &#34;/index.html&#34;,
  &#34;display&#34;: &#34;standalone&#34;,
  &#34;background_color&#34;: &#34;#3E4EB8&#34;,
  &#34;theme_color&#34;: &#34;#2F3BA2&#34;
}</code></pre>
<p>The manifest supports an array of icons, intended for different screen sizes. For this code lab, we&#39;ve included a few others since we needed them for our iOS integration.</p>
<aside class="special"><p><strong>TIP:</strong> To be installable, Chrome requires that you provide at least a 192x192px icon and a 512x512px icon. But you can also provide other sizes. Chrome uses the icon closest to 48dp, for example, 96px on a 2x device or 144px for a 3x device.</p>
</aside>
<h2 is-upgraded><strong>Add a link to the web app manifest</strong></h2>
<p>Next, we need to tell the browser about our manifest by adding a <code><link rel="manifest"...</code>  to each page in our app. Add the following line to the <code><head></code> element in your <code>index.html</code> file. </p>
<h3 is-upgraded><a href="https://github.com/googlecodelabs/your-first-pwapp/blob/master/public/index.html#L30" target="_blank"><strong>public/index.html</strong></a></h3>
<pre><code>&lt;!-- CODELAB: Add link rel manifest --&gt;
&lt;link rel=&#34;manifest&#34; href=&#34;/manifest.json&#34;&gt;</code></pre>
<h3 is-upgraded><strong>DevTools Detour</strong></h3>
<p>DevTools provides a quick, easy way to check your <code>manifest.json</code> file. Open up the <strong>Manifest</strong> pane on the <strong>Application</strong> panel. If you&#39;ve added the manifest information correctly, you&#39;ll be able to see it parsed and displayed in a human-friendly format on this pane.</p>
<p class="image-container"><img style="width: 624.00px" src="img\\ef3d17ae711968d7.png"></p>
<h2 is-upgraded><strong>Add iOS meta tags &amp; icons</strong></h2>
<p>Safari on iOS doesn&#39;t support the web app manifest (<a href="https://webkit.org/status/#specification-web-app-manifest" target="_blank">yet</a>), so you&#39;ll need to add the <a href="https://developer.apple.com/library/archive/documentation/AppleApplications/Reference/SafariWebContent/ConfiguringWebApplications/ConfiguringWebApplications.html" target="_blank">traditional <code>meta</code> tags</a> to the <code><head></code> of your <code>index.html</code> file:</p>
<h3 is-upgraded><a href="https://github.com/googlecodelabs/your-first-pwapp/blob/master/public/index.html#L31" target="_blank"><strong>public/index.html</strong></a></h3>
<pre><code>&lt;!-- CODELAB: Add iOS meta tags and icons --&gt;
&lt;meta name=&#34;apple-mobile-web-app-capable&#34; content=&#34;yes&#34;&gt;
&lt;meta name=&#34;apple-mobile-web-app-status-bar-style&#34; content=&#34;black&#34;&gt;
&lt;meta name=&#34;apple-mobile-web-app-title&#34; content=&#34;Weather PWA&#34;&gt;
&lt;link rel=&#34;apple-touch-icon&#34; href=&#34;/images/icons/icon-152x152.png&#34;&gt;</code></pre>
<h2 is-upgraded><strong>Bonus: Easy Lighthouse fixes</strong></h2>
<p>Our Lighthouse audit called out a few other things that are pretty easy to fix, so let&#39;s take care of those while we&#39;re here.</p>
<h3 is-upgraded><strong>Set the meta description</strong></h3>
<p>Under the SEO audit, Lighthouse noted our &#34;<a href="https://developers.google.com/web/tools/lighthouse/audits/description" target="_blank">Document does not have a meta description.</a>&#34; Descriptions can be displayed in Google&#39;s search results. High-quality, unique descriptions can make your results more relevant to search users and can increase your search traffic.</p>
<p>To add a description, add the following <code>meta</code> tag to the <code><head></code> of your document:</p>
<h3 is-upgraded><a href="https://github.com/googlecodelabs/your-first-pwapp/blob/master/public/index.html#L32" target="_blank"><strong>public/index.html</strong></a></h3>
<pre><code>&lt;!-- CODELAB: Add description here --&gt;
&lt;meta name=&#34;description&#34; content=&#34;A sample weather app&#34;&gt;</code></pre>
<h3 is-upgraded><strong>Set the address bar theme color</strong></h3>
<p>In the PWA audit, Lighthouse noted our app &#34;<a href="https://developers.google.com/web/tools/lighthouse/audits/address-bar" target="_blank">Does not set an address-bar theme color</a>&#34;. Theming the browser&#39;s address bar to match your brand&#39;s colors provides a more immersive user experience.</p>
<p>To set the theme color on mobile, add the following <code>meta</code> tag to the <code><head></code> of your document:</p>
<h3 is-upgraded><a href="https://github.com/googlecodelabs/your-first-pwapp/blob/master/public/index.html#L33" target="_blank"><strong>public/index.html</strong></a></h3>
<pre><code>&lt;!-- CODELAB: Add meta theme-color --&gt;
&lt;meta name=&#34;theme-color&#34; content=&#34;#2F3BA2&#34; /&gt;</code></pre>
<h2 is-upgraded><strong>Verify changes with Lighthouse</strong></h2>
<p>Run Lighthouse again (by clicking on the + sign in the upper left corner of the Audits pane) and verify your changes.</p>
<p><strong>SEO Audit</strong></p>
<ul>
<li><strong>✅ PASSED:</strong> Document has a meta description. </li>
</ul>
<p><strong>Progressive Web App Audit</strong></p>
<ul>
<li><strong>❗FAILED:</strong> Current page does not respond with a 200 when offline.</li>
<li><strong>❗FAILED: </strong><code>start_url</code> does not respond with a 200 when offline.</li>
<li><strong>❗FAILED:</strong> Does not register a service worker that controls page and <code>start_url.</code></li>
<li><strong>✅ PASSED:</strong> Web app manifest meets the installability requirements.</li>
<li><strong>✅ PASSED:</strong> Configured for a custom splash screen.</li>
<li><strong>✅ PASSED:</strong> Sets an address-bar theme color.</li>
</ul>


      </google-codelab-step>
    
      <google-codelab-step label="Provide a basic offline experience" duration="3">
        <p>There is an expectation from users that installed apps will always have a baseline experience if they&#39;re offline. That&#39;s why it&#39;s critical for installable web apps to never show Chrome&#39;s offline dinosaur. The offline experience can range from a simple, offline page, to a read-only experience with previously cached data, all the way to a fully functional offline experience that automatically syncs when the network connection is restored.</p>
<p>In this section, we&#39;re going to add a simple offline page to our weather app. If the user tries to load the app while offline, it&#39;ll show our custom page, instead of the typical offline page that the browser shows. By the end of this section, our weather app will pass the following audits:</p>
<ul>
<li>Current page does not respond with a 200 when offline.</li>
<li><code>start_url</code> does not respond with a 200 when offline.</li>
<li>Does not register a service worker that controls page and <code>start_url.</code></li>
</ul>
<p>In the next section, we&#39;ll replace our custom offline page with a full offline experience. This will improve the offline experience, but more importantly, it&#39;ll significantly improve our performance, because most of our assets (HTML, CSS and JavaScript) will be stored and served locally, eliminating the network as a potential bottleneck.</p>
<h2 is-upgraded><strong>Service workers to the rescue</strong></h2>
<p>If you&#39;re unfamiliar with service workers, you can get a basic understanding by reading <a href="https://developers.google.com/web/fundamentals/primers/service-worker/" target="_blank">Introduction To Service Workers</a> about what they can do, how their lifecycle works and more. Once you&#39;ve completed this code lab, be sure to check out the <a href="http://goo.gl/jhXCBy" target="_blank">Debugging Service Workers code lab</a> for a more in-depth look at how to work with service workers.</p>
<p>Features provided via service workers should be considered a progressive enhancement, and added only if supported by the browser. For example, with service workers you can cache the <a href="https://developers.google.com/web/fundamentals/architecture/app-shell" target="_blank">app shell</a> and data for your app, so that it&#39;s available even when the network isn&#39;t. When service workers aren&#39;t supported, the offline code isn&#39;t called, and the user gets a basic experience. Using feature detection to provide progressive enhancement has little overhead and it won&#39;t break in older browsers that don&#39;t support that feature.</p>
<aside class="warning"><p><strong>Remember:</strong> Service worker functionality is only available on pages that are accessed via HTTPS (http://localhost and equivalents will also work to facilitate testing).</p>
</aside>
<h2 is-upgraded><strong>Register the service worker</strong></h2>
<p>The first step is to register the service worker. Add the following code to your <code>index.html</code> file:</p>
<h3 is-upgraded><a href="https://github.com/googlecodelabs/your-first-pwapp/blob/master/public/index.html#L206" target="_blank"><strong>public/index.html</strong></a></h3>
<pre><code>// CODELAB: Register service worker.
if (&#39;serviceWorker&#39; in navigator) {
  window.addEventListener(&#39;load&#39;, () =&gt; {
    navigator.serviceWorker.register(&#39;/service-worker.js&#39;)
        .then((reg) =&gt; {
          console.log(&#39;Service worker registered.&#39;, reg);
        });
  });
}</code></pre>
<p>This code checks to see if the service worker API is available, and if it is, the service worker at <code>/service-worker.js</code> is registered once the page is <a href="https://developers.google.com/web/fundamentals/primers/service-workers/registration" target="_blank">loaded</a>.</p>
<p>Note, the service worker is served from the root directory, not from a <code>/scripts/</code> directory. This is the easiest way to set the <strong><code>scope</code></strong> of your service worker. The <code>scope</code> of the service worker determines which files the service worker controls, in other words, from which path the service worker will intercept requests. The default <code>scope</code> is the location of the service worker file, and extends to all directories below. So if <code>service-worker.js</code> is located in the root directory, the service worker will control requests from all web pages at this domain.</p>
<h2 is-upgraded><strong>Precache offline page</strong></h2>
<p>First, we need to tell the service worker what to cache. We&#39;ve already created a simple <a href="https://your-first-pwa.glitch.me/offline.html" target="_blank">offline page</a> (<code>public/offline.html</code>) that we&#39;ll display any time there&#39;s no network connection.</p>
<p>In your <code>service-worker.js</code>, add <code>'/offline.html',</code> to the <code>FILES_TO_CACHE</code> array, the final result should look like this:</p>
<h3 is-upgraded><a href="https://github.com/googlecodelabs/your-first-pwapp/blob/master/public/service-worker.js#L23" target="_blank"><strong>public/service-worker.js</strong></a></h3>
<pre><code>// CODELAB: Update cache names any time any of the cached files change.
const FILES_TO_CACHE = [
  &#39;/offline.html&#39;,
];</code></pre>
<p>Next, we need to update the <code>install</code> event to tell the service worker to pre-cache the offline page:</p>
<h3 is-upgraded><a href="https://github.com/googlecodelabs/your-first-pwapp/blob/master/public/service-worker.js#L29" target="_blank"><strong>public/service-worker.js</strong></a></h3>
<pre><code>// CODELAB: Precache static resources here.
evt.waitUntil(
    caches.open(CACHE_NAME).then((cache) =&gt; {
      console.log(&#39;[ServiceWorker] Pre-caching offline page&#39;);
      return cache.addAll(FILES_TO_CACHE);
    })
);</code></pre>
<aside class="special"><p><strong>Note:</strong> Service worker events and life cycle is covered in the next section.</p>
</aside>
<p>Our <code>install</code> event now opens the cache with <code>caches.open()</code> and provides a cache name. Providing a cache name allows us to version files, or separate data from the cached resources so that we can easily update one but not affect the other.</p>
<p>Once the cache is open, we can then call <code>cache.addAll()</code>, which takes a list of URLs, fetches them from the server and adds the response to the cache. Note that <code>cache.addAll()</code> will reject if any of the individual requests fail. That means you&#39;re guaranteed that, if the install step succeeds, you cache will be in a consistent state. But, if it fails for some reason, it will automatically try again the next time the service worker starts up.</p>
<h3 is-upgraded><strong>DevTools Detour</strong></h3>
<p>Let&#39;s take a look at how you can use DevTools to understand and debug service workers. Before reloading your page, open up DevTools, go the <strong>Service Workers</strong> pane on the <strong>Application</strong> panel. It should look like this:</p>
<p class="image-container"><img style="width: 624.00px" src="img\\c29355c1fa2b9f69.png"></p>
<p>When you see a blank page like this, it means that the currently open page does not have any registered service workers.</p>
<p>Now, reload your page. The Service Workers pane should now look like this:</p>
<p class="image-container"><img style="width: 624.00px" src="img\\7524e1e308754830.png"></p>
<p>When you see information like this, it means the page has a service worker running. </p>
<p>Next to the Status label, there&#39;s a number (<em>34251</em> in this case), keep an eye on that number as you&#39;re working with service workers. It&#39;s an easy way to tell if your service worker has been updated.</p>
<h2 is-upgraded><strong>Clean-up old offline pages</strong></h2>
<p>We&#39;ll use the <code>activate</code> event to clean up any old data in our cache. This code ensures that your service worker updates its cache whenever any of the app shell files change. In order for this to work, you&#39;d need to increment the <code>CACHE_NAME</code> variable at the top of your service worker file.</p>
<p>Add the following code to your <code>activate</code> event:</p>
<h3 is-upgraded><a href="https://github.com/googlecodelabs/your-first-pwapp/blob/master/public/service-worker.js#L36" target="_blank"><strong>public/service-worker.js</strong></a></h3>
<pre><code>// CODELAB: Remove previous cached data from disk.
evt.waitUntil(
    caches.keys().then((keyList) =&gt; {
      return Promise.all(keyList.map((key) =&gt; {
        if (key !== CACHE_NAME) {
          console.log(&#39;[ServiceWorker] Removing old cache&#39;, key);
          return caches.delete(key);
        }
      }));
    })
);</code></pre>
<h3 is-upgraded><strong>DevTools Detour</strong></h3>
<p>With the Service Workers pane open, refresh the page, you&#39;ll see the new service worker installed, and the status number increment.</p>
<p class="image-container"><img style="width: 624.00px" src="img\\3da7cf4c03c1b9dc.png"></p>
<p>The updated service worker takes control immediately because our <code>install</code> event finishes with <a href="https://developers.google.com/web/fundamentals/primers/service-workers/lifecycle#skip_the_waiting_phase" target="_blank"><code>self.skipWaiting()</code></a>, and the <code>activate</code> event finishes with <a href="https://developers.google.com/web/fundamentals/primers/service-workers/lifecycle#clientsclaim" target="_blank"><code>self.clients.claim()</code></a>. Without those, the old service worker would continue to control the page as long as there is a tab open to the page.</p>
<h2 is-upgraded><strong>Handle failed network requests</strong></h2>
<p>And finally, we need to handle <code>fetch</code> events. We&#39;re going to use a <a href="https://developers.google.com/web/fundamentals/instant-and-offline/offline-cookbook/#network-falling-back-to-cache" target="_blank">network, falling back to cache strategy</a>. The service worker will first try to fetch the resource from the network, if that fails, it will return the offline page from the cache.</p>
<p class="image-container"><img style="width: 624.00px" src="img\\b20b3e47d24c240b.png"></p>
<h3 is-upgraded><a href="https://github.com/googlecodelabs/your-first-pwapp/blob/master/public/service-worker.js#L43" target="_blank"><strong>public/service-worker.js</strong></a></h3>
<pre><code>// CODELAB: Add fetch event handler here.
if (evt.request.mode !== &#39;navigate&#39;) {
  // Not a page navigation, bail.
  return;
}
evt.respondWith(
    fetch(evt.request)
        .catch(() =&gt; {
          return caches.open(CACHE_NAME)
              .then((cache) =&gt; {
                return cache.match(&#39;offline.html&#39;);
              });
        })
);</code></pre>
<p>The <code>fetch</code> handler only needs to handle page navigations, so other requests can be dumped out of the handler and will be dealt with normally by the browser.  But, if the request <code>.mode</code> is <code>navigate</code>, use <code>fetch</code> to try to get the item from the network. If it fails, the <code>catch</code> handler opens the cache with <code>caches.open(CACHE_NAME)</code> and uses <code>cache.match('offline.html')</code> to get the precached offline page. The result is then passed back to the browser using <code>evt.respondWith()</code>. </p>
<aside class="special"><p>Wrapping the <code>fetch</code> call in <a href="https://developer.mozilla.org/en-US/docs/Web/API/FetchEvent/respondWith" target="_blank"><code>evt.respondWith()</code></a> prevents the browsers default fetch handling and tells the browser we want to handle the response ourselves. If you don&#39;t call <code>evt.respondWith()</code> inside of a <code>fetch</code> handler, you&#39;ll just get the default network behavior.</p>
</aside>
<h3 is-upgraded><strong>DevTools Detour</strong></h3>
<p>Let&#39;s check to make sure everything works as we expect it. With the Service Workers pane open, refresh the page, you&#39;ll see the new service worker installed, and the status number increment. </p>
<p>We can also check to see what&#39;s been cached. Go to the <strong>Cache Storage</strong> pane on the <strong>Application</strong> panel of DevTools. Right click <strong>Cache Storage</strong>, pick <strong>Refresh Caches</strong>, expand the section and you should see the name of your static cache listed on the left-hand side. Clicking on the cache name shows all of the files that are cached. </p>
<p class="image-container"><img style="width: 624.00px" src="img\\a6018c0bd27be04.png"></p>
<p>Now, let&#39;s test out offline mode. Go back to the <strong>Service Workers</strong> pane of DevTools and check the <strong>Offline</strong> checkbox. After checking it, you should see a little yellow warning icon next to the <strong>Network</strong> panel tab. This indicates that you&#39;re offline.</p>
<p class="image-container"><img style="width: 624.00px" src="img\\6e596e1241b683b3.png"></p>
<p>Reload your page and... it works! We get <strong>our</strong> offline panda, instead of Chrome&#39;s offline dino!</p>
<h2 is-upgraded><strong>Tips for testing service workers</strong></h2>
<p>Debugging service workers can be a challenge, and when it involves caching, things can become even more of a nightmare if the cache isn&#39;t updated when you expect it. Between the typical service worker lifecycle and a bug in your code, you may become quickly frustrated. <strong>But don&#39;t. </strong></p>
<h3 is-upgraded><strong>Use DevTools</strong></h3>
<p>In the Service Workers pane of the Application panel, there are a few checkboxes that will make your life much easier.</p>
<p class="image-container"><img style="width: 624.00px" src="img\\f2d615df2fe87890.png"></p>
<ul>
<li><strong>Offline</strong> - When checked simulates an offline experience and prevents any requests from going to the network.</li>
<li><strong>Update on reload</strong> - When checked will get the latest service worker, install it, and immediately activate it.</li>
<li><strong>Bypass for network</strong> - When checked requests bypass the service worker and are sent directly to the network.</li>
</ul>
<h3 is-upgraded><strong>Start Fresh</strong></h3>
<p>In some cases, you may find yourself loading cached data or that things aren&#39;t updated as you expect. To clear all saved data (localStorage, indexedDB data, cached files) and remove any service workers, use the Clear storage pane in the Application tab. Alternatively, you can also work in an Incognito window.</p>
<p class="image-container"><img style="width: 624.00px" src="img\\55cc9231dc5c72f7.png"></p>
<p>Additional tips:</p>
<ul>
<li>Once a service worker has been unregistered, it may remain listed until its containing browser window is closed.</li>
<li>If multiple windows to your app are open, a new service worker will not take effect until all windows have been reloaded and updated to the latest service worker.</li>
<li>Unregistering a service worker does not clear the cache!</li>
<li>If a service worker exists and a new service worker is registered, the new service worker won&#39;t take control until the page is reloaded, unless you <a href="https://developers.google.com/web/fundamentals/primers/service-workers/lifecycle#clientsclaim" target="_blank">take immediate control</a>.</li>
</ul>
<h2 is-upgraded><strong>Verify changes with Lighthouse</strong></h2>
<p>Run Lighthouse again and verify your changes. Don&#39;t forget to uncheck the Offline checkbox before you verify your changes!</p>
<p><strong>SEO Audit</strong></p>
<ul>
<li><strong>✅ PASSED:</strong> Document has a meta description.</li>
</ul>
<p><strong>Progressive Web App Audit</strong></p>
<ul>
<li><strong>✅ PASSED:</strong> Current page responds with a 200 when offline.</li>
<li><strong>✅ PASSED: </strong><code>start_url</code> responds with a 200 when offline.</li>
<li><strong>✅ PASSED:</strong> Registers a service worker that controls page and <code>start_url.</code></li>
<li><strong>✅ PASSED:</strong> Web app manifest meets the installability requirements.</li>
<li><strong>✅ PASSED:</strong> Configured for a custom splash screen.</li>
<li><strong>✅ PASSED:</strong> Sets an address-bar theme color.</li>
</ul>


      </google-codelab-step>
    
      <google-codelab-step label="Provide a full offline experience" duration="9">
        <p>Take a moment and put your phone into airplane mode, and try running some of your favorite apps. In almost all cases, they provide a fairly robust offline experience. Users expect that robust experience from their apps. And the web should be no different. Progressive Web Apps should be designed with offline as a core scenario.</p>
<aside class="special"><p>Designing for offline-first can drastically improve the performance of your web app by reducing the number of network requests made by your app, instead resources can be precached and served directly from the local cache. Even with the fastest network connection, serving from the local cache will be faster!</p>
</aside>
<h2 is-upgraded><strong>Service worker life cycle</strong></h2>
<p>The life cycle of the service worker is the most complicated part. If you don&#39;t know what it&#39;s trying to do and what the benefits are, it can feel like it&#39;s fighting you. But once you know how it works, you can deliver seamless, unobtrusive updates to users, mixing the best of the web and native patterns.</p>
<aside class="special"><p><strong>Dive Deeper:</strong> This codelab only covers the very basics of the service worker life cycle. To dive deeper, refer to <a href="https://developers.google.com/web/fundamentals/primers/service-workers/lifecycle" target="_blank">The Service Worker Lifecycle</a> article on WebFundamentals.</p>
</aside>
<h3 is-upgraded><strong><code>install</code></strong><strong> event</strong></h3>
<p>The first event a service worker gets is <code>install</code>. It&#39;s triggered as soon as the worker executes, and it&#39;s only called once per service worker. <strong>If you alter your service worker script the browser considers it a different service worker</strong>, and it&#39;ll get its own <code>install</code> event. </p>
<p class="image-container"><img style="width: 624.00px" src="img\\e75a38479ab6742.png"></p>
<p>Typically the <code>install</code> event is used to cache everything you need for your app to run. </p>
<h3 is-upgraded><strong><code>activate</code></strong><strong> event</strong></h3>
<p>The service worker will receive an <code>activate</code> event every time it starts up. The main purpose of the <code>activate</code> event is to configure the service worker&#39;s behavior, clean up any resources left behind from previous runs (e.g. old caches), and get the service worker ready to handle network requests (for example the <code>fetch</code> event described below).</p>
<h3 is-upgraded><strong><code>fetch</code></strong><strong> event</strong></h3>
<p>The fetch event allows the service worker to intercept any network requests and handle requests. It can go to the network to get the resource, it can pull it from its own cache, generate a custom response or any number of different options. Check out the <a href="https://developers.google.com/web/fundamentals/instant-and-offline/offline-cookbook/" target="_blank">Offline Cookbook</a> for different strategies that you can use.</p>
<h3 is-upgraded><strong>Updating a service worker</strong></h3>
<p>The browser checks to see if there is a new version of your service worker on each page load. If it finds a new version, the new version is downloaded and installed in the background, but it is not activated. It&#39;s sits in a waiting state, until there are no longer any pages open that use the old service worker. Once all windows using the old service worker are closed, the new service worker is activated and can take control. Refer to the <a href="https://developers.google.com/web/fundamentals/primers/service-workers/lifecycle#updates" target="_blank">Updating the service worker</a> section of the Service Worker Lifecycle doc for further details.</p>
<h2 is-upgraded><strong>Choosing the right caching strategy</strong></h2>
<p>Choosing the right <a href="https://developers.google.com/web/fundamentals/instant-and-offline/offline-cookbook/" target="_blank">caching strategy</a> depends on the type of resource you&#39;re trying to cache and how you might need it later. For our weather app, we&#39;ll split the resources we need to cache into two categories: resources we want to precache and the data that we&#39;ll cache at runtime.</p>
<h3 is-upgraded><strong>Caching static resources</strong></h3>
<p>Precaching your resources is a similar concept to what happens when a user installs a desktop or mobile app. The key resources needed for the app to run are installed, or cached on the device so that they can be loaded later whether there&#39;s a network connection or not.</p>
<p>For our app, we&#39;ll precache all of our static resources when our service worker is installed so that everything we need to run our app is stored on the user&#39;s device. To ensure our app loads lightning fast, we&#39;ll use the <a href="https://developers.google.com/web/fundamentals/instant-and-offline/offline-cookbook/#cache-falling-back-to-network" target="_blank">cache-first</a> strategy; instead of going to the network to get the resources, they&#39;re pulled from the local cache; only if it&#39;s not available there will we try to get it from the network.</p>
<p class="image-container"><img style="width: 624.00px" src="img\\5932a908f5a71ed2.png"></p>
<p>Pulling from the local cache eliminates any network variability. No matter what kind of network the user is on (WiFi, 5G, 3G, or even 2G), the key resources we need to run are available almost immediately.</p>
<aside class="warning"><p><strong>Caution:</strong> In this sample, static resources are served using a <a href="https://developers.google.com/web/fundamentals/instant-and-offline/offline-cookbook/#cache-falling-back-to-network" target="_blank"><code>cache-first</code></a> strategy, which results in a copy of any cached content being returned without consulting the network. While a <code>cache-first</code> strategy is easy to implement, it can cause challenges in the future.</p>
</aside>
<h3 is-upgraded><strong>Caching the app data</strong></h3>
<p>The <a href="https://developers.google.com/web/fundamentals/instant-and-offline/offline-cookbook/#stale-while-revalidate" target="_blank">stale-while-revalidate strategy</a> is ideal for certain types of data and works well for our app. It gets data on screen as quickly as possible, then updates that once the network has returned the latest data. Stale-while-revalidate means we need to kick off two asynchronous requests, one to the cache and one to the network. </p>
<p class="image-container"><img style="width: 624.00px" src="img\\56357fc9fd863b7d.png"></p>
<p>Under normal circumstances, the cached data will be returned almost immediately providing the app with recent data it can use. Then, when the network request returns, the app will be updated using the latest data from the network.</p>
<p>For our app, this provides a better experience than the network, falling back to cache strategy because the user does not have to wait until the network request times out to see something on screen. They may initially see older data, but once the network request returns, the app will be updated with the latest data.</p>
<h2 is-upgraded><strong>Update app logic</strong></h2>
<p>As mentioned previously, the app needs to kick off two asynchronous requests, one to the cache and one to the network. The app uses the <code>caches</code> object available in <code>window</code> to access the cache and retrieve the latest data. This is an excellent example of progressive enhancement as the <code>caches</code> object may not be available in all browsers, and if it&#39;s not the network request should still work.</p>
<p>Update the <code>getForecastFromCache()</code> function, to check if the <code>caches</code> object is available in the global <code>window</code> object, and if it is, request the data from the cache.</p>
<h3 is-upgraded><a href="https://github.com/googlecodelabs/your-first-pwapp/blob/master/public/scripts/app.js#L164" target="_blank"><strong>public/scripts/app.js</strong></a></h3>
<pre><code>// CODELAB: Add code to get weather forecast from the caches object.
if (!(&#39;caches&#39; in window)) {
  return null;
}
const url = `${window.location.origin}/forecast/${coords}`;
return caches.match(url)
    .then((response) =&gt; {
      if (response) {
        return response.json();
      }
      return null;
    })
    .catch((err) =&gt; {
      console.error(&#39;Error getting data from cache&#39;, err);
      return null;
    });</code></pre>
<p>Then, we need to modify <a href="https://github.com/googlecodelabs/your-first-pwapp/blob/master/public/scripts/app.js#L196" target="_blank"><code>updateData()</code></a> so that it makes two calls, one to <code>getForecastFromNetwork()</code> to get the forecast from the network, and one to <code>getForecastFromCache()</code> to get the latest cached forecast:</p>
<h3 is-upgraded><a href="https://github.com/googlecodelabs/your-first-pwapp/blob/master/public/scripts/app.js#L200" target="_blank"><strong>public/scripts/app.js</strong></a></h3>
<pre><code>// CODELAB: Add code to call getForecastFromCache.
getForecastFromCache(location.geo)
    .then((forecast) =&gt; {
      renderForecast(card, forecast);
    });</code></pre>
<p>Our weather app now makes two asynchronous requests for data, one from the cache and one via a <code>fetch</code>. If there&#39;s data in the cache, it&#39;ll be returned and rendered extremely quickly (tens of milliseconds). Then, when the <code>fetch</code> responds, the card will be updated with the freshest data direct from the weather API.</p>
<p>Notice how the cache request and the <code>fetch</code> request both end with a call to update the forecast card. How does the app know whether it&#39;s displaying the latest data? This is handled in the following code from <code>renderForecast()</code>:</p>
<h3 is-upgraded><a href="https://github.com/googlecodelabs/your-first-pwapp/blob/master/public/scripts/app.js#L85" target="_blank"><strong>public/scripts/app.js</strong></a></h3>
<pre><code>// If the data on the element is newer, skip the update.
if (lastUpdated &gt;= data.currently.time) {
  return;
}</code></pre>
<p>Every time that a card is updated, the app stores the timestamp of the data on a hidden attribute on the card. The app just bails if the timestamp that already exists on the card is newer than the data that was passed to the function.</p>
<h2 is-upgraded><strong>Pre-cache our app resources</strong></h2>
<p>In the service worker, let&#39;s add a <code>DATA_CACHE_NAME</code> so that we can separate our applications data from the app shell. When the app shell is updated and older caches are purged, our data will remain untouched, ready for a super fast load. Keep in mind, if your data format changes in the future, you&#39;ll need a way to handle that and ensure the app shell and content stay in sync.</p>
<h3 is-upgraded><a href="https://github.com/googlecodelabs/your-first-pwapp/blob/master/public/service-worker.js#L21" target="_blank"><strong>public/service-worker.js</strong></a></h3>
<pre><code>// CODELAB: Update cache names any time any of the cached files change.
const CACHE_NAME = &#39;static-cache-v2&#39;;
const DATA_CACHE_NAME = &#39;data-cache-v1&#39;;</code></pre>
<p>Don&#39;t forget to also update the <code>CACHE_NAME</code>; we&#39;ll be changing all of our static resources as well.</p>
<p>In order for our app to work offline, we need to precache all of the resources it needs. This will also help our performance. Instead of having to get all of the resources from the network, the app will be able to load all of them from the local cache, eliminating any network instability.</p>
<p>Update the <code>FILES_TO_CACHE</code> array with the list of files:</p>
<h3 is-upgraded><a href="https://github.com/googlecodelabs/your-first-pwapp/blob/master/public/service-worker.js#L23" target="_blank"><strong>public/service-worker.js</strong></a></h3>
<pre><code>// CODELAB: Add list of files to cache here.
const FILES_TO_CACHE = [
  &#39;/&#39;,
  &#39;/index.html&#39;,
  &#39;/scripts/app.js&#39;,
  &#39;/scripts/install.js&#39;,
  &#39;/scripts/luxon-1.11.4.js&#39;,
  &#39;/styles/inline.css&#39;,
  &#39;/images/add.svg&#39;,
  &#39;/images/clear-day.svg&#39;,
  &#39;/images/clear-night.svg&#39;,
  &#39;/images/cloudy.svg&#39;,
  &#39;/images/fog.svg&#39;,
  &#39;/images/hail.svg&#39;,
  &#39;/images/install.svg&#39;,
  &#39;/images/partly-cloudy-day.svg&#39;,
  &#39;/images/partly-cloudy-night.svg&#39;,
  &#39;/images/rain.svg&#39;,
  &#39;/images/refresh.svg&#39;,
  &#39;/images/sleet.svg&#39;,
  &#39;/images/snow.svg&#39;,
  &#39;/images/thunderstorm.svg&#39;,
  &#39;/images/tornado.svg&#39;,
  &#39;/images/wind.svg&#39;,
];</code></pre>
<p>Since we are manually generating the list of files to cache, every time we update a file we <strong>must update the </strong></p>
<p><strong><code>CACHE_NAME</code></strong>. We were able to remove <code>offline.html</code> from our list of cached files because our app now has all the necessary resources it needs to work offline, and won&#39;t ever show the offline page again.</p>
<aside class="warning"><p><strong>Caution:</strong> In this sample, we hand-rolled our own service worker. Each time we update any of the static resources, we need to re-roll the service worker and update the cache, otherwise the old content will be served. In addition, when one file changes, the entire cache is invalidated and needs to be re-downloaded. That means fixing a simple single character spelling mistake will invalidate the cache and require everything to be downloaded again—not exactly efficient. <a href="https://developers.google.com/web/tools/workbox/" target="_blank">Workbox</a> handles this gracefully, by integrating it into your build process, only changed files will be updated, saving bandwidth for users and easier maintenance for you!</p>
</aside>
<h3 is-upgraded><strong>Update the activate event handler</strong></h3>
<p>To ensure our <code>activate</code> event doesn&#39;t accidentally delete our data, in the <code>activate</code> event of <code>service-worker.js</code>, replace <code>if (key !== CACHE_NAME) {</code> with:</p>
<h3 is-upgraded><strong>public/service-worker.js</strong></h3>
<pre><code>if (key !== CACHE_NAME &amp;&amp; key !== DATA_CACHE_NAME) {</code></pre>
<h3 is-upgraded><strong>Update the fetch event handler</strong></h3>
<p>We need to modify the service worker to intercept requests to the weather API and store their responses in the cache, so we can easily access them later. In the stale-while-revalidate strategy, we expect the network response to be the ‘source of truth&#39;, always providing us with the most recent information. If it can&#39;t, it&#39;s OK to fail because we&#39;ve already retrieved the latest cached data in our app.</p>
<p>Update the <code>fetch</code> event handler to handle requests to the data API separately from other requests.</p>
<h3 is-upgraded><a href="https://github.com/googlecodelabs/your-first-pwapp/blob/master/public/service-worker.js#L42" target="_blank"><strong>public/service-worker.js</strong></a></h3>
<pre><code>// CODELAB: Add fetch event handler here.
if (evt.request.url.includes(&#39;/forecast/&#39;)) {
  console.log(&#39;[Service Worker] Fetch (data)&#39;, evt.request.url);
  evt.respondWith(
      caches.open(DATA_CACHE_NAME).then((cache) =&gt; {
        return fetch(evt.request)
            .then((response) =&gt; {
              // If the response was good, clone it and store it in the cache.
              if (response.status === 200) {
                cache.put(evt.request.url, response.clone());
              }
              return response;
            }).catch((err) =&gt; {
              // Network request failed, try to get it from the cache.
              return cache.match(evt.request);
            });
      }));
  return;
}
evt.respondWith(
    caches.open(CACHE_NAME).then((cache) =&gt; {
      return cache.match(evt.request)
          .then((response) =&gt; {
            return response || fetch(evt.request);
          });
    })
);</code></pre>
<p>The code intercepts the request and checks if it is for a weather forecast. If it is, use <code>fetch</code> to make the request. Once the response is returned, open the cache, clone the response, store it in the cache, and return the response to the original requestor.</p>
<p>We need to remove the <code>evt.request.mode !== 'navigate'</code> check because we want our service worker to handle all requests (including images, scripts, CSS files, etc), not just navigations. If we left that check in, only the HTML would be served from the service worker cache, everything else would be requested from the network.</p>
<h2 is-upgraded><strong>Try it out</strong></h2>
<p>The app should be completely offline-functional now. Refresh the page to ensure that you&#39;ve got the latest service worker installed, then save a couple of cities and press the refresh button on the app to get fresh weather data.  </p>
<p>Then go to the <strong>Cache Storage</strong> pane on the <strong>Application</strong> panel of DevTools. Expand the section and you should see the name of your static cache and data cache listed on the left-hand side. Opening the data cache should show the data stored for each city.</p>
<p class="image-container"><img style="width: 624.00px" src="img\\f140a63f77da9a1e.png"></p>
<p>Then, open DevTools and switch to the Service Workers pane, and check the Offline checkbox, then try reloading the page, and then go offline and reload the page. </p>
<p>If you&#39;re on a fast network and want to see how weather forecast data is updated on a slow connection, set the <code>FORECAST_DELAY</code> property in <code>server.js</code> to <code>5000</code>. All requests to the forecast API will be delayed by 5000ms.</p>
<h2 is-upgraded><strong>Verify changes with Lighthouse</strong></h2>
<p>It&#39;s also a good idea to run Lighthouse again.</p>
<p><strong>SEO Audit</strong></p>
<ul>
<li><strong>✅ PASSED:</strong> Document has a meta description. </li>
</ul>
<p><strong>Progressive Web App Audit</strong></p>
<ul>
<li><strong>✅ PASSED:</strong> Current page responds with a 200 when offline.</li>
<li><strong>✅ PASSED: </strong><code>start_url</code> responds with a 200 when offline.</li>
<li><strong>✅ PASSED:</strong> Registers a service worker that controls page and <code>start_url.</code></li>
<li><strong>✅ PASSED:</strong> Web app manifest meets the installability requirements.</li>
<li><strong>✅ PASSED:</strong> Configured for a custom splash screen.</li>
<li><strong>✅ PASSED:</strong> Sets an address-bar theme color.</li>
</ul>


      </google-codelab-step>
    
      <google-codelab-step label="Add install experience" duration="5">
        <p>When a Progressive Web App is installed, it looks and behaves like all of the other installed apps. It launches from the same place as other apps launch. It runs in an app without an address bar or other browser UI. And like all other installed apps, it&#39;s a top level app in the task switcher.</p>
<p class="image-container"><img style="width: 298.00px" src="img\\a4954aa2ceb43ba3.png"></p>
<p>In Chrome, a Progressive Web App can either be installed through the three-dot context menu, or you can provide a button or other UI component to the user that will prompt them to install your app. </p>
<aside class="special"><p><strong>Tip:</strong> Since the install experience in Chrome&#39;s three-dot context menu is somewhat buried, we recommend that you provide some indication within your app to notify the user your app can be installed, and an install button to complete the install process.</p>
</aside>
<h2 is-upgraded><strong>Audit with Lighthouse</strong></h2>
<p>In order for a user to be able to install your Progressive Web App, it needs to meet <a href="https://developers.google.com/web/fundamentals/app-install-banners/#criteria" target="_blank">certain criteria</a>. The easiest way to check is to use Lighthouse and make sure it meets the installable criteria.</p>
<p class="image-container"><img style="width: 624.00px" src="img\\3136dac18701946b.png"></p>
<p>If you&#39;re worked through this codelab, your PWA should already meet these criteria.</p>
<aside class="special"><p><strong>DevTip:</strong> For this section, enable the <strong>Bypass for network</strong> checkbox in the <strong>Service Workers</strong> pane of the <strong>Application</strong> panel in DevTools. When checked, requests bypass the service worker and are sent directly to the network. This simplifies our development process since we don&#39;t have to update our service worker while working through this section.</p>
</aside>
<h2 is-upgraded><strong>Add install.js to index.html</strong></h2>
<p>First, let&#39;s add the <code>install.js</code> to our <code>index.html</code> file.</p>
<h3 is-upgraded><a href="https://github.com/googlecodelabs/your-first-pwapp/blob/master/public/index.html#L204" target="_blank"><strong>public/index.html</strong></a></h3>
<pre><code>&lt;!-- CODELAB: Add the install script here --&gt;
&lt;script src=&#34;/scripts/install.js&#34;&gt;&lt;/script&gt;</code></pre>
<h2 is-upgraded><strong>Listen for </strong><strong><code>beforeinstallprompt</code></strong><strong> event</strong></h2>
<p>If the add to home screen <a href="https://developers.google.com/web/fundamentals/app-install-banners/#criteria" target="_blank">criteria</a> are met, Chrome will fire a <code>beforeinstallprompt</code> event, that you can use to indicate your app can be &#39;installed&#39;, and then prompt the user to install it. Add the code below to listen for the <code>beforeinstallprompt</code> event:</p>
<h3 is-upgraded><a href="https://github.com/googlecodelabs/your-first-pwapp/blob/master/public/scripts/install.js#L24" target="_blank"><strong>public/scripts/install.js</strong></a></h3>
<pre><code>// CODELAB: Add event listener for beforeinstallprompt event
window.addEventListener(&#39;beforeinstallprompt&#39;, saveBeforeInstallPromptEvent);</code></pre>
<h2 is-upgraded><strong>Save event and show install button</strong></h2>
<p>In our <code>saveBeforeInstallPromptEvent</code> function, we&#39;ll save a reference to the <code>beforeinstallprompt</code> event so that we can call <code>prompt()</code> on it later, and update our UI to show the install button.</p>
<h3 is-upgraded><a href="https://github.com/googlecodelabs/your-first-pwapp/blob/master/public/scripts/install.js#L34" target="_blank"><strong>public/scripts/install.js</strong></a></h3>
<pre><code>// CODELAB: Add code to save event &amp; show the install button.
deferredInstallPrompt = evt;
installButton.removeAttribute(&#39;hidden&#39;);</code></pre>
<h2 is-upgraded><strong>Show the prompt / hide the button</strong></h2>
<p>When the user clicks the install button, we need to call <code>.prompt()</code> on the saved <code>beforeinstallprompt</code> event. We also need to hide the install button, because <code>.prompt()</code> can only be called once on each saved event.</p>
<h3 is-upgraded><a href="https://github.com/googlecodelabs/your-first-pwapp/blob/master/public/scripts/install.js#L45" target="_blank"><strong>public/scripts/install.js</strong></a></h3>
<pre><code>// CODELAB: Add code show install prompt &amp; hide the install button.
deferredInstallPrompt.prompt();
// Hide the install button, it can&#39;t be called twice.
evt.srcElement.setAttribute(&#39;hidden&#39;, true);</code></pre>
<p>Calling <code>.prompt()</code> will show a modal dialog to the user, asking them to add your app to their home screen.</p>
<h2 is-upgraded><strong>Log the results</strong></h2>
<p>You can check to see how the user responded to the install dialog by listening for the promise returned by the <code>userChoice</code> property of the saved <code>beforeinstallprompt</code> event. The promise returns an object with an <code>outcome</code> property after the prompt has shown and the user has responded to it.</p>
<h3 is-upgraded><a href="https://github.com/googlecodelabs/your-first-pwapp/blob/master/public/scripts/install.js#L47" target="_blank"><strong>public/scripts/install.js</strong></a></h3>
<pre><code>// CODELAB: Log user response to prompt.
deferredInstallPrompt.userChoice
    .then((choice) =&gt; {
      if (choice.outcome === &#39;accepted&#39;) {
        console.log(&#39;User accepted the A2HS prompt&#39;, choice);
      } else {
        console.log(&#39;User dismissed the A2HS prompt&#39;, choice);
      }
      deferredInstallPrompt = null;
    });</code></pre>
<p>One comment about <code>userChoice</code>, the <a href="https://w3c.github.io/manifest/#beforeinstallpromptevent-interface" target="_blank">spec defines it as a property</a>, not a function as you might expect.</p>
<h3 is-upgraded><strong>Log all install events</strong></h3>
<p>In addition to any UI you add to install your app, users can also install your PWA through other methods, for example Chrome&#39;s three-dot menu. To track these events, listen for the appinstalled event.</p>
<h3 is-upgraded><a href="https://github.com/googlecodelabs/your-first-pwapp/blob/master/public/scripts/install.js#L51" target="_blank"><strong>public/scripts/install.js</strong></a></h3>
<pre><code>// CODELAB: Add event listener for appinstalled event
window.addEventListener(&#39;appinstalled&#39;, logAppInstalled);</code></pre>
<p>Then, we&#39;ll need to update the <code>logAppInstalled</code> function, for this codelab, we&#39;ll just use <code>console.log</code>, but in a production app, you probably want to log this as an event with your analytics software.</p>
<h3 is-upgraded><a href="https://github.com/googlecodelabs/your-first-pwapp/blob/master/public/scripts/install.js#L60" target="_blank"><strong>public/scripts/install.js</strong></a></h3>
<pre><code>// CODELAB: Add code to log the event
console.log(&#39;Weather App was installed.&#39;, evt);</code></pre>
<h2 is-upgraded><strong>Update the service worker</strong></h2>
<p>Don&#39;t forget to update the <code>CACHE_NAME</code> in your <code>service-worker.js</code> file since you&#39;ve made changes to files that are already cached. Enabling the <strong>Bypass for network</strong> checkbox in the Service Workers pane of the Application panel in DevTools will work in development, but won&#39;t help in the real world.</p>
<h2 is-upgraded><strong>Try it out</strong></h2>
<p>Let&#39;s see how our install step went. To be safe, use the <strong>Clear site data</strong> button in the Application panel of DevTools to clear everything away and make sure we&#39;re starting fresh. If you previously installed the app, be sure to uninstall it, otherwise the install icon won&#39;t show up again.</p>
<h3 is-upgraded><strong>Verify the install button is visible</strong></h3>
<p>First, let&#39;s verify our install icon shows up properly, be sure to try this on both desktop and mobile.</p>
<ol type="1" start="1">
<li>Open the URL in a new Chrome tab.</li>
<li>Open Chrome&#39;s three-dot menu (next to the address bar).<br>▢ Verify you see &#34;<em>Install Weather...</em>&#34; in the menu.</li>
<li>Refresh the weather data using the refresh button in the upper right corner to ensure we meet the <a href="https://developers.google.com/web/fundamentals/app-install-banners/#criteria" target="_blank">user engagement heuristics</a>.<br>▢ Verify the install icon is visible in the app header.</li>
</ol>
<h3 is-upgraded><strong>Verify the install button works</strong></h3>
<p>Next, let&#39;s make sure everything installs properly, and our events are properly fired. You can do this either on desktop or mobile. If you want to test this on mobile, be sure you&#39;re using remote debugging so you can see what&#39;s logged to the console. <br></p>
<ol type="1" start="1">
<li>Open Chrome, and in a new browser tab, navigate to your Weather PWA.</li>
<li>Open DevTools and switch to the Console pane.</li>
<li>Click the install button in the upper right corner.<br>▢ Verify the install button disappears<br>▢ Verify the install modal dialog is shown.</li>
<li>Click Cancel.<br>▢ Verify &#34;<em>User dismissed the A2HS prompt</em>&#34; is shown in the console output.<br>▢ Verify the install button reappears.</li>
<li>Click the install button again, then click the install button in the modal dialog.<br>▢ Verify &#34;<em>User accepted the A2HS prompt</em>&#34; is shown in the console output.<br>▢ Verify &#34;<em>Weather App was installed</em>&#34; is shown in the console output.<br>▢ Verify the Weather app is added to the place where you&#39;d typically find apps.</li>
<li>Launch the Weather PWA.<br>▢ Verify the app opens as a standalone app, either in an app window on desktop, or full screen on mobile.</li>
</ol>
<p>Note, if you&#39;re running on desktop from localhost, your installed PWA may show an address banner because localhost isn&#39;t considered a secure host.</p>
<h3 is-upgraded><strong>Verify iOS installation works properly</strong></h3>
<p>Let&#39;s also check the behavior on iOS. If you have an iOS device, you can use that, or if you&#39;re on a Mac, try the iOS Simulator available with Xcode.</p>
<ol type="1" start="1">
<li>Open Safari and in a new browser tab, navigate to your Weather PWA.</li>
<li>Click the <em>Share </em><img style="width: 19.00px" src="img\\7fc386fd0954ef1b.png"> button.</li>
<li>Scroll right and click on the <em>Add to Home Screen</em> button.<br>▢ Verify the title, URL and icon are correct.</li>
<li>Click <em>Add.<br></em>▢ Verify the app icon is added to the home screen.</li>
<li>Launch the Weather PWA from the home screen.<br>▢ Verify the app launches full screen.</li>
</ol>
<h2 is-upgraded><strong>Bonus: Detecting if your app is launched from the home screen</strong></h2>
<p>The <code>display-mode</code> media query makes it possible to apply styles depending on how the app was launched, or determine how it was launched with JavaScript. </p>
<pre><code>@media all and (display-mode: standalone) {
  body {
    background-color: yellow;
  }
}</code></pre>
<p>You can also check the <code>display-mode</code> media query in <a href="https://developers.google.com/web/fundamentals/app-install-banners/#detect-mode" target="_blank">JavaScript to see if you&#39;re running in standalone</a>.</p>
<h2 is-upgraded><strong>Bonus: Uninstalling your PWA</strong></h2>
<p>Remember, the <code>beforeinstallevent</code> doesn&#39;t fire if the app is already installed, so during development you&#39;ll probably want to install and uninstall your app several times to make sure everything is working as expected.</p>
<h3 is-upgraded><strong>Android</strong></h3>
<p>On Android, PWAs are uninstalled in the same way other installed apps are uninstalled.</p>
<ul>
<li>Open the app drawer.</li>
<li>Scroll down to find the Weather icon.</li>
<li>Drag the app icon to the top of the screen.</li>
<li>Choose <em>Uninstall.</em></li>
</ul>
<h3 is-upgraded><strong>ChromeOS</strong></h3>
<p>On ChromeOS, PWAs are easily uninstalled from the launcher search box.</p>
<ul>
<li>Open the launcher.</li>
<li>Type &#34;<em>Weather</em>&#34; into the search box, your Weather PWA should appear in the results.</li>
<li>Right click (alt-click) on the Weather PWA.</li>
<li>Click <em>Remove from Chrome...</em></li>
</ul>
<h3 is-upgraded><strong>macOS and Windows</strong></h3>
<p>On Mac and Windows, PWAs may be uninstalled through Chrome:</p>
<ul>
<li>In a new browser tab, open <em>chrome://apps</em>.</li>
<li>Right click (alt-click) on the Weather PWA.</li>
<li>Click <em>Remove from Chrome...</em></li>
</ul>
<p>You can also open the installed PWA, click the the dot menu in the upper right corner, and choose &#34;<em>Uninstall Weather PWA...</em>&#34;</p>


      </google-codelab-step>
    
      <google-codelab-step label="Congratulations" duration="0">
        <p>Congratulations, you&#39;ve successfully built your first Progressive Web App! </p>
<p>You added a web app manifest to enable it to be installed, and you added a service worker to ensure that your PWA is always fast, and reliable. You learned how to use DevTools to audit an app and how it can help you improve your user experience.</p>
<p>You now know the key steps required to turn any web app into a Progressive Web App.</p>
<h2 is-upgraded><strong>What&#39;s next?</strong></h2>
<p>Check out some of these codelabs...</p>
<ul>
<li><a href="https://codelabs.developers.google.com/codelabs/push-notifications/index.html" target="_blank">Adding Push Notifications to a Web App</a></li>
<li><a href="https://codelabs.developers.google.com/codelabs/debugging-service-workers/index.html" target="_blank">Debugging Service Workers</a></li>
<li><a href="https://codelabs.developers.google.com/codelabs/workbox-lab/index.html" target="_blank">Build a PWA using Workbox</a></li>
</ul>
<h2 is-upgraded><strong>Further reading</strong></h2>
<ul>
<li><a href="https://developers.google.com/web/fundamentals/primers/service-workers/high-performance-loading" target="_blank">High-performance service worker loading</a></li>
<li><a href="https://medium.com/dev-channel/service-worker-caching-strategies-based-on-request-types-57411dd7652c" target="_blank">Service Worker Caching Strategies Based on Request Types</a></li>
</ul>
<h2 is-upgraded><strong>Reference docs</strong></h2>
<ul>
<li><a href="https://developers.google.com/web/fundamentals/web-app-manifest" target="_blank">Web App Manifest docs</a></li>
<li><a href="https://developer.mozilla.org/en-US/docs/Web/Manifest#Members" target="_blank">Web App Manifest properties (MDN)</a></li>
<li><a href="https://developers.google.com/web/fundamentals/app-install-banners/" target="_blank">Install &amp; Add to Home Screen</a></li>
<li><a href="https://developers.google.com/web/fundamentals/primers/service-workers/" target="_blank">Service Worker Overview</a></li>
<li><a href="https://developers.google.com/web/fundamentals/primers/service-workers/lifecycle" target="_blank">Service Worker Lifecycle</a></li>
<li><a href="https://developers.google.com/web/fundamentals/primers/service-workers/high-performance-loading" target="_blank">High-performance service worker loading</a></li>
<li><a href="https://developers.google.com/web/fundamentals/instant-and-offline/offline-cookbook/#generic-fallback" target="_blank">Offline Cookbook</a></li>
</ul>


      </google-codelab-step>
    
  </google-codelab>

  <script src="https://storage.googleapis.com/codelab-elements/native-shim.js"></script>
  <script src="https://storage.googleapis.com/codelab-elements/custom-elements.min.js"></script>
  <script src="https://storage.googleapis.com/codelab-elements/prettify.js"></script>
  <script src="https://storage.googleapis.com/codelab-elements/codelab-elements.js"></script>
  <script src="//support.google.com/inapp/api.js"></script>

</body>
</html>