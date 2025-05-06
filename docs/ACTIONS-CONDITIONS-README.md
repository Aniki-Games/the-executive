# Actions and Conditions

## Arithmetic conditions:

All arithmetic conditions take two parameters:
 - "op": what operation to perform on the checked stat. Can be "<", "<=", "=", "!=", ">=", ">"
 - "value": the value to compare the checked stat to

## Global conditions

    { "if": "check-marker",          "name": "marker-id", "op": ">=", "value": 5 }
    { "if": "chance",                "threshold": 1 }
    { "if": "time",                  "duration": "5" | "after": "5" | "before": "5" }
    { "if": "time",                  "after": "1980/3" }
    { "if": "quest-stage-done",      "state": "any/success/failure" } 

## Studio conditions

    { "if": "cash",                  "op": ">=", "value": 100000 }
    { "if": "office-level",          "op": ">=", "value": 2 }
    { "if": "prestige",              "op": ">=", "value": 0.3 }

    { "if": "staff-count",           "op": ">=", "value": 2, "seniority": [ "seniority-id", ... ], "role": [ "role-id", ... ] }
    { "if": "task-staff-count",      "op": ">=", "value": 2, "id": "task-template-id" }
    { "if": "training-count",        "op": ">=", "value": 2, "seniority": [ "seniority-id", ... ], "role": [ "role-id", ... ] }
    { "if": "tech-count",            "op": ">=", "value": 2, "scope": [ "tech-id | tech-group-id", ... ] }
    { "if": "movie-released-count",  "op": ">=", "value": 2 }
    { "if": "movie-screened-count"   "op": ">=", "value": 2 }
    { "if": "movie-report-count",    "op": ">=", "value": 3 }
    { "if": "has-ancillary-deal",    "type": "home-ent" ]

    { "if": "on-staff-hired",        "seniority": [ "seniority-id", ... ], "role": [ "role-id", ... ] }
    { "if": "on-tech-unlocked",      "scope": [ "tech-id | tech-group-id", ... ] }
    { "if": "on-movie-award"         "stage": "nomination | award" }
    { "if": "on-movie-released"      }
    { "if": "on-movie-screened"      }
    { "if": "on-franchise-created"   }
    { "if": "on-movie-legendary"     }

## Movie conditions

    { "if": "movie-release-date",          "delta-months": -2 }
    { "if": "movie-season",                "scope": [ "season-id", ... ] }
    { "if": "movie-genre",                 "scope": [ "genre-id", ... ] }
    { "if": "movie-size",                  "value": "size-id" }  ?? array  ??
    { "if": "movie-theme",                 "value": "theme-id" }  ?? array  ??
    { "if": "movie-task",                  "id": "task-id" }
    { "if": "movie-franchise",             "entry-type": "entry-type-id" } 
    { "if": "movie-profit",                "op": ">=", "value": 100000 }
    { "if": "movie-critics",                "op": ">=", "value": 7 }
    { "if": "movie-audience",                "op": ">=", "value": 7 }
    { "if": "movie-boxoffice",             "op": ">=", "value": 100000 }
    { "if": "movie-perf",                  "op": ">=", "value": 2, "metric": "metric-id" }
    { "if": "movie-trend",                 "op": ">=", "value": 2, "scope": [ "genre-id | theme-id", ... ], "pattern": "any" }
    { "if": "movie-genre-count",           "op": ">=", "value": 2 }
    { "if": "movie-actor-count",           "op": ">=", "value": 2 }
    { "if": "movie-distrib-staff-count",   "op": ">=", "value": 2 }
    { "if": "movie-markets",               "method": "third | self | any", "scope": [ "marketId", ... ] }
    { "if": "movie-talent",                "slot": "director | lead | supp", "stat": "popularity-dom | critics" }

## Global actions

    { "do": "set-marker", "name": "marker-id", "value": 3 }
    { "do": "mod-marker", "name": "marker-id", "value": -2 }
    { "do": "mod-cash", "value": 100000 }
    { "do": "mod-prestige", "value": 2 }
    { "do": "mod-rp", "value": 20 }
    { "do": "new-staff", "seniority": "junior", "role": "producer" }
    { "do": "unlock-theme", "value": 2 }
    { "do": "unlock-tech", "value": 2, "scope": ["tech-id or tech-group-id"] }
    { "do": "trigger-incident", "id": "incident-name", "delayed": true }
    { "do": "trigger-quest", "id": "quest-name" }
    { "do": "unlock-achievement", "id": "achievement-id" }

## Action groups

    { "do": "group", "chance": 0.5, "items": [
        {"do": "action1"},
        {"do": "action2"} ] 
    }
    
    { "do": "selector", "weights": [ 0.5, 0.5 ], "candidates": [
        {"do": "action1"},
        {"do": "action2"} ]
    }

## Movie actions

    { "do": "mod-movie-bo", "value": 0.1 }  Note: % modifier
    { "do": "mod-movie-critics", "value": 0.5 }
    { "do": "mod-movie-audience", "value": 0.5 }
    { "do": "mod-residuals-hv", "value": 0.5 }  Note: % to add/deduct
    { "do": "mod-residuals-tv", "value": 0.5 }  Note: % to add/deduct
    { "do": "one-off-movie-cost", "value": 1000000 }
    { "do": "mod-task-duration", "id": "task-id", "value": 0.5 } Note: % to add/deduct
    { "do": "mod-task-cost", "id": "task-id", "value": 0.5 } Note: % to add/deduct


