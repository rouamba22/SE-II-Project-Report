# Feature: Administration

**Feature ID:** 6  
**Feature Name:** Administration  
**Status:** Draft  
**Branch:** `soccer-league`  
**Created:** 2026-10-22

---

## User Stories

### US-6.1: Choose a Role

**As a** person opening the system  
**I want to** choose Admin, Coach, Player, Referee, or Public  
**So that** I reach the correct area.

### US-6.2: Sign In with a Pass Code

**As an** Admin, Coach, Player, or Referee  
**I want to** enter my personal 4-digit pass code  
**So that** I reach my own role screen.

### US-6.3: Issue Pass Codes

**As an** Admin  
**I want to** create one pass code for each person  
**So that** each person has one role and one way to sign in.

### US-6.4: Send a Message

**As a** signed-in person  
**I want to** send a message inside the system to someone I am allowed to contact  
**So that** we can communicate without leaving the system.

---

## Functional Requirements

- **FR-001:** The system shall offer Admin, Coach, Player, Referee, and Public on the opening screen.
- **FR-002:** The system shall open the public information screen with no pass code when Public is chosen.
- **FR-003:** The system shall ask for a 4-digit pass code when Admin, Coach, Player, or Referee is chosen.
- **FR-004:** The system shall give each person exactly one role and exactly one pass code.
- **FR-005:** The system shall allow only the Admin to create each pass code.
- **FR-006:** The system shall tie a pass code to one person and to that person's role.
- **FR-007:** The system shall open the screen for that person's role when the pass code is correct.
- **FR-008:** The system shall say that the code does not correspond and ask for the code again when the pass code does not match the chosen role.
- **FR-009:** The system shall keep the role screen closed and tell the person to contact the Admin after 3 incorrect codes.
- **FR-010:** The system shall allow the Admin to send a message to anyone.
- **FR-011:** The system shall allow a Coach to send a message to anyone except a Referee.
- **FR-012:** The system shall allow a Referee to send a message to the Admin.
- **FR-013:** The system shall allow a Player to send a message only to the Coach of that Player's own team.
- **FR-014:** The system shall not ask the Public for a pass code and shall not let the Public send messages.
- **FR-015:** The system shall give each role its own screen after a correct pass code, limited to what that role can do and see.

---

## Key Entities

| Entity      | Description                                                                 |
| ----------- | --------------------------------------------------------------------------- |
| Person      | Someone with one role and, except for the Public, one pass code.            |
| Pass code   | A personal 4-digit code created by the Admin.                               |
| Message     | A message sent inside the system to a person the sender is allowed to contact. |
| Role screen | The screen opened for Admin, Coach, Player, or Referee after the correct pass code. |

---

## Initial Data Model

### Person

| Attribute | Type    | Description                                              |
| --------- | ------- | -------------------------------------------------------- |
| personId  | Integer | Unique person identifier                                 |
| role      | String  | Admin, Coach, Player, or Referee                         |
| passCode  | String  | One 4-digit code created by the Admin                    |
| teamId    | Integer | Present for a Coach and for a Player                     |

### Message

| Attribute  | Type    | Description                          |
| ---------- | ------- | ------------------------------------ |
| messageId  | Integer | Unique message identifier            |
| fromPerson | Integer | Person who sends the message         |
| toPerson   | Integer | Person who receives the message      |
| message    | String  | Message text sent inside the system  |

**Relationships:**

- Each person has one role and one pass code.
- A pass code matches only that person's role.
- The Public has no pass code.
- A Player has one team. A Coach is the one Coach of a team.
- An Admin may message anyone.
- A Coach may message anyone except a Referee.
- A Referee may message the Admin.
- A Player may message only the Coach of that Player's own team.

---

## Gherkin AC

### US-6.1: Choose a Role

#### AC-6.1.1: Public enters with no code

**Given** a person opens the system  
**When** the person chooses Public  
**Then** the public information screen opens  
**And** the system does not ask for a pass code.

### US-6.2: Sign In with a Pass Code

#### AC-6.2.1: A correct code opens the matching role screen

**Given** a Coach has pass code "2468"  
**When** the Coach chooses Coach  
**And** the Coach enters "2468"  
**Then** the Coach screen opens.

#### AC-6.2.2: A code for the wrong role does not correspond

**Given** a Player has pass code "1357"  
**When** the person chooses Coach  
**And** the person enters "1357"  
**Then** the system says the code does not correspond  
**And** the system asks for the code again  
**And** the Coach screen does not open.

#### AC-6.2.3: Three incorrect codes require the Admin

**Given** a person has chosen Player  
**When** the person enters an incorrect code 3 times  
**Then** the system does not open the Player screen  
**And** the system tells the person to contact the Admin.

### US-6.3: Issue Pass Codes

#### AC-6.3.1: The Admin creates one pass code

**Given** a person has one role  
**When** the Admin creates a 4-digit pass code for that person  
**Then** the system saves that one pass code for that person  
**And** the pass code matches that person's role.

### US-6.4: Send a Message

#### AC-6.4.1: A Player messages only the team Coach

**Given** a Player is signed in  
**When** the Player sends a message to the Coach of that Player's own team  
**Then** the Coach receives the message inside the system.

#### AC-6.4.2: A Player cannot message a Referee

**Given** a Player is signed in  
**When** the Player tries to send a message to a Referee  
**Then** the system does not send the message.

#### AC-6.4.3: A Coach cannot message a Referee

**Given** a Coach is signed in  
**When** the Coach tries to send a message to a Referee  
**Then** the system does not send the message.

#### AC-6.4.4: A Referee messages the Admin

**Given** a Referee is signed in  
**When** the Referee sends a message to the Admin  
**Then** the Admin receives the message inside the system.

---

## Screen Requirements

### SR-001: Opening Choices Screen

**Authorized user:** Anyone who opens the system

**Display:**

- Admin
- Coach
- Player
- Referee
- Public

**Actions:**

- Choose one role

**Behavior:**

- Choosing Public opens the public screen and does not ask for a code.
- Choosing any other role opens the pass-code entry.
- The five choices are shown even when no league data exists yet.

### SR-002: Pass Code Screen

**Authorized user:** A person who chose Admin, Coach, Player, or Referee

**Display:**

- A request for the 4-digit pass code

**Actions:**

- Enter the pass code

**Behavior:**

- A matching code opens that role's screen.
- A code that does not correspond shows that message and asks for the code again.
- After 3 incorrect codes, the role screen stays closed and the screen tells the person to contact the Admin.

### SR-003: Admin Home Screen

**Authorized user:** Admin, after the correct pass code

**Display:**

- Leagues, games, Coaches, teams, players, Referees, and player data
- A way to message anyone

**Actions:**

- Create pass codes
- Open the Admin areas for leagues, teams, players, official games, and results
- Send a message to anyone

**Behavior:**

- When no person has a pass code yet, the screen shows that no pass code has been created.

### SR-004: Messages Screen

**Authorized users:** Signed-in Admin, Coach, Player, or Referee

**Display:**

- The people that role is allowed to message
- The messages sent inside the system

**Actions:**

- Send a message to an allowed person

**Behavior:**

- A Coach cannot select a Referee.
- A Player can select only the Coach of that Player's own team.
- A Referee can select the Admin.
- The Public has no message action.
