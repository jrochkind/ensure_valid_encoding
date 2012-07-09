# EnsureValidEncoding

For ruby 1.9 strings, fail quickly in on invalid encodings _or_ replace 
bad bytes with replacement strings, _without_ a transcode to a different encoding. 

## Installation

Add this line to your application's Gemfile:

    gem 'ensure_valid_encoding'

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install ensure_valid_encoding

## Usage

~~~ruby
# \xE9 is not valid UTF-8
bad_utf8 = "M\xE9xico".force_encoding("UTF-8")  

EnsureValidEncoding.ensure_valid_encoding(bad_utf8)
# => raises a Encoding::InvalidByteSequenceError
~~~~

Uses the same options as String#encode, `:invalid => :replace`, possibly
combined with `:replace => custom_replace_string`. 

~~~ruby
EnsureValidEncoding.ensure_valid_encoding(bad_utf8, :invalid => :replace)
# => Replaces invalid bytes with default replacement char. 
#    For unicode encodings, that's unicode replacement code, "\uFFFD",
#    otherwise, '?'

EnsureValidEncoding.ensure_valid_encoding(bad_utf8, :invalid => :replace, :replace => "*")
# => "M*xico"
~~~

Mutate a string in-place with replacement chars? No problem, use the bang
version. 

~~~ruby
EnsureValidEncoding.ensure_valid_encoding!(bad_utf8, :invalid => :replace)
# bad_utf8 has been mutated
~~~

For convenience to save you some typing, methods defined as module instance
methods too:

~~~ruby
include EnsureValidEncoding
ensure_valid_encoding(bad_str)
~~~

## Developing/Contributiong

Some tests written with minitest/spec. Run with `rake test`. 

Gem built with bundler rake tests. `rake build`, `rake install`, `rake release`. 

Suggestions/improvements welcome. 

1. Fork it
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Added some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create new Pull Request
