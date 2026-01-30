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
| 1.1 Persistence Layer | ✅ Complete | `phase1-persistence` | localStorage save/load system |
| 1.2 Admin Area | ✅ Complete | `phase1-admin` | Math problem mapping UI in lobby |
| 1.3 Refactor showAction() | ✅ Complete | `phase1-admin` | Uses admin config, preserves weapon upgrades |
| 1.4 Mastery Tracking | ✅ Complete | `phase1-persistence` | Track accuracy per problem type (bundled with 1.1) |

### Phase 2: Core Engagement
**Goal:** Add progression systems that create "one more game" feeling

| Task | Status | Branch | Notes |
|------|--------|--------|-------|
| 2.1 XP & Leveling | ✅ Complete | `phase2-xp-levels` | Visual level progression with celebrations |
| 2.2 Classes/Upgrades | ✅ Complete | `phase2-classes` | Diamond spending, 10 class upgrades |
| 2.3 Streaks & Combos | ✅ Complete | `phase2-xp-levels` | Bundled with XP system |
| 2.4 Quest Board | ✅ Complete | `phase2-quests` | Daily quests with 13 quest types |
| 2.5 Achievement Expansion | ✅ Complete | `phase2-achievements` | 20 achievements, in-game status panel |

### Phase 3: Pets & Content
**Goal:** Add depth with pets and new things to discover

| Task | Status | Branch | Notes |
|------|--------|--------|-------|
| 3.1 Pet System | ✅ Complete | `phase3-pets` | Tameable cats, pigs, bees with abilities |
| 3.2 Structures | Not Started | `phase3-structures` | Explorable houses, caves |
| 3.3 New Creatures | Not Started | `phase3-creatures` | Alpha wolf, bear, snake, owl, ram |
| 3.4 First-Person Items | Not Started | `phase3-items` | Visible held items (await screenshot) |

### Phase 4: Gamification Polish
**Goal:** Add final engagement hooks and polish

| Task | Status | Branch | Notes |
|------|--------|--------|-------|
| 4.1 Boss Nights | Not Started | `phase4-bosses` | Special challenge every 10th night |
| 4.2 Daily Login Rewards | ✅ Complete | `phase4-daily-rewards` | 7-day reward cycle with streak bonuses |
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
- 80%+ overall accuracy
- 90%+ accuracy on last 10 attempts (should improve over time)

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

### Session 2 - Persistence Layer Implementation
**Date:** 2026-01-28
**Branch:** `phase1-persistence`
**What was done:**
- Implemented complete persistence layer in game.html
- Created `GameSave` object structure with all fields from spec
- Implemented core functions: `saveGame()`, `loadGame()`, `resetGame()`, `exportSave()`, `importSave()`
- Added mastery tracking system: `updateMastery()` called on every question answer
- Added lifetime stat tracking: wolves defeated, trees chopped, cultists defeated, nights survived
- Added daily login tracking with consecutive day counter
- Hooked persistence into game initialization (loadGame on init)
- Added save triggers on all key events (correct/wrong answers, combat, harvesting, night survival)
- All questions now include their level for proper mastery tracking

**Testing notes:**
- Open browser console and type `gameSave` to see current save state
- Use `exportSave()` to download save as JSON file
- Use `importSave(file)` with file input to restore save
- Use `resetGame()` to clear all progress (requires confirmation)
- Save auto-saves after every mastery update, stat update, and game start

**Next steps:**
- Phase 2: Core Engagement features

---

### Session 3 - Admin Area & Config System
**Date:** 2026-01-28
**Branch:** `phase1-admin`
**What was done:**
- Added admin panel to lobby (index.html) with settings button in top-right corner
- Created UI for configuring all 11 game actions:
  - Problem type dropdown (17 types: 12 addition, 5 subtraction)
  - Question count input (1-5)
  - Timer duration input (for timed encounters)
- Added problem types reference grid
- Implemented save/reset functionality connected to persistence layer
- Updated game.html to use admin config:
  - New `generateQuestionsFromAdminConfig()` function
  - New `getTimerFromAdminConfig()` function
  - Modified `showAction()` to use config while preserving weapon upgrade logic
  - Timer duration now configurable per action
- Phase 1 Foundation is now complete!

**How to use:**
1. Go to lobby (index.html)
2. Click the ⚙️ button in top-right
3. Configure which math problems appear for each action
4. Click "Save Settings"
5. Settings persist across sessions

**Next steps:**
- Phase 2.2: Classes/Upgrades (diamond spending)
- Phase 2.4: Quest Board

---

