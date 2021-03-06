The links that control the `visibilityFilter` do not currently behave like real links. I'd like to change it so that the background works and the current URL updates when I click on these links and change the current `visibilityFilter`.


![output](https://res.cloudinary.com/dg3gyk0gu/image/upload/v1553542110/transcript-images/javascript-redux-navigating-with-react-router-link-output.jpg)


I will add a parameter to my `Route` called `filter`, and I need to wrap it in parenthesis to tell `react-router` that it's optional, because if it's not specified, I want to show all todos.

**index.js**
```javascript
const Root = ({ store }) => (
  <Provider store={store}>
    <Router history={browserHistory}>
      <Route path='/(:filter)' component={App} />
    </Router>
  </Provider>
);
```

Now I'm opening the `Footer` component that displays the links. Right now, I'm using the custom convention for the filter prop, but I'm going to change this to align more closely with the paths I want to be displayed, so I'm using `active` and `completed` for the active and completed paths, and to avoid passing an empty string, I'll just use a null string to signify the default path.

**Footer.js**
```javascript
const Footer = () => (
  <p>
    Show:
    {" "}
    <FilterLink filter="SHOW_ALL">
      All
    </FilterLink>
    {", "}
    <FilterLink filter="SHOW_ACTIVE">
      Active
    </FilterLink>
    {", "}
    <FilterLink filter="SHOW_COMPLETED">
      Completed
    </FilterLink>
  </p>
);
```

My current implementation of the `filterLink` component dispatches an action every time it's clicked, and reads its `active` state from the store, comparing its `filter` prop to the `visibilityFilter` in the store.

However, I'm not going to need this implementation anymore because I want the `Router` to be in control of any state that is in the URL. This is why I import link from `react-router`, and my new implementation of the `FilterLink` component is going to use it.

**FilterLink.js**
```javascript
import React from 'react';
import { Link } from 'react-router';

const FilterLink = ()
```

It accepts filter as a prop, and it renders through the link provided by `react-router`. Its true prop corresponds to the path that we want the link to point to, so if the `filter` is `all` we're going to use the root path. Otherwise, we'll just use the `filter` itself as the path.

**FilterLink.js**
```javascript
import React from 'react';
import { Link } from 'react-router';

const FilterLink = ({ filter }) => (
  <Link
    to={filter === 'all' ? '' : filter}

);
```

I am also specifying the active style prop so that the link is styled differently when its true prop matches the current path. Finally, I'm passing children to the link itself, and I'm adding children as a prop to `FilterLink` so that the parent component can specify the children.

**FilterLink.js**
```javascript
import React from 'react';
import { Link } from 'react-router';

const FilterLink = ({ filter }) => (
  <Link
    to={filter === 'all' ? '' : filter}
    activeStyle={{
      textDecoration: 'none',
      color: 'black',
    }}
  >
    {children}
  </Link>
);

export default FilterLink;
```

Now I'm exporting filter link, and I have some cleanup to do. I can remove the `setVisibilityFilter` **action-creator**, as I don't use it anymore. I can also remove my custom link component, because now I'm using the link from `react-router` instead of it.

If I run the app now, clicking on the link will update the URL bar. It also works in the other direction. Clicking on the back or forward buttons gets the corresponding link active.


![output](https://res.cloudinary.com/dg3gyk0gu/image/upload/v1553542110/transcript-images/javascript-redux-navigating-with-react-router-link-output2.jpg)


In the next lesson, we will teach our component to read the current filter from the URL instead of **Redux** store.