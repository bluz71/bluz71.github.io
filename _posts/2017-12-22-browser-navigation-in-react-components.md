---
title: Browser Navigation in React Components
layout: default
comments: true
published: false
---

Browser Navigation in React Components
======================================

Traditional web applications generate pages server-side (on a remote computer)
for display in a browser. The modern web has also seen the rise of client-side
JavaScript frameworks, such as [React](https://reactjs.org/), and
[Single-Page-Applications](https://en.wikipedia.org/wiki/Single-page_application)
(SPAs) where a page is generated and updated client-side (inside the browser by
JavaScript).

Page content generated client-side is often completely independent of a URL
unlike server-side pages where pages and URLs are tightly coupled. The browser
back and forward buttons were created in the era where every web application
was server-side generated and each page matched a unique URL. Unfortunately the
back and forward, by default, do not work as users expect in client-side SPAs,
rather than transistioning back through page changes and updates, the back (and
forward) button will unexpectedly navigate completely out of the web
application.

Modern browsers do provide a solution for this client-side SPA navigation
issue, that being the [HTML5 History
API](https://developer.mozilla.org/en-US/docs/Web/API/Histor), more specially
the `pushState` function.

This post will describe how to use `pushState` in a React application such that
browser navigation correctly transistions through client-side page changes.
Note, I found there is surprisingly little consolidated information on this
topic on the interwebs, hence the reason for this post.

Caveat, I am a novice when it comes to React and JavaScript, hence the React
specific solution described below may not be optimal. I do welcome feedback and
suggestions on the topic.

Example Application
===================

For this article we will reference a simple React application. The main
component, named BookList, fetches, sets local component state, and then
renders a paginated list of books.

Next and previous links are provided, by a separate Paginator component, to
navigate through the list of books which are retrieved from a back-end API.

A rough outline of the BookList component would be:

```jsx

const BOOKS_ENDPOINT = `http://localhost:3000/books.json`;

class BookList extends Component {
  constructor(props) {
    super(props);

    this.params = {}; // URL parameter used to fetch a particular page of books.
                      // NOT component state since we do not want to re-render
                      // when it changes.

    this.state = {
      books: [],      // The actual list of books from the back-end API
      pagination: {}  // Pagination parameters from the back-end API
    };
  }

  componentDidMount() {
    this.fetchBooks();
  }

  // Invoked from the Paginator component when a user clicks next or previous
  // or a particular page.
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
        <h2>Book List</h2>
        <ul>{this.renderBooks()}</ul>
        <Paginator pagination={this.state.pagination} onPageChange={this.handlePageChange} />
      </div>
    );
  }
}

export default BookList;
```

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

Now push current the current state, `this.params` in this case, into history:

```jsx

  handlePageChange = (page) => {
    this.params.page = page;
    this.props.history.push('/', this.params);
    this.fetchBooks();
  }
```

Note, the URL page parameter, stored in `this.params`, is the only state we
need to push into history. This SPA will only change when a user clicks through
pages, hence the page parameter is the only state we need to record in history.

In the push call the first parameter (`'/'` above) should be the actual URL of
your component. In this application BookList will be mounted at the root URL.

Preventing render when location changes
=======================================

Note a minor issue here is that the BooksList component will be rendered twice
when `handlePageChange` is invoked, once when `this.props.history.push` is
called and once in the `fetchBooks` function when `setState` is called. In our
example pushing state into history will **always** be followed a fetch records.
Let's use the React lifecycle method `componentShouldUpdate` to ignore
locations changes:

```javascript
  shouldComponentUpdate(nextProps) {
    return _.isEqual(this.props.location, nextProps.location);
  }
```

Note, we need to use [Lodash isEqual](https://lodash.com/docs/#isEqual) to
correctly test object equivalence. In the above `shouldComponentUpdate` call we
will not re-render the component when the location changes. In our case that
will safe, but that may not be the case with your components. Treat
`shouldComponentUpdate` with extreme caution.


Browser navigation event handler
================================

The pressing of the back or forward button in a browser is an event. Our
component needs an handler for this event to correctly update the current page
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

XXX Add lots of documentation:

```jsx

const BOOKS_ENDPOINT = `http://localhost:3000/books.json`;

class BookList extends Component {
  constructor(props) {
    super(props);

    this.params = {}; // URL parameter used to fetch a particular set of books

    this.state = {
      books: [],      // The actual list of books from the back-end API
      pagination: {}  // Pagination parameters from the back-end API
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
    this.props.history.push('/artists', this.params);
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
        <h2>Book List</h2>
        <ul>{this.renderBooks()}</ul>
        <Paginator pagination={this.state.pagination} onPageChange={this.handlePageChange} />
      </div>
    );
  }
}

export default withRouter(BookList);
```
