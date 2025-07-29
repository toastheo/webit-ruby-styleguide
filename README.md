# webit! Ruby Coding Styleguide

Many of this was taken from https://github.com/styleguide/ruby, https://github.com/bbatsov/ruby-style-guide and https://github.com/bbatsov/rails-style-guide.

## Conventions

* Use 2 **spaces** no tabs.

* Never leave trailing whitespace.

* End each file with a newline.

* Use spaces around operators, after commas, colons and semicolons, around `{` and before `}`.
    ```Ruby
    sum = 1 + 2
    a, b = 1, 2
    1 > 2 ? true : false; puts "foobar"
    [1, 2, 3].each { |foo| puts foo }
    ```

* Use spaces in argument list and put one empty line between method definitions.
    ```Ruby
    def foo(arg1, arg2, arg3)
      # ...
    end

    def bar(arg1 = :default, arg2 = nil, arg3 = {})
      # ...
    end

    def qux(arg1: :default, arg2: nil, arg3: [])
      # ...
    end
    ```

* No spaces after `(`, `[`, `!` or before `]`, `)` or around `**`, `..` or in string interpolations.
    ```Ruby
    foo(bar).method
    [1, 2, 3].length
    !number?
    2**2
    1..5
    "foo#{bar}"
    ```

* Use single-quoted strings, unless the double-quoted are needed (e.g. need string interpolation or special symbols such as `\t`, `\n`, `'`, etc.)
    ```Ruby
    # bad
    "foobar"

    # good
    'foobar'

    # good
    "'foo'\n'bar'"

    # good
    "foo #{bar}"
    ```

* Large numeric literals.
    ```Ruby
    # bad
    num = 1000000

    # good
    num = 1_000_000
    ```

* Avoid the use of `!!`.
    ```Ruby
    # bad
    x = 'test'
    # obscure nil check
    if !!x
      # body omitted
    end

    x = false
    # double negation is useless on booleans
    !!x # => false

    # good
    x = 'test'
    unless x.nil?
      # body omitted
    end
    ```

* Use shorthand self assignment operators whenever applicable.
    ```Ruby
    # bad
    x = x + y
    x = x * y
    x = x**y
    x = x / y
    x = x || y
    x = x && y

    # good
    x += y
    x *= y
    x **= y
    x /= y
    x ||= y
    x &&= y
    ```

*  Leverage the fact that if and case are expressions which return a result.
    ```Ruby
    # bad
    if condition
      result = 'foo'
    else
      result = 'bar'
    end

    # good
    result =
      if condition
        'foo'
      else
        'bar'
      end
    ```

* Only one space for multiple line assignments.
    ```Ruby
    # bad
    foo       = bar
    foobar    = foo
    foobarqux = qux

    # good
    foo = bar
    foobar = foo
    foobarqux = qux
    ```

* Use `%i` or `%I` for an array of symbols.
    
    ```Ruby
    # bad
    array = [:foobar1, :foobar2, :foobar3]
    
    # good
    array = %i[foobar1 foobar2 foobar3]
    ```


* Multiple line assignment.
    ```Ruby
    # bad
    hash = { foo1: :bar1, foo2: :bar2, foo3: :bar3, foo4: :bar4, foo5: :bar5 }
    array = %i[foobar1 foobar2 foobar3 foobar4 foobar5 foobar6 foobar7 foobar8 foobar9]

    # good - the preferred way
    hash = {
      foo1: :bar1, foo2: :bar2, foo3: :bar3,
      foo4: :bar4, foo5: :bar5
    }
    array = %i[
      foobar1 foobar2 foobar3 foobar4 foobar5
      foobar6 foobar7 foobar8 foobar9
    ]

    # good - But not the preferred way. Move the left brace to the next line, if the hash gets unclear.
    hash =
      {
        foo1: :bar1, foo2: :bar2, foo3: :bar3,
        foo4: :bar4, foo5: :bar5
      }
    array =
      %i[
        foobar1 foobar2 foobar3 foobar4 foobar5
        foobar6 foobar7 foobar8 foobar9
      ]
    ```

