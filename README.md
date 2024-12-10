**Open Combat Ruleset (OCR)**

Introduction
============

Abstract
--------
In this turn-based combat system, characters engage in tactical combat through a combination of skills, actions, and
careful timing. Drawing inspiration from classic roguelikes while incorporating modern mechanics, the system emphasizes
meaningful choices and skill progression. Whether you're a seasoned warrior wielding a sword or an aspiring mage channeling
arcane energies, your success depends on understanding and mastering these interconnected systems.

Goals
-----
The goals of this system include:
-   provide an open-source rule-set anyone can use
-   be used in both a tabletop-setting and programmatically in a video game
-   be easily extensible and modifiable

tl;dr
=====
Two or more Characters (including both Player Characters (PCs) and Non-Player Characters (NPCs)) can engage in a Battle,
either in a free-for-all or in team Groups. Battles are resolved when only one Group (or one Character, during free-for-all's)
remains with any health points.

Priority and Turn Order
=======================

Understanding Priority
----------------------
The foundation of our combat system rests upon the Priority system --- a numerical representation of how quickly Actions
and Bonus Actions are executed in combat, thus determining action order. Our Priority system favors higher numbers,
where higher-priority values represent the split-second advantage that quick actions have over more powerful but slower techniques.
Individual Bonus/Actions will specify either a scalar Priority value, or a dice roll used to calculate their Priority
value (to be executed every time Priority is calculated). De/buffs can further impact the priority calculation.

### Tie breaking
When determining the Priority between Characters. values which are equal are considered Tied. For Priority
comparisons based on Bonus/Actions which are directly associated to a Skill, the Character with the higher associated
skill will go first; this is to reflect their superior experience. If either the Priority comparisons do NOT have an
associated skill, or the skill values are also tied, then all tied characters Priority is determined randomly.

### Priority Numeric Ranges
Priority values *generally*range from 1 to 120, with negative values rounded up to zero, and segmented into three main
tiers. These tiers have no functional purpose besides providing a guideline for game designers on how they should assign Priority to various Actions.

### Relative Priority
Certain actions, generally Bonus Actions in particular, may execute in a Priority relative to another action. Instead of
a numeric or dice-roll value, such priority will be detonated by "Before <condition>" and "After <condition>". For such
Priorities, they will execute as if their Priority value was set such that it would execute Before/After its stipulated action.

### Quick Actions (Priority **80**-**120**+)
These lightning-fast techniques represent the height of martial efficiency. Quick Actions excel at seizing the initiative,
though they typically trade raw power for speed. A master of Quick Actions can often act multiple times before a slower opponent completes a single heavy attack.

Examples of Quick Actions include:
- A rogue's swift dagger strike
- A ranger's snapshot
- A monk's rapid jab

### Standard Actions (Priority **40**-**79**)
The backbone of combat, Standard Actions represent balanced techniques that blend speed and power. Most combatants will
find themselves using these actions frequently, as they provide reliable performance without excessive risk or commitment.

Examples of Standard Actions include:
- A warrior's sword slash
- A mage's fireball
- A cleric's healing prayer

### **Slow** Actions (Priority 1-**39**)
These powerful but slow techniques can turn the tide of battle - if they connect. Heavy Actions require careful timing
and often leave the user vulnerable, but their devastating effects can make the risk worthwhile.

Examples of Heavy Actions include:
- A barbarian's mighty cleave
- A wizard's lightning storm
- A paladin's smiting strike

### Priority Modification
Base Priority values for Bonus/Actions are not necessarily static --- they can be modified by various factors:
-   Equipment: Light weapons or armor might increase Priority, while heavy weapons or armor could decrease it.
-   Status Effects: Conditions like "Haste" or "Slow" could directly modify Priority
-   Skill Mastery: Higher skill levels might grant Priority bonuses to associated actions
-   Momentum: Building combat momentum through action chains can temporarily improve Priority

Turn Resolution
---------------

When multiple Characters act in the same round:
1.  All participants declare their actions and/or bonus actions.
2.  Actions resolve in Priority order (highest to lowest)
3.  Ties in Priority are resolved by comparing Action associated Skill levels (highest to lowest), followed by random-order for any remaining ties.
4.  Bonus Actions follow the same Priority system but can be configured to execute relative to their owner's Main Action
Accuracy System
---------------

### Basic Accuracy Mechanics
Like the Priority system, Accuracy operates on a 1-120 scale where higher numbers represent greater precision.
When making an attack, the basic formula is elegantly simple:

```
Hit occurs when: accuracy > target's evasion
Critical hit: accuracy ≤ (10 - target's evasion)
```

This creates a dynamic where highly accurate attacks can overcome even skilled defenders, while less precise attacks may
still connect against less evasive targets.

### Accuracy Categories

#### Precise Attacks (1-5)
These represent the pinnacle of martial precision. Think of a master archer's carefully aimed shot or a fencer's perfectly
timed thrust. Precise attacks rarely miss their mark but often sacrifice raw power for reliability.

Precise attacks are ideal for:
- Targeting specific weaknesses
- Maintaining consistent damage output
- Setting up tactical advantages

Equipment System
----------------

### Overview
Equipment forms the foundation of a character's combat capabilities, going beyond simple statistical modifications. Each piece of equipment opens new tactical options while potentially closing others, creating meaningful choices in character development.

### Equipment Categories

#### Weapons
Weapons define the basic actions available to a character. Each weapon type provides:
- A set of basic actions (Attack, Guard, etc.)
- Unique special actions (Bow provides "Precise Shot", etc.)
- Modification to existing actions (Changes to damage dice, accuracy, etc.)

#### Armor
Beyond simple protection, armor influences:
- Evasion values
- Priority modifications
- Special defensive capabilities
- Status effect resistances

#### Accessories
These specialized items provide unique benefits:
- Modification of critical ranges
- Special action enhancements
- Unique tactical options
- Status effect interactions

### Equipment Properties

#### Basic Properties
Every piece of equipment has:
- Quality rating (affecting base statistics)
- Durability (if using wear mechanics)
- Weight class (affecting Priority)
- Required skill level

#### Special Properties
Equipment may also feature:
- Inherent Effects: "Flaming" adds 1d4 fire damage, "Quick-drawn" improves Priority by 1
- Conditional Effects: "Vengeful" improves critical range when below 50% health
- Momentum-based Effects: Enhanced benefits during action chains

#### Equipment Synergies
Some equipment pieces are designed to work together:
- Set bonuses for wearing matching armor
- Weapon and shield combinations
- Accessory complementary effects

### Equipment Progression
Equipment can be improved through:
- Finding higher quality versions
- Upgrading at craftsmen
- Unlocking hidden properties
- Combining with other equipment

Critical System
---------------

### The Nature of Critical Strikes
Critical strikes represent those perfect moments in combat where skill, opportunity, and fortune align. Unlike simpler
systems that merely multiply damage, our critical system creates meaningful tactical decisions through varied effects and interactions.

### Critical Success
A critical success can occur in two ways:
- Rolling within an action's critical range
- Achieving exceptional accuracy (accuracy ≤ 10 - target's evasion)

### Types of Critical Effects

#### Standard Criticals
The most straightforward form, enhancing an action's primary effect:
- Normal sword strike (2d6) becomes 4d6
- Healing spell (2d4+2) becomes 4d4+4

#### Effect Criticals
Add secondary effects beyond numerical enhancement:
- Frost spell might freeze the target
- Precise thrust might inflict bleeding
- Stunning blow could extend stun duration

#### Priority Criticals
Create tactical advantages:
- Reducing follow-up action Priority
- Granting immediate bonus action
- Creating momentum chain opportunities
Momentum System
---------------

### Understanding Momentum
Momentum represents combat flow - how actions build upon each other to create devastating combinations. Unlike rigid
combo systems, momentum builds organically through tactical choices and skill specialization.

### Building Momentum
#### Chain Requirements
-   Actions must be compatible (usually from same skill tree)
-   Chains typically last 3-5 actions before resetting
-   Interruption breaks the chain
-   Some equipment may preserve chains through interruptions

#### Chain Benefits
Progressive benefits as momentum builds:
- Strike 1: Normal effects
- Strike 2: -1 Priority, +1 accuracy
- Strike 3: +1d6 damage, -2 Priority
- Strike 4: Guaranteed critical on hit
- Strike 5: Maximum chain benefit - all previous bonuses plus special effect

Status Effects System
---------------------

### Core Concepts
Status effects add tactical depth by modifying character capabilities. Each effect operates independently but can
interact with other systems in meaningful ways.

### Effect Categories
#### Impairments
Negative effects that hinder performance:
- Slow: +2 to all Priority values
- Weak: -25% damage output
- Vulnerable: Take 25% more damage
- Confused: 25% chance to target wrong enemy

#### Enhancements
Positive effects that boost capabilities:
- Swift: -2 to all Priority values
- Strengthened: +25% damage output
- Protected: Take 25% less damage
- Focused: +2 to accuracy

#### Special States
Complex effects that change gameplay dynamics:
- Berserk: Double damage dealt and received
- Ethereal: Can pass through obstacles
- Charged: Next spell costs no mana
- Momentum Lock: Preserve momentum chain through interruption

Skills System
-------------
### Skill Categories
Skills are divided into three main branches:

#### Combat Skills
-   Weapon Mastery
-   Combat Techniques
-   Battle Tactics
-   Special Moves

#### Support Skills
-   Healing Arts
-   Buff Enhancement
-   Debuff Resistance
-   Resource Management

#### Utility Skills
-   Movement
-   Environment Interaction
-   Item Enhancement
-   Special Actions

### Skill Progression
Skills improve through:
- Direct use
- Training points allocation
- Special discoveries
- Equipment bonuses

### Skill Synergies
Skills can interact to create powerful combinations:
- Weapon + Movement skills for special attacks
- Support + Combat for enhanced effects
- Utility + Support for resource efficiency

Conclusion
----------
This combat system emphasizes:
- Tactical decision-making
- Skill progression
- Equipment customization
- Dynamic combat flow

Success requires mastering the interplay between Priority, Accuracy, Equipment, Critical strikes, Momentum, Status effects,
and Skills. Each system alone provides depth, but their interactions create endless possibilities for character development and combat strategy.

Appendix
========

Basic Actions (No Skill Required)
---------------------------------
-   **Move**: Priority 5, Move one tile in any direction
-   **Wait**: Priority 1, Skip turn and gain +1 to next action's accuracy
-   **Use Item**: Priority 8, Consume or activate held item
-   **Search**: Priority 10, Examine adjacent tiles for hidden features

Combat Skills & Actions
-----------------------
### Swordsmanship
-   **Quick Slash**: Priority 4, 1d6 damage, builds momentum
-   **Guard Stance**: Priority 3, Bonus Action, +2 evasion until next turn
-   **Power Strike**: Priority 12, 3d6 damage, breaks momentum

### Archery
-   **Snap Shot**: Priority 3, 1d8 damage, -2 accuracy
-   **Aimed Shot**: Priority 10, 2d8 damage, +2 accuracy
-   **Multi-Shot**: Priority 8, Hits 3 targets for 1d6 each

Utility Skills & Actions
------------------------
### Mining
-   **Prospect**: Priority 5, Identify mineral quality
-   **Quick Mine**: Priority 8, 50% resource yield
-   **Careful Mine**: Priority 12, 100% resource yield

### Crafting
-   **Quick Repair**: Priority 6, Restore 25% durability
-   **Analyze Item**: Priority 4, Identify item properties
-   **Enhance**: Priority 15, Improve item quality

Equipment Examples
------------------

### Weapons
-   **Short Sword**: Priority +0, 1d6 damage, enables Swordsmanship actions
-   **War Bow**: Priority +2, 1d8 damage, enables Archery actions
-   **Mining Pick**: Priority +1, 1d4 damage, enables Mining actions

### Armor
-   **Leather Armor**: +2 defense, no Priority penalty
-   **Chain Mail**: +4 defense, +1 Priority to all actions
-   **Plate Armor**: +6 defense, +2 Priority to all actions

### Accessories
-   **Quick Ring**: -1 Priority to all actions
-   **Critical Band**: Expands critical range by 1
-   **Momentum Charm**: Chain breaks require two interruptions instead of one