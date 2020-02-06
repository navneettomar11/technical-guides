# The App Shell Model
An **application shell** (or app shell) architecture is one way to build a Progressive Web App that reliably and instantly loads on your user's screens, similar to what you see in native applications.

The app "shell" is the minimal HTML, CSS and JavaScript required to power the user interface and when cached offline can ensure instant, reliably good performance to users on repeat visits. This means the application shell is not loaded from the network every time the user visits. Only the necessary content is needed from the network.

For single-page applications with JavaScript-heavy architectures, an application shell is a go-to approach. This approach relies on aggressively caching the shell(using a service worker) to get the application running. Next the dynamic content loads for each page using JavaScript. An app shell is useful for getting some initial HTML to the screen fast without a network.
(!AppShell Image)['images/appshell.png']

Put another way, the app shell is similar to the bundle of code that you'd publish to an app store when building a native app. It is the skeleton of your UI and the core components necessary to get your app off the ground, but likely does not contain the data.
>
## When to use the app shell model
