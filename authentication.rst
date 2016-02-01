Authentication
==============

Register an Application
-----------------------

In order to use the Imgur api, register an application at http://api.imgur.com/oauth2/addclient

Using the Imgur Api
-------------------

Once you have the application registered, you can use it by declaring an instance of the ImgurClient class.
You can declare an instance using the Client ID and Client Secret, or (as of v3.7.0) just the Client ID.

::

        var client = new ImgurClient("CLIENT_ID");
::

        var client = new ImgurClient("CLIENT_ID", "CLIENT_SECRET");

Using the Mashape Api
---------------------

If you will be using the Imgur api for commercial purposes (uploading more than 1250 images per day), you will need to use the Mashape api instead of the Imgur api. 
Register for Mashape at https://www.mashape.com/imgur/imgur-9

Once you have the application registered, you can use it by declaring an instance of the MashapeClient class.

::

        var client = new MashapeClient("CLIENT_ID", "MASHAPE_KEY");
::

        var client = new MashapeClient("CLIENT_ID", "CLIENT_SECRET", "MASHAPE_KEY");

More information on the Imgur api can be found at http://api.imgur.com/