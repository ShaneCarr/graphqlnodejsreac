# server
yarn init
npm init

yarn add apollo-server graphql axios
touch app.js



npm install
npm ci
// first add nodemon: yarn add nodemon --dev
 // first add nodemon: yarn add nodemon --dev
  "scripts": {
    "start": "nodemon src/app.js"
  }
npm install nodemon --dev
npm start


# client
npx create-react-app client-react-apollo
 yarn add apollo-boost @apollo/react-hooks graphql

 all the code 

npm run start


notes
Building the Client-side with React
The first thing we have to do is create a fresh React App by running the following command in the terminal:

npx create-react-app client-react-apollo
Next, we need to install the Apollo and GraphQl packages:

  yarn add apollo-boost @apollo/react-hooks graphql
Now, we can connect Apollo with our React App by updating the index.js file.

Connecting React to Apollo
index.js
import React from 'react';
import ReactDOM from 'react-dom';
import ApolloClient from 'apollo-boost'
import { ApolloProvider } from '@apollo/react-hooks';

import App from './App';
import './index.css';
import * as serviceWorker from './serviceWorker';

const client = new ApolloClient({
  uri: 'https://7sgx4.sse.codesandbox.io'
})


ReactDOM.render(
  <React.StrictMode>
    <ApolloProvider client={client}>
      <App />
    </ApolloProvider>
  </React.StrictMode>,
  document.getElementById('root')
);

serviceWorker.unregister();
As you can see, we start by importing ApolloClient and ApolloProvider. The first helps us inform Apollo about which URL to use when fetching data. And if no uri is passed to ApolloClient, it will take the current domain name plus /graphql.

The second is the Provider which expects to receive the client object to be able to connect Apollo to React.

That said, we can now create a component that shows the data.

Fetching the data
App.js
import React from "react"
import { useQuery } from "@apollo/react-hooks"
import gql from "graphql-tag"
import "./App.css"

const GET_USERS = gql`
  {
    users {
      id
      login
      avatar_url
    }
  }
`
Here, we have a simple GraphQL query that fetches the data. That query will be passed later to useQuery to tell Apollo which data to fetch.

App.js
const User = ({ user: { login, avatar_url } }) => (
  <div className="Card">
    <div>
      <img alt="avatar" className="Card--avatar" src={avatar_url} />
      <h1 className="Card--name">{login}</h1>
    </div>
    <a href={`https://github.com/${login}`} className="Card--link">
      See profile
    </a>
  </div>
)
This presentational component will be used to display a user. It receives the data from the App component and displays it.

Showing the data
App.js
function App() {
  const { loading, error, data } = useQuery(GET_USERS)

  if (error) return <h1>Something went wrong!</h1>
  if (loading) return <h1>Loading...</h1>

  return (
    <main className="App">
      <h1>Github | Users</h1>
      {data.users.map(user => (
        <User key={user.id} user={user} />
      ))}
    </main>
  )
}

export default App
The useQuery hook provided by Apollo receives the GraphQL query and returns three states: the loading, the error, and the data.

If the data are successfully fetched, we pass it to the User component. Otherwise we throw an error.

The complete App.js file
import React from "react"
import { useQuery } from "@apollo/react-hooks"
import gql from "graphql-tag"
import "./App.css"

const GET_USERS = gql`
  {
    users {
      id
      login
      avatar_url
    }
  }
`

const User = ({ user: { login, avatar_url } }) => (
  <div className="Card">
    <div>
      <img alt="avatar" className="Card--avatar" src={avatar_url} />
      <h1 className="Card--name">{login}</h1>
    </div>
    <a href={`https://github.com/${login}`} className="Card--link">
      See profile
    </a>
  </div>
)

function App() {
  const { loading, error, data } = useQuery(GET_USERS)

  if (error) return <h1>Something went wrong!</h1>
  if (loading) return <h1>Loading...</h1>

  return (
    <main className="App">
      <h1>Github | Users</h1>
      {data.users.map(user => (
        <User key={user.id} user={user} />
      ))}
    </main>
  )
}

export default App
