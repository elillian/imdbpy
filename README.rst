IMDbPY is a Python package for retrieving and managing the data
of the `IMDb`_ movie database about movies, people, characters,
and companies.

:Homepage: https://imdbpy.sourceforge.io/
:PyPI: https://pypi.python.org/pypi/imdbpy/
:Repository: https://github.com/alberanid/imdbpy

.. admonition:: Revamp notice
   :class: note

   Starting on November 2017, many things were improved and simplified:

   - moved the package to Python 3
   - removed dependencies: SQLObject, C compiler, BeautifulSoup
   - removed the "mobile" and "httpThin" parsers
   - introduced a test suite (`please help with it!`_)

   The Python 2 version is available in the *imdbpy-legacy* branch
   (mostly unsupported).


Main features
-------------

- written in Python 3

- platform-independent

- can retrieve data from both the IMDb's web server, or a local copy
  of the database

- simple and complete API

- released under the terms of the GPL 2 license

IMDbPY powers many other software and has been used in various research papers.
`Curious about that`_?


Installation
------------

Whenever possible, please use the latest version from the repository::

   pip install git+https://github.com/alberanid/imdbpy


But if you want, you can also install the latest release from PyPI::

   pip install imdbpy


Example
-------

Here's an example that demonstrates how to use IMDbPY:

.. code-block:: python

   from imdb import IMDb

   # create an instance of the IMDb class
   ia = IMDb()

   # get a movie
   movie = ia.get_movie('0133093')

   # print the names of the directors of the movie
   print('Directors:')
   for director in movie['directors']:
       print(director['name'])

   # print the genres of the movie
   print('Genres:')
   for genre in movie['genres']:
       print(genre)

   # search for a person name
   people = ia.search_person('Mel Gibson')
   for person in people:
      print(person.personID, person['name'])


Show all the information sets available for movies:

.. code-block:: python

   >>> ia.get_movie_infoset()
   ['airing', 'akas', 'alternate versions', 'awards', 'connections',
    'crazy credits', 'critic reviews', 'episodes', 'external reviews',
    'external sites', 'faqs', 'full credits', 'goofs', 'keywords', 'locations',
    'main', 'misc sites', 'news', 'official sites', 'parents guide',
    'photo sites', 'plot', 'quotes', 'release dates', 'release info',
    'reviews', 'sound clips', 'soundtrack', 'synopsis', 'taglines',
    'technical', 'trivia', 'tv schedule', 'video clips', 'vote details']

Update a movie with more information and show which keys were added:

.. code-block:: python

   >>> ia.update(matrix, ['vote details'])
   >>> matrix.infoset2keys['vote details']
   [['demographics', 'number of votes', 'arithmetic mean', 'median']]
   >>> matrix.get('median')
   9


Get the first result of a company search and update it to get the basic
information:

.. code-block:: python

   >>> ladd_company = ia.search_company('The Ladd Company')[0]
   >>> ia.update(ladd_company)
   >>> ladd_company.keys()
   >>> ladd_company.get('production companies')

Get 5 movies tagged with a keyword:

.. code-block:: python

   >>> dystopia = ia.get_keyword('dystopia', results=5)
   >>> dystopia
   [<Movie id:1677720[http] title:_Ready Player One (2018)_>,
    <Movie id:2085059[http] title:_Black Mirror (2011–) (None)_>,
    <Movie id:5834204[http] title:_The Handmaid's Tale (2017–) (None)_>,
    <Movie id:1663662[http] title:_Pacific Rim (2013)_>,
    <Movie id:1856101[http] title:_Blade Runner 2049 (2017)_>]

Get top 250 and bottom 100 movies:

.. code-block:: python

   >>> top250 = ia.get_top250_movies()
   >>> top250[0]
   <Movie id:0111161[http] title:_The Shawshank Redemption (1994)_>
   >>> bottom100 = ia.get_bottom100_movies()
   >>> bottom100[0]
   <Movie id:4458206[http] title:_Code Name: K.O.Z. (2015)_>


Getting help
------------

Refer to the web site https://imdbpy.sourceforge.io/ and
subscribe to the mailing list: https://imdbpy.sourceforge.io/support.html


Main objects and methods
------------------------

Create an instance of the IMDb class, to access information from the web
or a SQL database:

.. code-block:: python

    ia = imdb.IMDb()

Return an instance of a Movie, Person, Company, or Character class.
The objects have the basic information:

.. code-block:: python

   movie = ia.get_movie(movieID)
   person = ia.get_person(personID)
   company = ia.get_company(companyID)
   character = ia.get_character(characterID)

Return a list of Movie, Person, Company or Character instances. These objects
have only bare information, like title and movieID:

.. code-block:: python

    movies = ia.search_movie(title)
    persons = ia.search_person(name)
    companies = ia.search_company(name)
    characters = ia.search_characters(name)

Update a Movie, Person, Company, or Character instance with basic information,
or any other specified info set:

.. code-block:: python

    ia.update(obj, info=infoset)

Return all info sets available for a movie; similar methods are available
for other objects:

.. code-block:: python

    ia.get_movie_infoset()

Mapping between the fetched info sets and the keywords they provide;
similar methods are available for other objects:

.. code-block:: python

    movie.infoset2keys

The ID of the object:

.. code-block:: python

    movie.movieID
    person.personID
    company.companyID
    character.characterID

Get a key of an object:

.. code-block:: python

    movie['title']
    person.get('name')

Search for keywords similar to the one provided, and fetch movies matching
a given keyword:

.. code-block:: python

    keywords = ia.search_keyword(keyword)
    movies = ia.get_keyword(keyword)

Get the top 250 and bottom 100 movies:

.. code-block:: python

    ia.get_top250_movies()
    ia.get_bottom100_movies()

Character associated to a person who starred in a movie, and its notes:

.. code-block:: python

    person_in_cast = movie['cast'][0]
    notes = person_in_cast.notes
    character = person_in_cast.currentRole

Check whether a person worked in a given movie or not:

.. code-block:: python

    person in movie
    movie in person

.. _IMDb: https://www.imdb.com/
.. _please help with it!: https://sourceforge.net/p/imdbpy/mailman/message/36107729/
.. _Curious about that: https://imdbpy.sourceforge.io/ecosystem.html