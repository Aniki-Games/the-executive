# The Executive modding summary

## Mods locations

Mods should be placed in the 'Mods' folder next to the game's executable. Each mod requires its  
own subfolder.

Example:

    The Executable/  
		Mods/  
			MyFirstMod/  
			MySecondMod/

## Mod structure

A mod is a collection of files (see the 'Data structure' section). Inside each mod, you are free  
to organize config files as you want. Localization and image files require specific subfolders   
(see the relevant sections).

Each mod MUST contain a Metadata.json file, with the following fields:

    {  
    "type": "mod",  
    "context": "shared"  
    }

 - 'type' should always be 'mod'  
 - 'context' can be either 'shared' (for mods that alter the base experience) or an industry id  (eg. 'hollywood', 'chinawood') for mods specific to that industry
   
Each mod can contain a special 'Bootstrap' folder, whose contents are always loaded at game launch.  
It's purpose is to add data required before starting an actual run. In the base game, its use is   
for registering the different industries ids and localized names so they can be listed in the start  
game screen. Unless you're modding in a new industry, you probably shouldn't use it.

## Mod load order
 - mods in the 'shared' context are loaded
	 - mods of 'internal' type are loaded in alphabetical order
	 - mods of 'mod' type are loaded in alphabetical order
 - mods in the selected industry context are loaded
	 - mods of 'internal' type are loaded in alphabetical order
	 - mods of 'mod' type are loaded in alphabetical order

## Data structure

The game uses three types of data:
 - localization files
 - image files
 - config files 

### 1\. Localization files

All localization files must be located inside a mod subfolder named "Localization". They can have any name with the '.csv' extension.

Localization files are in CSV format. The first line is the header, specifying the language codes of the file's data. Entries that contain special characters need to be enclosed by double-quotes.

Any number of location files can be processed. Duplicated entries overwrite previous ones according to the file load order (see relevant section).

### 2\. Image files

Images must be in the PNG format, with an extension ".png".

### 3\. Config files

Game configuration includes nearly every aspect of the game, from the list of themes and genres to the formulas for computing box office and distributions. They are read one after the other, each JSON structure inside being concatenated into a single, main data object.

## Conflict resolution

### 1\. Localization files

Later entries in the load order override earlier entries with the same key and language code. New language codes can be added to the header (first line). Creating a localization file with new language codes does not erase the existing localization for other languages.

### 2\. Image files

Later entries in the load order override earlier entries with the same file name.

### 3\. Config files

The base rules are:

 - if key/index exists and val is not null and val is a literal (string or number): replace
 - if key/index exists and val is not null and val is an object (dictionary or array): merge
 - if key/index exists and val is null: delete  
 - if key/index does not exist: add

####     A. Dictionary example:

**original config:**

    "ex-dictionary":  
    {  
    "key-a": "value A",  
    "key-b": "value B",  
    "key-c": "value C",  
    }

**mod config:**

     "ex-dictionary":  
         {  
            "key-a": "value X",  
            "key-d": "value D",  
            "key-b": null,  
         }  
**resulting value:**  

     "ex-dictionary":  
         {  
            "key-a": "value X",  
            "key-c": "value C",  
            "key-d": "value D",  
         }

####     B. Array example:
**original config:**

     "ex-array":  
         [  
            "value A",  
            "value B",  
            "value C"  
         ]  
**mod config:** 
 
     "ex-array":  
         [  
            "value X",  
            null,  
            "value Y",  
            "value Z"  
         ]  
**resulting value:** 
 
     "ex-array":  
         [  
            "value X",  
            "value Y",  
            "value Z"  
         ]  
           

> Explanation:  
>      => at index 0, "value A" is replaced with "value X"  
>      => at index 1, "value B" is removed from the array  
>      => at index 2, "value C" is replaced with "value Y"  
>      => at index 3, "value Z" is added to the array
   
####      C. Array operators:
Arrays can have indices specified between brackets in the array key. This will override the base index order of the modded data.  

Rules are:  
 - key ends with an array of index operators: '\[a,b,c,...\]'
 - a numeral index operator redirects the item to the corresponding index  
 - a '+' index operator redirects the item to the end of the array  

