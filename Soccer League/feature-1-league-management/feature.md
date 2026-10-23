# Feature: League Management

**Feature ID:** 1  
**Feature Name:** League Management  
**Status:** Draft  
**Branch:** `soccer-league`  
**Created:** 2026-10-22

---

## User Stories

### US-1.1: Create a Season League

**As an** Admin  
**I want to** create a league for one season in a year  
**So that** the teams for that part of the year have a league to play in.

### US-1.2: Name the Divisions

**As an** Admin  
**I want to** name the four divisions in a league  
**So that** each division can be told apart.

### US-1.3: Set the Size of a Division

**As an** Admin  
**I want to** set how many teams a division can hold and change that number later  
**So that** the division matches the number of teams needed.

---

## Functional Requirements

- **FR-001:** The system shall allow the Admin to create a league for a year.
- **FR-002:** The system shall name a league by its season: Spring, Summer, Fall, or Winter.
- **FR-003:** The system shall allow at most one league for each season in the same year.
- **FR-004:** The system shall allow at most 4 leagues in one year.
- **FR-005:** The system shall give each league exactly 4 divisions.
- **FR-006:** The system shall allow the Admin to name each division.
- **FR-007:** The system shall allow the Admin to set the number of teams allowed in a division.
- **FR-008:** The system shall allow the Admin to change the number of teams allowed in a division.
- **FR-009:** The system shall record a date, a time, and a location for each league.
- **FR-010:** The system shall prevent a Coach, Player, Referee, or member of the Public from creating a league, naming a division, or changing a division team limit.

---

## Key Entities

| Entity   | Description                                                                 |
| -------- | --------------------------------------------------------------------------- |
| League   | One season in one year. The season is the league name.                      |
| Division | A named group inside one league. The Admin sets how many teams it can hold. |
| Year     | The calendar year that holds the season leagues. A year has at most 4 leagues. |

---

## Initial Data Model

### League

| Attribute  | Type    | Description                                      |
| ---------- | ------- | ------------------------------------------------ |
| leagueId   | Integer | Unique league identifier                         |
| year       | Integer | Calendar year of the league                      |
| seasonName | String  | Spring, Summer, Fall, or Winter. This is the name. |
| leagueDate | Date    | Date of the league                               |
| leagueTime | Time    | Time of the league                               |
| location   | String  | Location of the league                           |

### Division

| Attribute  | Type    | Description                                      |
| ---------- | ------- | ------------------------------------------------ |
| divisionId | Integer | Unique division identifier                       |
| leagueId   | Integer | League this division belongs to                  |
| name       | String  | Name given by the Admin                          |
| teamLimit  | Integer | Number of teams the Admin allows in this division |

**Relationships:**

- A year has at most one league for each season name.
- A year has at most 4 leagues.
- A league has exactly 4 divisions.
- A division belongs to one league.

---

## Gherkin AC

### US-1.1: Create a Season League

#### AC-1.1.1: Create a Spring league

**Given** the year does not already have a Spring league  
**And** the year has fewer than 4 leagues  
**When** the Admin creates a league named Spring for that year  
**And** the Admin enters the league date, time, and location  
**And** the Admin names the 4 divisions  
**And** the Admin sets a team limit for each division  
**Then** the system saves the Spring league  
**And** the league has 4 named divisions.

#### AC-1.1.2: Refuse a fifth league in the same year

**Given** the year already has a Spring, Summer, Fall, and Winter league  
**When** the Admin tries to create another league in that year  
**Then** the system does not save the league.

#### AC-1.1.3: Refuse a second league for the same season

**Given** the year already has a Fall league  
**When** the Admin tries to create another Fall league in that year  
**Then** the system does not save the league.

#### AC-1.1.4: A Coach cannot create a league

**Given** a Coach is signed in  
**When** the Coach tries to create a league  
**Then** the system does not save the league.

### US-1.2: Name the Divisions

#### AC-1.2.1: Save four division names

**Given** the Admin is creating a league  
**When** the Admin enters a name for each of the 4 divisions  
**Then** the system saves those 4 division names with the league.

### US-1.3: Set the Size of a Division

#### AC-1.3.1: Change a division team limit

**Given** a division has a team limit  
**When** the Admin changes that team limit  
**Then** the system shows the new team limit for that division.

---

## Screen Requirements

### SR-001: Admin League Setup Screen

**Authorized user:** Admin, after the correct pass code

**Display:**

- Leagues for the year
- Season name of each league
- League date, time, and location
- Four divisions, each division name, and each team limit

**Actions:**

- Create a league for Spring, Summer, Fall, or Winter
- Enter the league date, time, and location
- Name each of the 4 divisions
- Set or change the team limit for a division

**Behavior:**

- The system refuses a fifth league in the same year.
- The system refuses a second league with the same season name in the same year.
- The season name is Spring, Summer, Fall, or Winter.
- When the year has no league, the screen shows that no league has been created yet.
