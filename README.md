# Introduction
This repo was created to compile a list of resources, information, and suggestions for future programmers working at the LPMB Lab to make life easier and development smoother. Since Unity is constantly changing, this document, too, will inevitably change.

# Terminology
For lack of better terms, this document refers to the people conducting the study/experiment using a given program as "users" and those participating in the experiment/study as "participants." This is also the terminology used within the code itself, e.g. in variable naming conventions.

# Common tasks
Some things have to be done for every or almost every project. Here are some of the more tedious, complex, or just time consuming ones.

## User control
Programs built for use in the LPMB Lab tend to involve using triggers at certain distances and stimuli triggering at or lasting for given durations of time. These parameters are often subject to change as the users play around with the program and find that changing the values produces a better combination of stimuli for the experiment or study.

As such, it's recommended to give users control over as many parameters as possible using UI elements. Common UI elements and controlled parameters include:

- Sliders for changing durations of time or distances of environmental elements from a given point
  - Typically time sliders should be snapped to 0.1-second increments
  - Typically distance sliders should be snapped to 0.5-metre (or, rarely, 0.25-metre) increments
  - A useful formula for snapping slider intervals is
    - `Mathf.Round((sliderValue - minSliderValue) / increment) * increment`
- Dropdowns for letting users change the type of trial or sex of virtual pedestrians
  - Common trial types are:
    - Speed, typically fewer than 10, in which the participant walks in a straight line over multiple trials to get an average walking speed that can then be used to get the timming of stimuli accurate. Occasionally the walking speed measurement may be done in Practice/Familiarisation trals, with no Speed trials at all.
    - Practice/Familiarisation, in which participants get an opportunity to navigate in virtual reality, or get a simple preview of what they will have to do in the Actual/Recorded trials.
    - Control, in which no special stimulus occurs. Not all programs will have control trials; instead they may just have Actual/Recorded trials with varying degrees of stimulus, along with Speed and/or Practice/Familiarisation trials at the beginning.
    - Actual/Recorded, in which the participant performs the task on which the study or experiment is based, and the results of which are recorded along with the specifications of the trial.
- Rarely, text boxes to allow users to enter precise numerical information, like the shoulder width of a participant.

For simplicity, it may be useful to have the program start into a setup screen (i.e., just a dark grey UI panel with sliders, dropdowns, and buttons over it that the user can change trial settings in before clicking a "Start" button, at which point the panel and setup UI elements disappear and the trials can begin. If building for VR, a semi-transparent panel is good, since it provides enough contrast for the UI to be seen clearly, but also lets the users see what the participant sees in VR.

## Displaying trial information
Users usually want to keep track of how many trials have been completed and how many remain, as well as some information like the trial type. If it's a VR build, the head-mounted display (HMD) won't display UI canvases with the "Screen Space - Overlay" display mode, so you can include additonal information that the participant should not know, but the users may find useful.

## Implementing mouse + keyboard control for development
Naturally, not everybody has VR headset, so for development you'll typically need to create a script that moves the "player" using keyboard controls and rotates the view with a mouse or in set increments using keyboard keys.

It's probably good to have some familiarity with Unity's new input system, as that seems to be the direction in which Unity is moving.

## Finding models and animations
I use Mixamo, they have a several locomotion animation packs and simple models to apply them to, which makes it easy to find you're looking for. You'll need an Adobe account (or log in using Google or another one of the provided options).

The animations are motion-captured, so they are realistic looking. To download:
- Select a model (if you just want an animation, I usually pick the Y Bot or X Bot)
- Select an animation or animation pack and apply it to the model
- Download as an FBX for Unity
- Import, change Rig type from Generic to Humanoid
- Tick the "Loop time" option in each animation's import settings (unless, of course, the animation is a one-time thing, like a wave or jump)

Animation import sometimes gives an error saying a hip bone is missing. If that happens, try changing Avatar Definition from "Create From This Model" to "Copy From Other Model" and select the model you downloaded or another Mixamo model. If the error persists, re-download the model in the FBX for Unity format and try again with the new model.

And, of course, since you'll be working with animation a little bit, it's good to get used to Unity's Animator and triggering certain animations via code (usually just Idle -> Walking).

# Recommendations

## Basic project structure
LPMB Lab programs have the following basic components:
1. Trials
2. Trial structure
3. Some sort of (usually human) stimulus
4. User input
5. Data Collection
6. UI

As such I've found it beneficial to create scripts for each of these components and attach most of them to a central, empty `System Manager` GameObject. Scripts I almost always have are:
1. `Trial.cs`: contains the definition of a Trial, including a `TrialType` enum and variables for other trial parameters.
2. `TrialManager.cs`: controls what happens when trials start and end, as well as trial progression.
3. A script for e.g. virtual pedestrian management, PLD management, etc. This script typically goes onto another GameObject that serves as an anchor point for e.g. virtual pedestrian path intersection.
4. `InputManager.cs`: detecting inout and activating appropriate functions in other scripts when input is detected.
5. `DataManager.cs`: has methods for recording data into local files or on Google Sheets.
6. `UIManager.cs`: controls what happens when input UI elements are changed and changes UI text to reflect current trial.

## Modularity and flexibility
Program paradigms will often change as time goes on, so it's important to create functions in such a way that making changes to the paradigm is easy. Here are a few recommendations, based on my own experience:

1. `UIManager.cs`:
  1. Generically named `Handle` functions for each UI elements, e.g. `public void HandleAnimationDelaySliderChanged(float sliderValue)` so you can also call methods in e.g. `TrialManager.cs` from the `Handle` function when the slider value is changed.
  2. Generically named `Handle` functions in `TrialManager.cs` that take care of what happens when each trigger is entered or exited (e.g. `public void HandleStartTriggerEntered()` and `public void HandleStartTriggerExited()`) which are called, of course, by `OnTriggerEnter()` or `OnTriggerExit()` functions in scripts attached to each trigger.
  
