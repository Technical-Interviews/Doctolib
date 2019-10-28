## Technical Test @ Doctolib

The goal is to write an algorithm that finds availabilities in an agenda depending on the events attached to it.
The main method has a start date for input and is looking for the availabilities over the next 7 days.

There are two kinds of events:

 - 'opening', are the openings for a specific day and they can be reccuring week by week.
 - 'appointment', times when the doctor is already booked.
 
To init the project:

``` sh 
rails new doctolib-test
rails g model event starts_at:datetime ends_at:datetime kind:string weekly_recurring:boolean
```

Your Mission:
 - coded for rails 5.1
 - contained in two files named event.rb and event_test.rb
 - must pass the unit tests below
 - add tests for edge cases
 - be pragmatic about performance

```ruby
# test/models/event_test.rb

require 'test_helper'

class EventTest < ActiveSupport::TestCase
  
  test "one simple test example" do
    
    Event.create kind: 'opening', starts_at: DateTime.parse("2014-08-04 09:30"), ends_at: DateTime.parse("2014-08-04 12:30"), weekly_recurring: true
    Event.create kind: 'appointment', starts_at: DateTime.parse("2014-08-11 10:30"), ends_at: DateTime.parse("2014-08-11 11:30")

    availabilities = Event.availabilities DateTime.parse("2014-08-10")
    assert_equal Date.new(2014, 8, 10), availabilities[0][:date]
    assert_equal [], availabilities[0][:slots]
    assert_equal Date.new(2014, 8, 11), availabilities[1][:date]
    assert_equal ["9:30", "10:00", "11:30", "12:00"], availabilities[1][:slots]
    assert_equal [], availabilities[2][:slots]
    assert_equal Date.new(2014, 8, 16), availabilities[6][:date]
    assert_equal 7, availabilities.length
  end
  
end
```
