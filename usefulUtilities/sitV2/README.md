# Sit V2

## Description

Make any entity sittable whether it's a chair, bench, couch, log, or even a tree stump. One chair per entity.

To enable sit when clicking on chair set `canClickOnModelToSit` to `true` in userData. 


## Setup

1. Add sitClientV2.js as script to entity
2. Add sitServerV2.js as server script to entity


## Releases

2019-06-27_20-24-55 :: [06afa3e]
- [DEV 150](https://highfidelity.atlassian.net/browse/DEV-150) Added checks and a time out to help fix getting stuck in the animation when HMD is removed.

2019-06-24_10-56-00 :: [d3de76d]
- [Jira 773](https://highfidelity.atlassian.net/browse/BUGZ-773) Added a bump when an avatar gets out of a chair to help with getting stuck in the floor

2019-06-21_10-56-00 :: [75b55a27](https://github.com/highfidelity/hifi-content/pull/392/commits/75b55a27)
- [Jira 576](https://highfidelity.atlassian.net/browse/BUGZ-576) Removed animation restore logspam

2019-06-19_15-56-00 :: [6ee18e8](https://github.com/highfidelity/hifi-content/pull/392/commits/6ee18e8)
- [Jira 575](https://highfidelity.atlassian.net/browse/BUGZ-575) Ensure pin on joint is only cleared when standing or before setting new pin

2019-06-12_11-30-00 :: [65eb1ecb](https://github.com/highfidelity/hifi-content/pull/392/commits/65eb1ecb)
- [Jira 655](https://highfidelity.atlassian.net/browse/BUGZ-655) Removed utils.js dependency

2019-06-06_07-13-50 :: [c6ed100](https://github.com/highfidelity/hifi-content/pull/392/commits/c6ed100)
- [Jira 554](https://highfidelity.atlassian.net/browse/BUGZ-554) Changed standup to require one tap of jump movement key 

2019-06-05_12-36-00 :: [2f30d30](https://github.com/highfidelity/hifi-content/pull/392/commits/2f30d30)
- [Jira 299](https://highfidelity.atlassian.net/browse/BUGZ-299) Removed repositioning/reorienting of avatar upon standing. User must use space bar to stand now.

2019-05-23_17-00-00 :: [0eedf28](https://github.com/highfidelity/hifi-content/pull/392/commits/0eedf28)
- [Jira 350](https://highfidelity.atlassian.net/browse/BUGZ-350) Removed script caching

2019-05-22_17-00-00 :: [f5cb684](https://github.com/highfidelity/hifi-content/pull/392/commits/f5cb684)
- [Jira 307](https://highfidelity.atlassian.net/browse/BUGZ-307) Added a timeout back after it was previously removed and removed empty script from sit zones

2019-05-21_10-00-00 :: [db0d1e5](https://github.com/highfidelity/hifi-content/pull/392/commits/db0d1e5)
- Removed signal handler for skeletonModelURLChanged as it duplicated code handled by onLoadComplete

2019-05-20_13-29-00 :: [c5de1cd](https://github.com/highfidelity/hifi-content/pull/392/commits/c5de1cd)
- Emergency fix for multiplying sit zones

2019-05-13_12-40-00 :: [c9c58a1](https://github.com/highfidelity/hifi-content/pull/388/commits/c9c58a1)
- When standing up, the user returns to the world position where they were when they sat down
- Fixed bug where user would sit without moving to the chair, then immediately stand up again

2019-01-17_14-57-17 :: [sitScriptUpdate ded8ecbc78a44c3c830e1f42fed6dd8e9277a4c1]
- During Create Mode when the entity has 0.5 alpha value or less, a local visible cube is added for easier adjustments. The visible cube disappears once Create mode is closed.

## Known issues

### Solution to other entities taking the "Click to Sit" click events

Collisions with other entity's invisible collision hulls sometimes make it difficult to sit. Ensure entities near sit cubes have the property `ignorePickIntersection: true`.

### Sit animation does not apply to an avatar using a different default animation

Animation that is applied before sitting is applied while in the chair and continues after standup. 

### Race condition for making sure you stood up before you sat down when sitting

When a user clicks to sit on another chair while they are already sitting down, the intention is to have them stand up first and reset the sit state machine before sitting down again. We don't have a guarantee that this is done given the current code architecture, so we have implemented a number of workarounds to ensure that moving between seats works smoothly. Check out [BUGZ-809](https://highfidelity.atlassian.net/browse/BUGZ-809) for more details. 

### Unreliable method for making sure we know where the floor is when we standup

In the current Sit code architecture, we begin sitting in a seat by using the position of the clicked "sit cube", and then adding some constant to figure out where your hips should go during the sit animation. To figure out where to position the avatar upon stand-up, we explored saving the Y position of the avatar before sitting down, but there are several cases where that saved Y position might not be a useful reference - consider the user standing on a different vertical plane as the chair.
In this version of the code, we add a small, arbitrary value to the Y position of the user when standing. This should generally take care of the problems we were seeing with users stuck in the floor. However, there still exists the possibility of an edge case where the small increase in Y position is enough to force the user through a room's ceiling. 
[DEV-167](https://highfidelity.atlassian.net/browse/DEV-167) for further reference.  

### Future features

Configurable variables:
- Custom animations specified via userData
- All configurable variables specified via userData

## Sit Configurations

### Configurable adjustments in Create Mode
- Adjust seat center - update the "Pivot" (to adjust the "registrationPoint" entity property) Ex: a stool for an avatar to sit in the middle on top, Pivot = { x: 0.5, y: 1.0, z: 0.5 }

### Configurable variables in userData
- Can click on chair to sit boolean in userData

Happy sitting V2!