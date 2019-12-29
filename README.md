## Looty-ref

Library of ideas for my endavours, tinkering with [Looty](https://github.com/benjaminjackman/looty/) stash manager
to have all in one place remotely available

### TODO

- [ ] read [https://github.com/mleibman/SlickGrid/wiki](https://github.com/mleibman/SlickGrid/wiki) - grid used in Looty
- [ ] read [http://allaboutscala.com/](http://allaboutscala.com/) - nice introduction to scala

 #### Code:
 - [ ] add last caching time
 - [ ] add progress of retrieving stashes example: 5 of 25
   - [ ] add ETA to retrieve all stash tab data (and characters), using predictive query time of stash tab. There are: special tab, normal tab, quad tab having various number reqests needed to download them with API
 - [ ] add possibility to choose watched tabs, and those auto refresh on set timer (options or set value), rest will be refreshed only on hard reset(?) or.... ?how it is done now?
 - [ ] ?upon opening looty again/ when else? we could compare last used character (queries to www.pathofexile.com and scrapping profile char), with actual and refresh char invs of said chars.
 - [ ] add experiance progress bar of gems (?maybe characters too)
 - [ ] resolve Throttling errors, using limit headers values. Limit is 45 requests per 60s. 
Solution is to make 44-45 and wait 15sec for next batch. Or what im leaning to more is to do 1 request every 60/45 sec  ~ 1.4sec
 - [ ] add chaos equivalent price to your owned currency with [poe.ninja data](https://poe.ninja/api/Data/GetCurrencyOverview?league=Blight) [WealthView.scala ](https://github.com/benjaminjackman/looty/blob/b8b1c6fb370db9f94c56b9da6e26af521f719b64/looty/src/main/scala/looty/views/WealthView.scala)
 - [x] add jewel types for search/filter 
 - [ ] add optional properties : 
* elder
* shaper
* unidentified
* corrupted
* veiled (how it is written in json structure)
* crusader
* ...
* ...
* has implicit ( column or checkbox? )
* priced? Is there info for items priced individualy about their value?

#### How to automatize adding future options, affixes ? What/is the pattern to forsee the structure ("extended" field im looking at you -_-). And how to organise interface to be future features proof space wise :) ?

There is error because of this fragment of item json (which is not parsed by Looty yet) :
```
{
...
  properties: [{
    displayMode: 0,
    name: "Abyss"
  }]
  ...
}
```
  - [ ] Add support for all abyss/jewel mods on rare ver. (because Jewels are awesome!) :O
  **"Potential"** problems :
  * Where the hell all those mods fit into Select Column panel ?! Now it takes whole screen on my laptop (1280x800). 
 - [ ] parse not parsed properties, for jewels like Range, and put them in collapsed part of pannel by default
