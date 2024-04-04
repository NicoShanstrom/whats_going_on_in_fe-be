# What's Going On in the FE/BE?

The goal of this activity is to leverage our collective FE and BE knowledge to explain how this application works. At the end of this activity you should have more understanding of how the other side of the stack functions as well as a diagram that illustrates how information flows between FE and BE.

## Learning Goals

* Understand how information flows between the FE and BE
* Get exposure to what the other program has been working on
* Understand common misconceptions and pain points experienced by FE and BE devs
* Meet students in the other program

## Setup

Clone this repo and `cd` into it.

The rest of this setup assumes you have Homebrew, Node, and npm installed.

### 1. Install Ruby

The Backend repo uses Ruby 3.2.2, so if you haven't already you will need to install a Ruby version manager and install Ruby 3.2.2. Follow the steps under "Install rbenv" in [these instructions](https://mod0.turing.edu/computer-setup#install-rbenv-back-end-students-only). This will guide you through installing rbenv as well as Ruby 3.2.2.

### 2. Get the BE running on localhost:3001

* Navigate to the be directory: `cd be`
* Install dependencies: `bundle install`
    * If you get an error that Bundler is not installed, install it with `gem install bundler`
* Set up the database: `rails db:{drop,create,migrate,seed}`
* run the server with `rails server`

After you run the server you should see that it is waiting for requests. Open a new Terminal window to continue with setup.

### 3. Get the FE running on localhost:3000
In a different terminal window,

* Navigate to the fe directory: `cd fe`
* Install dependencies: `npm install`
* Run the app: `npm start`
* visit `localhost:3000` in your browser

If everything is set up properly you should see a friendly welcome message.

If everything _isn't_ set up correctly, you may see a message that starts with "ERROR"... Your first task will be to solve this error. To do this, you'll need to collaborate with the other developers on your team! 

1. FE: Look at DevTools' Console. Are there any errors present? Share these messages with your BE partner. 
2. BE: Check your server logs in terminal. Are there any errors present? Share this terminal with your FE partner.


## Solving CORS
CORS stands for `Cross-Origin Resource Sharing`. Read more about it [here](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS), or you can search for more topics on your own. 

* ❓ FE devs: What is the difference between a `FETCH` request, and a `GET` request? Help your BE bretheren understand this difference by researching/discussing together before continuing. 

In a nutshell, CORS is a rule that says a `FETCH` request is different from a normal `GET` request, and _servers_ (or BE applications) should govern which _origins_ (or applications that make FETCH requests) it will send responses to. To do this, the BE application can set a header for every request. That header would look like: 

```ruby

"Access-Control-Allow-Origin": "*"

```

... but, the above header would allow _every_ origin (or domain) on the internet to access the responses generated by our application. Does that sound secure to you? 🤔

Rather than allowing all, let's specify which domain is allowed access to a response from our app. Follow these instructions together: 

1. Open the `be/app/controllers/api/v1/welcome_controller.rb` file and look at line 13. This line is commented out. 
2. Uncomment it, and restart your rails server (`CTRL + C` in Terminal and `rails s` again). Does the request work now? 

This approach _can_ work, but is not a conventional way to solve this problem in Rails. It would also mean we'd have to have this header present on any controller, or any action in each controller, which can quickly get out of hand. 

Let's try a more sustainable approach: 

1. Re-comment out line 13. 
2. Find `be/config/initializers/cors.rb`. Read this file's instructions and the linked documentation. (By the way, this file was generated by Rails!)
3. Uncomment lines 8-16, changing `"example.com"` to the origin of your front-end (for our example, that's `"localhost:3000"`), and restart your app. Try the request again. Does the request still work? 

### Questions to answer as a team

Ask these questions out loud, and share answers. Ask your partner to help define any technical terms you are unfamiliar with.

* ❓ What was line 13 in our controller actually doing? 
* ❓ What is a Header? 
* ❓ In your own words, what is CORS?


## Exploration Activity

In small groups, designate one person to be the screen sharer.

1. FE students: explain to the group what happens when you visit `localhost:3000` in your browser. Start with `index.html` and move to `index.js`. From there, trace all the way through the code up to the point where a request to the BE is made.
1. BE students: explain to the group what happens when your Rails app receives a request. Trace all the way through the code up to the point that a response is sent back to the FE.
1. FE students:, explain what happens once the FE app receives the response.
1. Once you are finished exploring the code, draw a diagram that illustrates the entire process. Specify which parts are FE and which are BE. 

## Practice Activity

Now that you know how each part of the application handles their request/response cycles, how would you collaborate as a combined team to consume a new set of data? 

In both applications, implement the ability to list *all* the messages from the BE's database on a new FE page. Follow these steps: 

1. Determine what data will be consumed/exposed via API. (For this exercise, we recommend all the `messages` on an `index` page - you may want to add more to the `seeds.rb` file.)
2. Write a user story.<br>
   Example:
   
   ```text
   As a user,
   when I visit the messages index page (/messages)
   Then I see a list of all the messages in the system as links
   And when I click on one of the messages,
   I am taken to that message's show page (/messages/:id)
   where I only see that message.
   ```
4. Agree on a [JSON contract](https://gist.github.com/cjsim89/2597824466ceeef6c9f3c65cd8d57478) for any requests & their responses required to fulfill the user story.<br>
   Example:
   ```text
   REQUEST: 
   GET /messages
   - No request body
   - No additional headers or keys required

   RESPONSE:
   - JSON format

   {
      data: {
         "type": "message",
         "id": "1",
         "attributes": {
            "text": "Here is a message!"
         }
   
      }
   }
   ```
<br>

# Checks for Understanding

### For all students: 

* What are some similarities about both BE & FE that you learned about today?
* What are some tools you can use to ensure each person understands the requirements of a given project?

### For BE students:

* What is a Component in React?
* What is a lifecycle method?
* What is `fetch`?

### For FE students:

* What are routes?
* What are controllers? What are actions? 
* How can you find a controller action for a particular route?

<br>

# Extension

If you have time, answer these questions together:

* Is there any technical vocab words that the developers in the other program use that you don't understand? If so, try to help each other understand the meaning of those words.
* How could you get the app to display one of the other messages found in `seeds.rb`?
* What’s the difference between a database and an API? Is there an analogy that can help describe this?
* What are frustrations that BE developers might experience when working with FE developers (for example, building an API/database based on specific requests from the FE team)?
* What are frustrations that FE developers might experience when using technology built by BE developers (for example, accessing an API)?
* What are some similarities between React and Rails? 


