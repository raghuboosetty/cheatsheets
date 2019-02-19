# Ruby

## Lamda and Block
```ruby
# Block: A code surrounded by curly braces is a block
# Proc/lambda: Its referred an anonymous function or they are closures
# Closure: A function or a reference to a function together with a referencing environment. Unlike a plain function, closures allow a function to access non-local variables even when invoked outside of its immediate lexical scope

# lambda:
 # notation: lambda or ->
 # lambda checks for arguments passed
 # is variant or type of Proc
 # a return statement returns from itself(doesn't exit the control)
 # it takes array as a block and doesn't expand(it doesn't take array elements as individual arguments)
 
def a_method
  lambda { return "we just returned from the block" }.call
  return "we just returned from the calling method"
end
puts a_method
# '->' literal form is a shorter version of Kernel#lambda.

# Proc:
 # notation: Proc.new or proc
 # doesn't check for arguments passed
 # its an object
 # a return statement returns from the main method(or returns the method i.e control itself)
 # it expands array arguments(or say it can take arguments as an array)

def a_method
  Proc.new { return "we just returned from the block" }.call
  return "we just returned from the calling method"
end
puts a_method
# 'proc' is a method

```

## Mechanize
### Gmail login example
```ruby
require 'mechanize'

email = 'email'
password = 'password'

a = Mechanize.new
a.get('http://www.google.com') do |page|
  # Click the Gmail link
  gmail_login_page = a.click(page.link_with(:text => "Gmail"))

  # Submit the login form
  gmail_page = gmail_login_page.form_with( :action => 'https://accounts.google.com/ServiceLoginAuth' ) do |f|
    f.Email  = email
    f.Passwd = password
  end.click_button

  # List all the links in the personal gmail page
  gmail_page.links.each do |link|
    text = link.text.strip
    next unless text.length > 0
    puts text
  end
end
```
