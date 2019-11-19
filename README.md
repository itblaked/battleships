Battleships
=========

## Overview

This Ansible role simulates the classic game 'Battleships'.

https://en.wikipedia.org/wiki/Battleship_(game).

Shots are provided as a list of grid points stored in the variable `shots`.

Ships are stored in the variable `ships` as a list of dictionaries, each list item containing the `name`, `health` and `location` of each ship.

See the variable section below for examples of how to write these and where.

The game is based on a 10x10 grid:

```yaml
___________________________________________
|  A   B   C   D   E   F   G   H   I   J
|1 x   x   x   x   x   x
|2     x                                                      
|3                     s       p   p            
|4         x           s           x                 
|5     b               s                            
|6     b                                      
|7     b       x                   x            
|8     b                                     
|9             x                                              
|10            x               d   d   d   
|__________________________________________
```
### Ships

|No|Class of Ship|Size| 
|---|---|---|
|1|Carrier|5|
|2|Battleship|4|
|3|Destroyer|3|
|4|Submarine|3|
|5|Patrol Boat|2|

## Installation

Each player should install the role by cloning it from this repository https://github.com/itblaked/battleships. The role will be published on Ansible-Galaxy in the future.

## Game play

The game is played by running this role from an Ansible Playbook by each player.

Players each prepare to play by updating their ships in the role as detailed below in the game setup section.

Once ships are arranged, players decide who will shoot first. The first shooter should then provide the defender with the grid point of their first shot(s). _(Typically it's one shot per turn, however it really depends on how fast you want to complete the game.)_

The `shots` variable can be provided to the role as an extra variable in the playbook, as a default variable in the role or as an extra variable on the command line. The list of shots should accumulate throughout the game and never regress.

The list of shots are fired at the ships till all of a ships location grid points are destroyed, at which point the ship is sunk. When a ship is sunk, the 'spysub' feature activates.

The game includes the concept of 'spysubs' which become an active part of the game when a ship has been sunk. There are two spysubs; `spysub1` and `spysub2`. 

These spysubs each provide a different benefit to the defender *_if_* the defender is able to guess the correct access code of each sub. `spysub1` provides a bonus shot on the defenders next attack and `spysub2` provides 2 bonus shots on the defenders next attack.

Each time the role is run, the access code for each spysub is randomly selected from a list of predefined numbers; `12, 8, 7, 14, 20`. The defender should provide their access code guess as an integer in the `spysub1_accesscode` and `spysub2_accesscode` variables, e.g. `spysub1_accesscode: 12`.

These two variables can be provided to the role in the same way as the `ships` variable.

### Game setup

#### Ships
Each player should update the ship locations in the `defaults/main.yml` file in the role. When completing this, standard battleship rules apply, see Wikipedia article for specifics however, some key points are:

1. No overlapping grid points between ships.
2. Grid points must be consequtive horizontally or vertically (no diagnals).
3. Grid points must be provided as a yaml list in the location key of each ship.


Requirements
------------

Nil

Role Variables
--------------

```yaml

# A list of shots to be fired against the defenders ships.

shots:
  - a1
  - c2
  - a8
  - e4
  - f8


# Defenders guess of what the spysub access codes will be during the next turn. 
# Choose the number you believe will be the access code for each spysub out of 12, 8, 7, 14, 20.

spysub1_accesscode: 12

spysub2_accesscode: 20



# Defenders ship locations, health and names. 
# Renaming the ships or changing the health will not affect game play, as long as
# the health value is equal to the number of location grid points per ship.

ships:
  - name: carrier
    health: 5
    location:
      - a1
      - b1
      - c1
      - d1
      - e1
  - name: battleship
    health: 4
    location:  
      - d8
      - d7
      - d6
      - d5
  - name: destroyer
    health: 3
    location:
      - j10
      - h10
      - i10
  - name: submarine
    health: 3
    location:
      - f5
      - f4
      - f3
  - name: patrolboat
    health: 2
    location:
      - h3
      - i3

```

Dependencies
------------

Nil.

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

```yaml
    - name: Play Battleships
      hosts: localhost
      connection: local
      vars:
        shots:
          - a1
          - b8
          - f3
        spysub1_accesscode: 12
        spysub2_accesscode: 20
        ships:
        - name: carrier
          health: 5
          location:
            - a1
            - b1
            - c1
            - d1
            - e1
        - name: battleship
          health: 4
          location:  
            - d8
            - d7
            - d6
            - d5
        - name: destroyer
          health: 3
          location:
            - j10
            - h10
            - i10
        - name: submarine
          health: 3
          location:
            - f5
            - f4
            - f3
        - name: patrolboat
          health: 2
          location:
            - h3
            - i3
      tasks:
        - name: Launch Battleships
          include_role:
            name: battleships
```
License
-------

BSD

Author Information
------------------

Blake Douglas
