# Quest System

## Core Structure:

1/. QUESTS are listed as a dictionary, each with its own unique ID:

    {
    	"quests":
    	{
    		"quest-a": { ... }
    	}
    }

2/. quests are structured into BLOCKS, listed as an array under the "blocks" key:

    {
    	"quests":
    	{
    		"quest-a":
    		{
    			"blocks": [ { ... }, { ... } ]
    		}
    	}
    }

3/. blocks contain OBJECTIVES, listed as an array under the "objectives" key:

    {
    	"quests":
    	{
    		"quest-a":
    		{
    			"blocks":
    			[
    				{
    					"objectives": [ { ... }, { ... } ]
    				}
    			]
    		}
    	}
    }

4/. each quest / block / objective can contain STATE CHANGE sections: running, stopped, failure, success.
Each of these sections is an array of CONDITIONS and ACTIONS.

    ...
    {
	    ...,
	    "running": [ { ... }, { ... }],
	    ...
    }
    ...

5/. each STATE CHANGE can contain CONDITIONS ("if"). If all conditions are true, the stage changes to the corresponding state.

6/. each STATE CHANGE can contain ACTIONS ("do"). These actions are executed when the state is entered.

## Quest parameters:

 - "auto-start": if true, starts the quest as soon as a game session starts
 - "hidden": if true, doesn't display the quest in the quest HUD

## Objectives parameters:

 - "hidden" (optional): if true, doesn't display the objective in the quest HUD
 - "desc-key" (optional): localization key of the block title string
 - "count": number of times the objective must be completed before it changes to the success state
 - "optional": objective is optional :P

## Actions parameters:

 - "chance": the percentage chance (from 0.0 to 1.0) that this action will be executed

## Available actions & conditions

See document [ACTIONS-CONDITIONS-README.md](ACTIONS-CONDITIONS-README.md)
