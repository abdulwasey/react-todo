# React Todo App
To be able to make a user, make the user log in, make the user crud objects linked with him, make the user log out with his data saved.

## APPLICATION
User is able to make todos with content subsection to remind himself things.

#### FUNCTIONS
 - User,Todo can be CRUD.
 - Todo can be marked important.
 - Todos have time since they have been created.
 - Completeing todos and adding more makes the plant grow.
 - Todos are in different sections like Code,Work,Home,Gym,Class,Movies, more can be added.
 - Small number guessing game in bottom. 
 - Motivational thought everyday.
 - Countdown till the end of the day in splash screen saying time doesnt wait.
 - Pending todos will be shown.
 - Todo added, done in the week will be shown under profile.

## FLOW 

### REACT

**TO CRUD OBJECTS**

- **UI** - Making forms for the user to type in .
- **STATE** - Sets the data given by thr user in the form of the model of the todo and pass it on to the context in a function with relavant info. All the function are placed in the globalcontext file. which hjas one extra purpose of holding the initialstate of the app. 
- **REQUEST FROM REACT** - for a todo to be registered it has to be a mongo object. the task and content are given from the form and are passed to the request in json with relavent params if required.

**INTEGRATIONPOINT** to the backend

- **RESPONSE** - take the response from  after passing the request. the mongo object or relavent info will be returned in json, the object is also saved in the db now. error handle with clg err if err exist in the response json.  
- **CHANGESTATE** - collect the relavant data from the response for the change of state.here the todos have to be added to the already existing todos.  

**FOR AUTH**
- **PRIVATE ROUTES** - Make private routes. get the info of user loggedin and show components.
- **PUSH USER** - push the user to new url when logged in.
- **PROTECT** - the token we get from response when loggedin request is made. this token is  passed to every request made as a header. this token is verified with the token made for our user when logged in. if verified we can be user is logged in and has acces to make requests.This is all donw in the backend. when react gets the token from response of logged in its stored in the localstorage. this item in localstorage is reffrenced to get that token everytime to put in the header of the requests made in the functions in the globalcontext
- **INITIALIZINGSTATE** - When the user is logged in call a function which requests to get all the info required. here it is all the todos and userinfo this is done by using a useeffecthook in which our function lies whcih requests and gets the response with user and todos, then this response is used to set state.
- **LOGOUT** - the localstrogae token is delted hence cutting of the header. auth file in backend will not verify this anymore so cant make requests

### FLOW IN EXPRESS

 - **MAKE MODELS** - making all the models required for the app. here todo,user,content(as its a subsection in todo),section(todo as subsection)
 - **ROUTES** - configuring routes for every function with relavent parameters if required. configuring is initialising a path and method type. 

##### INTEGRATION POINT

When a call is made from react with parametrs and request data the method type and url is seen and gets matched to the configured routes and calls the respective function. 

 - **FUNCTION CONTROLLER** - when the request data is sent to this function with url params if exists. this function CRUD mongo objects constructed from the request info in json. then relavent info is given back as the response. error handle with try catch with sending error message as response.  
 - **PROTECT ROUTES** - last step is to ensure if user is logged in to make all the requests to get responses. for security use jwt. make all the functions which need the user to be logged in protected by adding auth file as middleware to all the route functions in the controller in logged in route function make the user a token and pass that token in the response for react. react later uses this token as a header to make requests to the backend. the middleware chekcs this token with that of which has been created with the same secret.
 - **MIDDLEWARE** - when a user is logged in we make all the route functions in the controller have access one more piece of info which is the userid and token which we get from header of the request from react. also the userid can be used in the functions. to add a todo to a particular user we can take this userid and use that.


                 
**EXAMPLE OF ADDING A CONTENT TO A Todo**

- UI FORM in login component takes input [uname,pass] as login credentials
- Login Component sets the state in json and send this to globalcontext function. this function send request with uname and pass. gets response with toke. thos token is saved in local storage.
- UI FORM in add content comp takes input [content_text = "sometext"]
- add content Component state converts input into json and passes to globalcontext function [addContent(json of content_text from above),todo id]
- Todo id is a parameter which is passed through the component. the todo id comes from todo list comp when mapping each todo from the state.
- Todos exist in the state cuz gettodos is called as todo list comp is rendered. the gettodos func from globalcontext requests with token 
   and gets response of all todos associated with the user. 
- Addcontent makes request with json of user input and todo id gets back response of todo and new content its id cuz contwnt is a model which 
    is created then this created content is updated in todo obj.
- We want only updated content text so we make the dispatch payload as res.data.todo.content and tid s the tid from the param in func
- In appreducer we now update state by spreading state then spreading todo then adding the payload which is new content to todo.content array.
- Now the content is added. 
