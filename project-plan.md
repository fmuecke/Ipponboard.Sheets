Domain Language: see https://github.com/fmuecke/Ipponboard/blob/vNEXT/docs/domain-language.md
## 1. Scope and Vision

1. What is the exact purpose of the new system?

   * It should be a list management system for individual competition. Ipponboard should be the "display" and control unit on the individual tatami.
   * What problem(s) it should solves:
    - Managing athletes: names, clubs(nationality), weight, category
    - Managing draws/distributing athletes to the draw sheets
      - initial draw sheet
      - tracking during competition (wins, losses, points/penalties, contest duration)
    - Providing a (REST) service for Ipponboard:
      - To determine the next contest
      - To set the score for a individual contest
    - Supports:
      - More than 1 tatami

2. What key components should the system ultimately include?

   * List service
   * Athlete management (import/export) for the purpose of the competition 
   * Administration interface
   * Display interface (for the lists, not the individual contests --> Ipponboard)
   * (later) Possibly scale / weighing
   * draw
   * results service / export

3. What should Ipponboard continue to handle itself in the future, and what should be moved to the list service?

   * Display --> yes
   * Match control and results entry? --> yes
   * Announcing the next match? --> potentially
   * Will time control / scoreboard remain with Ipponboard? --> yes

4. Is the goal:

   * a single local system for small tournaments,
   * a network system in a hall,
   * or, in the long term, a distributed system with multiple clients and mats?
  --> start small. But I definitely need to manage multiple mats at an early point in time.

5. Which problem do you want to solve first—the one that’s causing the most pain today?

---

## 2. Competition Scope

6. For which competition types must Version 1 work?

   * Individual championship
   * Team
   * Pool
   * Double-elimination
   * Single-elimination with consolation round
   * League / Round-robin
   * Open fun tournaments / Special formats

7. What is the smallest practical initial scope?

   * e.g., only singles, only pools and double-elimination, no teams

8. Do tournament formats need to be fully compliant with DJB/IJF rules, or is “practically usable for club tournaments” sufficient for now?
   This question is important because it significantly affects the level of complexity.

9. Should the system support different sets of rules as configurable options?

   * DJB
   * IJF
   * Regional Association
   * Organizer’s house rules

10. Should age groups, weight classes, match times, and placement rules be configurable, or should they be fixed according to a specific set of rules?

---

## 3. Core Business Objects

11. What is the top-level business object for you?

* Event
* Tournament
* Championship
* Competition Day
* Division / Competition / Class

12. How do you structure a competition from a business perspective?
    Example:

* Event
* Competition per age group/weight class/gender
* Within that: lists / pools / brackets
* Within that: matches

13. Should an athlete be able to participate in multiple competitions within a single event?

* Usually no
* Perhaps yes for Open Class / Team / Special Categories

14. How do you model teams?

* As a separate object distinct from athletes?
* With a lineup per match?
* With a starting team and substitutes?

15. Do you also need clubs, state associations, nations, coaches, and referees as actual domain objects?


---

---

## 4. Roles and User Groups

16. Who will be using the system?

* List Manager
* Tournament Director
* Mat Staff
* Referees
* Announcer
* Coach / Club
* Spectators

17. What permissions do each role have?

* Who can edit athletes?
* Who can start the draw?
* Who can correct results?
* Who can move matches to different mats?

18. Should there be a rights and role system, or is “all operators can do everything” sufficient for now?

19. Do you need separate views for:

* Central tournament management
* Mat operations
* View-only
* Mobile access / preparation

---

## 5. Pre-tournament workflow

20. How do athletes enter the system?

* Manual entry
* CSV/Excel import
* Import from external tournament software
* Online registration
* Club-based registration

21. What is the minimum athlete metadata you need?

* Last name
* First name
* Gender
* Year of birth / Date of birth
* Club
* Country / National federation
* Weight
* Eligibility to compete
* Bib number / ID

22. Does the system need to support weigh-ins?

* Enter weight only
* Official weigh-in with check-in
* Log / weigh-in list
* Verification of tolerances / minimum weights

23. Should the system automatically check:

* Age group
* Weight class
* Eligibility to compete
* Double entries
* Missing information

24. When is a list generated?

* Automatically after the registration deadline
* Only after weigh-in
* Manually by the operator

25. Should there be seeding rules?

* By ranking
* By club
* Random
* Mixed formats
---

## 6. Tournament Procedure

26. How is it decided which match goes next?

* Strictly according to the order on the list
* Centrally by tournament officials
* Automatically based on availability and break times
* Manually per mat

27. Should the system actively manage mat assignments?

* i.e., assign matches to mats, reassign them, prioritize them

28. Can matches in a single class be distributed across multiple mats?

