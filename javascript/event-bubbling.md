## Event Bubbling
It is a concept in the HTML DOM(Document Object Model), that when a child receives an event it propogates(spreads or passes on to) the event on to its ancestors till it reaches the root element.

### lets undestand with an example

#### HTML
Nested some elements so we can see how the event gets propagated when an event gets triggered.

```
    <section id="event-bubbling-example">
      <h4>Event Bubbling Example</h4>
      <article class="parent">
        Parent
        <div class="child">
          Child
          <div class="grand-child">Grand Child</div>
        </div>
      </article>
    </section>
```
### CSS
```
    .parent {
        border: 1px solid rgb(255, 255, 255);
    }

    .child {
      border: 1px solid limegreen;
    }

    .grand-child {
      border: 1px solid magenta;
    }

```

### JavaScript
added event listener to body and other nested elements.

```
    const body = document.querySelector("#body-test");
    const container = document.querySelector("#event-bubbling-example");
    const parent = document.querySelector("#event-bubbling-example .parent");
    const child = document.querySelector("#event-bubbling-example .child");
    const gChild = document.querySelector("#event-bubbling-example .grand-child");

    body.addEventListener("click", function (e) {
        console.log("body clicked");
    });
    container.addEventListener("click", function (e) {
        console.log("container clicked");
    });

    parent.addEventListener("click", function (e) {
        console.log("parent clicked");
    });

    child.addEventListener("click", function (e) {
        console.log("child clicked");
    });

    gChild.addEventListener("click", function (e) {
        console.log("grand-child clicked");
    });
```

The output when we click on the "grand-child" div:

```
    grand-child clicked
    child clicked
    parent clicked
    container clicked
    body clicked
```

All of **grand-child**'s ancestors' event gets trogerred even though only "grand-child" was clicked on, this happens because when you click on "grand-child" you are clicking on all of its ancestors in this case section, article, div.

The output when we click on the "child" div:

```
    child clicked
    parent clicked
    container clicked
    body clicked
```

The "grand-child" div doesn't get triggered anymore since it is not the ancestor of "child" div.

### How can you prevent this?
You can prevent this by calling "stopPropagation" method which can be accessed with the event object.
You call the "stopPropagation" method on top of the event function whenre you want the propogation to stop.

```
    gChild.addEventListener("click", function (e) {
        e.stopPropagation()
        console.log("grand-child clicked");
    });
```

### Let's see the output when we click on the "grand-child" div after this change:
Only **grand-child**'s event gets triggered since it non longer propogates the event on to its ancestors.

```
    grand-child clicked
```
