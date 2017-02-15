# Rails API Relationships Diagnostic

Place your responses inside the fenced code-blocks where indivated by comments.

1.  Describe a reason why a join tables may be valuable.

  ```md
join tables allow us to more accurately model relationships by giving a middle
ground to two tables to compare them in.  This middle ground can store extra
data as well.
  ```

1.  Provide a database table structure and explain the Entity Relationship that
  describes a many-to-many relationship for `Profiles`, `Movies` and `Favorites`
  (Think of Netflix). A `Profile` has a `given_name`, `surname` and `email` and a
  `Movies` have `title`, `release_date`, and `length` and `Favorites` would be the
  join table with references to `Movies` and `Profiles`.

  ```md
  Profile would be a main resourcewith `given_name`, `surname` and `email` as
  columns. It has a has_many relationship to Favorites and a has_many
  relationship to Movies through favorites
  Movies would be another main resource with `title`, `release_date`, and `length`.
  it has a has_many relationship to Favorites and and has_many relationship to
  Profile through Favorites
  Favorites would have references to Movies and Profiles and belong to both of
  them as well.
  ```

1.  For the above example, what needs to be added to the Model files?

  ```rb
  class Profile < ActiveRecord::Base
  has_many :movies, through: :favorites
  has_many :favorites

  validates :given_name, presence: true
  validates :surname, presence: true
  validates :email, presence: true
  end
  ```

  ```rb
  class Movie < ActiveRecord::Base
  has_many :users, through: :favorites
  has_many :favorites

  validates :title, presence: true
  validates :release_date, presence: true
  validates :length, presence: true
  end
  ```

  ```rb
  class Favorite < ActiveRecord::Base
  belongs_to :movie
  belongs_to :profile
  end
  ```

1.  What is the purpose of a serializer? What would our `Profile` serializer look
like to show all movies favorited by a profile on
`http://localhost:3000/profiles/1`

  ```md
  the serializer changes things into and out of JSON.
  ```

  ```rb
  class ProfileSerializer < ActiveModel::Serializer
  attributes :id, :given_name, :surname, :email, :movies

  def movies
    object.movies.pluck(:id)
  end
  end
  ```

1.  What would the command be to _scaffold_ out a **join table** for Favorites from
the above `Movies` and `Profiles`.

  ```sh
  #So, I am not entirely sure if htis is is a trick question or not - in our
  #demo/labs we did NOT scaffold join tables, but the command to do that would be
  bin/rails generate scaffold Favorites movie:references profile:references
  #however, we just generated the model in class, and it's not clear if that was
  #for teaching or best practice, and do that we would
  bin/rails generate model Favorites movie:references profile:references
  ```

1.  What is `Dependent: Destroy` and where/why would we use it?

  ```md
  Dependent: Destroy tells rails to destroy resources that are dependant on the
  main resource when the main resource is destroyed.  Without this we'd get an
  error message that we would be unable to delete them.  We use this in the model.
  ```

1.  Think of **ANY** example where you would have a one-to-many relationship as well
as a many-to-many relationship in an application. You only need to list the
description about the resources and how they relate to one another.

  ```md
  this is from my project:
  Users have many games and many games played
  games are played many times and may be played by multiple users
  an individual game session is just a single game
  ```
