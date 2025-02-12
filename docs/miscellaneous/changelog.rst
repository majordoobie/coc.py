.. currentmodule:: coc

.. _whats_new:

Changelog
===========
This page keeps a fairly detailed, human readable version
of what has changed, and whats new for each version of the lib.

v2.0.1
------
- New achievements for clan capital

- Switched to using Tuba's new links API (Thanks, Tuba)

- Get the IP Address from logging into the developer site to save an API call (Thanks, Roshan)


v2.0
-----
Please see :ref:`migrating_to_v2_0` for more info, as changes are described more in-depth there.

v1.3.0
------
- Added Dragon Rider and Rocket Balloon

- Added `cls` parameter for :meth:`Clan.get_detailed_members()`

- Troops no longer included in `Player.home_troops`

- :exc:`InvalidArgument` will now return a more helpful message.

- ``limit`` parameter is now correctly cached when using ``search_x`` endpoints.

- Added more typings and documentation improvements


v1.2.1
------
- Fixes an issue where `Clan.members` was empty if `Clan.get_member` was called before `Clan.members`.
    There was a similar issue with `Player.heroes` and `Player.spells`.

v1.2.0
------

- Adds new achievements and updated the order of existing ones.

- Added :attr:`Player.super_troops` and :attr:`Player.hero_pets`.

- Added :attr:`Troop.is_active` to check whether a super troop is active.

- Added :attr:`WarAttack.duration`

- Added :attr:`Clan.chat_language` which is a :class:`ChatLanguage` object.

- Fixed errors when coc.py tried to parse 5xx (e.g. 502) errors as HTML when they were dicts.

- Improved docs for :attr:`RankedPlayer.previous_rank`.


v1.1.0
------

- Adds support for the new Player API verification endpoint - see :meth:`Client.verify_player_token`

- Fixes a bug where members in `clan.get_detailed_members()` wouldn't have the `clan_rank` attribute, for example::

    # before
    async for member in clan.get_detailed_members():
        print(member.clan_rank)  # prints "None"

    # after
    async for member in clan.get_detailed_members():
        print(member.clan_rank)  # prints "1" or "10" or their clan rank.


- Fixes a bug where getting the warlog failed for clans with (>3yr) old war logs. See <https://github.com/mathsman5133/coc.py/issues/72>

- Fixed docs for :attr:`Player.clan_previous_rank` since it was misleading before.

v1.0.4
------

- Support aiohttp v3.7

- Add retry handling to the HTTPClient - if a request fails with a GatewayError (or TimeoutError), it will sleep and then retry for a maximum of 5 times.

- Super Minion, Super Wizard, Ice Hound, Log Launcher and Invisibility Spells were added.

v1.0.3
------

- Fixed an issue where the HTTP cache layer was not being utilised.

- Fixed an issue where :meth:`utils.get_season_start` and :meth:`utils.get_season_end` got the previous season information
if the system time had not yet passed 5am UTC.


v1.0.2
------

- Fixed an issue where hitting Maintenance errors when using the normal Client would raise an Attribute Error.

- Fixed an issue where using `Clan.get_member` without calling `Clan.members` would build an incorrect player lookup dict.

- Fixed an issue where events wouldn't start because the maintenance event wasn't set properly.


v1.0.1
------

- Maintenance event poller logic has been reworked to enable the use of :class:`EventsClient` without any player/clan/war
updates.

- 5min preparation time has been included in the list of valid prep times for friendly wars.

- The warlog example has been updated to properly close the client, and a typo fixed in the README example.

- The ``correct_tags`` parameter has been changed to default to ``True``. There is no side-effects of having this enabled.

v1.0
-----
Please see :ref:`migrating_to_v1_0` for more info, as the change-set is too large to describe here.

v0.3.3
-------
Breaking Changes
~~~~~~~~~~~~~~~~~~
- ``SearchPlayer.versus_attacks_wins`` has been renamed to ``SearchPlayer.versus_attack_wins``.
- The ``on_player_versus_attacks_change`` event has been renamed to ``on_player_versus_attack_change`` to match the above change.
- There was a typo with the spelling of the ``coc.Maintenance`` exception. It has been renamed from ``coc.Maitenance`` to ``coc.Maintenance``.

