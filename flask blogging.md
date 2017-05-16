# Flask Blogging Permissioning

I am very big fan of Flask framework, there are no restrictions just few guidelines on how to setup the 
app and there are tons of extensions to add more functionality. I like that.

When i started with Flask for my first web app it was very basic flask application with not much of extensions but when i 
started creating my own site i wanted to have assets (css/js) added in nice way to my base templates (Flask-Assets), Send emails
so there came Flask-Email. I had email support, nice looking landing page but how do i put the blog? Hmm, i started with something from
scratch but then after trying for few hours i felt this is not right and i am trying to re-invent the wheel, i checked online if i can 
integrate some blogging platform with Flask but that didn't return anything usable but then i found Flask-Blogging extension. To be honest
i dont know why didnt search for extension in first place?

Its super easy only problem is that how do i allow only **myself** to create and edit posts? Add the authentication and only authorised 
users can add/edit/delete post and as this is my personal site only i will have login access and no one else. Problem solved!

Now i am working on site where other users need to login in order to use the funcitonality and even this site has the blog and again i 
have Flask-Blogging to the rescue. But as more users will have login access i needed Authorisation control this time which is inbuilt 
provided in Flask-Blogging with help of Flask-Principal but i have user model already set and just to check if user has role "blogging" 
i am importing whole new module and adding few methods seemed bit more than what i wanted. What the solution? There are many extensions
which allow Authentication + authorization, some of them are not actively maintained and some of them are way to complicated to integrate. 

All i wanted was simple code which will add role like functionality without affecting anything in current setup. I found [http://flask.pocoo.org/snippets/98/] code snippet to add simple role based authorization which was great for needs. But i did not
want to change the Flask-Blogging code to add this decorator this will make me create custom package for my needs and then stick with 1 specific version of Flask-Blogging. 

Then i clicked to check if i can inherit the main class and orverload the method which is checking for permissions and provide my implementation, but code was not in class and just python file with other useful functions. What if i import that file seperately and change the implemetation with my one? Sounds like a good thing, 

    @Util.roles_required('admin')
    @login_required
    def custom_is_blogger(*args):    
      return current_user.get_id() in ('xyz')
       
    # changing the implementation of the is_blogger in views.py
    views._is_blogger = custom_is_blogger
    
 And i am done!  (roles_required is copied from the snippet mentioned above)
       
