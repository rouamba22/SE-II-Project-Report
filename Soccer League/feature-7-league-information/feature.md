# Feature: League Information

**Feature ID:** 7  
**Feature Name:** League Information  
**Status:** Draft  
**Branch:** `soccer-league`  
**Created:** 2026-10-22

---

## User Stories

### US-7.1: See the Public League Information

**As a** member of the Public  
**I want to** see the league result, the teams, and the next official game  
**So that** I can follow the league without a pass code.

### US-7.2: See the Schedule Early

**As a** Coach  
**I want to** see the date and location of each game and each league one month before other people  
**So that** I can prepare my team.

### US-7.3: See My Upcoming Games

**As a** Player  
**I want to** see every upcoming game with its location, date, and time  
**So that** I know where I need to be.

### US-7.4: See the League Around My Assignments

**As a** Referee  
**I want to** see the games I will work and the games I am not working  
**So that** I know the league dates and locations.

---

## Functional Requirements

- **FR-001:** The system shall show the result of the season or league on the public information screen.
- **FR-002:** The system shall show the teams on the public information screen.
- **FR-003:** The system shall show the Public only the next official game, including the teams that play and that game's date, location, and time.
- **FR-004:** The system shall show the Public every upcoming game a Coach organizes, including the date, time, location, and team or teams.
- **FR-005:** The system shall let the Public open this information with no pass code.
- **FR-006:** The system shall show a Coach the date and location of each game and each league one month before Players, Referees, and the Public.
- **FR-007:** The system shall show a Coach all official games and the teams that play.
- **FR-008:** The system shall hide the Referees from a Coach.
- **FR-009:** The system shall show a Coach the result of each league.
- **FR-010:** The system shall show a Coach the performance data for every player on that Coach's own team.
- **FR-011:** The system shall show a Player every upcoming game, official or not, with the location, date, and time.
- **FR-012:** The system shall show a Player the league location, date, and time.
- **FR-013:** The system shall show a Player whether they were chosen for the next official game.
- **FR-014:** The system shall show a Player their own performance and the last league's details for each game.
- **FR-015:** The system shall show a Player league results, game information, and league data.
- **FR-016:** The system shall show a Referee the date, time, location, and teams for games that Referee will work.
- **FR-017:** The system shall show a Referee the games that Referee is not working, including the date and location of each game and league.
- **FR-018:** The system shall show the Admin the leagues, teams, Coaches, players, Referees, games, results, and player data.

---

## Key Entities

| Entity             | Description                                                                 |
| ------------------ | --------------------------------------------------------------------------- |
| Public league view | The season or league result, the teams, the next official game, and the upcoming Coach games. |
| Early notice       | The date and location of a game or league, shown to Coaches one month before the other roles. |
| Player game view   | Every upcoming game, the Player's selection for the next official game, and the Player's own performance. |
| Referee game view  | Assigned games with date, time, location, and teams, plus unassigned games with date and location. |

---

## Initial Data Model

### Information Shown by Role

| Attribute | Type   | Description                                                                 |
| --------- | ------ | --------------------------------------------------------------------------- |
| role      | String | Public, Coach, Player, Referee, or Admin                                    |
| visibleItems | String | The information that role can see                                        |
| passCodeRequired | Boolean | No for Public. Yes for the other roles.                              |

### Early Notice

| Attribute | Type    | Description                                                                 |
| --------- | ------- | --------------------------------------------------------------------------- |
| itemType  | String  | Game or league                                                              |
| itemDate  | Date    | Shown to Coaches first                                                      |
| location  | String  | Shown to Coaches first                                                      |
| leadTime  | String  | 1 month before Players, Referees, and the Public can see that date and location |

**Relationships:**

