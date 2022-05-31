# Task 2

Now your User model is working when you send requests using Postman, make sure you can also register using your **React frontend**.

Remember, to register using your frontend, you need to complete the React form and click "Register an account". Once the button has been clicked, there are **2 stages**:

- **Stage 1**: in `/views/Register.js` (**frontend**), send a HTTP POST request to your server's "/register" path. The `registerPost` controller function for this endpoint (**backend**) should create a new user document in your database and send back a 201 response containing a copy of the new document. **If you completed yesterday's task, this should already work!**
    - You can check this by registering a new user in your browser. Even though you will get an error message, if you check you MongoDB shell / Compass, you will see the user was created. :-)
    - Note that in the `registerPost` controller function (**backend**), when the 201 response is sent you include a copy of the **whole** new document (username, password etc...). 
    - But also note that in `/views/Register.js` (**frontend**), you **only** use the user's unique id from the response. The rest of the data is not needed!
        - Change `/controllers/registerController.js` (**backend**) to send back **only** a unique "id" for the user.
        - Be careful with what you are sending back here! Check out your new document in the MongoDB Shell / Compass if you are unsure...
        - Also make sure in `/views/Register.js` (**frontend**) that you are using **exactly** what you receive in the server response to set the `currentUserId` state variable in `App.js`!

- **Stage 2**: Next, the `Albums` view will be rendered in the browser. As soon as the view is rendered, a `useEffect` will be called to your server's GET /user endpoint to retrieve the necessary details about the user who just registered.
    - You do not need to change the `useEffect` in `/views/Albums.js` (**frontend**). Instead, refactor ("rewrite") your **backend** code to make sure the `fetch` request in the `useEffect` is successful.
    - Remember: your data now lives in MongoDB, not `db.json`!
    - In the controller function (**backend**) for the GET /user route, try to research and implement the new Mongoose method **Model.findById()** to find the correct user document using its id.
    - You can tell if your changes have worked because you will see "Welcome [user's firstname]!" in the top-left corner of the `Albums` view.
    - Make sure you are still also sending back the user's `albums` array from the backend, even though a newly registered user will not have any albums at the moment.
    - Finally, make sure to add error handling (like yesterday) whenever you use Mongoose to query your MongoDB collection.

---

# Task 1

Create and implement a "User" model in the `backend` directory of your new "Albums Project v2" repo!

For now, we will only implement the model when we want to **register** a new user.

## Instructions

1. Install `mongoose` into the `backend` directory of your "albums project v2" repo using npm.
2. Use mongoose in `index.js` to connect to a db called "albums-project" in MongoDB.
3. Inside the `backend` directory, create a `models` directory and add a file called `User.js`.
4. In `User,js`, import `mongoose` and use it to create a **schema** to define how a "User" document should look in your database.
    - What keys do you need to create a "User" document? Also, make sure all the keys are required!
5. Create a **model**, based on the schema, to provide an interface to a collection called `users`.
6. Import your model into `/controllers/registerController.js`.
7. Rewrite the `registerPost` controller function to use your model!
    - New "user" documents should now be saved in the `users` collection of your MongoDB database, instead of the `/data/db.json` file. To do this, you will need to use your **model** to replace all parts of the function currently using LowDB.
    - Remove all parts of the code which are no longer relevant (e.g. imports which are no longer needed, anything we do not need to handle ourselves as MongoDB will automatically handle it...)  
    - Make sure to also add extra **error handling**, in case something goes wrong when using your model to:
        - find your user, or 
        - save your user.
8. Test your changes using **Postman**.
    - Make sure that when you create a user correctly, you receive back a response containing that user's details, **plus**, `albums`, `_id` and `_v` fields.
    - Also try to create a user document without some of the correct fields, and make sure your error handling is working as expected.
    - You can look in the "exercises" Slack channel for examples of how successful and unsuccessful responses should look in Postman. :-)

---

Please copy all tasks into this file! This will create a record of the work you did to complete the project.

Each new task should go to the top of the file. :-)