New Things
~~~~~~~~~~~
- Add a default `Unranked` league to ``BasicPlayer.league``.
- Added TH13 content.

BugFixes
~~~~~~~~~
- Fixed ``client.get_members`` raising ``AttributeError`` if no clan is found in the cache.
- ``client.get_warlog`` was only returning league wars. This has been fixed.


v0.3.2
-------
New Features.
~~~~~~~~~~~~~~~
- Rename :attr:`~SearchClan.member_dict` --> :attr:`~SearchClan.members_dict` for consistency.
- New helper function: :func:`LeagueGroup.get_wars(round_number)` which will return a
  `LeagueWarIterator` of all SCCWL wars in that round.

Bug Fixes
~~~~~~~~~~~
- BugFix for cache not updating properly on clans going into prep.
- Add list of possible friendly war prep times (in seconds).
  If prep time is not in that list, assume random war.
  This helps when the API reports weird prep times that aren't exactly 23 hours for a random war.

v0.3.1
--------
New Features.
~~~~~~~~~~~~~~
- :ref:`on_clan_member_trophy_change`, :ref:`on_clan_member_versus_trophy_change` and
  :ref:`on_clan_member_league_change` were added as new events.
- Add ability to pass async and other iterables into methods that return iterators.
- Add the `HogGlider` to list of builder base troops.
- Added support for `SearchClan.labels` and `SearchPlayer.labels`. They will return a :class:`~coc.Label` object.
- Add a special parameter to `Client` to automatically "fix" or call `utils.correct_tag` on any tags passed in to calls.
- `Client.get_clan_labels()` and `Client.get_player_labels()` are now valid client calls.

BugFixes
~~~~~~~~~~~~
- Getting clans and players for a location will now default to getting for the global leaderboard.
- `LeagueWar` will no longer throw an `AttributeError` when no clan is found.
- Never update the cache automatically when the `EventsClient` is used.
- Fixed :ref:`on_war_state_change` not firing properly when a clan entered `preparation` or `warEnded`.