- The Public sees the season or league result, the teams, the next official game only, and every upcoming Coach game.
- A Coach sees date and location one month before Players, Referees, and the Public.
- A Coach sees official games and the teams that play, and does not see Referee names.
- A Player sees all upcoming games, league location, date, and time, selection for the next official game, and personal performance.
- A Referee sees assigned games in full and other games by date and location.

---

## Gherkin AC

### US-7.1: See the Public League Information

#### AC-7.1.1: Show the next official game and the league result

**Given** an upcoming official game exists  
**And** a league result exists  
**When** a person chooses Public  
**Then** the screen shows the league result and the teams  
**And** the screen shows the next official game with its teams, date, location, and time  
**And** the screen does not ask for a pass code.

#### AC-7.1.2: Show only the next official game

**Given** two official games are upcoming  
**When** a person chooses Public  
**Then** the screen shows only the next official game.

#### AC-7.1.3: Show upcoming Coach games

**Given** a Coach has an upcoming practice  
**And** a Coach has an upcoming game against another team  
**When** a person chooses Public  
**Then** the screen shows both Coach games with the date, time, location, and team or teams.

### US-7.2: See the Schedule Early

#### AC-7.2.1: Show Coaches the date and location one month early

**Given** a game has a date and a location  
**When** that date is still inside the Coach's one-month lead  
**Then** a Coach can see the date and location  
**And** a Player, a Referee, and the Public cannot see that date and location yet.

#### AC-7.2.2: Hide Referees from a Coach

**Given** an official game has Referees assigned  
**When** the Coach views the game  
**Then** the teams are shown  
**And** the Referees are not shown.

### US-7.3: See My Upcoming Games

#### AC-7.3.1: A Player sees every upcoming game

**Given** an official game and a Coach game are both upcoming  
**When** the Player opens the Player screen  
**Then** the Player sees the location, date, and time of both games  
**And** the Player sees whether they were chosen for the next official game.

### US-7.4: See the League Around My Assignments

#### AC-7.4.1: A Referee sees work and other games

**Given** a Referee is assigned to one game  
**And** another game does not include that Referee  
**When** the Referee opens the Referee screen  
**Then** the assigned game shows the date, time, location, and teams  
**And** the other game shows the date and location.

---

## Screen Requirements

### SR-001: Public Information Screen

**Authorized user:** Anyone who chooses Public. No pass code.

**Display:**

- Result of the season or league
- Teams
- Next official game, with the teams, date, location, and time
- Every upcoming Coach game, with the date, time, location, and team or teams

**Actions:**

- View that information

**Behavior:**

- Official games beyond the next one are not shown.
- Referee names are not shown.
- When no official game is upcoming, the screen shows that no official game is scheduled.
- When no Coach game is upcoming, the screen shows that no Coach game is scheduled.
- When no league result exists, the screen shows that no league result has been entered.

### SR-002: Coach Information Screen

**Authorized user:** Coach, after the correct pass code

**Display:**

- Date and location of each game and league, one month before the other roles
- All official games and the teams that play
- Result of each league
- Performance data for every player on the Coach's team

**Actions:**

- View that information

**Behavior:**

- The screen does not show Referees.
- When a date and location are outside the Coach's one-month window, the screen does not show them early.

### SR-003: Player Information Screen

**Authorized user:** Player, after the correct pass code

**Display:**

- Every upcoming official and non-official game, with location, date, and time
- League location, date, and time
- Whether the Player was chosen for the next official game
- The Player's own performance and the last league's per-game details
- League results and game information

**Actions:**

- View that information
- Send a message to the Coach of the Player's own team

**Behavior:**

- When the Player has no upcoming game, the screen shows that no game is coming up.

### SR-004: Referee Information Screen

**Authorized user:** Referee, after the correct pass code

**Display:**

- Games the Referee will work, with date, time, location, and teams
- Games the Referee is not working, with date and location
- League date and location

**Actions:**

- View that information
- Send a message to the Admin

**Behavior:**

- When the Referee has no assigned game, the screen shows that no game is assigned.