### Session 4 - XP & Leveling System
**Date:** 2026-01-28
**Branch:** `phase2-xp-levels`
**What was done:**
- Implemented complete XP & Leveling system:
  - 10 levels with increasing thresholds (100 → 32,000 XP)
  - XP rewards for correct answers (+10), first try bonus (+5), speed bonuses (+5/+10)
  - XP for surviving nights (+50) and defeating all cultists (+25)
  - Streak bonuses (+2 per streak level, capped at 10)
- Added visual XP bar in game UI showing level, progress, and XP count
- Added streak badge that appears at 3+ consecutive correct answers
- Added floating "+XP" indicators when earning XP
- Added level-up celebration with full-screen animation
- Level rewards preview (pet taming at L2, class slots at L3/L7, quests at L5)
- Streaks system integrated - tracks consecutive correct answers, resets on wrong

**How it works:**
- Answer questions correctly to earn XP
- Faster answers = more XP (under 3s or 2s)
- Build streaks for bonus XP
- Level up to unlock features
- Survive nights and defeat cultists for bonus XP

**Next steps:**
- Phase 2.4: Quest Board - daily challenges

---

### Session 5 - Classes/Upgrades System
**Date:** 2026-01-28
**Branch:** `phase2-classes`
**What was done:**
- Added Class Shop to lobby (🏪 button) with 10 purchasable classes:
  - Thick Skin (10💎) - -10% wolf damage
  - Lucky Axe (15💎) - 20% chance bonus wood
  - Quick Thinker (20💎) - +2 seconds on timers
  - Sharp Memory (25💎) - Hint after wrong answer (planned)
  - Iron Stomach (15💎) - Food restores +25% more hunger
  - Fire Master (20💎) - Campfire burns 20% slower
  - Eagle Eye (30💎) - See wolves from further (visual - planned)
  - Math Prodigy (50💎) - +50% XP from questions
  - Pet Whisperer (25💎) - Pet abilities +25% (when pets added)
  - Night Walker (40💎) - Move 10% faster at night
- Added diamond and level display in lobby top-left
- Classes are level-gated (require certain level to purchase)
- Implemented class effects in game.html:
  - getWolfDamage() - reduces wolf damage
  - getLuckyAxeBonus() - random bonus wood
  - getTimerBonus() - extra timer time
  - getFoodHealingMultiplier() - food bonus
  - getFireDrainMultiplier() - slower fire drain
  - getXPMultiplier() - bonus XP
  - getNightMovementMultiplier() - night speed boost
- Classes persist across sessions

**How to use:**
1. Earn diamonds by surviving nights and collecting rare items
2. Go to lobby and click 🏪 button
3. Purchase classes that match your level
4. Effects apply automatically in game!

**Next steps:**
- Phase 2.5: Achievement Expansion
- Phase 3: Pets & Content

---

### Session 6 - Quest Board System
**Date:** 2026-01-28
**Branch:** `phase2-quests`
**What was done:**
- Added Quest Board to lobby (📋 button) with daily quests:
  - 13 different quest types across easy/medium/hard difficulties
  - 3 random quests per day (auto-refresh daily)
  - Spin button to get new quests anytime
- Quest types include:
  - Chop trees (5/15)
  - Defeat wolves (2/5)
  - Answer questions correctly (10/25)
  - Get streaks (5/10)
  - Survive nights (1/3)
  - Defeat all cultists
  - Find rare items
  - Survive night without damage
- Real-time progress tracking during gameplay
- Quest completion notifications in-game
- Diamond rewards claimable in lobby
- Session stats tracked for quest progress:
  - treesChopped, wolvesDefeated, questionsCorrect
  - bestStreak, nightsSurvived, cultistsDefeated
  - rareItemsFound, perfectNights

**How it works:**
1. Open quest board in lobby (📋 button)
2. View your 3 daily quests and progress
3. Play the game - progress updates automatically
4. When quest completes, notification shows in-game
5. Return to lobby to claim diamond rewards
6. Quests refresh daily or spin for new ones

**Next steps:**
- Phase 2.5: Achievement Expansion
- Phase 3: Pets & Content

---

### Session 7 - Achievement Expansion
**Date:** 2026-01-30
**Branch:** `phase2-achievements`
**What was done:**
- Implemented comprehensive achievement system with 20 achievements across 7 categories:
  - **Math Master** (4): First Steps, Century Club, Math Machine, Math Legend
  - **Streak Star** (4): On Fire, Unstoppable, Perfect Mind, Legendary
  - **Survivor** (4): First Dawn, Week Warrior, Night Master, Eternal Survivor
  - **Speed Demon** (2): Quick Draw, Lightning Fast
  - **Hunter** (3): Wolf Slayer, Pack Leader, Cult Crusher
  - **Collector** (2): Lucky Find, Treasure Hunter
  - **Lumberjack** (2): Tree Chopper, Deforestation
