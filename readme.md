# Project Purpose

This project is meant to store day to day information about myself and various other people
within my network. This data is meant to be as secure as possible, so an additional REST
abstraction layer will be put in place on top of an SQL interface. This project will provide
information for greater insight and statistics into various things such as habits, movements,
etc.

# Pros

Within Minecraft, there are various factions that might cause trouble within our own
territory. The target Minecraft server runs plugins that allow a certain block type to alert an
integrated discord bot if the player is within its range. I currently have a backend set up
to alert if untrustworthy people are within key areas, but I have no way to visualize the
collected raw data in a way thats meaningfull. This will allow for greater insight into the
movement of players and provide greater security to the players within our borders.

# Cons / Limitations

The target hardware for this server is old and antiquated. It is equipped to run a basic
SQL server though. We can eliminate some of the overhead and technical debt with a secondary 
Raspberry PI, which will run the REST abstraction layer. This PI will then connect directly
to the main storage PC via Ethernet.

## Working Model
                                           
```   _______________________________________________________________________
     | _____________________         Communication Model      ______________ |
     ||                     |                                |              || 
     ||	     Main Server    |                                | Raspberry PI ||
     || ___________________ |        Ethernet connection     | ____________ ||
     ||| PostgreSQL server <---------------------------------->  REST API  |||
     |||_________-|-_______||             ________           ||_____-^-____|||
     ||__________-|-________|  Response  |        |  Request |______-|-_____||
     |            +----------------------> Client -------------------+       |
     |                                   |________|                          |
     |_______________________________________________________________________|

     ________________________________________________________________________________________
    |                                       SQL Models                                       |
    |                                                                                        |
    |                                         Legend:                                        |
    |                       |                                                                |
    |                       +--+ (connecting line) = referential value                       |
    |                          |                                                             |
    |                                                                                        |
    |                                table 1: PlayerRecords                                  |
    |    ________________________________________________________________________________  | |
    |   | internalID (int) | threatLevel (int 0-3) | nation (string) | username (string) | | |  
    |   |________-|-_______|__________-|-__________|_______-|-_______|___________________| | |
    |             |                    |                    |                                |  
    |             |                    |                    +------------------------------+ |
    |             |                    +-------------------------------------------------+ | |
    |             |                                                                      | | |
    |             |                  table 2: DetectionData                              | | |
    |    ________-|-__________________________________________________________________   | | |
    |   | internalID (int) | locationName (string) | coordinates (bigint) | timestamp |  | | |  
    |   |__________________|_______________________|______________________|___________|  | | | 
    |   | namelayer (string) | relayGroup (string) |                                     | | | 
    |   |____________________|_-|-_________________|                                     | | |  
    |                           |                                                        | | |
    |       +-------------------+                             +--------------------------+ | |
    |       |                        table 3: RelayGroups     |                 +----------+ |
    |    __-|-_______________________________________________-|-_______________-|-_______    |
    |   | relayGroup (string) | autoAlerts (boolean) | threshold (int) | nation (string) |   |
    |   |_____________________|______________________|_______-|-_______|_______-|-_______|   |
    |   | description (string) |                              |                 |            |
    |   |______________________|                              |                 |            |
    |        +------------------------------------------------+                 |            | 
    |        |                   +----------------------------------------------+            |
    |        |                   |   table 4: Nations                                        |
    |   ____-|-_________________-|-____________________________                              |
    |   | threat (int 0-3) | name (int) | description (string) |                             | 
    |   |__________________|____________|______________________|                             |
    |________________________________________________________________________________________|

```


### Ideal time for deployment: 2 days
1 day for infrastructure deployment, and 1 day for bug testing.

### Resources 
Raspberry PI 3
Ethernet Cable
USB Type C Cable
Pentium Quad Core CPU
GTX 650ti GPU
General purpose modem for broader internet connection

