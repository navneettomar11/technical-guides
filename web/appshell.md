# The App Shell Model
An **application shell** (or app shell) architecture is one way to build a Progressive Web App that reliably and instantly loads on your user's screens, similar to what you see in native applications.

The app "shell" is the minimal HTML, CSS and JavaScript required to power the user interface and when cached offline can ensure instant, reliably good performance to users on repeat visits. This means the application shell is not loaded from the network every time the user visits. Only the necessary content is needed from the network.

For single-page applications with JavaScript-heavy architectures, an application shell is a go-to approach. This approach relies on aggressively caching the shell(using a service worker) to get the application running. Next the dynamic content loads for each page using JavaScript. An app shell is useful for getting some initial HTML to the screen fast without a network.

![AppShell Image](images/appshell.png)

Put another way, the app shell is similar to the bundle of code that you'd publish to an app store when building a native app. It is the skeleton of your UI and the core components necessary to get your app off the ground, but likely does not contain the data.
>
## When to use the app shell model
Building a PWA does not mean starting from scratch. If you are building a modern single-page app, then you are probably using something similar to an app shell already whether you call it that or not. The details might vary a bit depending upon which libraries or frameworks you are using, but the concept itself is framework agnostic.
An application shell architecture makes the most sense for apps and sites with relatively unchanging navigation but changing content. A number of modern Javascript frameworks and libraries already encourage splitting your application logic from its content, making this architecture more straightforward to apply. For a certain class of websites that only have static content you can still follow the same model but ths site is 100% app shell.

## Benefits
The benefits of an app shell architecture with a service worker include:
- **Reliable performance that is consistently fast**. Repeat visits are extremely quick. Static assets and the UI (e.g. HTML, Javascript, images and CSS) are cached on the first visit so that they load instantly on repeat visits. Content may be cache on the first visit, but its 
