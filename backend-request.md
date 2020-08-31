# Notes for Calling Backend in React

## Use Fake Backend [JSONPlaceHolder](https://jsonplaceholder.typicode.com/)

## Package send HTTP requests

`npm install axios`

## Axios request return a promise

```
import axios from 'axios';

const apiEndpoint = 'https://jsonplaceholder.typicode.com/posts';

# GET
async componentDidMount() {
  const {data: posts} = await axios.get(apiEndpoint);

  this.setState({ posts });
}

# POST
handleAdd = async () => {
  const obj = {title: 'a', body: 'b'};
  const {data: post} = await axios.post(apiEndpoint, obj);

  const posts = [post, ...this.state.posts];
  this.setState({ posts });
}

# PUT / PATCH
handleUpdate = async (post) => {
    post.title = "UPDATED";
    await axios.put(`${apiEndpoint}/${post.id}`, post);
    //await axios.patch(`${apiEndpoint}/${post.id}`, { title: post.title });
  };
```

## Lifecyle of a Request

Every http request has a property called method that determines the intent of the request.
The common methods we have are:

- Get: getting data
- Post: for creating data
- Put: for update all of properties of data
- Patch: for udpate a property of data
- Delete: for deleting data
- Options: cause frontend & backend are in a 2 different domains, for security reason, whenever an application send an
  ajax request to different domain the browser always sends an option request

### HTTP Code

- 201: in response of post request --> data created.

## Optimistic vs Pessimistic Updates

### Pessimistic Update

When we are calling the server first and then we are updating the view or the ui.
=> if an error occur while calling server, the rest of this function will not be executed.
=> we're not sure if the call to server is succeeded or failed

### Optimistic Update

We assume that most of the time the call to service succeeded.
Instead of calling the server first, which is going to take sometime, we're going to go ahead and update and udpate the ui.
Then we will call the server. If this call fails, we can reverse ui back to the previous state.

### How to implement Optimistic Update

- Keep the original state
- Update the UI with new state
- Call the server to update
- Catch if something goes wrong:
  - show error or alert
  - rollback the state to the original state