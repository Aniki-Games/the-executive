# Incident System

## Core Structure:

1/. INCIDENTS are listed as a dictionary, each with its own unique ID:

    {
    	"incidents":
    	{
    		"incident-a": { ... }
    	}
    }

2/. incidents are structured into CHOICES, listed as an array under the "choices" key:

    {
    	"incidents":
    	{
    		"incident-a":
    		{
    			"choices": [ { ... }, { ... } ]
    		}
    	}
    }

3/. each incident / choice can contain STATE CHANGE sections: running, stopped, failure, success.
Each of these sections is an array of ACTIONS.
NOTE: STATE CHANGES are driven by the player choice in dialog. CONDITIONS ("if") are not valid in incident state changes.

    ...
    	{
    		...,
    		"running": [ { ... }, { ... }],
    		...
    	}
    ...

4/. each incident contains a VALIDITY section ("valid"). This section contains CONDITIONS ("if") to check if the incident should run.

5/. each STATE CHANGE can contain ACTIONS ("do"). These actions are executed when the state is entered.

## Incident parameters:

 - "title-key": loc key for the incident dialog title
 - "desc-key": loc key for the incident dialog main text
 - "valid": list of CONDITIONS
 - "trigger": list of CONDITIONS, used to select when a "movie" incident triggers
 - "pool": category defining its spawning logic. Valid values are:
	 - "history": executed always, checked at the start of each month
	 - "movie": executed if a movie is selected to have an incident (param "movie-chance"), checked on movie creation
	 - "studio": executed when the studio is eligible for an incident (param "studio-interval"), checked each day

## Choice parameters:

 - "id": unique id (per incident, not globally) of the choice
 - "desc-key": localization key of the choice description string
 - "effect-key": localization key of the choice effects string

## Actions parameters:

 - "chance": the percentage chance (from 0.0 to 1.0) that this action will be executed

## Available actions & conditions

See document [ACTIONS-CONDITIONS-README.md](ACTIONS-CONDITIONS-README.md)
