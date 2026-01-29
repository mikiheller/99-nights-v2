# 99 Nights Math Forest - Development Planning Document

> **IMPORTANT: Instructions for AI Agents**
> 
> When starting a new session, follow these steps:
> 1. **Read this entire document** before doing anything else
> 2. **Identify where we are in the plan** by checking the Status column in each phase
> 3. **Create a new branch** based on what we're supposed to do next (naming convention: `phase1-persistence`, `phase2-xp-levels`, etc.)
> 4. **Tell the user** what phase we're on and what the plan is, so they can give feedback before coding begins
> 5. **Update this document** as you code - mark tasks as "In Progress" or "Complete" with dates
> 6. **When finishing a phase**, ensure this doc is updated and changes are pushed to main

---

## Table of Contents
1. [Project Context](#project-context)
2. [Current Codebase Summary](#current-codebase-summary)
3. [Development Phases](#development-phases)
4. [Detailed Feature Specifications](#detailed-feature-specifications)
5. [User Preferences](#user-preferences)
6. [Technical Decisions Log](#technical-decisions-log)
7. [Known Bugs & Issues](#known-bugs--issues)
8. [Testing Checklist](#testing-checklist)
9. [Session History](#session-history)

---

## Project Context

### What is 99 Nights Math Forest?
An educational math game built for a child, designed to make practicing math problems engaging and fun. The game features a 3D voxel-style forest environment where players survive nights by solving math problems during various game interactions (fighting wolves, chopping trees, collecting resources, etc.).

### The Goal
Make the game so engaging that the child *wants* to play, thereby practicing math without it feeling like homework. We're leaning into game design principles that make games addictive: progression systems, rewards, streaks, collections, pets, quests, and more.

### Target Player
- Age: Elementary school
- Interests: Cats, pigs, bees, games like the real "99 Nights"
- Math focus: Currently addition and subtraction, expanding to multiplication/division

### Design Philosophy
- **Math should be invisible** - The game is fun first, educational second
- **Configurable curriculum** - Admin can change which math problems appear for each game action
- **Track mastery** - Know what the child has learned and what needs work
- **Persistent progression** - Progress saves between sessions, creating investment
- **Hooks everywhere** - Streaks, daily rewards, quests, achievements, pets, classes

---

## Current Codebase Summary

### File Structure
```
/99 Nights Math 1.2/
├── index.html      # 3D Portal Lobby (Three.js) - ~680 lines
├── lobby.html      # Duplicate of index.html (untracked)
├── game.html       # Main game - ~7800 lines
├── README.md       # Basic project info
└── Archive/        # Old versions
```

### Key Code Locations in game.html

#### Math Problem System
| Function | Line | Purpose |
|----------|------|---------|
| `showAction()` | 4420-4508 | Main entry point - triggers math problems based on action type |
| `generateQuestionForLevel()` | 4036-4216 | Generates questions from curriculum |
| `generateTierQuestion()` | 4219-4222 | Maps tier to curriculum level |
| `handleSubmit()` | 4624-4718 | Processes answer (correct/incorrect) |
| `completeEncounter()` | 4720-5026 | Awards rewards after math encounter |
| `ADDITION_CURRICULUM` | 3854-3867 | 12 levels of addition problems |
| `SUBTRACTION_CURRICULUM` | 3875-3881 | 5 levels of subtraction problems |

#### Math Trigger Points (11 total)
| Action Key | Line | Questions | Timer |
|------------|------|-----------|-------|
| `fightWolf` | 4434-4443 | 2-3 | 7 sec |
| `fightCultist` | 4444-4453 | 2-3 | 7 sec |
| `chop` (tree) | 4454-4470 | 1-3 | No |
| `collectFuel` | 4471-4475 | 1 | No |
| `salvageFan` | 4476-4480 | 1 | No |
| `salvageBolt` | 4481-4485 | 1 | No |
| `salvageRadio` | 4486-4490 | 1 | No |
| `collectKey` | 4491-4496 | 3 | No |
| Crafting | 6890-6941 | 1 | No |
| Farm Harvest | 7180-7205 | 1 | No |
| Crock Pot | 7207-7258 | 1 | No |

#### Game State & Inventory
| Variable/Function | Line | Notes |
|-------------------|------|-------|
| `playerHealth` | 3246 | Starts at 100 |
| `diamondsEarned` | 3308 | Counter only - no spending! |
| `rewardsState` | 3316-3379 | Tracks rare items, keys, chests, milestones, weapons |
| `craftingRecipes` | 6738 | All craftable items |
| Resources stored in DOM | various | `woodCount.textContent`, etc. (not JS vars!) |

#### Creatures
| Function | Line | Notes |
|----------|------|-------|
| `createWolf()` | 1535 | Wolf 3D model |
| `createCultist()` | 2246 | Cultist 3D model |
| `createDeerMonster()` | 2006 | Deer boss model |
| `spawnCultists()` | 5877 | Spawns 3 cultists at night |
| `spawnDeerMonster()` | 5969 | Every 3rd night |

### Critical Finding: No Persistence
- **Nothing saves between sessions** - all progress is lost on refresh
- Diamonds are earned but cannot be spent
- This is the #1 priority to fix

---

## Development Phases

### Phase 1: Foundation
**Goal:** Create the infrastructure that all other features depend on

| Task | Status | Branch | Notes |
|------|--------|--------|-------|
| 1.1 Persistence Layer | Not Started | `phase1-persistence` | localStorage save/load system |
| 1.2 Admin Area | Not Started | `phase1-admin` | Math problem mapping UI in lobby |
| 1.3 Refactor showAction() | Not Started | `phase1-refactor` | Use admin config instead of hardcoded |
| 1.4 Mastery Tracking | Not Started | `phase1-mastery` | Track accuracy per problem type |

### Phase 2: Core Engagement
**Goal:** Add progression systems that create "one more game" feeling

| Task | Status | Branch | Notes |
|------|--------|--------|-------|
| 2.1 XP & Leveling | Not Started | `phase2-xp` | Visual level progression |
| 2.2 Classes/Upgrades | Not Started | `phase2-classes` | Diamond spending, persistent upgrades |
| 2.3 Streaks & Combos | Not Started | `phase2-streaks` | Consecutive correct answers bonus |
| 2.4 Quest Board | Not Started | `phase2-quests` | Daily quests with spin mechanic |
| 2.5 Achievement Expansion | Not Started | `phase2-achievements` | More badges and milestones |

### Phase 3: Pets & Content
**Goal:** Add depth with pets and new things to discover

| Task | Status | Branch | Notes |
|------|--------|--------|-------|
| 3.1 Pet System | Not Started | `phase3-pets` | Tameable cats, pigs, bees |
| 3.2 Structures | Not Started | `phase3-structures` | Explorable houses, caves |
| 3.3 New Creatures | Not Started | `phase3-creatures` | Alpha wolf, bear, snake, owl, ram |
| 3.4 First-Person Items | Not Started | `phase3-items` | Visible held items (await screenshot) |

### Phase 4: Gamification Polish
**Goal:** Add final engagement hooks and polish

| Task | Status | Branch | Notes |
|------|--------|--------|-------|
| 4.1 Boss Nights | Not Started | `phase4-bosses` | Special challenge every 10th night |
| 4.2 Daily Login Rewards | Not Started | `phase4-daily` | Incentive to return daily |
| 4.3 Speed Bonuses | Not Started | `phase4-speed` | Reward fast answers |
| 4.4 Close Call Feedback | Not Started | `phase4-polish` | Screen effects, sounds |
| 4.5 Bug Fixes | Not Started | `phase4-fixes` | Address issues from testing |

---

## Detailed Feature Specifications

### 1.1 Persistence Layer

**Data to Save (GameSave object):**
```javascript
const GameSave = {
  // Version for migration support
  version: 1,
  
  // Player progression
  totalDiamondsEarned: 0,
  diamondsSpent: 0,
  playerLevel: 1,
  playerXP: 0,
  
  // Mastery tracking (per problem type ID)
  mastery: {
    // "add_level1": { totalAttempts: 50, totalCorrect: 45, last10: [1,1,1,0,1,1,1,1,1,1], masteryAchieved: false }
  },
  
  // Purchased classes/upgrades
  classes: {
    // "thick_skin": { purchased: true, purchaseDate: "2024-01-15" }
  },
  
  // Pet data
  pet: null, // or { type: 'cat', name: 'Whiskers', level: 1, happiness: 100 }
  
  // Admin configuration
  adminConfig: {
    mathMappings: {
      fightWolf: { problemType: 'subtract_level1', questionCount: 3, timerSeconds: 7 },
      // ... etc for each action
    }
  },
  
  // Quests
  dailyQuests: [],
  lastQuestRefresh: null,
  
  // Achievements
  achievements: [],
  
  // Lifetime stats
  stats: {
    totalNightsSurvived: 0,
    totalWolvesDefeated: 0,
    totalTreesChopped: 0,
    totalCultistsDefeated: 0,
    bestNightReached: 0,
    totalQuestionsAnswered: 0,
    totalQuestionsCorrect: 0,
    currentStreak: 0,
    bestStreak: 0,
    totalPlayTime: 0,
    gamesPlayed: 0,
  },
  
  // Daily login tracking
  lastLoginDate: null,
  consecutiveLoginDays: 0,
  
  // Last session state (for resuming)
  lastGameMode: 'addition',
}
```

**Functions:**
- `saveGame()` - Serialize GameSave to localStorage
- `loadGame()` - Load from localStorage, return default if none
- `resetGame()` - Clear all progress (with confirmation)
- `exportSave()` - Download save as JSON file
- `importSave()` - Load save from JSON file

**Storage Key:** `99nights_save_v1`

---

### 1.2 Admin Area

**Location:** Hidden admin portal in lobby (accessible via secret keystroke or special position)

**UI Components:**
1. **Math Mappings Panel**
   - List of all 11 game actions
   - Dropdown for each to select problem type
   - Show problem type description and example
   - Question count selector (1-5)
   - Timer toggle and duration

2. **Problem Type Library**
   - All curriculum levels from addition and subtraction
   - Future: multiplication, division, mixed
   - Each shows: ID, name, description, example problem

3. **Controls**
   - Save Configuration button
   - Reset to Defaults button
   - Test Problem button (generates sample)

**Default Math Mappings:**
```javascript
const DEFAULT_MATH_MAPPINGS = {
  fightWolf: { problemType: 'subtract_level1', questionCount: 3, timerSeconds: 7 },
  fightCultist: { problemType: 'subtract_level2', questionCount: 3, timerSeconds: 7 },
  chop: { problemType: 'add_level3', questionCount: 2, timerSeconds: 0 },
  collectFuel: { problemType: 'add_level1', questionCount: 1, timerSeconds: 0 },
  salvageFan: { problemType: 'add_level2', questionCount: 1, timerSeconds: 0 },
  salvageBolt: { problemType: 'add_level2', questionCount: 1, timerSeconds: 0 },
  salvageRadio: { problemType: 'add_level2', questionCount: 1, timerSeconds: 0 },
  collectKey: { problemType: 'add_level5', questionCount: 3, timerSeconds: 0 },
  crafting: { problemType: 'subtract_level1', questionCount: 1, timerSeconds: 0 },
  harvestFarm: { problemType: 'add_level6', questionCount: 1, timerSeconds: 0 },
  makeStew: { problemType: 'subtract_level3', questionCount: 1, timerSeconds: 0 },
}
```

**Problem Type IDs:**
```javascript
const PROBLEM_TYPES = [
  // Addition
  { id: 'add_level1', curriculum: 'addition', level: 1, name: 'Easy Addition', desc: 'Adding numbers 0-5' },
  { id: 'add_level2', curriculum: 'addition', level: 2, name: 'Bridging Numbers', desc: 'Adding 0-5 and 9,10' },
  { id: 'add_level3', curriculum: 'addition', level: 3, name: 'Medium Addition', desc: 'Adding 4-10' },
  { id: 'add_level4', curriculum: 'addition', level: 4, name: 'Missing Addend', desc: '? + b = c format' },
  { id: 'add_level5', curriculum: 'addition', level: 5, name: 'Equation Balance', desc: 'a + b = c + ?' },
  { id: 'add_level6', curriculum: 'addition', level: 6, name: 'Triple Addition', desc: 'a + b + c' },
  { id: 'add_level7', curriculum: 'addition', level: 7, name: 'Triple Missing', desc: 'a + b + ? = d' },
  { id: 'add_level8', curriculum: 'addition', level: 8, name: 'Tens Plus Ones', desc: '70 + 3' },
  { id: 'add_level9', curriculum: 'addition', level: 9, name: 'Adding Ten', desc: 'Two-digit + 10' },
  { id: 'add_level10', curriculum: 'addition', level: 10, name: 'Make Ten Triple', desc: '8 + 1 + 2 (pairs to 10)' },
  { id: 'add_level11', curriculum: 'addition', level: 11, name: 'Complement to 20', desc: 'a + ? = 20' },
  { id: 'add_level12', curriculum: 'addition', level: 12, name: 'Two-Digit Plus One', desc: '45 + 7' },
  
  // Subtraction
  { id: 'subtract_level1', curriculum: 'subtraction', level: 1, name: 'Basic Subtraction', desc: '10 - 4' },
  { id: 'subtract_level2', curriculum: 'subtraction', level: 2, name: 'Missing Subtrahend', desc: '8 - ? = 5' },
  { id: 'subtract_level3', curriculum: 'subtraction', level: 3, name: 'Teens Minus Single', desc: '13 - 4' },
  { id: 'subtract_level4', curriculum: 'subtraction', level: 4, name: 'Minus Ten', desc: '46 - 10' },
  { id: 'subtract_level5', curriculum: 'subtraction', level: 5, name: 'Double Minus Single', desc: '63 - 4' },
]
```

---

### 1.4 Mastery Tracking

**Mastery Criteria:**
- Minimum 20 attempts on this problem type
- 90%+ overall accuracy
- 80%+ accuracy on last 10 attempts

**UI: Mastery Dashboard (in lobby)**
- Grid of problem types
- Each shows:
  - Problem type name
  - Progress bar (attempts toward 20 minimum)
  - Accuracy percentage
  - Last 10 indicator (dots: green=correct, red=wrong)
  - Checkmark badge when mastered
- Filter by: All / Mastered / In Progress / Not Started

---

### 2.1 XP & Leveling

**XP Sources:**
| Action | XP |
|--------|-----|
| Correct answer | +10 |
| Correct first try | +5 bonus |
| Streak bonus (per level) | +2 |
| Speed bonus (<3 sec) | +5 |
| Speed bonus (<2 sec) | +10 |
| Survive night | +50 |
| Complete quest | +100 |
| Defeat cultists | +25 |

**Level Thresholds:**
| Level | Total XP Required |
|-------|-------------------|
| 1 | 0 |
| 2 | 100 |
| 3 | 250 |
| 4 | 500 |
| 5 | 1,000 |
| 6 | 2,000 |
| 7 | 4,000 |
| 8 | 8,000 |
| 9 | 16,000 |
| 10 | 32,000 |

**Level Rewards:**
- Level 2: Unlock pet taming
- Level 3: +1 class slot
- Level 5: Unlock daily quests
- Level 7: +1 class slot
- Level 10: Special cosmetic

---

### 2.2 Classes/Upgrades

**Class Shop (in lobby):**

| Class | Cost | Effect | Requires Level |
|-------|------|--------|----------------|
| Thick Skin | 10 | -10% wolf damage | 1 |
| Lucky Axe | 15 | 20% chance bonus wood | 1 |
| Quick Thinker | 20 | +2 seconds on timers | 2 |
| Sharp Memory | 25 | Hint after wrong answer | 3 |
| Iron Stomach | 15 | Food heals +25% more | 2 |
| Fire Master | 20 | Campfire burns 20% longer | 2 |
| Eagle Eye | 30 | See wolves from further | 4 |
| Math Prodigy | 50 | +50% XP from questions | 5 |
| Pet Whisperer | 25 | Pet abilities +25% | 3 |
| Night Walker | 40 | Move 10% faster at night | 4 |

**UI:** Grid of class cards, locked/unlocked state, purchase button

---

### 2.3 Streaks & Combos

**Streak System:**
- Counter for consecutive correct answers (across all encounters)
- Displayed in game HUD: "Streak: 5"
- Persists between sessions
- Lost on any wrong answer
- Milestone effects:
  - 5: Flame icon appears
  - 10: Sparkle effect, sound
  - 25: Special glow
  - 50: Trophy notification
  - 100: Achievement unlocked

**Combo System (within encounter):**
- Speed-based multiplier
- Answer < 3 sec: "FAST!" +5 XP
- Answer < 2 sec: "SUPER FAST!" +10 XP
- Answer < 1 sec: "LIGHTNING!" +20 XP

---

### 2.4 Quest Board

**Location:** Special structure in lobby (or button in UI)

**Quest Refresh:**
- Spin lever animation (like real 99 Nights)
- Get 3 random quests
- Refreshes daily (or on demand for diamonds?)

**Quest Types:**
| Quest | Reward | Difficulty |
|-------|--------|------------|
| Chop 5 trees | 5 | Easy |
| Chop 15 trees | 15 | Medium |
| Defeat 2 wolves | 10 | Easy |
| Defeat 5 wolves | 25 | Medium |
| Answer 10 questions correctly | 5 | Easy |
| Answer 25 questions correctly | 15 | Medium |
| Get a 5-streak | 8 | Easy |
| Get a 10-streak | 20 | Medium |
| Survive until dawn | 5 | Easy |
| Reach night 5 | 15 | Medium |
| Find a rare item | 20 | Hard |
| Defeat all cultists | 15 | Medium |
| Don't lose health tonight | 25 | Hard |

**Quest Tracking:**
- Progress shown in real-time during game
- Completion popup with reward animation
- Multi-night quests persist

---

### 3.1 Pet System

**Pet Types:**
| Pet | How to Get | Abilities |
|-----|------------|-----------|
| Cat | Random forest encounter | Alert to wolves, +10% rare drop |
| Pig | Random forest encounter | Find buried food, +1 harvest |
| Bee | Random forest encounter | Produce honey (heals), faster farm |

**Pet Data:**
```javascript
pet: {
  type: 'cat',
  name: 'Whiskers',  // Player names it
  level: 1,          // Levels up with player
  happiness: 100,    // Decreases over time, feed to restore
  abilities: ['wolfAlert']
}
```

**Pet Happiness:**
- Decreases by 5 per day
- Feed with carrots to restore (+20 per carrot)
- Unhappy pet (< 30) = abilities disabled
- Happy pet (> 80) = abilities boosted

**Taming Encounter:**
- Random chance when exploring during day
- "A wild cat appears!"
- Answer 3 questions correctly to befriend
- Name your pet popup

---

## User Preferences

### Math Curriculum
- **Primary focus:** Addition and subtraction
- **Current levels unlocked:** Addition portal, Subtraction portal
- **Future:** Multiplication, division, mixed operations

### Game Preferences
- Likes cats, pigs, bees (for pets)
- Enjoys the real 99 Nights game (quest board with lever)
- Responds well to visible progression (numbers going up)
- Needs "one more minute" hooks

### Parent Preferences (Admin)
- Ability to configure which math appears where
- Track mastery to know what's learned
- No inappropriate content
- Persistent progress so effort isn't lost

---

## Technical Decisions Log

| Date | Decision | Rationale |
|------|----------|-----------|
| 2024-01-XX | Keep single-file HTML structure | Avoids CORS issues when running locally |
| 2024-01-XX | Use localStorage for persistence | Simple, no backend needed |
| 2024-01-XX | Admin area in lobby | Parent can configure without child seeing |

---

## Known Bugs & Issues

| ID | Description | Severity | Status |
|----|-------------|----------|--------|
| (None yet) | | | |

*Add bugs here as discovered during development*

---

## Testing Checklist

### Phase 1 Testing
- [ ] Save game, refresh page, load game - data persists
- [ ] Admin config changes persist after refresh
- [ ] Changing math mapping affects game encounters
- [ ] Mastery tracking updates after answering questions
- [ ] Mastery dashboard shows correct stats

### Phase 2 Testing
- [ ] XP increases after correct answers
- [ ] Level up notification appears
- [ ] Classes can be purchased with diamonds
- [ ] Class effects apply in game
- [ ] Streak counter works correctly
- [ ] Streak resets on wrong answer
- [ ] Quest board generates 3 quests
- [ ] Quest progress tracks correctly
- [ ] Quest completion awards diamonds

### Phase 3 Testing
- [ ] Pet encounter appears randomly
- [ ] Pet can be named
- [ ] Pet abilities work
- [ ] Pet happiness decreases over time
- [ ] Pet can be fed

*(Add more as phases are developed)*

---

## Session History

### Session 1 - Initial Planning
**Date:** 2024-01-XX
**Agent:** Planning session
**What was done:**
- Explored codebase
- Created comprehensive plan
- Set up new GitHub repo (99-nights-v2)
- Created this PLANNING.md document

**Next steps:**
- Phase 1.1: Build persistence layer

---

*This document should be updated by each agent session. Keep it current!*