- Added achievement unlock celebration popup with animation
- Added new stats tracking: fastAnswers3sec, fastAnswers2sec, totalRareItemsFound
- **In-game status panel** (📊 button) showing:
  - Overview tab: Level, XP, streaks, diamonds, stats with XP progress bar
  - Achievements tab: All achievements with progress tracking
  - Quests tab: Current daily quests with progress
- **Achievement gallery in lobby** (🏆 button):
  - Browse all achievements by category
  - See earned count and total rewards
  - Progress bars for incomplete achievements

**How it works:**
1. Achievements auto-unlock when requirements are met
2. Diamond rewards are awarded immediately upon unlock
3. Celebration popup shows with achievement details
4. Check progress anytime via 📊 button in-game or 🏆 button in lobby

**New Stats Tracked:**
- `fastAnswers3sec` - Answers under 3 seconds
- `fastAnswers2sec` - Answers under 2 seconds
- `totalRareItemsFound` - Lifetime rare items collected

**Next steps:**
- Phase 3.1: Pet System
- Phase 4.2: Daily Login Rewards

---

### Session 8 - Pet System
**Date:** 2026-01-30
**Branch:** `phase3-pets`
**What was done:**
- Implemented complete pet system with 3 pet types:
  - **Cat 🐱**: Wolf Alert (warns when wolves nearby), +10% rare item chance
  - **Pig 🐷**: Find Carrots (occasionally finds buried carrots), +1 harvest bonus
  - **Bee 🐝**: Produce Honey (heals player over time), faster farm growth
- Pet encounter system:
  - Random encounters during daytime exploration (Level 2+ required)
  - Taming mini-game: answer 3 questions correctly to befriend
  - Pet naming popup with suggested names
- Pet happiness system:
  - Happiness decreases over time (5 per day)
  - Feed carrots to restore happiness (+25 per carrot)
  - Abilities deactivate when happiness < 30%
- Pet 3D models that follow the player
- Pet UI display in game showing happiness bar and feed button
- Pet panel in lobby (🐾 button) showing full pet stats and abilities

**Pet Data Structure:**
```javascript
pet: {
  type: 'cat',           // cat, pig, or bee
  name: 'Whiskers',      // Player-chosen name
  level: 1,              // Levels up with player
  happiness: 100,        // 0-100, affects abilities
  adoptedDate: '...',    // When pet was adopted
  lastFed: '...',        // For happiness decay
  totalCarrotsFed: 0     // Lifetime stat
}
```

**How it works:**
1. Reach Level 2 to unlock pet encounters
2. Explore during daytime - random chance to find a pet
3. Answer 3 questions correctly to befriend
4. Name your new pet
5. Feed with carrots to keep abilities active
6. Pet follows you and provides passive bonuses

**Next steps:**
- Phase 4.2: Daily Login Rewards

---

### Session 9 - Daily Login Rewards
**Date:** 2026-01-30
**Branch:** `phase4-daily-rewards`
**What was done:**
- Implemented daily login reward system:
  - 7-day reward cycle with escalating diamonds (5, 10, 15, 20, 25, 30, 50)
  - Automatic popup when opening lobby with unclaimed reward
  - Streak tracking: 7+ consecutive days = 1.5x bonus multiplier
  - Visual progress indicator showing all 7 days
  - Confetti celebration on claim!
- Added 🎁 Daily Reward button in lobby
  - Pulses when reward available
  - Shows progress when already claimed
- Fixed gameSave reference in lobby (was undefined)

**Reward Structure:**
| Day | Base Reward | With Streak Bonus (7+ days) |
|-----|-------------|----------------------------|
| 1 | 5💎 | 7💎 |
| 2 | 10💎 | 15💎 |
| 3 | 15💎 | 22💎 |
| 4 | 20💎 | 30💎 |
| 5 | 25💎 | 37💎 |
| 6 | 30💎 | 45💎 |
| 7 | 50💎 | 75💎 |

**How it works:**
1. Open lobby each day
2. Popup shows with current day's reward
3. Click "Claim Reward!" to collect diamonds
4. Streak continues if you play consecutive days
5. After 7+ days, all rewards get 1.5x multiplier
6. Missing a day resets streak to 1

**Data tracked:**
- `lastRewardClaimed`: Date of last claim
- `consecutiveLoginDays`: Current streak count

---

*This document should be updated by each agent session. Keep it current!*
