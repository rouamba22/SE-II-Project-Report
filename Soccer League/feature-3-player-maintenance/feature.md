# Feature: Player Maintenance

**Feature ID:** 3  
**Feature Name:** Player Maintenance  
**Status:** Draft  
**Branch:** `soccer-league`  
**Created:** 2026-10-22

---

## User Stories

### US-3.1: Add a Player

**As an** Admin  
**I want to** add a player to a team with a name and a jersey number  
**So that** the team has a roster.

### US-3.2: Remove a Player

**As an** Admin  
**I want to** remove a player from a team  
**So that** the roster stays current.

### US-3.3: Choose the Official Lineup

**As a** Coach  
**I want to** choose 11 players from my team for an official game  
**So that** those players are the ones who play that game.

---

## Functional Requirements

- **FR-001:** The system shall allow the Admin to add a player to a team.
- **FR-002:** The system shall allow the Admin to remove a player from a team.
- **FR-003:** The system shall record a name and a jersey number for each player.
- **FR-004:** The system shall not record a shirt color for a player.
- **FR-005:** The system shall allow a player to belong to only one team.
- **FR-006:** The system shall keep each jersey number unique on a team.
- **FR-007:** The system shall keep a team roster between 11 and 22 players.
- **FR-008:** The system shall prevent the Admin from saving a roster with fewer than 11 players or more than 22 players.
- **FR-009:** The system shall prevent a Coach from adding a player, removing a player, or otherwise editing the roster.
- **FR-010:** The system shall allow the Coach of a team to choose exactly 11 players from that roster for each official game.
- **FR-011:** The system shall show a chosen player that they were chosen for the next official game.
- **FR-012:** The system shall show a player who was not chosen that they were not chosen for the next official game.

---

## Key Entities

| Entity          | Description                                                         |
| --------------- | ------------------------------------------------------------------- |
| Player          | A person with a name and a jersey number, belonging to one team.    |
| Roster          | The players on one team. The roster holds 11 to 22 players.         |
| Official lineup | The 11 players the Coach chooses from the roster for one official game. |

---

## Initial Data Model

### Player

| Attribute    | Type    | Description                                      |
| ------------ | ------- | ------------------------------------------------ |
| playerId     | Integer | Unique player identifier                         |
| name         | String  | Player name                                      |
| jerseyNumber | Integer | Jersey number, unique on the same team           |
| teamId       | Integer | The one team the player belongs to               |

### Official Lineup

| Attribute      | Type    | Description                                      |
| -------------- | ------- | ------------------------------------------------ |
| lineupId       | Integer | Unique lineup identifier                         |
| officialGameId | Integer | Official game this lineup is for                 |
| teamId         | Integer | Team the lineup belongs to                       |
| playerId       | Integer | One of the 11 players chosen from that roster    |

**Relationships:**

- A player belongs to only one team.
- A team has at least 11 players and at most 22 players.
- Two players on the same team cannot share a jersey number.
- A player record does not include a shirt color.
- An official lineup contains exactly 11 players from one team's roster.

---

## Gherkin AC

### US-3.1: Add a Player

#### AC-3.1.1: Add a player to a team

**Given** a team has fewer than 22 players  
**And** no player on that team already has the entered jersey number  
**When** the Admin adds a player with a name and that jersey number to that team  
**Then** the system places the player on that team  
**And** the player is not on any other team.

#### AC-3.1.2: Refuse a duplicate jersey number

**Given** a team already has a player with jersey number 7  
**When** the Admin tries to add another player with jersey number 7 to that team  
**Then** the system does not save the new player.

#### AC-3.1.3: Refuse a second team for the same player

**Given** a player already belongs to a team  
**When** the Admin tries to add that same player to another team  
**Then** the player remains on the original team only.

#### AC-3.1.4: Refuse a roster above 22 players

**Given** a team already has 22 players  
**When** the Admin tries to add another player  
**Then** the system does not save the player.

### US-3.2: Remove a Player

#### AC-3.2.1: Keep at least 11 players on a roster

**Given** a team has 11 players  
**When** the Admin tries to remove a player  
**Then** the player stays on the team.

### US-3.3: Choose the Official Lineup

#### AC-3.3.1: Choose 11 players for an official game

**Given** a Coach's team has at least 11 players  
**And** an official game is scheduled for that team  
**When** the Coach chooses 11 players from that roster for the official game  
**Then** those 11 players are the lineup for that team in that game  
**And** each chosen player can see that they were chosen for the next official game.

#### AC-3.3.2: A Coach cannot edit the roster

**Given** a Coach is signed in  
**When** the Coach tries to add or remove a player  
**Then** the roster stays unchanged.

---

## Screen Requirements

### SR-001: Admin Roster Screen

**Authorized user:** Admin, after the correct pass code

**Display:**

- Players on a team
- Each player's name
- Each player's jersey number

**Actions:**

- Add a player to a team
- Remove a player from a team

**Behavior:**

- A player requires a name and a jersey number.
- The jersey number must be unused on that team.
- The roster stays from 11 to 22 players.
- A player can be on only one team.
- The screen does not ask for a shirt color.

### SR-002: Coach Lineup Screen

**Authorized user:** Coach of the team, after the correct pass code

**Display:**

- The Coach's own roster, with name and jersey number
- The official game the lineup is for

**Actions:**

- Choose exactly 11 players for that official game

**Behavior:**

- The Coach cannot add or remove players.
- The choice must be exactly 11 players from that roster.

### SR-003: Player Selection Notice Screen

**Authorized user:** Player, after the correct pass code

**Display:**

- Whether that player was chosen for the next official game

**Actions:**

- View the selection

**Behavior:**

- The Player does not change the lineup.