* Indent `when` as deep as `case`.
    ```Ruby
    kind =
      case foo
      when 'bar'
        'foobar'
      when 'foo'
        'foofoo'
      else
        'foo'
      end
    ```

* The keywords `and` and `or` have lower precedence than assignments in Ruby and therefore often lead to unexpected behavior.
Always use the operators `&&` and `||` instead.
    ```Ruby
    # bad – leads to surprising results
    foo = 'ThisIsNotABoolean'
    bar = false
    result = foo and bar
    # => "ThisIsNotABoolean"
    # What actually happened:
    # (result = foo) and bar
    
    # good - as expected
    result = foo && bar
    # => false
    ```

* Multiline method calls must have parentheses.
    ```Ruby
    # good
    object.method arg
    object.method(arg)

    # bad
    object.method arg1,
                  arg2,
                  arg3

    # good
    object.method(
      arg1,
      arg2,
      arg3
    )
    ```

* Each argument in a multi-line method call must start on a separate line.
    ```Ruby
    # bad
    object.method(
      arg1, arg2,
      arg3
    )

    # good
    object.method(
      arg1,
      arg2,
      arg3
    )
    ```
    
* Multi-line method chaining: put points at the end of lines and methods on new lines. If using any newlines put every method to a new line
    ```Ruby
    # ok
    foo.bar.qux.baz

    # bad
    foo.bar.qux.baz.bla.blub.first.last.all.includes(:myself).order(:left)

    # bad
    foo.bar.qux.baz.bla.
      blub.first.last.
      all.includes(:myself).
      order(:left)

    # good
    foo.
      bar.
      qux.
      baz.
      bla.
      blub.
      first.
      last.
      all.
      includes(:myself).
      order(:left)
    ```

* Avoid the usage of `is_` or `has_` at the beginning of method names.
    ```Ruby
    # bad
    is_admin?
    has_super_permissions?

    # good
    admin?
    super_permissions?
    ```

* Many arguments in method definition
    ```Ruby
    # bad
    def foobarqux(text, small_text: nil, text_class: nil, no_text: (text.blank? && small_text.blank?), extra_content: nil, icon: nil, color_type: :default, large_button: large_button_default?, no_button: false, icon_only: false, icon_position: :left, disabled: false, text_icon_space: (no_button ? '' : ' ').html_safe, **options)
      # ...
    end

    # good
    def foobarqux(
      text,
      small_text: nil,
      text_class: nil,
      no_text: (text.blank? && small_text.blank?),
      extra_content: nil,
      large_button: large_button_default?,
      icon_position: :left,
      disabled: false,
      text_icon_space: (no_button ? '' : ' '),
      **options
    )
      # ...
    end
    ```
* Use named parameters for methods with optional parameters.
  ```Ruby
  # bad
  def foo(options = {})
    bar = options.fetch(:bar, 'default')
    puts bar
  end

  # good
  def foo(bar: 'default')
    puts bar
  end

  # foo => 'default'
  # foo(bar: 'baz') => 'baz'
  ```  

