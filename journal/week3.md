# Week 3 — Decentralized Authentication

## AWS Cognito 
AWS Cognito offers user authentication, authorization, and user management functionality to web and mobile applications.  
We create an user pool using AWS Cognito 
![image](https://user-images.githubusercontent.com/93335543/224342559-7a576a3a-f073-49ad-b301-fb738302570a.png)

## AWS Amplify 
Install AWS Amplify with the following command in the frontend-react-js directory
```
npm i aws-amplify --save
```
You can verify that you have installed AWS, if you search for it in the package.json file 
![image](https://user-images.githubusercontent.com/93335543/224379042-c250b014-150b-4aa5-b46d-5a208e36603c.png)

### Install Amplify
```
import { Amplify } from 'aws-amplify';

Amplify.configure({
  "AWS_PROJECT_REGION": process.env.REACT_AWS_PROJECT_REGION,
  "aws_cognito_identity_pool_id": process.env.REACT_APP_AWS_COGNITO_IDENTITY_POOL_ID,
  "aws_cognito_region": process.env.REACT_APP_AWS_COGNITO_REGION,
  "aws_user_pools_id": process.env.REACT_APP_AWS_USER_POOLS_ID,
  "aws_user_pools_web_client_id": process.env.REACT_APP_CLIENT_ID,
  "oauth": {},
  Auth: {
    // We are not using an Identity Pool
    // identityPoolId: process.env.REACT_APP_IDENTITY_POOL_ID, // REQUIRED - Amazon Cognito Identity Pool ID
    region: process.env.REACT_AWS_PROJECT_REGION,           // REQUIRED - Amazon Cognito Region
    userPoolId: process.env.REACT_APP_AWS_USER_POOLS_ID,         // OPTIONAL - Amazon Cognito User Pool ID
    userPoolWebClientId: process.env.REACT_APP_AWS_USER_POOLS_WEB_CLIENT_ID,   // OPTIONAL - Amazon Cognito Web Client ID (26-char alphanumeric string)
  }
});
```

###Configuring conditional display of components based on whether user is logged in or logged out
We went to the file called "HomeFeedPage.js" and added the followings commands
```
import { Auth } from 'aws-amplify';
// check if we are authenicated
const checkAuth = async () => {
  Auth.currentAuthenticatedUser({
    // Optional, By default is false. 
    // If set to true, this call will send a 
    // request to Cognito to get the latest user data
    bypassCache: false 
  })
  .then((user) => {
    console.log('user',user);
    return Auth.currentAuthenticatedUser()
  }).then((cognito_user) => {
      setUser({
        display_name: cognito_user.attributes.name,
        handle: cognito_user.attributes.preferred_username
      })
  })
  .catch((err) => console.log(err));
};

// check when the page loads if we are authenicated
React.useEffect(()=>{
  loadData();
  checkAuth();
}, [])
```

We added the following in the ProfilInfo.js
//Add this to the import block
import { Auth } from 'aws-amplify';

//replace existing signOut function with the following function
  const signOut = async () => {
    try {
        await Auth.signOut({ global: true });
        window.location.href = "/"
    } catch (error) {
        console.log('error signing out: ', error);
    }
 
 ![image](https://user-images.githubusercontent.com/93335543/224398584-ddd10354-0b4c-4d43-9ead-ed22c081f40d.png)

  
  
