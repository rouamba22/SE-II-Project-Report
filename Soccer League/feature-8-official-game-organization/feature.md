# Feature: Official Game Organization

**Feature ID:** 8  
**Feature Name:** Official Game Organization  
**Status:** Draft  
**Branch:** `soccer-league`  
**Created:** 2026-10-22

---

## User Stories

### US-8.1: Schedule an Official Game

**As an** Admin  
**I want to** enter each official game by hand  
**So that** the division has its official schedule.

### US-8.2: Assign the Referee Crew

**As an** Admin  
**I want to** assign one main Referee and four sideline Referees to an official game  
**So that** the game has its full crew.

### US-8.3: Schedule a Ranking Game

**As an** Admin  
**I want to** schedule an extra official game between two teams that finished with the same points  
**So that** those teams can settle their rank.

### US-8.4: See Assigned Games

**As a** Referee  
**I want to** see the games I will work and the games I am not working  
**So that** I know the official schedule.

---

## Functional Requirements

- **FR-001:** The system shall have the Admin enter each official game by hand.
- **FR-002:** The system shall not build the official schedule automatically.
- **FR-003:** The system shall record a date, a time, a location, and the two teams for each official game.
- **FR-004:** The system shall assign 5 Referees to an official game: 1 main Referee and 4 others, with 2 Referees on each sideline.
- **FR-005:** The system shall treat a game as official only when the Admin schedules it.
- **FR-006:** The system shall have the Admin schedule each team to play every other team in that division one time.
- **FR-007:** The system shall set an official game length of 90 minutes.
- **FR-008:** The system shall have the Admin schedule one extra official game when two teams in a division finish the series with the same points.
- **FR-009:** The system shall record that extra game with a date, a time, a location, the two teams, and 5 Referees.
- **FR-010:** The system shall show a Coach all official games.
- **FR-011:** The system shall hide the assigned Referees from a Coach.
- **FR-012:** The system shall show a Referee the date, time, location, and teams for each game that Referee will work.
- **FR-013:** The system shall show a Referee the games that Referee is not working, including the date and location.
- **FR-014:** The system shall show the Public only the next official game.

---

## Key Entities

| Entity       | Description                                                              |
| ------------ | ------------------------------------------------------------------------ |
| Official game | A game the Admin schedules by hand between two teams in a division.     |
| Referee crew | One main Referee and four sideline Referees, two on each side.          |
| Series       | The official games in which every team in a division plays every other team once. |
| Ranking game | The extra official game the Admin schedules when two teams have the same points. |

---

## Initial Data Model

### Official Game

| Attribute         | Type    | Description                                              |
| ----------------- | ------- | -------------------------------------------------------- |
| officialGameId    | Integer | Unique official game identifier                          |
| gameDate          | Date    | Date of the game                                         |
| gameTime          | Time    | Time of the game                                         |
| location          | String  | Location of the game                                     |
| teamOneId         | Integer | First team                                               |
| teamTwoId         | Integer | Second team                                              |
| divisionId        | Integer | Division the two teams belong to                         |
| lengthMinutes     | Integer | 90                                                       |
| mainRefereeId     | Integer | The one main Referee                                     |
| sidelineRefereeIds | String | Four Referees, two for each sideline                    |
| official          | Boolean | Yes                                                      |
| rankingGame       | Boolean | Yes only when scheduled because the two teams have the same points |
| countsInPoints    | Boolean | Yes for a series game. No for a ranking game.            |

**Relationships:**

- An official game belongs to one division and has two teams.
- An official game has one main Referee and four sideline Referees.
- In a series, each pair of teams in the division plays once.
- A ranking game is official and does not add points.
- A Coach can see the official game and cannot see the five Referees.

---

## Gherkin AC

### US-8.1: Schedule an Official Game

#### AC-8.1.1: Enter an official game by hand

**Given** two teams are in the same division  
**And** those two teams have not already played an official series game  
**When** the Admin enters the date, time, location, and the two teams  
**And** the Admin assigns 1 main Referee and 4 sideline Referees, 2 on each sideline  
**Then** the system saves the game as an official game  
**And** the game length is 90 minutes.

#### AC-8.1.2: A Coach game is not official

**Given** a Coach creates a game  
**When** the game is saved  
**Then** the game is not an official game.

#### AC-8.1.3: Each team plays each other team once in the series

**Given** a division has its teams  
**When** the Admin finishes the official series for that division  
**Then** every team has one official series game against every other team in that division.

### US-8.2: Assign the Referee Crew

#### AC-8.2.1: Hide Referees from a Coach

**Given** an official game has 5 Referees assigned  
**When** the Coach views that official game  
**Then** the Coach sees the teams  
**And** the Coach does not see the Referees.

### US-8.3: Schedule a Ranking Game

#### AC-8.3.1: Schedule an extra official game for a tie

**Given** two teams in a division have the same points after the series  
**When** the Admin schedules an extra game between those two teams  
**Then** the extra game is official  
**And** the extra game has a date, a time, a location, the two teams, and 5 Referees.

### US-8.4: See Assigned Games

#### AC-8.4.1: A Referee sees assigned and unassigned games

**Given** a Referee is assigned to one official game  
**And** another official game does not include that Referee  
**When** the Referee opens the Referee screen  
**Then** the Referee sees the date, time, location, and teams for the assigned game  
**And** the Referee sees the date and location for the game the Referee is not working.

#### AC-8.4.2: The Public sees only the next official game

**Given** more than one official game is upcoming  
**When** a member of the Public opens the public screen  
**Then** the public screen shows only the next official game.

---

## Screen Requirements

### SR-001: Admin Official Schedule Screen

**Authorized user:** Admin, after the correct pass code

**Display:**

- Official games
- Date, time, location, two teams, and division
- Five Referees
- Which games are ranking games

**Actions:**

- Enter an official game by hand
- Assign 1 main Referee and 4 sideline Referees
- Schedule one extra official game between two teams that have the same points

**Behavior:**

- An official game requires a date, time, location, two teams, and 5 Referees.
- The series schedules each pair of teams in the division one time.
- A Coach-created game cannot be marked official from this screen.
- When a division has no official game, the screen shows that no official game has been scheduled.

### SR-002: Coach Official Games Screen

**Authorized user:** Coach, after the correct pass code

**Display:**

- All official games
- The teams that play

**Actions:**

- View the official games
- Choose the 11 players for the Coach's team in an official game

**Behavior:**

- The screen does not show the Referees.

### SR-003: Referee Schedule Screen

**Authorized user:** Referee, after the correct pass code

**Display:**

- For games this Referee will work: date, time, location, and teams
- For games this Referee is not working: date and location

**Actions:**

- View those games
- Send a message to the Admin

**Behavior:**

- When the Referee has no assigned game, the screen shows that no game is assigned.
