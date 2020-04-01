# Rails Review

For each of the following topics, please answer each of the questions. You can use links and images to support your answer. Once done please make a pull request.

## Has many Through

- [Rails Active Record Intro](https://github.com/sei-entropy/lesson-w11d02-rails-active-record#active-record-associations)
-[Hospital App](https://github.com/sei-entropy/hw-w11d02-rails-hospital)

### Questions

1. What is the role of a join table in a many-to-many relationship?

Answer:

```txt
It matches a declared model with zero or more instances of another model by proceeding through a third model.

Let's say for example we have a Student model and Course model, the relationship between them is many-to-many, we would join the two models through a third one, ex: Enrollment.
```

2. What two columns must be present in a join table?

Answer:

```ruby
#belongs_to column that references to each model. In our previous example we will have something like this:

    class Student < ActiveRecord
      has_many :enrollments
      has_many :course, through: :enrollments
    end

    class Enrollment < ActiveRecord
      belongs_to :student
      belongs_to :course
    end

    class Course < ActiveRecord
      has_many :enrollments
      has_many :students, through: :enrollments
    end
```

3. Given the example below, edit the code to define a has many :through relationship.

    ```ruby
    class Customer < ActiveRecord::Base
    end

    class Product < ActiveRecord::Base
    end

    class Purchase < ActiveRecord::Base
    end
    ```

Answer:

```ruby 
    class Customer < ActiveRecord::Base
      has_many :purchases
      has_many :products, through: :purchases
    end

    class Product < ActiveRecord::Base
      has_many :purchases
      has_many :customers, through: :purchases
    end

    class Purchase < ActiveRecord::Base
      belongs_to :customer
      belongs_to :product
    end
```

4. Based on #3, give an example of associating two instances via the join model.

Answer:

```ruby
class CreatePurchases < ActiveRecord::Migration[5.0]
  def change
    create_table :customers do |t|
      t.string :name
      t.timestamps
    end
 
    create_table :products do |t|
      t.string :name
      t.timestamps
    end
 
    create_table :purchases do |t|
      t.belongs_to :customers
      t.belongs_to :products
      t.datetime :purchase_date
      t.timestamps
    end
  end
end
```

## Devise

- [Devise Lesson](https://github.com/sei-entropy/lesson-w11d03-rails-devise)

### Questions

1. What does the `current_user` method that the Devise gem provides?

Answer:

```txt
The helper method provides the current signed-in user.
```

2. What does the `authenticate_user!` method that the Devise gem provides?

Answer:

```txt
It is used in the controller with the before_action chain to specify which actions the user can not do before being authenticated.
```

3. Write a signout link using the `link_to` rails helper and a devise path.

Answer:

```ruby
# Check if user is signed-in 
<% if user_signed_in? %>
  <%= link_to 'Sign Out', destroy_user_session_path, method: :delete %>
<% end %>
```

4. How do I generate a devise model in the terminal?

Answer:

```ruby
$ rails generate devise MODEL
```

5. What are the trade offs for using a gem for authentication over a handrolled solution? (no real right answer)

Answer:

```txt
The advantage of this here is that we don't have to start from scratch which will save us time. But on the other side, we don't know how it works exactly.
```

## Validations

- [Validations Lesson](https://github.com/sei-entropy/lesson-w11d03-rails-model-validations)

### Questions

1. Which component, of Rails MVC, is responsible for the business logic?

Answer:

```txt
The Model which is the Active Record.
```

2. Assume the user's age is an optional field.  Write the validation to verify that a User's age is between 13 and 125 (inclusive).

Answer:

```txt
validates :age, inclusion: { in: 13..125 }
```

3. What would `user.errors.messages` return (for the above User), if you assigned `user.age = 12`?

Answer:

```txt
The default error message for the helper is: "Age is not included in the list".
```

4. Assume you visit "/customers/new" and enter some invalid information.  Given this controller code, what url would your browser be on after pressing "Create Customer"?

``` ruby
class CustomersController < ApplicationController
  def new
    @customer = Customer.new
  end

  def create
    @customer = Customer.new(customer_params)
    if @customer.save
      redirect_to customer_path(@customer)
    else
      render :new
    end
  end
  ...
  private
  def customer_params
    params.require(:customer).permit(:first_name, :last_name, :age)
  end
end
```
Answer:

```txt
It will redirect me to the URL /customers which still is the form page.
```

5. Give one reason why we might have the similar validations in the browser, model, and database layer of our application.

Answer:

```txt

```