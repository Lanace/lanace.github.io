  ---
layout: article
title: "Top-10-javascript-errors"
categories: articles
modified: 2016-07-13
tags: [js]
comments: true
ads: true
image:
  feature: 
  teaser: 
  thumb: 
---

To give back to our community of developers, we looked at our database of thousands of projects and found the top 10 errors in JavaScript. We’re going to show you what causes them and how to prevent them from happening. If you avoid these "gotchas," it'll make you a better developer.

Because data is king, we collected, analyzed, and ranked the top 10 JavaScript errors. Rollbar collects all the errors for each project and summarizes how many times each one occurred. We do this by grouping errors according to their fingerprints. Basically, we group two errors if the second one is just a repeat of the first. This gives users a nice overview instead of an overwhelming big dump like you’d see in a log file.

We focused on the errors most likely to affect you and your users. To do this, we ranked errors by the number of projects experiencing them across different companies. If we looked only at the total number of times each error occurred, then high-volume customers could overwhelm the data set with errors that are not relevant to most readers.

Here are the top 10 JavaScript errors:

![그림]()

Each error has been shortened for easier readability. Let’s dive deeper into each one to determine what can cause it and how you can avoid creating it.

### Uncaught TypeError: Cannot read property

아마 Javascript 개발자라면 아마 이 error를 많이 봤을것이다. 
Chrome에서 undefined 객체를 가져오려고 할 때 발생하게 되는데, Chrome Developer console에서 아주 쉽게 확인해 볼 수 있다.

![그림]]()

This can occur for many reasons, but a common one is improper initialization of state while rendering the UI components. Let’s look at an example of how this can occur in a real-world app. We’ll pick React, but the same principles of improper initialization also apply to Angular, Vue or any other framework.
 
```javascript
class Quiz extends Component {
  componentWillMount() {
    axios.get('/thedata').then(res => {
      this.setState({items: res.data});
    });
  }

  render() {
    return (
      <ul>
        {this.state.items.map(item =>
          <li key={item.id}>{item.name}</li>
        )}
      </ul>
    );
  }
}
```