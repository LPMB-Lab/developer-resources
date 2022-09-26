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
    - Mathf.Round((sliderValue - minSliderValue) / increment) * increment
- Dropdowns for letting users change the type of trial or sex of virtual pedestrians
  - Common trial types are:
    - Speed, typipcally fewer than 10, in which the participant walks in a straight line over multiple trials to get an average walking speed that can then be used to get the timming of stimuli accurate. Occasionally the walking speed measurement may be done in Practice/Familiarisation trals, with no Speed trials at all.
    - Practice/Familiarisation, in which participants get an opportunity to navigate in virtual reality, or get a "teaser" of what they will have to do in the Actual/Recorded trials.
    - Control, in which no special stimulus occurs. Not all programs will have control trials; instead they may just have Actual/Recorded trials with varying degrees of stimulus, along with Speed and/or Practice/Familiarisation trials at the beginning.
    - Actual/Recorded, in which the participant performs the task on which the study or experiment is based, and the results of which are recorded along with the specifications of the trial.
- Rarely, text boxes to allow users to enter continuous and precise physical information, like the shoulder width of a participant.