* Long argument line in method call
    ```Ruby
    # bad
    simple_form_for @object, url: long_route_method_path , html: { class: 'form-horizontal', multipart: true, data: { data: :data, another_data: another_data, some_more_data: some_more_data }} do |f|
      # ...
    end

    # good
    simple_form_for(
      @object,
      url: long_route_method_path,
      html: {
        class: 'form-horizontal',
        multipart: true,
        data: {
          data: :data,
          another_data:,
          some_more_data:
        }
      }
    ) do |f|
      # ...
    end
    ```

    ```Ruby
    # bad
    create(:user, :user => build(:user, :email => 'user@webit.de',
                                 :password => 'password',
                                 :password_confirmation => 'password'))

    # good
    create(
      :user,
      user:
        build(
          :user,
          email: 'user@webit.de',
          password: 'password',
          password_confirmation: 'password'
        )
    )

    # bad
    Mailer.deliver(to: 'bob@example.com', from: 'us@example.com', subject: 'Important message', body: source.text)

    # good
    Mailer.deliver(
      to: 'bob@example.com',
      from: 'us@example.com',
      subject: 'Important message',
      body: source.text
    )

    # bad
    pdf.invoice_info(infos: {
        'user_name' => user_name,
        'invoice_number' => invoice_number,
        'customer_number' => customer_number
    })

    # good
    pdf.invoice_info(
      infos: {
        'user_name' => user_name,
        'invoice_number' => invoice_number,
        'customer_number' => customer_number
      }
    )
    ```

* If there are many conditions, use 4 spaces relatively to the beginning of the statement for the next lines
    ```Ruby
    # bad
    if condition_a && condition_b && condition_c && condition_f && condition_e
      # ...
    end

    # good
    if condition_a && condition_b && condition_c &&
        condition_f && condition_e
      # ...
    end

    # good
    unless condition_a && condition_b && condition_c &&
        condition_f && condition_e
      # ...
    end
    ```

*  Use one expression per branch in a ternary operator. This also means that ternary operators must not be nested. Prefer if/else constructs in these cases.
    ```Ruby
    # bad
    some_condition ? (nested_condition ? nested_something : nested_something_else) : something_else

    # good
    if some_condition
      nested_condition ? nested_something : nested_something_else
    else
      something_else
    end
    ```

* Use the Ruby 1.9 hash literal syntax when your hash keys are symbols.

    ```Ruby
    # bad
    hash = { :one => 1, :two => 2, :three => 3 }
    
    # bad
    hash = { 'one': 1, 'two': 2, 'three': 3 }

    # good
    hash = { one: 1, two: 2, three: 3 }
    ```

* Favor the use of Array#join over the fairly cryptic Array# with a string argument.
    ```Ruby
    # bad
    %w(one two three) * ', '
    # => 'one, two, three'

    # good
    %w(one two three).join(', ')
    # => 'one, two, three'
    ```

* Comments
    ``` ruby
    # bad
    =begin
    comment line
    another comment line
    =end

    # bad
    #comment without leading whitespace

    # good
    # comment line
    # another comment line
    ```

* Use [TomDoc](http://tomdoc.org/) for documentation.
    ```Ruby
    # Public: Duplicate some text an arbitrary number of times.
    #
    # text  - The String to be duplicated.
    # count - The Integer number of times to duplicate the text.
    #
    # Examples
    #
    #   multiplex('Tom', 4)
    #   # => 'TomTomTomTom'
    #
    # Returns the duplicated String.
    def multiplex(text, count)
      text * count
    end
    ```

* Do not use `fail`.
    ```Ruby
    # bad
    if password.length < 8
      fail "Password too short"
    end

    # good
    if password.length < 8
      raise "Password too short"
    end
    ```

## Rails

* Do not use `refute`.
    ```Ruby
    # bad
    refute valid?

    # good
    assert !valid?
    ```

* Use underscore for HTML IDs
    ```Ruby
    # bad
    <input id="foo-bar" />

    # bad
    <input id="fooBar" />

    # good
    <input id="foo_bar" />
    ```

* Use lambdas instead of strings for `assert_difference` and `assert_no_difference`

    ```Ruby
      # bad (no code highlight, no inspections etc.)
      should 'filter old news when wanted' do
        assert_difference 'News.count' do
          assert_no_difference 'News.just_new.count' do
            News.create(published_at: 6.months.ago - 1.day)
          end
        end
      end
      
      # good
      should 'filter old news when wanted' do
        assert_difference -> { News.count } do
          assert_no_difference -> { News.just_new.count } do
            News.create(published_at: 6.months.ago - 1.day)
          end
        end
      end
     ```
