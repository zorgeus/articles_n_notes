![](<https://i.imgur.com/aGZu7tZ.jpg>)

[Original article](<http://blog.dscpl.com.au/2012/10/why-are-you-using-embedded-mode-of.html>)

To determine if your WSGI application is running in embedded mode, replace its WSGI script with the test WSGI script as follows:

`
   import sys
    def application(environ, start_response):  status = '200 OK'  name = repr(environ['mod_wsgi.process_group'])  output = 'mod_wsgi.process_group = %s' % name  response_headers = [('Content-type', 'text/plain'),  ('Content-Length', str(len(output)))]  start_response(status, response_headers)  return [output] `

If the configuration is such that the WSGI application is running in embedded mode, then you will see:

`
   mod_wsgi.process_group = ''
  `

  That is, the process group definition will be an empty string.

If instead you are already running in the preferred daemon mode, you would see a non empty string giving the name of the daemon process group.

