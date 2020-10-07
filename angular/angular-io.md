# Hiding and Showing Elements
We can hide any element by adding an attribute called `hidden` to the element in HTML, so we could hide the punchline like so:
```html
<p class="card-text" hidden>{{joke.punchline}}</p>
```
So we add the following markuo:
```html
<p class="card-text" [hidden]="true">{{joke.punchline}}</p>
```
# Introduction to Angular concepts
