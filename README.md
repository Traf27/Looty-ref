## Looty-ref

Library of ideas for my endavours, tinkering with [Looty](https://github.com/benjaminjackman/looty/) stash manager
to have all in one place remotely avaiable

[GitKraken board](https://app.gitkraken.com/glo/board/XdYli-AbPwAPFRzD)

---

### TODO
mostly the same as in glo boards, thus its easier sometimes for me to add here, and update there :)

#### Planned improvements:
#### Design:
 - [ ] move menu
 - [ ] create help menu/panel 
 - [ ] Solve problem with column selection, other words big soup of mods you can have as columns
 - [ ] Hovering Warning for user that looty has no creditentials - poe site with loged wasnt visited. Explain why 
```
Why its not working:
Looty is authorized to fetch you item data upon sending your POESESSID.
It takes it from chrome localStorage - thats feature of every extension which registers it in manifest.json being part of ext.
To be able to do that you have to visit pathofexile.com site even once first. Then its sits in browser storage.
Third party aps can't do that, thus you give them this yourself.
```
 thus
 
 ### User selecting mods/affixes for columns
 
 Several problems with that. As of current version:
 * tight space for choosing mods
 * categories not visible enough and/or eyebaling left for cat. name and right for desired mod and shorthands because of lack of space, often lead to sacrifices in cost of readability.

Readability has to be improven. Random thoughts below.

left menu with accordion (open/close) with similar categories as of now : General, Defense, i'd do this as parent of resists.

- General
  - location
  - name 
  - rarity
  - ...
- Defense
  - resists
  - regen
- Offense
- ...

 mockup 
  
 #### Code:
 - [ ] resolve Throtteling errors, using limit headers values
 - [ ] add chaos equivalent price to your owned currency with [poe.ninja data](https://poe.ninja/api/Data/GetCurrencyOverview?league=Blight)
 - [ ] add jewel types for search/filter like: 
 
  ```
  { 
    "jewelTypes":[
    "Searching Eye Abyss Jewel",
    "Murderous Eye Abyss Jewel",
    "Ghastly Eye Abyss Jewel",
    "Hypnotic Eye Abyss Jewel",
    "Crimson Jewel",
    "Viridian Jewel",
    "Cobalt Jewel"
    ]
  }
  ```
hmm *looty does show this info*, **but** if we choose column tpeln, id say its very ambigous... Solution? when user choose column type than propose tpeln , or add it to default view
Ad. there is error because of this fragment of item json (which is not parsed by Looty yet) :
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
Still I coul glue it to Jewel type like "Abyss Jewel" so user can filter it that way, instead of leaving it like now.

  
  - [ ] Make more visible information when making new Select Column save -> "New: column_filter_name" -> something maybe tooltip  
  - [ ] Probably hard as fuck for me. Add support for all abyss/jewel mods on rare ver. (because Jewels are awesome!)
  
  **"Potential"** problems :
  * How am i going to figure out where the hell all those mods fit into Select Column panel ?! now it takes whole screen on my laptop. *Wink wink with* side menu maybe
    * (And i was secretly thinking about porting it to mobile one day, foolish me!) 

  
  
  Other: 
 - [ ] experiment with icons for every row/ or for row with Jewels
 - [ ] I have discovered [Semantic UI](https://github.com/Semantic-Org/Semantic-UI) and its counterpart in React - I'm in Love <3 (was thinking, how the heck will I style this redesign thing ...) ... hmm is it worth using in here?
