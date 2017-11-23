 <div>
  ![](<https://i.imgur.com/aGZu7tZ.jpg>) </div>
 <div>
 </div>
 <div>
 </div>
 <div>
 </div>
 <div>
  [Original article](<http://blog.dscpl.com.au/2012/10/why-are-you-using-embedded-mode-of.html>) </div>
 <div>
 </div>
 <div>
  Even if your WSGI application is running in daemon mode, if the only thing you are using Apache for is to host the WSGI application and serve static files, then it is also recommended that you use worker MPM rather than prefork MPM as worker MPM will cut down on memory use by the Apache child processes.
 </div>
 <div>
 </div>
 <div>
  To determine if you are using prefork MPM or worker MPM, you could try and work it out by looking at what operating system packages are installed, but the definitive way of doing it, is to run the Apache binary with the '
  -V  ' option. </div>
 <div>
  `
   sudo /usr/sbin/apache2ctl -V
  ` </div>
 <div>
  (or $ /usr/sbin/httpd -V for RHLE)  </div>
 <div>
 </div>
 <div>
  [su_spoiler title="Result:" style="fancy" icon="caret"]

![](<https://i.imgur.com/2qK1KPH.png>)

[/su_spoiler]

</div>
 <div>
  <div>
   The '
   Server MPM  ' field will tell you which MPM your Apache has been compiled for. </div>
  <div>
  </div>
  <div>
   If for some reason you can't work out which is the Apache binary, because your Linux distribution calls it something other than '
   httpd  ', or they have modified it so it will not run unless some magic environment variables are set, then you can also guess what is running by using the following WSGI script. </div>
  <div>
  </div>
  <div>
       import sys
    def application(environ, start_response):
        status = '200 OK'
        output = 'wsgi.multithread = %s' % repr(environ['wsgi.multithread'])
        response_headers = [('Content-type', 'text/plain'),
                            ('Content-Length', str(len(output)))]
        start_response(status, response_headers)
        return [output]

</div>
  <div>
  </div>
  <div>
   If you get the output:
  </div>
  <div>
  </div>
  <div>
   wsgi.multithread = True  </div>
  <div>
  </div>
  <div>
   you are likely running worker MPM and otherwise are running prefork MPM.
  </div>
 </div>
