# Content Creation Usecases

In this section you will find guidelines for the most common modding use cases.

## Notes on localization

Generally, the localization prefixes are defined in one of the various SETTINGS Json files available in the Data folder. If unclear, we advise you to look at the Base localization files in the Data folder to understand the prefix structure of the various elements listed below.

## Notes on sprite

Generally, the Sprite prefixes ("icon-prefix") are defined in one of the various SETTINGS Json files available in the Data folder. If unclear, we advise you to look at the Base sprite names in the Data>Shared>Images folder to understand the prefix structure of the various elements listed below.

A few sprites don't have a declared prefix. To update those sprites, the new sprite must have the same name and be located inside a mod subfolder named "Ui". They only work with industry specific mods (not shared).
	 - Base location: Data > Hollywood OR Chinawood > Ui

## Adding Content
###  1. Add an IP source

 - Create a new “ip-sources” record 
	 - Base location: Data > Shared > Industry > IpLicenses-SETTINGS.json
 - Localize the IP source name in a csv file in a folder named “Localization” 
 - Add a 24x24 and a 120x120 images
 - OPTIONAL: Update / create “ip-licenses” records to use the new IP source in the "ip-source" key

### 2. Add an IP license

 - Create a new "ip-licenses" record
	 - Base location: Data > Shared > Industry > IpLicenses.json
 - Localize the IP name and description in a csv file in a folder named “Localization” 
 - OPTIONAL: Add a 54x68 image with the name “IP.Poster.XXX” with XXX the value in the key "poster-key" of the associated record
> NOTES ON SETUP: The UI/UX only allows to list:
>         - Max 1 genre per record
>         - Max 1 theme per record
>         - Max 1 rating per record

###  3. Add a talent

 - Create a new “actors” or “directors” record
	 - Base location: Data > Hollywood OR Chinawood > Industry > Actor.json OR Director.json
 - Localize the talent’s name in a csv file in a folder named “Localization” 
> NOTE ON SETUP: Requires one history record per active year

### 4. Add an award

 - Create a new “awards” record
	 - Base location: Data > Shared > Industry > Awards.json
 - Localize the various name and description keys in a csv file in a folder named “Localization” 
 - Add the award various images:
	 - Back of the nominee card: 220x280
	 - Award Statue: 362x730
	 - Award Statue Shiny VFX: 362x730
	 - Award winner card logo: 70x70

> NOTE ON SETUP: The sprites in the movie details window can't be added as the list of awards is static (limited to Romy and Gaspar).

### 5. Add a seasonal event 

 - Create a new “seasons” record
	 - Base location: Data > Shared > Other > Seasons.json
 - Localize the seasonal event’s name in a csv file in a folder named “Localization” 
 - Add a 24x24 image

> NOTE ON SETUP: The UI can only show up to two seasonal events for any given month

### 6. Add a distributor

 - Create a new “distributors” record (note: you need one history record per active decade)
	 - Base location: Data > Hollywood OR Chinawood > Industry > Distributors.json
 - Localize the distributor’s name in a csv file in a folder named “Localization”  
 - OPTIONAL: Add a 180x110 logo image

> NOTE ON SETUP: The UI can only show up to two favorite genres listed in the “affinities” array at any given time

### 7. Add a competing movie

 - Create a new “movies” record
	 - Base location: Data > Hollywood OR Chinawood > Industry > Movies.json
 - Localize the distributor’s name in a csv file in a folder named “Localization”
 - OPTIONAL: Add a 66x100 poster image with the name “Movie.Poster.XXX” with XXX the value in the key "poster-key" of the associated record

### 8. Add a theme

 - Create a new “theme” record
	 - Base location: Data > Shared > Other > Enums.json
 - Localize the theme’s name in a csv file in a folder named “Localization” 
 - Add a “genre-vs-theme” row to “affinities”
    - Base location: Data > Shared > Other > Affinities.json
  - Add the new theme to the "theme-affinity" dictionary of each “features” record
	  - Base location: Data > Shared > Other > Features.json
  - Add the new theme to the "theme-score" dictionary of each “directors” and “actors” record
  - Add a 24x24 and a 90x90 images
  - OPTIONAL: Update / Create “ip-licenses” records to use the new theme as a required characteristic in the “themes” array

### 9. Add a genre

> NOTE: mods adding a genre need to be at the industry level (i.e. not Shared) as some of the required modifications listed below can only be done at the industry level.

 - Create a new “genre” record
	 - Base location: Data > Shared > Other > Enums.json
 - Localize the genre’s name in a csv file in a folder named “Localization”
 - In “affinities”:
	 - Add a column and row to “genre-vs-genre”
	 - Add a column to “genre-vs-rating”
	 - Add a column to “genre-vs-planning”
	 - Add a column to “genre-vs-theme”
	 - Base location: Data > Shared > Other > Affinities.json
 - Add the new genre to the "genre-score" dictionary of each “directors” and “actors” record
 - Add a column to the “genre-vs-size” rule in the “bo-baseline” section of each “markets” record
	 - Base location: Data > Hollywood OR Chinawood > Markets > Domestic-BoxOffice.json AND International-BoxOffice.json
 - Add a column to the “base-genre-01” and “base-genre-02” rules in the “baseline” section of “audience” and “critics”
	 - Base location: Data > Hollywood OR Chinawood > Metrics > Audience.json AND Critics.json
 - Add the new genre to the "genre-bonus" dictionary of each “seasons” record
	 - Base location: Data > Shared > Other > Seasons.json
 - OPTIONAL: Update / Create “distributors” records to use the new genre as a favorite one in the “affinities” array
 - OPTIONAL: Update / Create competing “movies” records to use the new genre in the "genre-01" or "genre-02" keys
 - OPTIONAL: Update / Create “ip-licenses” records to use the new genre as a required characteristic in the “genres” array
 - Add a 24x24 and a 90x90 images

### 10. Add a rating

> NOTE: mods adding a rating need to be at the industry level (i.e. not Shared) as some of the required modifications listed below can only be done at the industry level.

 - Create a new “ratings” record
 - Localize the rating’s name (including short version) in a csv file in a folder named “Localization” 
 - Add a “genre-vs-rating” row to “affinities”
	 - Base location: Data > Shared > Other > Affinities.json
 - Add the new rating to the "rating-affinity" dictionary of each “features” record
	 - Base location: Data > Shared > Other > Features.json
 - Add a column to the “base-rating” rule in the “bo-baseline” section of each “markets” record
	 - Base location: Data > Hollywood OR Chinawood > Markets > Domestic-BoxOffice.json AND International-BoxOffice.json
 - Add a column to the “base-rating” rule in the “baseline” section of “audience” and “critics”
	 - Base location: Data > Hollywood OR Chinawood > Metrics > Audience.json AND Critics.json
 - OPTIONAL: Update / Create  “movies” records to use the new rating in the "rating" key
