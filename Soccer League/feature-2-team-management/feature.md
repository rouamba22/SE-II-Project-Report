# Feature: Team Management

**Feature ID:** 2  
**Feature Name:** Team Management  
**Status:** Draft  
**Branch:** `soccer-league`  
**Created:** 2026-10-22

---

## User Stories

### US-2.1: Create a Team

**As an** Admin  
**I want to** create a team with a name and one Coach  
**So that** the team can be placed in a division.

### US-2.2: Edit a Team

**As an** Admin  
**I want to** edit a team's name and Coach  
**So that** the team record stays correct.

### US-2.3: Keep the Team Record with the Admin

**As an** Admin  
**I want to** let the Coach view the team without changing it  
**So that** the team name and membership stay under the Admin.

---

## Functional Requirements

- **FR-001:** The system shall allow the Admin to create a team.
- **FR-002:** The system shall require a team name and exactly one Coach for each team.
- **FR-003:** The system shall allow the Admin to edit the team name.
- **FR-004:** The system shall allow the Admin to change the Coach assigned to a team.
- **FR-005:** The system shall place each team in one division.
- **FR-006:** The system shall prevent the Admin from adding a team to a division that has reached its team limit.
- **FR-007:** The system shall prevent a Coach from editing the team name, changing the Coach, or otherwise changing the team.
- **FR-008:** The system shall prevent a Player, Referee, or member of the Public from creating or editing a team.

---

## Key Entities

| Entity   | Description                                              |
| -------- | -------------------------------------------------------- |
| Team     | A named team with one Coach, placed in one division.     |
| Coach    | The one person assigned to that team.                    |
| Division | The division that holds the team, up to its team limit.  |

---

## Initial Data Model

### Team

| Attribute  | Type    | Description                          |
| ---------- | ------- | ------------------------------------ |
| teamId     | Integer | Unique team identifier               |
| teamName   | String  | Name of the team                     |
| coachId    | Integer | The one Coach assigned to the team   |
| divisionId | Integer | Division the team belongs to         |

**Relationships:**

- A team belongs to one division.
- A division contains teams only up to its team limit.
- A team has exactly one Coach.
- The Coach can view the team and cannot edit the team record.

---

## Gherkin AC

### US-2.1: Create a Team

#### AC-2.1.1: Create a team in a division

**Given** a division has not reached its team limit  
**When** the Admin creates a team with a team name and one Coach in that division  
**Then** the system saves the team in that division  
**And** the team shows that team name and that Coach.

#### AC-2.1.2: Refuse a team when the division is full

**Given** a division has reached its team limit  
**When** the Admin tries to create another team in that division  
**Then** the system does not save the team.

### US-2.2: Edit a Team

#### AC-2.2.1: Change the team name

**Given** a team already exists  
**When** the Admin changes the team name  
**Then** the system shows the new team name.

### US-2.3: Keep the Team Record with the Admin

#### AC-2.3.1: A Coach cannot change the team

**Given** a Coach is signed in  
**And** the Coach is assigned to a team  
**When** the Coach tries to edit the team name or the Coach  
**Then** the team record stays unchanged.

---

## Screen Requirements

### SR-001: Admin Teams Screen

**Authorized user:** Admin, after the correct pass code

**Display:**

- Team name
- Coach for each team
- Division each team belongs to

**Actions:**

- Create a team
- Edit the team name
- Change the Coach assigned to a team
- Place the team in a division that still has room

**Behavior:**

- A team requires a team name and one Coach.
- A team cannot be added when the division is at its team limit.
- When a division has no team, the screen shows that the division has no team yet.

### SR-002: Coach Team View Screen

**Authorized user:** Coach of that team, after the correct pass code

**Display:**

- The Coach's own team name
- The team information the Coach is allowed to see

**Actions:**

- View the team

**Behavior:**

- The Coach has no action that edits or changes the team.
- An attempt to edit the team name or the Coach does not change the team.
