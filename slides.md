class: center, middle, inverse

# Redux Best Practices

---

# Agenda

1.

---

# Our Requirements

1. Get the initial state
1. Draw stuff based on that state
1. Handle interactions with the UI
1. Make requests and keep things in state with your data stores
1. Update things and redraw things

---

class: center, middle, inverse

# Use Routes to ensure you have the right data for your components

---

This is nice because it separates rendering from data collection. Use the onEnter method of a Route to issue commands to gather what it needs to render. You don’t need this method to actually wait until the collection is done becuase you can…

---

class: center, middle, inverse

# Use smart components to make sure your dumb components are ready to render

---

Your smart components should be the components in your Route.

In their  render function we can have a method that makes sure it has the data it needs:

```js
render () {
  if (this.hasData()) {
    return this.renderComponent();
  } else {
    return this.renderLoadingSpinner();
  }
}
```

---

class: center, middle, inverse

# Use smart components to do as much pre-processing as possible

---

This way your dumb components can be really dumb. For instance, when you pass a handler to your dumb component, curry it with the id, so the dumb component doesn't need to know it:

```jsx
render() {
  return <DumbComponent
    onSelect={this.selectItem.bind(this, this.props.item.id)}
  >;
}
```

---

class: center, middle, inverse

# Separate your concerns

---

Use dumb components to render everything. Don't even put a `<div>` in a smart component. It should only ever be composing dumb components for you.

---

class: center, middle, inverse

# Use smart components to generate actions creators

---

When a dumb component has an interaction from a user, it shouldn't handle any logic itself . Instead, it should blindly call a function which is passed to it by a smart container and let it do the work. The smart container should then marshal the necessary data and pass it to an action creator.

---

class: center, middle, inverse

# Use action creators to translate application data structures to API data structures

---

Your action creators are responsible for converting the data in your application's format to the format of the API to which you are communicating. This is in both directions : when making the request or handling the response.

---

Because the output of an action is handled by a reducer that has no knowledge of how it was called, you might find that sometimes you can’t merely return the result of an API call — you need to enrich it extra data.

---

For example, if your action is `PROJECT_UPDATE`, and you pass a new project name and the project id, and the API just returns:

```json
{ "createdAt": "some date" }
```

then you will need to pass the data from the arguments into the response:

---

```js
function updateProject(projectId, projectName) {
  request.put(`/project/${projectId}`, {projectName}).then(
    response => Object.assign(
      {projectId, projectName},
      response.data
    )
  );
}
```

---

class: center, middle, inverse

# Use reducers to keep your state in sync

---

The interesting thing here is that a reducer can handle any action at all. One neat thing to do is when a user logs out, clear all your stores.

```js
switch (action.type) {
   ...
   case USER_LOGOUT:
     return {}
}
```

---

class: center, middle, inverse, thanks

## .secondary[Slides:] https://bit.ly/asd

## .secondary[Email:] fknussel@gmail.com

## .secondary[Twitter/GitHub:] @fknussel
