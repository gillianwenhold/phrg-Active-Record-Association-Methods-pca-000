# Active Record Association Methods

## Objectives

1. Understand the common methods we have access to from our ActiveRecord associations 
2. Use the methods that ActiveRecord gives you based on your associations

Previously, we learned what Active Record associations are and how to use them. In this lab, we are going to start with the association relationships already coded for `Songs`, `Genres`, and `Artists`. These associations look like this:

* Artists have many songs and a song belongs to an artist. 
* Artists have many genres through songs. 
* Songs belong to a genre. 
* A genre has many songs. 
* A genre has many artists through songs. 

You may recall that by writing a few migrations and making use of the appropriate ActiveRecord macros, we will be able to:

* ask an Artist about its songs and genres
* ask a Song about its genre and its artist
* ask a Genre about its songs and artists.



We will build these associations through the use of Active Record migrations and macros. 

### Building our Migrations

You can take a look at the migration, if you need a reminder of the tables' structures. Run `rake db:migrate` in your terminal to execute our table creations. 

### Building our Associations using AR Macros

We used the following AR macros (or methods): [`has_many`](http://guides.rubyonrails.org/association_basics.html#the-has-many-association), [`has_many through`](http://guides.rubyonrails.org/association_basics.html#the-has-many-through-association), [`belongs_to`](http://guides.rubyonrails.org/association_basics.html#the-belongs-to-association). They helped us associate `songs`, `genres`, and `artists`.

```ruby
class Song < ActiveRecord::Base
  belongs_to :artist
  belongs_to :genre
end
```

```ruby
class Artist < ActiveRecord::Base
  has_many :songs
  has_many :genres, through: :songs
end
```

```ruby
class Genre < ActiveRecord::Base
  has_many :songs
  has_many :artists, through: :songs
end
```

And that's it! With this relatively small amount of code, we now have access to a whole host of methods provided by Active Record.

## Association Methods

Go ahead and run the test suite and you'll see that we are passing the first 14 tests. Our associations are all working, just because of our migrations and use of macros.

We can now call methods on the objects we associated with one another. Let's play around with our code using the console task we wrote for you in the Rakefile.

```bash
rake console
```

```ruby
hello = Song.create(name: "Hello")
=> #<Song:0x007fc75a8de3d8 id: nil, name: "Hello", artist_id: nil, genre_id: nil>
```

```ruby
adele = Artist.create(name: "Adele")
=> #<Artist:0x007fc75b8d9490 id: nil, name: "Adele">
```

So, we know that an individual song has an `artist_id` attribute. We *could* associate `hello` to `adele` by setting `hello.artist_id=` equal to the `id` of the `adele` object. BUT! Active Record makes it so easy for us. The macros we implemented in our classes allow us to associate a song object directly to an artist object:

```ruby
hello.artist = adele
=> #<Artist:0x007fc75b8d9490 id: nil, name: "Adele">
```

Now, we can ask `hello` who its artist is:

```ruby
hello.artist
=> #<Artist:0x007fc75b8d9490 id: nil, name: "Adele">
```

We can even chain methods to ask `hello` for the *name* of its artist:

```ruby
hello.artist.name
=> "Adele"
```

We can tell the artist about their song:

```ruby
rolling_in_the_deep = Song.create(name: "Rolling in the Deep")
=> #<Song:0x007fc75bb4d1e0 id: nil, name: "Rolling in the Deep", artist_id: nil, genre_id: nil>
```

```ruby
adele.songs << rolling_in_the_deep
=> [ #<Song:0x007fc75bb4d1e0 id: nil, name: "Rolling in the Deep", artist_id: nil, genre_id: nil>]

rolling_in_the_deep.artist
=> #<Artist:0x007fc75b8d9490 id: nil, name: "Adele">
```

# Let's get started with the lab
Be sure to run `rake db:migrate`

We are going to write some methods of our own. We want to take advantage of our new methods, thanks to the ActiveRecord macros. Therefore, every method we write will use some code that was generated by a macro. For example:

```ruby
class Artist
  def get_first_song
    
  end
end
```

How would you write the `#get_first_song` method so that it returns the first `song` object saved to the artist it's called on? By using the macros! Just like above when we called `adele.songs`, we now want to call `songs` on the instance that the method will be called on in the future. How do we do that? Yes, `self`!

```ruby
class Artist
  def get_first_song
    self.songs
  end
end
```

This will return an array of the artist's songs. Since our method is specifically looking for the first song, we just have to chain on a `first`.

```ruby
class Artist
  def get_first_song
    self.songs.first
  end
end
```

We'll do a handful of methods like this one for the `Song`, `Artist`, and `Genre` classes. This lab is test driven, so you can follow the specs.

## Does this need an update?
Please open a [GitHub issue](https://github.com/learn-co-curriculum/phrg-Active-Record-Association-Methods/issues) or [pull-request](https://github.com/learn-co-curriculum/phrg-Active-Record-Association-Methods/pulls). Provide a detailed description that explains the issue you have found or the change you are proposing. Then "@" mention your instructor on the issue or pull-request, and send them a link via Connect.

<p data-visibility='hidden'>PHRG Active Record Association Methods</p>
