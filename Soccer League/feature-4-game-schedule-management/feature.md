# Feature: Game Schedule Management

**Feature ID:** 4  
**Feature Name:** Game Schedule Management  
**Status:** Draft  
**Branch:** `soccer-league`  
**Created:** 2026-10-22

---

## User Stories

### US-4.1: Arrange a Game with Another Team

**As a** Coach  
**I want to** message the other team's Coach and agree on a date, time, and location  
**So that** our teams can play a game that is not an official league game.

### US-4.2: Schedule a Practice

**As a** Coach  
**I want to** create a practice for my own team with a date, time, and location  
**So that** the team can practice without an opposing team.

### US-4.3: Choose the Referee for a Coach Game

**As a** Coach  
**I want to** choose one Referee for a game I create  
**So that** someone is assigned to that game.

---

## Functional Requirements

- **FR-001:** The system shall allow a Coach to create a game against another team.
- **FR-002:** The system shall allow a Coach to create a practice for that Coach's own team.
- **FR-003:** The system shall keep a game created by a Coach as a non-official game.
- **FR-004:** The system shall record the date, time, location, and two teams for a Coach's game against another team.
- **FR-005:** The system shall save a game against another team only after both Coaches agree on the date, time, and location.
- **FR-006:** The system shall allow the two Coaches to send messages to each other inside the system to agree on that date, time, and location.
- **FR-007:** The system shall record the date, time, location, and the Coach's own team for a practice.
- **FR-008:** The system shall record a practice with no opposing team.
- **FR-009:** The system shall assign one Referee, chosen by the Coach, to a Coach's game.
- **FR-010:** The system shall not assign the five-Referee crew to a Coach's game.
- **FR-011:** The system shall show the Public every upcoming Coach game, including the date, time, location, and team or teams.
- **FR-012:** The system shall leave a Coach game out of the league ranking.

---

## Key Entities

| Entity             | Description                                                                 |
| ------------------ | --------------------------------------------------------------------------- |
| Coach game         | A non-official game created by a Coach. It is against another team or a practice. |
| Practice           | A Coach game for one team, with no opposing team.                           |
| Agreement          | Both Coaches' agreement on the date, time, and location before the game is saved. |
| Referee assignment | The one Referee the Coach chooses for that Coach game.                      |

---

## Initial Data Model

### Coach Game Against Another Team

| Attribute       | Type    | Description                                              |
| --------------- | ------- | -------------------------------------------------------- |
| coachGameId     | Integer | Unique Coach game identifier                             |
| gameDate        | Date    | Agreed date                                              |
| gameTime        | Time    | Agreed time                                              |
| location        | String  | Agreed location                                          |
| teamOneId       | Integer | First team                                               |
| teamTwoId       | Integer | Second team                                              |
| refereeId       | Integer | The one Referee chosen by the Coach                      |
| official        | Boolean | No                                                       |
| countsInRanking | Boolean | No                                                       |
| agreed          | Boolean | Yes only after both Coaches agree on date, time, and location |

### Practice

| Attribute       | Type    | Description                         |
| --------------- | ------- | ----------------------------------- |
| practiceId      | Integer | Unique practice identifier          |
| gameDate        | Date    | Practice date                       |
| gameTime        | Time    | Practice time                       |
| location        | String  | Practice location                   |
| teamId          | Integer | The Coach's own team                |
| refereeId       | Integer | The one Referee chosen by the Coach |
| official        | Boolean | No                                  |
| countsInRanking | Boolean | No                                  |

**Relationships:**

- A practice has one team and no opposing team.
- A game against another team has two teams and one Referee.
- That game is saved only after both Coaches agree on the date, time, and location.
- Neither game is official, and neither game counts in the league ranking.

---

## Gherkin AC

### US-4.1: Arrange a Game with Another Team

#### AC-4.1.1: Save a game after both Coaches agree

**Given** a Coach wants a game against another team  
**When** the two Coaches message each other inside the system  
**And** they agree on a date, a time, and a location  
**And** the Coach chooses one Referee  
**Then** the system saves the game  
**And** the game is not official  
**And** the game does not count in the league ranking.

#### AC-4.1.2: Hold the game until the other Coach agrees

**Given** a Coach has entered a date, a time, and a location  
**And** the other team's Coach has not agreed  
**When** the Coach tries to save the game  
**Then** the system does not save the game.

#### AC-4.1.3: Show upcoming Coach games to the Public

**Given** a Coach game is saved and is still upcoming  
**When** a member of the Public opens the public screen  
**Then** the public screen shows that game's date, time, location, and team or teams.

### US-4.2: Schedule a Practice

#### AC-4.2.1: Create a practice for the Coach's own team

**Given** a Coach is signed in  
**When** the Coach creates a practice for that Coach's own team  
**And** the Coach enters a date, a time, and a location  
**And** the Coach chooses one Referee  
**Then** the system saves the practice with no opposing team  
**And** the practice is not official  
**And** the practice does not count in the league ranking.

### US-4.3: Choose the Referee for a Coach Game

#### AC-4.3.1: Assign one Referee

**Given** a Coach is creating a Coach game  
**When** the Coach chooses one Referee  
**Then** the system assigns that one Referee to the game  
**And** the game does not receive a five-Referee crew.

---

## Screen Requirements

### SR-001: Coach Game Setup Screen

**Authorized user:** Coach, after the correct pass code

**Display:**

- The Coach's own team
- The other teams the Coach can play
- The date, time, and location being discussed
- The one Referee the Coach chooses

**Actions:**

- Start a game against another team
- Message the other Coach
- Save the game after both Coaches agree on the date, time, and location
- Create a practice for the Coach's own team
- Choose one Referee

**Behavior:**

- A game against another team is not saved until both Coaches agree on the date, time, and location.
- A practice has no opposing team.
- The game is marked not official.
- When the Coach has no upcoming Coach game, the screen shows that no Coach game is scheduled.

### SR-002: Public Upcoming Coach Games Screen

**Authorized user:** Public, with no pass code, after choosing Public

**Display:**

- Every upcoming Coach game
- Date, time, location, and team or teams

**Actions:**

- View the upcoming Coach games

**Behavior:**

- When no Coach game is upcoming, the screen shows that no Coach game is scheduled.
