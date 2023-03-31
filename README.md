# how to use json-server-auth with react (login and register and logout)

First Download This Repo And install Npm package

```bash
npm i
```

To use json-server-auth with React, you can follow the following steps to implement login, register, and logout functionalities:

Install json-server-auth and axios

 packages using npm or yarn in your React project:

```bash
npm install json-server-auth axios
```
or

```bash
yarn add json-server-auth axios
```

Create a db.json file in your project's root directory and add the following content:

```bash
{
  "users": []
}
```

Create a server.js file in your project's root directory and add the following code:

```bash
const jsonServer = require('json-server');
const auth = require('json-server-auth');
const server = jsonServer.create();
const router = jsonServer.router('db.json');
const middlewares = jsonServer.defaults();

server.use(middlewares);
server.db = router.db;
server.use(auth);
server.use(router);

server.listen(3001, () => {
  console.log('JSON Server is running');
});
```

Run the server.js file using the following command:

```bash
node server.js
```

In your React app, create a AuthService.js file and add the following code to implement the login, register, and logout functionalities:

```bash
import axios from 'axios';

const API_URL = 'http://localhost:3001';

class AuthService {
  login(email, password) {
    return axios
      .post(`${API_URL}/login`, { email, password })
      .then((response) => {
        if (response.data.token) {
          localStorage.setItem('user', JSON.stringify(response.data));
        }
        return response.data;
      });
  }

  logout() {
    localStorage.removeItem('user');
  }

  register(name, email, password) {
    return axios.post(`${API_URL}/register`, { name, email, password });
  }

  getCurrentUser() {
    return JSON.parse(localStorage.getItem('user'));
  }
}

export default new AuthService();
```

In your React components, you can use the AuthService methods to implement the login, register, and logout functionalities:

```bash
import React, { useState } from 'react';
import AuthService from './AuthService';

function Login() {
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');

  const handleLogin = (e) => {
    e.preventDefault();

    AuthService.login(email, password)
      .then(() => {
        window.location.reload();
      })
      .catch((error) => {
        console.log(error);
      });
  };

  return (
    <div>
      <form onSubmit={handleLogin}>
        <input type="email" placeholder="Email" onChange={(e) => setEmail(e.target.value)} />
        <input type="password" placeholder="Password" onChange={(e) => setPassword(e.target.value)} />
        <button type="submit">Login</button>
      </form>
    </div>
  );
}

function Register() {
  const [name, setName] = useState('');
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');

  const handleRegister = (e) => {
    e.preventDefault();

    AuthService.register(name, email, password)
      .then(() => {
        window.location.reload();
      })
      .catch((error) => {
        console.log(error);
      });
  };

  return (
    <div>
      <form onSubmit={handleRegister}>
        <input type="text" placeholder="Name" onChange={(e) => setName(e.target.value)} />
        <input type="email" placeholder="Email" onChange={(e) => setEmail
```

Complete

Oops, it seems that my previous response got cut off. Here's the complete 

answer:

In your React components, you can use the AuthService methods to implement the login, register, and logout functionalities:

```bash
import React, { useState } from 'react';
import AuthService from './AuthService';

function Login() {
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');

  const handleLogin = (e) => {
    e.preventDefault();

    AuthService.login(email, password)
      .then(() => {
        window.location.reload();
      })
      .catch((error) => {
        console.log(error);
      });
  };

  return (
    <div>
      <form onSubmit={handleLogin}>
        <input type="email" placeholder="Email" onChange={(e) => setEmail(e.target.value)} />
        <input type="password" placeholder="Password" onChange={(e) => setPassword(e.target.value)} />
        <button type="submit">Login</button>
      </form>
    </div>
  );
}

function Register() {
  const [name, setName] = useState('');
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');

  const handleRegister = (e) => {
    e.preventDefault();

    AuthService.register(name, email, password)
      .then(() => {
        window.location.reload();
      })
      .catch((error) => {
        console.log(error);
      });
  };

  return (
    <div>
      <form onSubmit={handleRegister}>
        <input type="text" placeholder="Name" onChange={(e) => setName(e.target.value)} />
        <input type="email" placeholder="Email" onChange={(e) => setEmail(e.target.value)} />
        <input type="password" placeholder="Password" onChange={(e) => setPassword(e.target.value)} />
        <button type="submit">Register</button>
      </form>
    </div>
  );
}

function Header() {
  const currentUser = AuthService.getCurrentUser();

  const handleLogout = () => {
    AuthService.logout();
    window.location.reload();
  };

  return (
    <div>
      {currentUser ? (
        <div>
          <span>{currentUser.name}</span>
          <button onClick={handleLogout}>Logout</button>
        </div>
      ) : (
        <div>
          <a href="/login">Login</a>
          <a href="/register">Register</a>
        </div>
      )}
    </div>
  );
}

export { Login, Register, Header };
```

Finally, you can use the Login, Register, and Header components in your React app as follows:

```bash
import React from 'react';
import { BrowserRouter as Router, Route, Switch } from 'react-router-dom';
import { Login, Register, Header } from './components';

function App() {
  return (
    <Router>
      <Header />
      <Switch>
        <Route exact path="/login" component={Login} />
        <Route exact path="/register" component={Register} />
      </Switch>
    </Router>
  );
}

export default App;
```

