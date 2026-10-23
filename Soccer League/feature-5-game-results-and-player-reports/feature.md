# Feature: Game Results and Player Reports

**Feature ID:** 5  
**Feature Name:** Game Results and Player Reports  
**Status:** Draft  
**Branch:** `soccer-league`  
**Created:** 2026-10-22

---

## User Stories

### US-5.1: Record an Official Result

**As an** Admin  
**I want to** enter the official result and each player's game details at the end of the game  
**So that** the division ranking can be updated.

### US-5.2: Record a Coach-Game Result

**As a** Referee  
**I want to** enter the score and each player's details at the end of a Coach's game  
**So that** the game has a record without changing the league ranking.

### US-5.3: Read the Ranking

**As a** Coach  
**I want to** see the result of each league  
**So that** I know how the teams finished.

### US-5.4: Read My Performance

**As a** Player  
**I want to** see my performance for each game  
**So that** I can review my goals, assists, passes, playtime, cards, and injuries.

---

## Functional Requirements

- **FR-001:** The system shall treat the team with the higher score as the winner of a 90-minute official game.
- **FR-002:** The system shall record an official game as a draw when both teams have the same score at 90 minutes.
- **FR-003:** The system shall accept any score. There is no maximum score.
- **FR-004:** The system shall award 2 points for a win, 1 point for a draw, and 0 points for a loss.
- **FR-005:** The system shall add points only from official series games, where each team plays every other team in the division once.
- **FR-006:** The system shall rank teams in a division by the total of those points.
- **FR-007:** The system shall add no win, draw, or loss points for a ranking game.
- **FR-008:** The system shall rank the winner of a ranking game higher than the other team.
- **FR-009:** The system shall keep an equal rank when a ranking game is a draw.
- **FR-010:** The system shall allow a score for each team in a Coach game against another team, and that score shall not count in the ranking or the league.
- **FR-011:** The system shall record one score for a practice, the number of goals for that team, and that score shall not count in the ranking or the league.
- **FR-012:** The system shall have the Admin enter the official result at the end of the game.
- **FR-013:** The system shall have the Admin enter, for an official game, each player's goals, assists, passes, playtime, yellow card, red card, whether the player was injured, and whether the player injured someone else.
- **FR-014:** The system shall have the Referee enter, at the end of a Coach's game, the score and those same player details.
- **FR-015:** The system shall keep a saved result unchanged.
- **FR-016:** The system shall show a Player their own performance for each game: whether they scored, number of goals, assists, passes, playtime, yellow card, red card, whether they were injured, and whether they injured someone else.
- **FR-017:** The system shall show a Player those same details for each game in the last league.
- **FR-018:** The system shall show a Coach the result of each league and the performance data for every player on that Coach's team.

---

## Key Entities

| Entity             | Description                                                                 |
| ------------------ | --------------------------------------------------------------------------- |
| Result             | The score of a game. Official series results add points. Ranking games, Coach games, and practices do not. |
| Standing           | The point total and rank of a team inside its division.                     |
| Player performance | The details recorded for one player in one game.                            |
| Player report      | A Player's own performance, shown for each game and for each game in the last league. |

---

## Initial Data Model

### Official Series Result

| Attribute | Type    | Description                                      |
| --------- | ------- | ------------------------------------------------ |
| resultId  | Integer | Unique result identifier                         |
| gameId    | Integer | Official series game                             |
| teamId    | Integer | One of the two teams                             |
| goals     | Integer | Goals for that team. No maximum.                 |
| outcome   | String  | Win for the higher score, or draw when equal     |
| points    | Integer | 2 for a win, 1 for a draw, 0 for a loss          |
| saved     | Boolean | Yes after save. The result cannot be changed.    |

### Ranking-Game Result

| Attribute  | Type    | Description                                              |
| ---------- | ------- | -------------------------------------------------------- |
| resultId   | Integer | Unique result identifier                                 |
| gameId     | Integer | Extra official game                                      |
| teamId     | Integer | One of the two teams                                     |
| goals      | Integer | Goals for that team                                      |
| rankEffect | String  | Winner ranks higher. A draw leaves an equal rank.        |
| points     | Integer | 0. No points are added.                                  |

### Coach-Game Result

| Attribute   | Type    | Description                                              |
| ----------- | ------- | -------------------------------------------------------- |
| resultId    | Integer | Unique result identifier                                 |
| coachGameId | Integer | Coach game or practice                                   |
| teamId      | Integer | Team the score belongs to                                |
| goals       | Integer | Goals. A practice stores one number for that team.       |
| enteredBy   | Integer | Referee who entered the score at the end of the game     |
| countsInRanking | Boolean | No                                                |

### Player Performance