##### Example 1:
**original config:**  

    "ex-array":  
	    [  
		    "value A",  
		    "value B",  
		    "value C"  
	    ]
**mod config:**

    "ex-array[1]":  
	    [  
		    null  
	    ]
  
**resulting value:**  

    "ex-array":  
	    [  
		    "value A",  
		    "value C"  
	    ]

##### Example 2:  
**original config:**  

    "ex-array":  
    [  
	    "value A",  
	    "value B",  
	    "value C"  
    ]  
**mod config:**  

    "ex-array[2]":  
	    [  
		    "value Z"  
	    ]  

**resulting value:**  

    "ex-array":  
	    [  
		    "value A",  
		    "value B",  
		    "value Z"  
	    ]

##### Example 3:  
**original config:**  

    "ex-array":  
	    [  
		    "value A",  
		    "value B",  
		    "value C"  
	    ]  

**mod config:**  

    "ex-array[+]":  
	    [  
		    "value X"  
	    ] 

 
**resulting value:**  

    "ex-array":  
	    [  
		    "value A",  
		    "value B",  
		    "value C",  
		    "value X",  
	    ]

##### Example 4:  
**original config:**  

    "ex-array":  
	    [  
		    "value A",  
		    "value B",  
		    "value C",  
		    "value D",  
		    "value E"  
	    ]  

**mod config:**  

    "ex-array[1,3,+]":  
	    [  
		    null,  
		    "value X",  
		    "value Z"  
	    ]

  
**resulting value:**  

    "ex-array":  
	    [  
		    "value A",  
		    "value C",  
		    "value X",  
		    "value E",  
		    "value Z"  
	    ]

####      D. Merge examples:

##### Example 1:
**original config:**  

    "ex-dictionary":
    {
       "key-a":  
           {
               "subkey-a": "subvalue A",
               "subkey-b": "subvalue B",
           },
       "key-b": 
           {
               "subkey-a": "subvalue A",
               "subkey-b": "subvalue B",
           },
       "key-c":  
           {
               "subkey-a": "subvalue A",
               "subkey-b": "subvalue B",
           },
    }

**mod config:**

    "ex-dictionary":
    {
       "key-a":  
           {
               "subkey-a": "subvalue X",
               "subkey-b": null,
           },
       "key-b": null,
       "key-d":  
           {
               "subkey-a": "subvalue A",
               "subkey-b": "subvalue B",
           }
    }

**resulting value:**  

    "ex-dictionary":
    {
       "key-a":  
           {
               "subkey-a": "subvalue X",
           },
       "key-c":  
           {
               "subkey-a": "subvalue A",
               "subkey-b": "subvalue B",
           },
       "key-d":  
           {
               "subkey-a": "subvalue A",
               "subkey-b": "subvalue B",
           },
    }

##### Example 2:
**original config:**  

    "ex-dictionary":
    {
       "key-a": [ 1, 2, 3 ],  
       "key-b": [ 1, 2, 3 ],
       "key-c": [ 1, 2, 3 ],  
    }

**mod config:**

    "ex-dictionary":
    {
       "key-a[1]": [ 8 ],
       "key-b": null,
       "key-c[1]": null,
       "key-d": [ 1, 2, 3 ]
    }

**resulting value:**  

    "ex-dictionary":
    {
       "key-a": [ 1, 8, 3 ],  
       "key-c": [ 1, 3 ],
       "key-d": [ 1, 2, 3 ],  
    }

##### Example 3:
**original config:**  

    "ex-array":
    [
       {
           "key-a": "value A",
           "key-b": "value B",
       },
       {
           "key-a": "value C",
           "key-b": "value D",
       },
       {
           "key-a": "value E",
           "key-b": "value F",
       }
    ]

**mod config:**

    "ex-array[0,1,+]":
    [
       {
           "key-a": "value X"
       },
       null,
       {
           "key-a": "value Y",
           "key-b": "value Z",
       }
    ]

**resulting value:**  

    "ex-array":
    [
       {
           "key-a": "value X",
           "key-b": "value B",
       },
       {
           "key-a": "value E",
           "key-b": "value F",
       },
       {
           "key-a": "value Y",
           "key-b": "value Z",
       }
    ]
