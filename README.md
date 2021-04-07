# AnilistPython
Anilist Python library that allows you to **easily search up and retrieve anime, manga, animation studio, and character information.** This library is both beginner-friendly and offers the freedom for more experienced developers to interact with the retrieved information.

## Overview
This library is currently developed by a team of one and many operations in the library are yet to be optimized. However, the current version (0.0.7) has been tested and it is completely functional at its current state.  The code base drew a lot of inspiration from the PyMoe library which mainly offers data block retrieval with no auto json parsing. 

Currently, only anime and character data retrival has been optimized for beginner-friendly data parsing. 

## How to use?
**Step One:** Library Installation
``` ruby
pip install AnilistPython==0.0.7
```
**Step Two:** Instance Creation
```ruby
from AnilistPython import Anilist
anilist = Anilist()
```
**Step Three**: Usage

Starting off, there are a set of commands optimized for auto json parsing. Currently, they are only available for anime and characters. All of the following functions uses Anilist's new GraphQL API.
```ruby
# ANIME
anilist.getAnimeInfo("Code Geass")             # returns a dictionary with anime info (parsed)
anilist.getAnimeID("ReZero")                   # returns Re:Zero's ID on Anilist
anilist.printAnimeInfo("Madoka Magica")        # prints all information regarding the anime Madoka Magica

#CHARACTER
anilist.getCharacterInfo("Emilia")             # returns a dictionary containing the info about Emilia-tan (parsed)
anilist.getCharacterID("Milim")                # returns character Milim's ID on Anilist
anilist.printCharacterInfo("Misaka Mikoto")    # prints all information regarding the character Misaka Mikoto (desc not parsed)
```
Once the commands above are executed, the program will automatically search and retrieve the request information. When multiple targets are found, **three** results will be shown in the terminal. Pick your desired character to retrieve their information.
```ruby
1. Madonna (Movie)
2. Mahou Shoujo Madoka☆Magica
3. Hanoka
Please select the anime that you are searching for in number: <enter 1, 2, or 3>
```

For retrieved dictionaries from the the step above by using `.getAnimeInfo()` or `.getCharacterInfo()`, the data has be parsed and reformatted into more readable and easily accessible json objects. The **keys** to the correspounding information are of below:
```ruby
#ANIME                 #CHARACTER
- name_romaji          - first_name
- name_english         - last_name
- starting_time        - native_name 
- ending_time          - desc 
- cover_image          - image
- banner_image
- airing_format
- airing_status
- airing_episodes
- season
- desc
- average_score
- genres
- next_airing_ep
```
One simple example would be:
```Crystal
>>> extracted_data = anilist.getAnimeInfo("Tensei Slime")
>>> print(extracted_data["name_romaji"])
Tensura Nikki: Tensei Shitara Slime Datta Ken
```
<br/>

## Here are some more advanced usages:
(Works for all anime, manga, characters, studio, and staff)

Retrieved data takes the form of json objects or lists of json objects. In order to retrieve the desired data, title/item ID is required. Data extraction does not accept string inputs and can only take int parameters as ID's. The process of chaining ID request and data request together has been included in the optimized functions above, but this is not built-in if you would like to directly request for data blocks in the form or orginial/raw json objects.  

Note that it is possible to directly call functions from the subclasses of the driver code in `ExtractID` and `ExtractInfo` with the Anilist instance.
```ruby
# RETRIEVING JSON OBJ CONTAINING DATA
anilist.extractInfo.anime(21610) # Return data on Okusama ga Seitokaichou
anilist.extractInfo.manga(64127) # Return data on Mahouka Koukou no Rettousei
anilist.extractInfo.staff(121963) # Return data on Keisuke Nishijima (Nisizima)
anilist.extractInfo.studio(94) # Return data on Telecom Animation Film
anilist.extractInfo.review(2113) # Return review #2113 and format the review body in HTML

# RETRIEVING JSON OBJ CONTAINING ID NUM
anilist.extractID.anime("King") # Anime search results for King.
anilist.extractID.manga("King") # Manga search results for King.
anilist.extractID.character("Kei") # Character search results for Kei.
anilist.extractID.staff("Keisuke") # Staff search results for Keisuke.
anilist.extractID.studio("Ghibli") # Studio search result for Ghibli. (There's only one)
```
The functions above returns raw json objects that requires extensive parsing before it can be used. Note that `.extractID` does NOT directly return the ID of the given item. Instead, it returns a json object that contains the item's ID number. The only useful information from `.extractID` is the ID number of the provided item. All other data retrieved from `ExtractID` can be found in `ExtractInfo` with the latter having more details.

Parsing the json obj is annoying. For example, if you would like to view the character desc of Rem:
```ruby
data = anilist.extractID.character("Rem")

for i in range(len(data["data]["Page"]["characters']):
  first_name = data["data"]["Page"]["characters"][i]['name']["first"]
  last_name = data["data"]["Page"]["characters"][i]['name']["last"]
  print(f"{first_name}, {last_name}")
  
 user_input = int(input("Select the anime character that you are trying to find: "))
 <checks for corner cases>
 
 info = anilist.extractInfo.character(data['data']['Page']["characters'][user_input - 1]["id"]
 print(info["data"]["Character"]["description"])
```
However, worry not! I will be sure to push out more user-friendly functions like the ones I showed at first.

In addition to that, pagination is taken cared of by the new API. By default you retrieve three results but this can be edited to your liking:
```ruby
extractID(term, page = 1, perpage = 3)
```
