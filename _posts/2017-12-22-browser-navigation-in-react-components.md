---
title: Browser Navigation in React Components
layout: default
comments: true
published: true
---

Browser Navigation in React Components
======================================

The modern web has seen the rise of client-side frameworks, such as
[React](https://reactjs.org/), and
[Single-Page-Applications](https://en.wikipedia.org/wiki/Single-page_application)
(SPAs) where pages are generated and updated client-side (inside the browser by
JavaScript) unlike traditional web applications where pages are generated
server-side (on a remote computer) for display in a browser.

Page content generated client-side is often independent of a URL unlike
server-side pages where pages and URLs are tightly coupled. The browser back
and forward buttons were created in an era where every web application was
server-side generated and each page matched a unique URL. Unfortunately the
back and forward buttons, by default, do not work as users expect in
client-side SPAs, rather than transitioning back through page changes and
updates, the back (and forward) button will unexpectedly navigate completely
out of the SPA.

Using hash-bangs in URLs were a solution of sorts a few years ago but their
usage is now [frowned
upon](http://danwebb.net/2011/5/28/it-is-about-the-hashbangs). However, modern
browsers do provide an elegant solution for this client-side SPA navigation
issue, that being the [HTML5 History
API](https://developer.mozilla.org/en-US/docs/Web/API/History_API), more specially
the `pushState` function.

This post will describe how to use `pushState` in a simple React application
such that browser navigation correctly transitions through client-side page
changes.

Example Application
===================

For this article we will reference a simple React application. The main
component, named `BookList`, will primarily render a paginated list of books
fetched from an API endpoint.

In-page next and previous links are provided by a separate `Paginator`
component (not documented here), to navigate through the list of books.

A rough outline of the `BookList` component would be:

```jsx
const BOOKS_ENDPOINT = 'http://localhost:3000/books.json';

class BookList extends Component {
  constructor(props) {
    super(props);

    this.params = {};
    this.state = {
      books: [],
      pagination: {}
    };
  }

  componentDidMount() {
    this.fetchBooks();
  }

  handlePageChange = (page) => {
    this.params.page = page;
    this.fetchBooks();
  }

  booksURL() {
    const params = queryString.stringify(this.params);

    return params.length > 0 ? `${BOOKS_ENDPOINT}?${params}` : BOOKS_ENDPOINT;
  }

  fetchBooks() {
    axios.get(this.booksURL())
      .then(response => {
        this.setState({
          books: response.data.books,
          pagination: response.data.pagination,
        });
      });
  }

  renderBooks() {
    return (
      this.state.books.map(book => <li key={book.id}>{book.title}</li>)
    );
  }

  render() {
    return (
      <div>
        <h1>Book List</h1>
        <ul>{this.renderBooks()}</ul>
        <Paginator pagination={this.state.pagination} onPageChange={this.handlePageChange} />
      </div>
    );
  }
}

export default BookList;
```

Note, the query parameters for the API end-point are stored in a member
variable, as against component state, since we do not want to re-render when it
changes. Also, Axios can be replaced with `fetch` if you so choose.

Pushing State into History
==========================

Access to the history API is not provided with React. You will need to install
the [React Router](https://github.com/ReactTraining/react-router) package to
gain access to that API. Most non-trivial React applications will already be
using React Router, if not then install it:

```
yarn add react-router-dom
```

Next you may need to wrap your component with `withRouter` to gain access to
needed history and location properties. If your component is
already a `Route` component then nothing needs to be done otherwise please wrap
as follows:

```jsx
import { withRouter } from 'react-router-dom';
...
export default withRouter(BookList);
```

Now push the required state, `this.params` in this case, into history:

```jsx
  handlePageChange = (page) => {
    this.params.page = page;
    this.props.history.push('/', this.params);
    this.fetchBooks();
  }
```

The URL page parameter, stored in `this.params`, is the only state we need to
push into history. This SPA will only change when a user clicks through pages,
hence the page parameter is the only state we need to record in browser
history.

Note, in the `push` call the first parameter (`'/'` above) should be the actual
URL of your component. In this simple application `BookList` will be mounted at
the root URL.

Preventing render when location changes
=======================================

A minor issue above is that the `BooksList` component will now be rendered
twice when `handlePageChange` is invoked, once when `this.props.history.push`
is called and once in the `fetchBooks` function when `setState` is called. In
our example pushing state into history will **always** be followed
`fetchBooks`. Lets use the React lifecycle method `componentShouldUpdate` to
ignore locations changes:

```javascript
  shouldComponentUpdate(nextProps) {
    return _.isEqual(this.props.location, nextProps.location);
  }
```

In the above `shouldComponentUpdate` call we will not re-render the component
when the location changes. In our case that will safe, but that may not be the
case with your components. Treat `shouldComponentUpdate` with extreme caution.

Note, we need to use [Lodash isEqual](https://lodash.com/docs/#isEqual) to
correctly test object equivalence since JavaScript `===` does not work as one
would expect.

Browser navigation event handler
================================

The pressing of the back or forward button in a browser is an event. Our
component needs a handler for this event to correctly update the current page
to a previous state.

The event of interest is `window.onpopstate`:

```javascript
  componentDidMount() {
    window.onpopstate = this.handlePopState;
    this.applyParams();
  }

  componentWillUnmount() {
    window.onpopstate = null;
  }

  handlePopState = (event) => {
    event.preventDefault();
    this.applyParams();
  }

  applyParams() {
    this.params = this.props.location.state || {};
    this.fetchBooks();
  }
```

Note, it is important to unset the `onpopstate` when this component is being
unmounted otherwise the event handler will persist beyond the lifetime of the
component.

Updated example
===============

Adding all the pieces together results in the following enhanced `BookList`
component:

```jsx
const BOOKS_ENDPOINT = 'http://localhost:3000/books.json';

class BookList extends Component {
  constructor(props) {
    super(props);

    this.params = {};
    this.state = {
      books: [],
      pagination: {}
    };
  }

  componentDidMount() {
    window.onpopstate = this.handlePopState;
    this.applyParams();
  }

  componentWillUnmount() {
    window.onpopstate = null;
  }

  shouldComponentUpdate(nextProps) {
    return _.isEqual(this.props.location, nextProps.location);
  }

  handlePopState = (event) => {
    event.preventDefault();
    this.applyParams();
  }

  applyParams() {
    this.params = this.props.location.state || {};
    this.fetchBooks();
  }

  handlePageChange = (page) => {
    this.params.page = page;
    this.props.history.push('/', this.params);
    this.fetchBooks();
  }

  booksURL() {
    const params = queryString.stringify(this.params);

    return params.length > 0 ? `${BOOKS_ENDPOINT}?${params}` : BOOKS_ENDPOINT;
  }

  fetchBooks() {
    axios.get(this.booksURL())
      .then(response => {
        this.setState({
          books: response.data.books,
          pagination: response.data.pagination,
        });
      });
  }

  renderBooks() {
    return (
      this.state.books.map(book => <li key={book.id}>{book.title}</li>)
    );
  }

  render() {
    return (
      <div>
        <h1>Book List</h1>
        <ul>{this.renderBooks()}</ul>
        <Paginator pagination={this.state.pagination} onPageChange={this.handlePageChange} />
      </div>
    );
  }
}

export default withRouter(BookList);
```

A user will now be able to browse through the application pages and then
navigate backward and forward through these pages without issue.

Caveat
======

I am a novice when it comes to JavaScript and React, hence the solution
described above may not be optimal. I do welcome feedback and suggestions on
the topic.