Note that you may need to adjust the API URL (http://localhost:3001 in this example) depending on the configuration of your json-server instance.



use fetch function
Sure, you can use the fetch function in place of axios in the AuthService methods. Here's how you can modify the AuthService to use fetch instead:

```bash
const API_URL = 'http://localhost:3001';

const AuthService = {
  login: (email, password) => {
    return fetch(`${API_URL}/auth/login`, {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({ email, password }),
    })
      .then((response) => {
        if (!response.ok) {
          throw new Error('Network response was not ok');
        }
        return response.json();
      })
      .then((data) => {
        if (data.token) {
          localStorage.setItem('user', JSON.stringify(data));
        }
        return data;
      });
  },

  register: (name, email, password) => {
    return fetch(`${API_URL}/auth/register`, {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({ name, email, password }),
    })
      .then((response) => {
        if (!response.ok) {
          throw new Error('Network response was not ok');
        }
        return response.json();
      })
      .then((data) => {
        if (data.token) {
          localStorage.setItem('user', JSON.stringify(data));
        }
        return data;
      });
  },

  logout: () => {
    localStorage.removeItem('user');
  },

  getCurrentUser: () => {
    return JSON.parse(localStorage.getItem('user'));
  },
};

export default AuthService;
```

The fetch function returns a Promise that resolves with a Response object. You can check if the response was successful by checking the ok property of the Response object. If the response was not successful, you can throw an error or handle the error as appropriate.

In the login and register methods, you can pass the email and password or name, email, and password respectively in the request body as a JSON string using JSON.stringify().

In the then callback, you can parse the response JSON using the json() method of the Response object. If the response contains a token, you can store it in localStorage using localStorage.setItem().


Complete

Here's an example of how you can use the modified AuthService with fetch in a React component:

```bash
import React, { useState } from 'react';
import AuthService from './AuthService';

const Login = () => {
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');

  const handleLogin = (e) => {
    e.preventDefault();
    AuthService.login(email, password)
      .then((data) => {
        console.log(data);
        // Do something with the response data
      })
      .catch((error) => {
        console.error(error);
        // Handle the error as appropriate
      });
  };

  return (
    <form onSubmit={handleLogin}>
      <label>
        Email:
        <input type="email" value={email} onChange={(e) => setEmail(e.target.value)} />
      </label>
      <label>
        Password:
        <input type="password" value={password} onChange={(e) => setPassword(e.target.value)} />
      </label>
      <button type="submit">Login</button>
    </form>
  );
};

export default Login;
```

In this example, we create a login form with email and password input fields. When the form is submitted, we call the handleLogin function. In the handleLogin function, we call the AuthService.login method with the email and password entered by the user.

We then handle the response data using the then method. In this example, we simply log the response data to the console, but you can do something more useful with the data, such as updating the UI or storing it in a state variable.

If an error occurs during the login process, we catch the error using the catch method and handle it as appropriate.

You can use a similar approach for the register and logout methods in your React components.

Complete

Here's an example of how you can use the modified AuthService with fetch in a React component for registering a new user:

```bash
import React, { useState } from 'react';
import AuthService from './AuthService';

const Register = () => {
  const [name, setName] = useState('');
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');

  const handleRegister = (e) => {
    e.preventDefault();
    AuthService.register(name, email, password)
      .then((data) => {
        console.log(data);
        // Do something with the response data
      })
      .catch((error) => {
        console.error(error);
        // Handle the error as appropriate
      });
  };

  return (
    <form onSubmit={handleRegister}>
      <label>
        Name:
        <input type="text" value={name} onChange={(e) => setName(e.target.value)} />
      </label>
      <label>
        Email:
        <input type="email" value={email} onChange={(e) => setEmail(e.target.value)} />
      </label>
      <label>
        Password:
        <input type="password" value={password} onChange={(e) => setPassword(e.target.value)} />
      </label>
      <button type="submit">Register</button>
    </form>
  );
};

export default Register;
```

In this example, we create a registration form with name, email, and password input fields. When the form is submitted, we call the handleRegister function. In the handleRegister function, we call the AuthService.register method with the name, email, and password entered by the user.

We then handle the response data using the then method. In this example, we simply log the response data to the console, but you can do something more useful with the data, such as updating the UI or storing it in a state variable.

If an error occurs during the registration process, we catch the error using the catch method and handle it as appropriate.

You can use a similar approach for the logout method in your React components.


The getCurrentUser() function is a part of an authentication service that uses localStorage to store user information. It retrieves the currently logged-in user's data from localStorage and returns it as a JSON object. Here's an example of how to use the getCurrentUser() function:

```bash
import AuthService from './AuthService';

const user = AuthService.getCurrentUser();
if (user) {
  console.log(user.email); // Output: the email of the currently logged-in user
} else {
  console.log('No user is currently logged in.');
}
```

This code will retrieve the currently logged-in user's information from localStorage and log their email to the console. If there is no user currently logged in, it will log the message "No user is currently logged in." to the console.

Note that getCurrentUser() returns null if there is no user currently logged in. Therefore, you should always check whether the returned value is null before trying to access any properties on it.

The logout() function is a part of the same authentication service as the getCurrentUser() function. It is used to clear the currently logged-in user's data from localStorage.

Here's an example of how to use the logout() function:

```bash
import AuthService from './AuthService';

AuthService.logout(); // clears the user data from localStorage
```

This code will call the logout() function of the AuthService and clear the user data from localStorage. After calling this function, the getCurrentUser() function will return null until a user logs in again.

You can call the logout() function whenever you need to log out a user. For example, you might want to call this function when a user clicks on a "Logout" button in your app.
