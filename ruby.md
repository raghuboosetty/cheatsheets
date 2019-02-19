# Ruby

## Lambda and Block
* Block: A code surrounded by curly braces is a block
* Proc/lambda: Its referred an anonymous function or they are closures
* Closure: A function or a reference to a function together with a referencing environment. Unlike a plain function, closures allow a function to access non-local variables even when invoked outside of its immediate lexical scope

### Lambda
* notation: lambda or ->
* lambda checks for arguments passed
* is variant or type of Proc
* a return statement returns from itself(doesn't exit the control)
* it takes array as a block and doesn't expand(it doesn't take array elements as individual arguments)

```ruby 
def a_method
  lambda { return "we just returned from the block" }.call
  return "we just returned from the calling method"
end
puts a_method
# '->' literal form is a shorter version of Kernel#lambda.
```
### Proc
* notation: Proc.new or proc
* doesn't check for arguments passed
* its an object
* a return statement returns from the main method(or returns the method i.e control itself)
* it expands array arguments(or say it can take arguments as an array)
 
```ruby
def a_method
  Proc.new { return "we just returned from the block" }.call
  return "we just returned from the calling method"
end
puts a_method
# 'proc' is a method
```

### Differences Summary
  - Procs are objects, blocks are not
  - At most one block can appear in an argument list
  - Lambdas check the number of arguments, while procs do not
  - Lambdas and procs treat the ‘return’ keyword differently
  - Lambdas and procs treat array arguments differently

### Advantages:
  - can pass whole lamda/proc block as a parameter to a method
  - in Rails, you use them as scopes
  - Methods are bound to Objects, and Procs are bound to the local variables in scope

### Notes:
  - Some argue that blocks aren't closures in Ruby, since methods can only take one block.

### Examples:
```ruby
def counter
 n = 0
 puts n
 return ->(a){ n+= 1 }
end
a = counter
a.call
a.call

b = counter
b.call
a.call

n=0
proc_obj = ->{ n+= 1 }
proc_obj.call
proc_obj.call

n=0
 ->{ n+= 1 }.call
 ```

#### References:
* http://awaxman11.github.io/blog/2013/08/05/what-is-the-difference-between-a-block/
* https://rubymonk.com/learning/books/4-ruby-primer-ascent/chapters/18-blocks/lessons/64-blocks-procs-lambdas


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
