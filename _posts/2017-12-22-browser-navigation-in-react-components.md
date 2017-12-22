---
title: Browser Navigation in React Components
layout: default
comments: true
published: false
---

Browser Navigation in React Components
======================================

A story, one day you develop a front-end application in
[React](https://reactjs.org) because it is the current hotness. All
goes well, you build a few components, you have them fetch content from a
back-end API, you assemble them and all appears to work well. You then hit the
browser back button in this React application and all suddenly all is not well in
*single-page-application* (SPA) land. Rather than unwinding your navigation
within your React app the browser will entirely exit your application.

[JavaScript SPAs](https://en.wikipedia.org/wiki/Single-page_application)
update content dynamically and often independently of the URL.
Such transistions from one state to the next will frequently not change the
URL. By contrast traditional server side web applications will bind a page to a
URL. The backward and forward buttons of a browser are designed to navigate
from page-to-page, not state-to-state.

Modern browsers do provide a solution for this SPA issue, that being the
[HTML5 History API](https://developer.mozilla.org/en-US/docs/Web/API/Histor),
more specially the `pushState` functionality. Note, you as the SPA developer
will be required to glue all the pieces together, `pushState` is not automatic
or free.

Note, I am a novice when it comes to React, hence the solution described below
may not be the most optimal. I welcome comments.

Example Application
===================

For this article we will reference a simple React application. That main
component, named BookList, simply fetches and renders a paginated list of
books. Next and previous links are provided, bu a Paginator component, to
navigate through the list of books which are retrieved from a back-end API. The
rough outline of the BookList component would be:

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
the React Router package to gain access to that API. Most non-trivial React
applications will already be using React Router, if not then install it:

```
yarn add react-router-dom
```

Next you may need to wrap the component in `withRouter` to gain access to
needed history and location properties. If your component is
already a `Route` component then nothing needs to be otherwise please wrap as
follows:

```jsx
import { withRouter } from 'react-router-dom';
...
export default withRouter(BookList);
```

Now push current state `this.params` into history:

```jsx

  handlePageChange = (page) => {
    this.params.page = page;
    this.props.history.push('/artists', this.params);
    this.fetchBooks();
  }
```

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


Handle back and forward button press via popstate event handler
===============================================================

The pressing of the back or forward button in a browser is an event. Our
component needs an event handler for these events.

```javascript
  componentDidMount() {
    window.onpopstate = this.handlePopState;
    this.applyState();
  }

  componentWillUnmount() {
    window.onpopstate = null;
  }

  handlePopState = (event) => {
    event.preventDefault();
    this.applyState();
  }

  applyState() {
    this.params = this.props.location.state || {};
    this.fetchBooks();
  }
```

Note, it is important to unset the `onpopstate` event handler when this
component is being unmounted otherwise the event handler will persist even
beyond the lifetime of the component.

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
    this.applyState();
  }

  componentWillUnmount() {
    window.onpopstate = null;
  }

  shouldComponentUpdate(nextProps) {
    return _.isEqual(this.props.location, nextProps.location);
  }

  handlePopState = (event) => {
    event.preventDefault();
    this.applyState();
  }

  applyState() {
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
