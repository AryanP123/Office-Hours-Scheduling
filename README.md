# Office-Hours-Scheduling
Provides best times to host office hours based on schedules, availability, and rankings.

Front-End
- Implemented through React
- Executed out of the App.js file (routing demonstrated there)
- There are five main pages
  - Login page (.../login)
    - Authentication is used by requesting the list of login credentials and verifying the email and password across the entries
    - Will need to create users for your “own” experience
      - This is described below in the sign up page section, but if need be the following credentials will work: nlhardy@umass.edu, password
  - Sign up page (…/signup, SignupComponent.jsx)
    - A form component with necessary fields
    - Currently unable to POST entries to the login endpoint, so manually enter new registrants at the API page (check “API Endpoints” below)
  - Home page (.../home, Home.js)
    - Displays the schedule that will be deployed for the entire course
    - Right now it is being grabbed from the professor’s offered times, not an individual endpoint for the selected OH
    - separate view for professor that allows them to modify the selected schedule
      - Under construction due to endpoint issues
    - The Wrapper.jsx function is applied to this page (see below)
  - Calendar page (.../calendar)
    - This page is to display the office hours schedule with a calendar view
    - Unfinished
    - The Wrapper.jsx function is applied to this page (see below)
  - Profile page (.../profile)
    - Here is where the user is able to view the offered office hours time, select available times, and rank the offered times
    - Lacking some essential fetch calls due to issues with displaying availability on the grid
    - Check console to see the times being pulled in through the fetch
    - Issues with the useEffect function and persistent student data
    - Wrapper function is applied to this page (see below)
- Some additional components
  - The Wrapper component (wrapper.jsx)
    - Applies the sidebar and headers to each page
  - Sidebar component (Sidebar folder)
    - Provides navigation capabilities through icon buttons
    - May be causing some issues with persistent global context state and user information to be forewarned
- Notes
  - Endpoint connection
    - Due to issues with POST and PATCH, we were unable to send any changes made on the frontend to the backend
    - CORS policy causes issues when doing non-generic fetch requests because of it’s aim to prevent malicious capabilities
  - Loading root
    - I believe that there are issues with rendering the website after having it already running where the opened page may not be Login
    - If that is the cause, enter “.../login” in URL and start from their
  - Getting it running
    - I have uploaded the ZIP for the project excluding the node modules due to size constraints. However, creating your own app and adding the src folder to it will allow the app to run locally. I have found that things like React-router-dom, axios, and big-calendar still need to be installed in the directory to run, because they are not a part of the native modules.
    - To run:
      - Pull the source folder into an app created using “npx create-react-app [chosen_name]”
      - Type “npm install” to cover as many dependencies as possible
      - Type “npm start” to then run the app
      - Check error codes to see if your Node JS is missing dependencies on your local machine. This is when you may need to install the libraries mentioned above.

- Back-End
  - Wrote a Python script that generates random office hours for professors and students to help implement our matching algorithm and web pages
    - Creates unique IDs for students and manages their availability and schedules.
    - Saves data as a JSON or CSV file
  - API Endpoints ( https://django-deployment-office-hours.vercel.app/?format=api )
    - Created a Django project that connects to our PostgreSQL database with models representing data tables for each type: Student 
Schedules, Students Availability, Professor Options, Student Rankings, Login
    - Hosted Django project to be remotely accessible using Vercel
      - Data can be obtained, updated, deleted, and added using JavaScript Fetch API
    - Created database in PostgreSQL, hosted on AWS RDS for remote access

  - Algorithm Implementation in Python:
    - Can be accessed from OH Scheduling Algorithm /schedule_generator.py in Github
    - Instructions to run the program to get appropriate results at the end of the file under ‘How to Run’, line 132
    - Utilizes files "Student_Availability.csv" and "Student_Rankings.csv" to access corresponding tables
    - 3 functions can be called corresponding to three different office hours schedules:
      - The first schedule generates the n most popular office hour times 
      - The second schedule generates the n most popular office hour times which are spread out across the week
      - The third schedule generates the office hour schedule with n times which maximizes students attending at least one office hour
    - API endpoints in Flask help access the function, however the implementation has the server running locally

  - Algorithm Logic:
    - Each of the functions first convert the date/times from the tables into a hashed format of {[office hour time]: [number of students available/who prefer this time]}.
    - To get the most popular office hours, these times are weighted according to student rankings for available office hours weighted by a weighting factor which can be given as input (default weight is 1) and top ‘n’ are chosen
    - To  spread these times out across the week, time slots on unique days are given high preference, so that there is at least one (or more) office hours time offered on each day of the week
    - To maximize student attendance, a brute force algorithm randomly samples a large number of possible office hours schedules, and chooses the best one by dynamically updating the schedule. 
    - The professor can choose any of these schedules. The algorithm also displays the percentage of the class covered by each of these schedules to help facilitate the professor’s decision