v0.3.0
-------
Bug Fixes
~~~~~~~~~~~
- All iterators have been fixed in python 3.7
- :ref:`on_clan_member_join` will now fire events correctly
- Properly parse HTML errors thrown by CloudFlare
- Accessing the ``League.badge`` attribute has been fixed
- Clan events now sleep for the correct interval
- ``WarMember.town_hall`` has been fixed
- The API used to fetch IP has been changed (##19) to https://api.ipify.org/
- Ensure the clan is in war before trying to find prep time (##21)

New Things
~~~~~~~~~~~~~
- Check out the `Cache` tab in sidebar for a how-to work with the new cache. It still works out of the box!
- You can now call utils with ``coc.utils.X``
- All events now have callbacks as an extra layer of security to stop them from failing.
- New Properties: ``Clan.share_link`` and ``Player.share_link``.
- Add ``utils.maybe_sort()`` as an easy tool to sort clan war attacks.
- All attributes that were prefaced with a ``_`` to dictate being iterables have been changed to be prefixed
  with ``iter``, ie. ``_attacks`` becomes ``iterattacks``.
- Rename ``SearchPlayer.level`` to ``SearchPlayer.exp_level`` - keep in line with API naming.
- Default value can be passed to ``BasicPlayer.league``; defaults to `None`
- Default value for ``SearchPlayer.builder_hall`` is 0.
- New Error: `PrivateWarLog`:

  - Subclass of `Forbidden` and a special case for when a 403 is thrown from trying to access war info for a clan with a private war log.
  - Redirect all `Forbidden` thrown errors in get_WARS methods to throw `PrivateWarLog`

  - A valid operation is to do either:

.. code-block:: python3

    try:
        await coc.get_current_war(...)
    except coc.Forbidden:
        pass

    # or:

    try:
        await coc.get_current_war(...)
    except coc.PrivateWarLog:
        pass

- ``EventsClient.add_X_update`` now accepts either a string or iterable.
- New Method: ``client.remove_events()`` which works in the same way as ``client.add_events()``
- Speed up `utils.get`
- New Events:
    - :ref:`on_player_clan_join` - when a player joins a clan
    - :ref:`on_player_clan_leave` - when a player leaves a clan
    - :ref:`on_player_clan_level_change` - when a player's clan's level changes
    - :ref:`on_player_clan_badge_change` - when a player's clan's badges change.
    - `on_client_close` which is dispatched upon closing the client

- Rename `x_achievement_update` --> `x_achievement_change` for consistency
- Add ``localised_name`` and ``localised_short_name`` attributes to :class:`League` and :class:`Location`
    - These have no effect at present.

Documentation
~~~~~~~~~~~~~~~
- Lots of the docs have had tidy-ups, with 2 new how-to's dedicated to Cache and the Events Client.



v0.2.0
--------
EventsClient
~~~~~~~~~~~~~~
- :class:`EventsClient`
- Provides all functionality of :class:`Client`, as well as an events-like system.
- It will constantly request to the API every X seconds and detect indifferences between the cached and new results
  returned by API. It will then send out 'events', basically calling functions that you must register, to tell you that
  these things have happened
- Split into 3 categories: player, clan and war

Player

    - All events regarding anything in the API that can change.
    - E.g, name, troop levels (and unlocking), spells, heroes, donations, trophies etc.

Clans

    - All events regarding anything in the API that can change.
    - E.g. description, type (invite only etc.), ranks, donations etc. of members, levelups.

Wars

    - All events regarding anything in the API that can change.
    - E.g. new war attack, war state change

- You must register the funtions events will call with :meth:`EventsClient.add_events`
- You must 'subscribe' any clans, players or (clans in) wars you want to get with :meth:`EventsClient.add_clan_updates`,
  :meth:`EventsClient.add_player_update`, :meth:`EventsClient.add_war_update`.

- This can be a script that you run and will continue to run forever, calling your functions as events come through,
  it doesn't have to be integrated into a bot. To ease this use-case, :meth:`EventsClient.run_forever` is handy.

Other Importants
~~~~~~~~~~~~~~~~~
- Cache has had another overhaul about how it works, is called and default operational use.

- From above, ``default_cache`` is a kwarg, and method of :class:`Client`. It defaults to the inbuilt method,
  however you can pass your own function into this.

- Logging in: the new recommended way of logging in is via ``client = coc.login(email, pass, **kwargs)`` with ``client``
  being one of these kwargs: pass in either :class:`EventsClient` or :class:`Client` to use respective clients. This
  makes both Client class creation and HTTP logging in easy through one function. Any additional kwargs passed will become
  kwargs for the client you are using.

- ``CurrentWar`` has been renamed, revamped and relooked at. A regular clan-war is now a :class:`ClanWar`, with
  ``WarIterator`` being renamed to ``ClanWarIterator``. ``LeagueWarIterator`` and ``CurrentWarIterator`` now exist,
  Current wars being a mix of either clan or league wars.

- :meth:`Client.get_clan_war` now retrieves the current :class:`ClanWar`

- :meth:`Client.get_current_war` now attempts to retrieve the current :class:`ClanWar`, and if in the ``notInWar`` state,
  will attempt to search for a leauge war and return that, if found. This makes getting league wars and
  clan wars from the API much easier than before.
- :attr:`ClanWar.type` and :attr:`LeagueWar.type` now return a string of either ``cwl, friendly, random`` - which war type it is.
- :attr:`Timestamp.time` has been renamed to :attr:`Timestamp.raw_time`, and replaced with :attr:`Timestamp.utc_timestamp` (now called :attr:`Timestamp.time`)
- Add :attr:`ClanWar.status` returns a string ``winning, losing, tied, won, lost, tie`` depending on stars + destruction.

BugFixes
~~~~~~~~~
- Lots of little ones with cache
- Performance upgrades with use of ``__slots__`` on more classes
- Trying to iterate over used up iterators
- Only log requests throttled as debug
- Trying to pop a cache item failed
- Few little regex and other bugs in cache.

v0.1.3
--------
BugFixes
~~~~~~~~~
- TypeError will no longer be raised if no tags were found
- Iterators will continue to search for next item if one fails

Important
~~~~~~~~~~
New Properties/Attributes

    - :attr:`WarMember.is_opponent` indicates if the member is a clanmate (false) or opponent (true)
    - :attr:`SearchPlayer.ordered_home_troops`, :attr:`SearchPlayer.ordered_builder_troops` - returns an
      :class:`collections.OrderedDict` of players troops, in the order found in game.
      Note: Siege Machines are included at the end of this.
    - :attr:`SearchPlayer.ordered_spells` - same, but for spells
    - :attr:`SearchPlayer.ordered_heroes` - same, but for heroes.
    - :attr:`BaseWar.clan_tag` - all wars now have a permenant `clan_tag` attribute regardless of war state.
    - :attr:`cache.fully_populated` - helper bool to indicate if all possible items are cached,
      for eg. with locations and leagues - static information

New Methods:

    - :meth:`client.get_league_named()` - get a league (ie. Bronze III etc.) by name.
    - :meth:`client.get_location_named()` - get a location (ie. Australia etc.) by name.
    - :meth:`cache.clear()` - reset the cache and clear all objects inside for that instance.
    - :meth:`cache.get_all_values()` - returns all values in the cache.
    - :meth:`cache.get_limit(limit)` - get the first limit number of items in cache.

New Iterators:

    - :class:`PlayerIterator`, :class:`ClanIterator`, :class:`WarIterator` - returned when a function eg.
      :meth:`client.get_players(tags)` is called. These allow normal dot notion to be used inside `async for`,
      eg. `async for clan in client.get_clans(tags): print(clan.name)`.
    - :meth:`Iterator.flatten()` will return a list of all objects inside the iterator. Note: operation may be slow.

Changed Attribute:

    - :attr:`SearchPlayer.troops_dict` has been changed to both :attr:`SearchPlayer.home_troops_dict` and
      :attr:`SearchPlayer.builder_troops_dict`, returning a dict of either home, or builder troops respectively.

    - :attr:`SearchPlayer.ordered_troops_dict` has been changed to both :attr:`SearchPlayer.ordered_home_troops_dict`
      and :attr:`SearchPlayer.ordered_builder_troops_dict`, returning a dict of either home, or builder troops respectively.

Removed Dependency:

    - `lru-dict` has been removed as a dependency due to a few windows problems while installing,
      and utilising :class:`collections.OrderedDict` appears to be faster.


Documentation
~~~~~~~~~~~~~~

- Many type-hints were added to functions to aid IDE integration
- Documentation was re-written to use the NumPy style.
- Discord Bot examples were updated


v0.1.2
--------
BugFixes
~~~~~~~~~
- Fixed 2 problems which meant automatic token resets weren't working.
  Please report any more bugs!

v0.1.1
--------
BugFixes
~~~~~~~~~
- Stop nested asyncio loops from failing.

Important
~~~~~~~~~~

- New methods

    - :meth:`.Client.get_clans(tags)` returns an AsyncIterator of clans.
    - :meth:`.Client.get_current_wars(tags)` returns an AsyncIterator of current wars
    - :meth:`.Client.get_players(tags)` returns an AsyncIterator of players
    - :meth:`.SearchClan.get_detailed_members` returns an AsyncIterator of :class:`.SearchPlayer` for clans members
    - :meth:`.Client.set_cache(*cache_names, max_size, expiry)` enables you to override the default cache settings
      on a per-cache basis. Expiry is in seconds.

- Removed parameters

    - ``json=False`` on all calls has been removed. Use :attr:`DataClass._data` to get the dict as returned by the API
      if you so desire

- Implemented ratelimits

    - ``throttle_limit`` has been added as a parameter to :class:`.Client`. This is the number of calls per token, per second,
      to be made

- asyncio.Semaphore lock has been implemented

- New cache structure and implementation.

    - Max size and expiry (in seconds) can be set with :meth:`Client.set_cache`
    - New instances of cache on a per-object (returned) basis, so different methods will implement
      different instances of the cache.
    - ``lru-dict`` has been added as a requirement.
    - LRU is very fast and memory efficient, written in C.

- Enum for :class:`CacheType` has been implemented. This is the preferred way to pass in ``cache_names`` to :meth:`Client.set_cache`
  as string names may change.

    - Can be called with :meth:`Client.set_cache(CacheType.search_clans, max_size=128, expiry=10)`

- New Exception: :exc:`InvalidCredentials`

    - This essentially replaces the (now redundant) :exc:`InvalidToken` exception, and is called when the email/pass pair
      passed is incorrect.

- New util function: :func:`coc.utils.clean_tag(tag, prefix='#')` will return a 'cleaned up' version of the tag.
  It will:

    - Make all letters UPPERCASE
    - Replace o ('oh') with 0 (zero)s
    - Remove non-alphanumeric and whitespace



v0.1.0
---------
BugFixes
~~~~~~~~~~
- Fixed bug with loops breaking when reloading the client in a discord cog.
- A more specific error, ``aiohttp.ContentTypeError`` is raised when parsing non-json responses.

Important
~~~~~~~~~~~
- Big thanks to Jab for some of these.

- Big one! Client now only accepts an email/password pair rather than tokens.
  This pair is what you use to login to https://developer.clashofclans.com/#/login
  and will allow the client to automatically use, create, reset and find tokens,
  making it a much more streamlined process.


- As such, the following parameters to client have been added:

    - ``key_count``: int: the number of tokens to rotate between when making API requests.
      This defaults to 1, and can be between 1 and 10

    - ``key_names``: str: The name to use when creating tokens on the developer page.
      This defaults to `Created with coc.py Client`

- Email and Password are now mandatory parameters and must be passed

- `update_tokens` parameter has been removed. The client will automatically reset bad tokens.

- In order to keep consistency with the official API docs, `token` has been renamed to `key`.
  This affects the following method/parameters:

    - ``on_token_reset(new_token)`` --> ``on_key_reset(new_key)``
    - ``HTTPClient.login()`` --> ``HTTPClient.get_keys()``

  and otherwise consistent use of `key` during internals, docs, code and examples.

- `pytz` and `python-dateutil` have both been removed as dependencies due to the ability to
  parse timestamps manually. This has been added to utils as a function: ``from_timestamp(ts)``,
  returning a utc-datetime object.

- Dataclasses have received a makeover! Many new attributes are present, these are listed below.
  Most importantly, any property beginning with an underscore (_) use and return iterator objects.
  These are **not** lists, and relevant python documentation is here:
  https://docs.python.org/3/glossary.html#term-iterator.

  These are up to 12x faster than lists, and
  as such for those who are concerned about speed, performance and memory should use these, while
  for the majority, calling the regular property should be fine (usually returning a list rather than iter).

    -   :attr:`SearchClan._members`
    -   :attr:`WarClan._members`
    -   :attr:`WarClan._attacks`
    -   :attr:`WarClan._defenses`
    -   :attr:`WarMember._attacks`
    -   :attr:`WarMember._defenses`
    -   :attr:`SearchPlayer._achievements`
    -   :attr:`CurrentWar._attacks`
    -   :attr:`CurrentWar._members`
    -   :attr:`LeagueClan._members`
    -   :attr:`LeagueGroup._clans`

- The following **new** attributes were added:

    -   :attr:`SearchClan.member_dict`
    -   :attr:`WarClan.member_dict`
    -   :attr:`WarClan.attacks`
    -   :attr:`WarClan.defenses`
    -   :attr:`WarMember.attacks`
    -   :attr:`WarMember.defenses`
    -   :attr:`SearchPlayer.achievements_dict`
    -   :attr:`SearchPlayer.troops_dict`
    -   :attr:`SearchPlayer.heroes_dict`
    -   :attr:`SearchPlayer.spells_dict`
    -   :attr:`Timestamp.time`


- The folowwing **new** methods were added:

    -   `SearchClan.get_member(tag)`
    -   `CurrentWar.get_member(tag)`

- New utility functions:

    - `utils.get(iterable, **attrs)`
        - Searches the iterable until a value with the given attribute is found.
          Unlike ``filter()``, this will return when the first value is found.
    - `utils.find(function, iterable)`
        - Searches through the iterable until a value which satisfies the function is found.

    - `from_timestamp(ts)`
        - Parses an ISO8601 timestamp as returned by the COC API into a datetime object


Documentation:
~~~~~~~~~~~~~~~~
- Many docstrings were reformatted or worded, with punctuation and other typo's fixed
- All new properties, attributes and methods have been documented.
- Update some examples, including a `clan_info` function in discord bots (Thanks, Tuba).



v0.0.6
--------
BugFixes
~~~~~~~~~
- Fix bug with always raising RuntimeError

v0.0.5
-------
BugFixes
~~~~~~~~~
- Fixed how the lib detects an invalid IP error, as SC changed how the error message works
- Fixed bug with semi-complete URL when using the API dev site
- ``email`` and ``password`` in :class:`Client` are now ``None`` by default. This was throwing
  and error before.
- str() for :class:`Achievement`, :class:`Hero`, :class:`Troop`, :class:`Spell` now all return
  respective names

Important
~~~~~~~~~~

- Added a new exception: :exc:`Forbidden`. This is thrown when a 403 is returned, but the error is not
  one of invalid token, instead when you aren't allowed to get the resource eg. private war log.

- A :exc:`RuntimeError` will be raised if you try to pass ``update_stats`` as ``True`` but don't set
  the ``email`` or ``password``

- Added the :func:`Client.on_token_reset` which is called whenever the lib updates your token.
  By default this does nothing, however you can override it by either subclassing or
  using the decorator ``@Client.event()`` above your new ``async def on_token_reset``.
  This function can be a regular or coroutine.

Documentation
~~~~~~~~~~~~~~
- Add examples. I will expand on these as I see fit. Feel free to let me know if you want more.
- Fix broken codeblock examples
- Update incorrect function name in the example in README.rst (``player_name`` --> ``get_some_player``

v0.0.4
-------
BugFixes
~~~~~~~~~
- Fix some problems comparing naive and aware timestamps in :class:`.Timestamp`
- Add a private ``_data`` attribute to all data classes.
  This is the json as the API returns it. It makes ``json=True`` parameters in
  requests easy to handle.
- Only cache complete clan results - ie. ``Client.search_clans`` only returned a :class:`BasicClan`,
  so in order to add some cache consistency, cached clans now only contain :class:`SearchClan`.

Important
~~~~~~~~~~
- New Class - :class:`.LeagueWarLogEntry` is similar to :class:`WarLog`, however it has it's own
  set of attributes to ensure it is easier to use and know which ones are present and not.
- This new class is utilised in ``Client.get_warlog``, which returns a ``list`` of both
  ``LeagueWarLogEntry`` and ``WarLog``, depending on the war.

Documentation
~~~~~~~~~~~~~~
- Utilise `sphinx_rtd_theme` for the RTD page
- Add this changelog
- Continue to fix typos and little errors as they are found.


v0.0.2
-------
BugFixes
~~~~~~~~~
- Fix some attributes from inherited classes not being present
- Fix some :exc:`AttributeError` from being thrown due to incomplete data from API
- When a clan is not in war, :class:`.WarClan` will not be present.
  Some errors were being thrown due to incomplete data being given from API
- Allow for text-only responses from API (ie. not json)


Important Changes
~~~~~~~~~~~~~~~~~~
- Actually specify that the package coc needs to be installed when installing with pip
- Fix incorrect spelling of both :class:`.Achievement` and :exc:`InvalidArgument`
- Update the examples in the README to work (search_players is not a thing)


v0.0.1
-------
Initial Commit!
