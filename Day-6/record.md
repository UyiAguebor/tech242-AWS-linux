# record script

Hi my name is uyi and i will go through the process of a 2 tier deployment for an application

there are 3 tiers of deployment:
application tier
logic tier
and data tier

the application tier is usually what is presented to the end user
the logic tier is the business logic connecting the application tier and data tier
the data tier is usually a database that runs behind the application tier

in this case we have a application that has an application tier and data tier which means it is a 2 tier deployment however since the application uses mongoDB we havent deployed the database which means we are only responsible for the deployment of the application tier

here is the deployed app as you can see we do not use any ports and it takes us straight to the page.

for this deployment we are using an AWS EC2 instance which allows us to control one of thier computers to run our application.

i first planned out what i wanted the script to do
i then tested manually each command
after all this was done i put all the commands inside my script and ran it

to speed up the process we can use the userdata section when creating an instance where we place our script

to speed up automation one more step we can then create an image of the working instance