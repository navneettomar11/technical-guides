# HttpSession

HTTP is a stateless protocol, that means client always needs to open a new connecton for a new request.

Sometimes a server side application needs to track a user across multiple requests e.g. shopping cart application. A single HTTP session is a session is a sequence of multiple request-response transactions for a single client. As HTTP protocol is stateless, the application can apply one of the following methods to remember a user.
- HTTP cookie: small name/value pairs are saved on the client browser.
- URL Rewriting: the session information is added to the URL. e.g. - http://www.example.com/page.html;sessionid=3452345
- Hidden Form fields: Server side generates dynamic pages that include session id for a particular request. When client submit the form to the server, server uses the hidden form field to track back the user.
```html
<input type="hidden" name="sessionid" value="3452345">
```