| Attribute          | Type    | Description                                |
| ------------------ | ------- | ------------------------------------------ |
| performanceId      | Integer | Unique performance identifier              |
| playerId           | Integer | Player                                     |
| gameId             | Integer | Game                                       |
| scored             | Boolean | Whether the player scored                  |
| goals              | Integer | Number of goals                            |
| assists            | Integer | Number of assists                          |
| passes             | Integer | Number of passes                           |
| playtime           | String  | Player's playtime in the game              |
| yellowCard         | Boolean | Whether the player received a yellow card  |
| redCard            | Boolean | Whether the player received a red card     |
| injured            | Boolean | Whether the player was injured             |
| injuredSomeoneElse | Boolean | Whether the player injured someone else    |

**Relationships:**

- Official series results add points to the division standing.
- A ranking-game result changes rank and adds no points.
- A Coach-game result and a practice result stay out of the ranking and the league.
- A player performance belongs to one player and one game.

---

## Gherkin AC

### US-5.1: Record an Official Result

#### AC-5.1.1: A higher score wins an official game

**Given** an official series game has ended after 90 minutes  
**And** the score is 3 to 1  
**When** the Admin saves the result  
**Then** the team with 3 goals wins  
**And** that team gains 2 points  
**And** the other team gains 0 points.

#### AC-5.1.2: An equal official score is a draw

**Given** an official series game has ended after 90 minutes  
**And** the score is 2 to 2  
**When** the Admin saves the result  
**Then** the game is a draw  
**And** each team gains 1 point.

#### AC-5.1.3: A saved official result stays unchanged

**Given** the Admin has saved an official result  
**When** the Admin tries to change that result  
**Then** the saved result stays as it was entered.

#### AC-5.1.4: A ranking game changes rank without points

**Given** two teams have the same points  
**And** the Admin has scheduled a ranking game  
**When** the ranking game ends 1 to 0  
**And** the Admin saves the result  
**Then** the winning team ranks higher  
**And** neither team gains points from that game.

#### AC-5.1.5: A drawn ranking game leaves the rank equal

**Given** a ranking game ends with both teams on the same score  
**When** the Admin saves the result  
**Then** the two teams keep an equal rank  
**And** neither team gains points from that game.

#### AC-5.1.6: Any score can be saved

**Given** an official game has ended  
**When** the Admin saves a score of 0, 8, or 15 for a team  
**Then** the system saves the result.

### US-5.2: Record a Coach-Game Result

#### AC-5.2.1: A Coach game score stays out of the ranking

**Given** a Referee enters a score at the end of a Coach's game  
**When** the score is saved  
**Then** the system keeps the score  
**And** the league points stay unchanged.

#### AC-5.2.2: A practice score is one number

**Given** a practice has no opposing team  
**When** the Referee saves a score of 4 for that team  
**Then** the practice record shows 4 goals  
**And** the league points stay unchanged.

### US-5.3: Read the Ranking

#### AC-5.3.1: A Coach sees each league result

**Given** league results have been saved  
**When** the Coach opens the league results  
**Then** the system shows the result of each league  
**And** the system shows the performance data for every player on that Coach's team.

### US-5.4: Read My Performance

#### AC-5.4.1: A Player sees performance for each league game

**Given** a Player's goals, assists, passes, playtime, cards, and injuries were saved for a league game  
**When** the Player opens the last league's data  
**Then** the Player sees those details for that game.

---

## Screen Requirements

### SR-001: Admin Result Entry Screen

**Authorized user:** Admin, after the correct pass code

**Display:**

- Official game and the two teams
- Score
- Each player's goals, assists, passes, playtime, yellow card, red card, injury, and whether the player injured someone else
- Division points and rank

**Actions:**

- Enter the official result and the player details at the end of the game

**Behavior:**

- A saved result cannot be changed.
- Series games add 2, 1, or 0 points.
- A ranking game adds no points.
- There is no maximum score.
- When an official game has no saved result, the screen shows that the result has not been entered.

### SR-002: Referee Coach-Game Report Screen

**Authorized user:** Referee chosen for that Coach game, after the correct pass code

**Display:**

- Coach game
- Team or teams
- Score
- Player details

**Actions:**

- At the end of the game, enter the score and each player's goals, assists, passes, playtime, yellow card, red card, injury, and whether the player injured someone else

**Behavior:**

- The score does not change league points.
- A practice accepts one number of goals for that team.

### SR-003: Coach League Results Screen

**Authorized user:** Coach, after the correct pass code

**Display:**

- Result of each league
- Performance data for every player on that Coach's team

**Actions:**

- View the league results and the team's player data

**Behavior:**

- When a league has no saved result, the screen shows that no result has been entered.

### SR-004: Player Performance Screen

**Authorized user:** Player, after the correct pass code

**Display:**

- The Player's own performance for each game
- Those details for each game in the last league

**Actions:**

- View that performance

**Behavior:**

- The Player does not edit the performance.
- When the Player has no saved performance, the screen shows that no performance has been recorded.
