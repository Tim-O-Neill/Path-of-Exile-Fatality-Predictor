# Path of Exile Fatality Predictor

## What is Path of Exile?

Path of Exile is a free-to-play action role-playing video game developed and published by Grinding Gear Games on October 23rd, 2013. Players select from one of six base classes and fight across the continent of Wraeclast, walking the path of the exile as they go. It has a thriving multiplayer community and a number of various game modes to complete, of which the best performing players are displayed on the forums.

## The Goal

My goal with this project is to provide a free and easy-to-use app that will allow players (or simply curious users) to input character data such as class, level, league and (eventually, hopefully) skills and equipment in order to predict the likelihood of character death, the location of possible character death, and the probable cause of death. At present, the models are 94% accurate with respect to character death, but do not predict location or cause.

## The Data

Grinding Gear Games freely publishes information regarding player standing and rankings for each of their active leagues on their forums ([here.](https://www.pathofexile.com/forum/view-forum/leagues-and-race-events)) To minimize noise and interference in the predictive model, I chose to only pull data from leagues with the Hardcore, Solo Self-Found tags. This means that character deaths result in being permanently banned from the league, and players are not allowed to trade gear or group up with other players. 

|Feature|Type|Dataset|Description|
|---|---|---|---|
|Rank|int|1-2000|The leaderboard rank of a specific character| 
|Account|object|name|Account name associated with the character|
|Character|object|name|Name of the ranked character|
|Class|object|name|Name of character's associated class|
|Level|int|1-100|Level of listed character|
|Experience|int|0 - 4250334444|Number of experience points character has acquired|
|Dead|int|1-0|Whether a character has died or not. A 1 indicates death.|
|League|int|1-0|Indicator of which League a character originated in. A 1 means the character was created in the given League|

Note: the League columns are not native to the PoE raw files. They are an engineered feature.

## Current Status

At present, the project takes in 16,000 characters across eight leagues, clusters them via KMeans, and then attempts to predict whether they died or not. It is very much a WIP, proof-of-concept skeleton. That said, the models are very accurate, reaching 93-94% accurate predicted mortality rate. This far exceeds the null model's output of 63.12%.

## Future Steps

This project has many more steps to go before it reaches completion. Amongst them are the development of a Streamlit app in order to facilitate an easy-to-use and intuitive user interface, an expansion of current character observations to include all active and completed Leagues, the collection of enemy data to facillitate prediction of probable cause/location of character death, collection of skill and equipment data to further enhance cause/location predictions, and possibly an expansion to non-SSF Leagues to see if current models are more or less effective at predicting fatality rates.



### Known and Suspected Hurdles

While Future Steps provides a blanket wish-list of features I want to include in the project, I am anticipating difficulties in the following sections:

- Equipment, Skill, and Enemy damage data is almost certainly not easily worked with, if it's even possible to find it. While they will almost certainly exist as data tables themselves, using them to predict what enemy or damage type kills the given character will almost certainly require simulating combat according to the game's engine, something far beyond my current capabilities. Therefore, until I come up with a workaround, predicting how a character dies is likely impossible.

- The model currently clusters 16,000 characters in order to generate predictions on whether they die. When a user creates a new character using the app, they'll have to be clustered accordingly in order to benefit from the increased accuracy. That being said, I'll need to create and test a cluster-predicting model for these new characters, which may prove difficult.

- Similar to damage data above, collecting zone data for predicting location of death is currently not possible for mee, although it may be something GGG has collected for internal use. 

