# Project Overview:
Welcome to the Training Log Application! 

The purpose of this application is to give users the ability to track information for
their respective workouts in order to assist in their progressive overload training.
Once a user has registered and logged in, they will be able to navigate through various
page(s) of the training log that shows up to 10 workouts per page and on each page, they
can click the 'View Workout' link associated with a workout to see more information about
that workout, like the exercises it contains.

CRUD access is not universally available. Specifically, once logged in, a user can only
create, update, and delete workouts and exercises that are associated with their specific
user account. A workout is tied to 1 user. A user can only read another user's records.
This choice was made, so that no user can mess with another user's workout information. 
Users are allowed to view other users' workouts in order to gain inspiration and new
exercise ideas to potentially add to their own routine. In terms of CRUD, a user can also
delete their user record and all associated workout and exercise data.

# Version Info:
PostgreSQL: 9.6.22
Ruby: 2.6.3
Browser: Google Chrome Version 100.0.4896.88

# How To Run The Application:
Part 1: Setting Up the SQL Database + Installing Dependencies

Brief Overview:
The database consists of 3 tables and two 1:Many relationships. A user
can have many workouts, but a workout is tied to only one user. A workout
can have many exercises, but an exercise is associated with only one workout.

Please note that I use Cloud9, and as a result the following steps include
'sudo -u postgres' prepended to the front of commands to reflect the content required
for the commands to properly execute specifically within the Cloud9 environment. Outside
of the Cloud9 environment, 'sudo -u postgres' should be not required. I apologize if your
environment has other specific command content requirements that I didn't include below;
I can follow-up with updated information once your specific environment is provided.

i. Start up PostgreSQL in the Cloud9 environment* by executing the following from
   the terminal:
        sudo service postgresql96 start

*This step assumes postgresql is already installed. I can provide additional
installation notes if needed.

ii. Create a new database with the appropriate name by executing the following
    in the terminal:
        sudo -u postgres createdb training_log

iii. Integrate the desired table schemas and initial seed data into the
     'training_log' database by executing the following from the terminal:
        sudo -u postgres psql -d training_log < schema.sql

iv. Navigate to the training_log folder. The 'cd' command can be used from the
terminal to change folders. Example:
# if currently located in the ~/environment/trails folder and the training_log
#  folder is located in the environment folder, then executing the following
#  from the terminal would result in navigating successfully to the training_log
#  subfolder:
cd ..
cd training_log

v. Once in the training_log folder, execute the following from the terminal to
install all gem dependencies required for this application.
  bundle install

Part 2: Logging In & Running the App
i. As long as the postgreSQL service is still active**, then you may launch the
   app through executing the following command from the terminal assuming you are
   currently within the 'training_log' folder***:
        ruby workouts.rb

**If the service is no longer active, then I believe it is sufficent in all
environments, it is at least in Cloud9, to restart thepostgreSQL service by
re-executing the command listed under substep (i) of the 'Part 1: Setting Up
the SQL Database' section above. I can follow-up if this is incorrect for
your particular environment.

***If you're located in a different folder and not in the training_log folder,
then you can use the 'cd' command to navigate to the 'training_log' folder and
then subsequently execute 'ruby workouts.rb' from the terminal. Alternatively,
you could add your respective folderpath to the front of 'workouts.rb' in the
command. For example, if I was in my environment folder that contains the
'training_log' subfolder, then I could start the application by executing:
        'ruby training_log/workouts.rb'

ii. The seed data currently contains 2 different users. You may either signup
and register a new test account, or you may login as one of the current users,
whose login information is below. I have used the BCrypt gem to encrypt passwords
and authenticate a user's password during the login process.

username: 'Jane Doe'
password: 'thisisafeebletestpassword'

iii. If you didn't enter in a url to a specific route to be redirected to prior to
logging in, then by default, after logging in, you will be redirected to
/training_log/1/workouts. From this page, you can log a new workout, navigate to
other training log pages to see other workouts, or you can click on a 'View Workout'
to see the exercises associated with that workout; if the particular workout viewed is
associated with your user record then you have the option to edit details about that
workout, delete the workout, edit an exercise tied to that workout, add an exercise
to the workout (assuming it doesn't have 10 exercises yet), or delete an exercise
from the workout.


# Other Design Choices and Their Tradeoffs:
i. 10 workouts are loaded per page and are sorted alphabetically by associated username and
    then descendingly by date of workout. Specifically, when a user is examining their
    workout records within a training log page, their most recent workout will be topmost
    relative to all of their other workout records. This ordering choice was selected with
    progressive overload training in mind; a main focus of progressive overload training is
    to increase workout intensity over time. As a result, a user will be most interested in
    visiting their most recent workout(s) in order to plan for their next workout, so that
    they can increase either the number of sets, reps, or weights they use relative to what
    they used for their most recent workout.

ii. A further enhancement considered, but not pursued, would be to allow for filtering to
    workouts only associated with a particular user and/or to allow for quick searching to a
    new page number. Currently, a user with a name that's at the end of the alphabet will have
    to flip through single pages using the 'prior page' and 'next page' navigation buttons, or
    by randomly guessing what training_log page number their workouts start to appear on, and
    entering that page number in the url (in the format: /training_log/:pagenumber/workouts),
    which could make for a poor user experience if this application were to scale sizably to
    more users. If this application were to scale, these further enhancements to page navigation
    would be pursued.

iii. A user will not need to enter >10 exercises per workout. The choice to limit the number
     of exercises added per workout was based on actual training. A demanding workout can be
     achieved with 4 exercises only, so it seemed reasonable to set a cap.

iv. A user is not allowed to add more than 1 workout per day. Bodybuilders and some athletes
    may do 2+ workouts in a day, so there could be a need to allow for >1 workout per day, but
    the assumption is that the current userbase is satisfied with the capability of logging one
    workout per day.

v. Checks have been added to help ensure that the user enters valid exercise and workout content.
   However, more robust validation could potentially be added for checking descriptions. For
   example, I currently have added checks to ensure that for one workout, a user can't log
   'Bench Press.', 'Bench     Press', and 'BenchPress'. However, it is currently possible for
   users to enter nonsensical strings, like 'broccoli', for both an exercise's description and
   a workout's name. I considered changing the input for exercise description and workout name to
   dropdown formats with constrained options, but didn't pursue this due to the vast amount of
   possible exercises and the desire for a user to be able to able to enter their specific
   terminology that I may not be aware of.
