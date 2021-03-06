= Akismetor 1.1

This version includes rSpec tests and a method to verify the Akismet key.

This was originally created by Ryan Bates as a  plugin (Railscasts episode 65), being forked from http://svn.railscasts.com/public/plugins/akismetor

This gem provides the Akismet class which you can use to communicate with Akismet. Akismet [http://akismet.com/] is a collaborative spam filtering service.

You can use the class Akismet and its methods, wherever you want inside your Rails application.


== Usage:

# submits key to Akismet, and returns a string with "valid" or "invalid".
Akismetor.valid_key?(attributes)

# submits comment to Akismet, and returns true if the comment is spam, and false in case it's not.
Akismetor.spam?(attributes)

# submits hash as spam to Akismet. Returns a message string.
Akismetor.submit_spam(attributes)

# submits hash as ham to Akismet. Returns a message string.
Akismetor.submit_ham(attributes)



Let's see a real usage:

comment_attributes = {
  :key => '123456789', 
  :blog => 'http://www.my-site.com', 
  :user_ip => '200.10.20.30',
  :user_agent => 'Mozilla/5.0 (Macintosh; U; PPC Mac OS X 10.5; en-US; rv:1.9.0.3) Gecko/2008092414 Firefox/3.0.3',
  :referrer => 'http://www.another-site.com',
  :permalink => 'http://www.my-site.com/posts/2',
  :comment_type => 'comment',
  :comment_author => 'John Doe',
  :comment_author_email => 'john@doe.com',
  :comment_author_url => 'http://www.doe.com',
  :comment_content => 'I beg to differ....'
}

check = Akismetor.spam?(comment_attributes)
puts "Is this spam? #{check.to_s}."

== The Akismet API

Akismet API Documentation Version 1.1 is at http://akismet.com/development/api/

blog  (required)
    The front page or home URL of the instance making the request. For a blog or wiki this would be the front page. Note: Must be a full URI, including http://. 
user_ip (required)
    IP address of the comment submitter.
user_agent (required)
    User agent information.
referrer (note spelling)
    The content of the HTTP_REFERER header should be sent here.
permalink
    The permanent location of the entry the comment was submitted to.
comment_type
    May be blank, comment, trackback, pingback, or a made up value like "registration".
comment_author
    Submitted name with the comment
comment_author_email
    Submitted email address
comment_author_url
    Commenter URL.
comment_content
    The content that was submitted.
Other server enviroment variables
    In PHP there is an array of enviroment variables called $_SERVER which contains information about the web server itself as well as a key/value for every HTTP header sent with the request. This data is highly useful to Akismet as how the submited content interacts with the server can be very telling, so please include as much information as possible.

This call returns either "true" or "false" as the body content. True means that the comment is spam and false means that it isn't spam. If you are having trouble triggering you can send "viagra-test-123" as the author and it will trigger a true response, always. 

