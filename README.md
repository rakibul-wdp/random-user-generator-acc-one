# Random User Generator

Welcome to my first ever backend-only project using <b>Software Architectural Pattern</b>. Here I used MVC (Module View Controller). The messy backend code I wrote in the past has become so much pretty that its like a dream. I love <b>MVC</b>. Kudos to [Trygve Mikkjel Heyerdahl Reenskaug](<https://en.wikipedia.org/wiki/Trygve_Reenskaug#:~:text=Trygve%20Mikkjel%20Heyerdahl%20Reenskaug%20(born,Xerox%20Palo%20Alto%20Research%20Center)>) for inventing such an amazing pattern.

[Live Heroku Server](https://floating-fortress-65518.herokuapp.com/) <br>
[Live Vercel Server](https://random-user-genarator.vercel.app/) (on development)

### Technologies

- node
- express
- cors
- axios
- heroku

### APIs

<pre>
https://floating-fortress-65518.herokuapp.com/api/v1/user   Base URL

1. /all               gives all users and limit number of users by using query
  - https://floating-fortress-65518.herokuapp.com/api/v1/user/all
  - https://floating-fortress-65518.herokuapp.com/api/v1/user/all?num=5
  
2. /random            gives a random user
  - https://floating-fortress-65518.herokuapp.com/api/v1/user/random

3. /save              saves a new user
  - https://floating-fortress-65518.herokuapp.com/api/v1/user/save

4. /update?query      updates the user with the id
  - https://floating-fortress-65518.herokuapp.com/api/v1/user/update?id=4

5. /delete?query      deletes a user
  - https://floating-fortress-65518.herokuapp.com/api/v1/user/delete?id=3

6. /bulk-update       updates a number of users
</pre>

### Problems

1. When hit on the /random route, with and without query, first time data shows perfectly. If hit multiple times with and without query the expected result is not shown. Query is used for limiting the number of users. <br>
   This problem arises when I read the users file once at the starting of the users.controllers.js file. <br>

<b>Problem code</b>

```
const data = fs.readFileSync("users.json");
const users = JSON.parse(data);

module.exports.getAllUser = (req, res) => {
if (req.query.num === undefined) {
res.send(users);
} else {
const usersToBeShown = req.query.num;
const result = users.splice(0, usersToBeShown);
res.send(result);
}
};
```

But if I read the file every time using getUser() function the problem is solved. <br>
I solved it on my own. But can not understand why it's happening.

<b>Solved code</b>

```
function getUsers() {
  const data = fs.readFileSync("users.json");
  const users = JSON.parse(data);
  return users;
}

module.exports.getAllUser = (req, res) => {
  if (req.query.num === undefined) {
    const users = getUsers();
    console.log("hit", users);
    res.send(users);
  } else {
    const users = getUsers();
    const usersToBeShown = req.query.num;
    const result = users.splice(0, usersToBeShown);
    console.log("hit with query");
    res.send(result);
  }
};
```
