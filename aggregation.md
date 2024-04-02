# Practice questions to learn MongoDB Aggregation frameworks

Note: The data are available in this link [click me](https://gist.github.com/mohammad-1105)

1. How many active users are there ?

```javascript
[
  // this is the first stage
  {
    $match: {
      isActive: true,
    },
  },
  // this is the second stage
  {
    $count: "activeUsers",
  },
];
```

2. How many users have green eyes

```javascript
[
  {
    $match: {
      eyeColor: "green",
    },
  },
  {
    $count: "usersWithGreenEyes",
  },
];
```
