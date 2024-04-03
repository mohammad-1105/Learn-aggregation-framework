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

3. What is the average age of all the users ?

```javascript
[
  {
    $group: {
      _id: null, // group the whole document in one doc
      // write custom field
      averageAge: {
        $avg: "$age", // calculate the avg based on age
      },
    },
  },
];
```

4. What is the average age of female and male users ?

```javascript
[
  {
    $group: {
      _id: "$gender", // group the whole document in one doc

      // write custom field
      averageAge: {
        $avg: "$age", // calculate the avg based on age
      },
    },
  },
];

// output: will be like

_id: "female";
averageAge: 29.81854043392505;

_id: "male";
averageAge: 29.851926977687626;
```

5. List the top 5 common fruits among the users.

```javascript
[
  {
    $group: {
      _id: "$favoriteFruit", // group whole docs based on favoriteFruit field

      // write custom field name
      countFavFruit: {
        $sum: 1, // used accumulator operator
      },
    },
  },
  {
    $sort: {
      countFavFruit: -1, // sorted in descending order
    },
  },
  {
    $limit: 5, // set the limit of 5 to show 5 common favoriteFruit
  },
];
```

6. How many users are from 'USA' ?

```javascript
[
  {
    $match: {
      "company.location.country": "USA", // filter the docs having 'USA'
    },
  },
  {
    $count: "no_of_users_from_usa", // count the docs having 'USA'
  },
];
```

7. Find the total numbers of male and female

```javascript
[
  {
    $group: {
      _id: "$gender",
      count: { $sum: 1 },
    },
  },
];
```

8. Which country has the highest numbers of registered users?

```javascript
[
  {
    $group: {
      _id: "$company.location.country",
      registeredUsers: {
        $sum: 1,
      },
    },
  },
  {
    $sort: {
      registeredUsers: -1,
    },
  },
  {
    $limit: 1,
  },
];
```

9. List all the unique eye colors present in the collection

```javascript
[
  {
    $group: {
      _id: "$eyeColor",
    },
  },
];
```

10. What is the average number of tags per user ?

`Example 1`

```javascript
[
  // 1st stage , $addFields add new field in existing doc
  {
    $addFields: {
      numberOfTags: { $size: "$tags" }, // $size count the number of element in array
    },
  },
  {
    $group: {
      _id: null,
      averageNumberOfTags: {
        $avg: "$numberOfTags",
      },
    },
  },
];
```

`Example 2`

```javascript
[
  {
    $unwind: "$tags",
  },
  {
    $group: {
      _id: "$_id",
      numberOfTags: { $sum: 1 },
    },
  },
  {
    $group: {
      _id: null,
      averageNumberOfTags: {
        $avg: "$numberOfTags",
      },
    },
  },
];
```

11. How many users have 'enim' as one of their tags ?

```javascript
[
  {
    $match: {
      tags: "enim",
    },
  },
  {
    $count: "numberOfUsersWithEnimTags",
  },
];
```

12. What are the names and the age of users who are inactive and have 'velit' as a tag ?

```javascript
[
  {
    $match: {
      isActive: false,
      tags: "velit",
    },
  },
  {
    $project: {
      _id: 0,
      name: 1,
      age: 1,
    },
  },
];
```

13. How many users have a phone number starting with '+1 (940)' ?

```javascript
[
  {
    $match: {
      "company.phone": /^\+1 \(940\)/,
    },
  },
  {
    $count: "usersWithSpecialPhoneNumber",
  },
];
```

14. List 5 users registered recently

```javascript
[
  {
    $sort: {
      registered: -1,
    },
  },
  {
    $limit: 5,
  },
  {
    $project: {
      _id: 0,
      name: 1,
      registered: 1,
      age: 1,
    },
  },
];
```

15. Categorize users by their favorite Fruits.

```javascript
[
  {
    $group: {
      _id: "$favoriteFruit",
      userNames: { $push: "$name" }, // push creates an array and accumulate the required fields
    },
  },
];
```

16. How many users have 'ad' tag as the second tag in thier list of tags ?

```javascript
[
  {
    $match: {
      "tags.1": "ad", // "tags.1" means to check in the 2nd element of tags array
    },
  },
  {
    $count: "usersHavingAdAsASecondTag",
  },
];
```

17. Find users who have both 'enim' and 'id' as thier tags.

```javascript
[
  {
    $match: {
      tags: {
        $all: ["enim", "id"], // $all is a shortcut specifically for checking if an array contains all the listed values.
      },
    },
  },
];
```

18. List all the companies located in the "USA" with thier corresponding users count.

```javascript
[
  {
    $match: {
      "company.location.country": "USA",
    },
  },
  {
    $group: {
      _id: "$company.title",
      count: { $sum: 1 },
    },
  },
];
```
