## Improving React + Redux performance with Reselect

Reference repository for the blog post [located here](http://blog.rangle.io/react-and-redux-performance-with-reselect/).

### Getting Started

```
git clone https://github.com/neilff/react-redux-performance/

npm install
npm run dev
```

### Additional Resources

- https://github.com/reactjs/reselect
- http://redux.js.org/docs/recipes/ComputingDerivedData.html

### Redux connect() with multiples actions / states
To achieve what you want you could simply do:

(state) => {
  const { instagram, facebook } = state;
  return {
    state: { instagram, facebook }
  };
}
and

(dispatch) => {
  return {
     actions: bindActionCreators(Object.assign({}, instagramActions, facebookActions), dispatch)
  };
}

###  Redux action creators that are sendMessage and deleteMessage

import React, { Component } from 'react'
import { connect } from 'react-redux'
import { sendMessage, deleteMessage } from './actions'
class ChatComponent extends Component {
  handleSend = message => {
    this.props.sendMessage(message)
  }
  handleDelete = id => {
    this.props.deleteMessage(id)
  }
  render() {
    // ...
  }
}
const mapStateToProps = state => state
// ▽ ▽ ▽ ▽ ▽ ▽ ▽ ▽ ▽ ▽
const mapDispatchToProps = /* this part is to be discussed */
// △ △ △ △ △ △ △ △ △ △
export default connect(
  mapStateToProps,
  mapDispatchToProps,
)(ChatComponent)

### 1 Wrap into Dispatch Manually
const mapDispatchToProps = dispatch => ({
  sendMessage: message => dispatch(sendMessage(message)),
  deleteMessage: id => dispatch(deleteMessage(id)),
})

### 2 Wrap into Dispatch Automatically with Connect
const mapDispatchToProps = {
  sendMessage, // will be wrapped into a dispatch call
  deleteMessage, // will be wrapped into a dispatch call
};

### 3 Use bindActionCreators
import { bindActionCreators } from 'redux'
// ...
const mapDispatchToProps = dispatch => bindActionCreators(
  {
    sendMessage,
    deleteMessage,
  },
  dispatch,
)

### 4 Use bindActionCreators and Extend the Props
import { bindActionCreators } from 'redux'
// ...
const mapDispatchToProps = dispatch => ({
  ...bindActionCreators(
    {
      sendMessage,
      deleteMessage,
    },
    dispatch,
  ),
  otherService, // this is not to be wrapped into dispatch
})

### 5 Wrap into Dispatch Automatically
import * as messageActions from './messageActions'
import * as userActions from './userActions'
// ...
const mapDispatchToProps = {
  ...messageActions,
  ...userActions,
};




- https://stackoverflow.com/questions/35454633/redux-connect-with-multiples-actions-states
- https://blog.benestudio.co/5-ways-to-connect-redux-actions-3f56af4009c8
