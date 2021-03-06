---
title: Mock Axios in React Jest+Enzyme Tests
layout: default
comments: true
published: true
---

Mock Axios in React Jest+Enzyme Tests
=====================================

[React](https://reactjs.org) applications generated by
[create-react-app](https://github.com/facebookincubator/create-react-app)
provide a pre-configured [Jest](https://facebook.github.io/jest) test runner.
Adding the [Enzyme](http://airbnb.io/enzyme) test utilities to the mix makes
testing simple React components a breeze.

However, not all React components are simple, some may speak to a back-end API,
using [Axios](https://github.com/axios/axios) or Fetch for example, to populate
state for later rendering. Components such as these can be a challenge to test
if your goal is to keep these tests isolated and fast. Ideally we want to
eliminate network hops in tests.

This post will describe how to use Jest
[Mocks](https://facebook.github.io/jest/docs/en/mock-functions.html#content) to
simulate Axios in React component tests.

Example Application
===================

For this article we will reference a simple React application. The main
component, named `BookList`, will render a list of books fetched from an API
endpoint.

A rough outline of the `BookList` component would be:

```jsx
const BOOKS_ENDPOINT = 'http://localhost:3000/books.json';

class BookList extends Component {
  constructor(props) {
    super(props);

    this.state = {
      books: [],
    };
  }

  componentDidMount() {
    axios.get(BOOKS_ENDPOINT)
      .then(response => {
        this.setState({
          books: response.data.books
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
      </div>
    );
  }
}

export default BookList;
```

Axios Mock
==========

Since we do not want to speak to a back-end API in our tests, we need to mock
Axios. This is best done by creating an `axios.js` file in a top-level
`__mocks__` directory as follows:

```javascript
import { data as books } from './books.json';

const BOOKS_ENDPOINT = 'http://localhost:3000/books.json';

module.exports = {
  get: jest.fn((url) => {
    switch (url) {
      case BOOKS_ENDPOINT:
        return Promise.resolve({
          data: books
        });
    }
  })
};
```

This mock uses a pre-cooked list of books stored in a JSON file `books.json`
also stored in the `__mocks__` directory:

```json
{
  "data": {
    "books": [
      {
        "id": 1,
        "title": "ABC"
      },
      {
        "id": 2,
        "name": "DEF"
      }
    ]
  }
}
```

Note, the `axios.js` mock provides scope to return unique JSON responses for
different URLS by simply extending the `switch` statement.

Component Test
==============

An Ezyme snapshot test for a simple component would look something like:

```jsx
  it('MyComponent snapshot test', () => {
    const wrapper = shallow(<MyComponent />);
    expect(wrapper).toMatchSnapshot();
  });
```

However, such a test will not work for an Axios based component, even our mocked
version, since the retrieval of the books is an asynchronous operation done in
the background. If the above test template where applied to our `BookList`
component, the snapshot would end up containing no books.

The test needs to wait for list of books to populate component state. That is
best done by waiting for the `Promise` queue to be flushed as follows:

```jsx
const flushPromises = () => new Promise(resolve => setImmediate(resolve));

it('BookList snapshot test', async () => {
  const wrapper = shallow(<BookList />);
  await flushPromises();
  wrapper.update();
  expect(wrapper).toMatchSnapshot();
});
```

We have marked our test function as being `async`; after we create the shallow
component we `await` the flushing of the `Promise` queue before re-rendering
the component with the `update` function. Now our snapshot will contain the
list of books retrieved from the Axios mock.

This test will now correctly confirm that the `BookList` component can render a
list of books retrieved from a back-end API whilst mocking away the network
hop.

References
==========

Most of the information of this post was gleamed from these bits 'n pieces on
the interwebs:

* [how do i test axios in jest](https://stackoverflow.com/questions/45016033/how-do-i-test-axios-in-jest)

* [Test Methods and Mock Dependencies in Vue.js with Jest](https://alexjoverm.github.io/2017/09/25/Test-Methods-and-Mock-Dependencies-in-Vue-js-with-Jest)

* [Provide an API to flush the Promise resolution queue](https://github.com/facebook/jest/issues/2157#issuecomment-279171856)

* [Testing Promise Side Effects with Async/Await](https://blog.rescale.com/testing-promise-side-effects-with-asyncawait)