29. Can classes switch between mats during the tournament?

30. Must the system observe minimum rest periods between an athlete’s matches?

31. What should happen if an athlete does not show up?

* Single call
* Multiple calls
* Manual entry of Fusen-gachi / No-show
* Time window / Grace period logic

32. Who records the match result?

* Directly at the mat
* Centrally by tournament management
* Reported back from the Ipponboard

33. Which result details must be recorded?

* Only winner/loser
* Type of victory
* Scores
* Penalties
* Match duration
* Golden Score
* Hansoku-make / Kiken-gachi / Fusen-gachi
* Medical stoppage

34. Must it be possible to correct results retroactively?
    If so: with logging?

35. Should the system automatically calculate subsequent matches and rankings?

---

## 7. Communication with Ipponboard

36. Who takes the lead?

* The list service tells Ipponboard which match is next
* Or Ipponboard actively requests the next match




37. Should communication be:

* Pull-based: Ipponboard checks regularly
* Push-based: List service reports changes
* Both

38. What is the minimum information Ipponboard needs per match?

* Names
* Clubs
* Colors / First called / Second called
* Weight class
* Round
* Match number
* Remaining status of the competition
* Mat number

39. Does Ipponboard need to write back results?

* Winner
* Complete scores
* Time
* Penalties
* Match status

40. Should Ipponboard be allowed to start / stop / end a match itself, or should it only display and send results?

41. Do you need a unique match ID that is consistent across systems?

42. Does the API need to be offline-robust, i.e., with retries, idempotent requests, and state synchronization?

---

## 8. Display and Operator UX

43. What displays do you need besides the actual scoreboard?

* Call monitor
* Mat layout
* Class progress
* Results lists
* Rankings
* Waiting area / On-deck / In-hold

44. Should there be a screen showing “now,” “next,” and “after” for each mat?

45. Do athletes or coaches need to be able to see where they currently stand in the tournament?

46. Should the administration interface be tabular or visual with brackets/pools?

47. Does the interface need to update in real time when multiple clients are working simultaneously?


## 9. Special Cases and Corrections

48. What must be possible during live operation?

* Withdraw an athlete
* Replace an athlete
* Move an athlete to a different weight class
* Regenerate a pool/bracket
* Annul a result
* Repeat a match
* Merge weight classes

49. Must the system be able to handle late athletes?

50. Must it be possible to merge or split pools if participants are missing after weigh-in?

51. Should the system actively prevent rule violations or merely issue warnings?

52. How important is traceability?

* Change log
* Who changed what and when
* Revision history

---

## 10. Output, Export, Documents

53. What outputs do you need?

* Weigh-in lists
* Lists / Pools / Brackets
* Match sheets
* Mat lists
* Results lists
* Certificate/award ceremony data
* Export for federations

54. In what format?

* PDF
* Print preview
* CSV/Excel
* JSON via API

55. Do you need interfaces to existing programs or only to Ipponboard?

---

## 11. Technical Requirements

56. Where should the list service run?

* Locally on a laptop
* On the gym network
* On a mini-PC/Raspberry Pi
* Server on the network
* Cloud later

57. Which clients do you want to support?

* Windows
* macOS
* Linux
* Tablet
* Smartphone
* Browser only

58. Is browser-based management the goal, or rather a desktop app?

59. Does the system need to function fully without an internet connection?

60. How important is simultaneous multi-user operation?

61. How important is reliability?

* Can a system restart cause an event to fail?
* Do you need autosave, backup, export, and recovery features?

---

## 12. Priorities and Non-Goals

62. What are the explicit non-goals for Version 1?
    This is important; otherwise, the system will immediately try to do everything:

* no teams
* no online registration portal
* No association integration
* No referee scheduling
* No payment processing
* No cloud

63. Which three features are indispensable for you to consider Version 1 “useful”?

64. Which three things would be nice to have, but should be deliberately put on hold?

65. In the end, would you prefer

* a clean technical requirements document,
* a domain model,
* user stories,
* or everything one after the other?

## My advice on the scope

You should keep the initial scope brutally small. Otherwise, you’ll accidentally end up building a complete tournament software system right away.

A sensible V1 scope would likely be:

* Individual competitions
* Manage age/weight classes
* Enter/import athletes
* Weigh-in / assign weight
* Generate pools and/or double-elimination brackets
* Schedule matches on mats
* REST connection to Ipponboard
* Revoke and correct results
* Calculate rankings

Not in V1:

* Teams
* Complex association processes
* Online registration
* Unusual tournament formats
* Extensive rights management

If you’d like, I can turn this into a structured interview guide with 10–15 key questions in a logical order, so we can work through them together step by step.
