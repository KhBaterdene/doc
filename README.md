# Create eccommerce
## Auth
Authentication API Routes allow you to manage a customer's session, such as login or log out
### Get Current Customer
query:
 ```gql
 query clientPortalCurrentUser {
    clientPortalCurrentUser {
      _id
      email
      firstName
      lastName
      type
      erxesCompanyId
      phone
      avatar
      customer {
        addresses
      }
      erxesCustomerId
      companyRegistrationNumber
    }
  }
```
code:
```jsx
useQuery(queries.currentUser, {
    fetchPolicy: 'network-only',
    onCompleted(data) {
      const { clientPortalCurrentUser } = data || {};
      // logic
    },
  });
```
Example: [https://github.com/pages-web/techstore/blob/main/src/modules/auth/currentUser.tsx]

### Customer Login
mutation:
 ```gql
mutation ClientPortalLogin(
    $clientPortalId: String!
    $login: String!
    $password: String!
  ) {
    clientPortalLogin(
      clientPortalId: $clientPortalId
      login: $login
      password: $password
    )
  }
```
code:
```jsx
 const [login, { loading }] = useMutation(mutations.login, {
    refetchQueries: [{ query: queries.currentUser }, 'clientPortalCurrentUser'],
    onError(error) {
      return toast.error(error.message);
    }
  });
```
### Customer Logout
mutation:
 ```gql
const logout = gql`
  mutation {
    clientPortalLogout
  }
`;
```
code:
```jsx
 const [login, { loading }] = useMutation(mutations.login, {
    refetchQueries: [{ query: queries.currentUser }, 'clientPortalCurrentUser'],
    onError(error) {
      return toast.error(error.message);
    }
  });
```
Example: [https://github.com/pages-web/techstore/blob/main/src/modules/auth/Login.tsx]

#Auth


