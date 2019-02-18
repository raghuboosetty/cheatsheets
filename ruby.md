# Ruby

## Lamda and Block
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
