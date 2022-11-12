# AWS-Cognito-demo
This is a demo of using AWS Cognito services for implementing user sign-up and sign-in. I 

> Amazon Cognito offers user pools and identity pools. User pools are user directories that provide sign-up and sign-in options for your app users. 
With Cognito User Pools, you can easily and securely add sign-up and sign-in functionality to your mobile and web apps with a fully-managed service 
that scales to support hundreds of millions of users.

## Reference 
https://aws.amazon.com/blogs/mobile/accessing-your-user-pools-using-the-amazon-cognito-identity-sdk-for-javascript/


## Workflow
First, we need to create a user pool on AWS console. To access the Cognito user pool from the web application, we need to
[install](https://www.npmjs.com/package/amazon-cognito-identity-js) the Amazon Cognito Identity SDK for JavaScript. This can be done in two ways -- either by downloading the 
sdk into our project directory and import it as a script into our javascript code, or by calling the npm function through command line. 

    npm i amazon-cognito-identity-js

Having installed the SDK, the next step is to create a `CognitoUserPool` object by providing a UserPoolId and a ClientId as shown below. 

    var userpooldata = {
            UserPoolId: _config.cognito.UserPoolId,
            ClientId: _config.cognito.ClientId,
          }; 
    var userpool = new AmazonCognitoIdentity.CognitoUserPool(userpooldata);
   
 Next, provides methods to let user sign-in. Optionally, we can create user attributes to be included in the user's attribute list. The attributes will be saved in the user pool along with 
 their password and username. 
 
    //create user attribute
      var emaildata = {
        Name: 'email',
        Value: email,
      };
      var emailAtt = new AmazonCognitoIdentity.CognitoUserAttribute(emaildata);
  
    //signing up by using a username, password, attribute list, and validation data.
      userpool.signUp(username, password, [emailAtt], null, (err, result) => {
        if (!err) {
          alert('success! please enter your verification code below');
          //verification
          form2.classList.add('enter');
        } else {
          console.log(err);
        }
      }
   
 What goes next is to prompt the user to confirm their email address by entering the code that they receive through email. The relevant method here is `cognitoUser.confirmRegistration(code, true, callback function).
 `There can be two ways of accessing the `cognitoUser` object.  
 
 Create it by providing the user's username and user pool
 
    var userData = {
        Username: username,
        Pool: userpool,
      };
    var cognitoUser = new AmazonCognitoIdentity.CognitoUser(userData);
 
 Alternatively, 
 
    userPool.signUp('username', 'password', attributeList, null, function(err, result){
    cognitoUser = result.user;
    console.log('user name is ' + cognitoUser.getUsername());
    });
  


